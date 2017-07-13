---
layout: post
title:  "Working with unix init scripts from Serenity"
date:   2017-07-12 08:00:47 +0300
author: "Alexander Romanov"
description: "How to verify whether specific unix init script is running on remote machine?"

---

_In this short blog post I will talk about simple way of checking whether unix services on remote machine are working or not._ 

**Target audience:** QA Automation engineers / SDET 

### The context 

Sometimes while implementing automated tests for some UI website or testing web services, you need to make sure that some specific unix scripts on remote machine are started / stopped or even - you need to restart a couple of such services.  

The following short post will explain, how to integrate such checks into existing Serenity based test automation solution.   

### What are unix init scripts? 

Unix - based systems have a number of predefined System V [init scripts][init-scripts] under /etc/init.d directory as well user - defined ones. In order to manipulate with these scripts (such as starting, stopping or getting it's status) you can use one suitable command - "service".  
The pattern of usage is simple: **service your_script_name chosen_command**.  

**status** command is used for retrieving services status - whether it's running of not.  

**start** and **stop** commands are pretty obvious - used for starting or stopping given service.  

**restart** command performs a full restarting of the given service.  

For our example, I will use such script as [syslog-ng][syslog-ng]. It's an open-source log management solution, which are widely used in development.  

### Algorithm for testing

The method of verification is simple:  
 - connect to the remote machine by SSH;  
 - verify service status before running the command;  
 - perform desired command for the chosen service;  
 - verify that command was successfully run by getting new service's status;  

### Preparing BDD scenarios

For sample purposes, I have prepared the most basic scenarios using init scripts - starting, stopping and restarting - all using service command.  

{% highlight gherkin %}
Scenario: Service should be running by service start command
Meta:
@ssh
Given 'syslog-ng' service is stopped
When admin runs the command 'service syslog-ng start'
Then 'syslog-ng' service should be started

Scenario: Service should be stopped by service stop command
Meta:
@ssh
Given 'syslog-ng' service is started
When admin runs the command 'service syslog-ng stop'
Then 'syslog-ng' service should be stopped

Scenario: Service should be restarted by service service restart command
Meta:
@ssh
Given 'syslog-ng' service is started
When admin runs the command 'service syslog-ng restart'
Then 'syslog-ng' service should be started
{% endhighlight %}

### Implementing scenario steps

{% highlight java %}
public class ServiceDefinitions {
    @Given("'$serviceName' service is stopped")
    public void stopServiceByName(final String serviceName) throws IOException {
        SshUtils.runCommand("service " + serviceName + " stop");
        String output = SshUtils.runCommand("service " + serviceName + " status");
        assertThat("Service is started", output.contains(serviceName + " is not running"));
    }

    @Given("'$serviceName' service is started")
    public void startServiceByName(final String serviceName) throws IOException {
        SshUtils.runCommand("service " + serviceName + " start");
        String output = SshUtils.runCommand("service " + serviceName + " status");
        assertThat("Service is not started", output.contains(serviceName + " is running"));
    }

    @When("admin runs the command '$command'")
    public void runCommandOnRemoteMachine(final String command) throws IOException {
        String output = SshUtils.runCommand(command);
        Serenity.setSessionVariable("output").to(output);
    }

    @Then("'$serviceName' service should be started")
    public void verifyServiceIsStarted(final String serviceName) throws IOException {
        String output = SshUtils.runCommand("service " + serviceName + " status");
        assertThat("Service is not properly started", output.contains(serviceName + " is running"));
    }

    @Then("'$serviceName' service should be stopped")
    public void verifyServiceIsStopped(final String serviceName) throws IOException {
        String output = SshUtils.runCommand("service " + serviceName + " status");
        assertThat("Service is not properly stopped", output.contains(serviceName + " is not running"));
    }
}
{% endhighlight %}

### Connecting to the remote machine

For ssh connection I am using [sshj][sshj] - it is an open - source library for Java. Only host, user login and password are required for the connection. RSA keys can be provided as keys.  

The interesting point here, is the usage of small library - [awaitility][awaitility], for performing smarter waiting operations. In this particular case - waiting till command status will not be equal null. 

{% highlight java %}
public class SshUtils {

    private static final int COMMAND_TIMEOUT = 10;
    private static final String APP_HOST = "127.0.0.1";
    private static final String USER_NAME = "root";
    private static final String USER_PASSWORD = "";

    public static String runCommand(String command) throws IOException {
        String commandOutput = "";
        try (final SSHClient sshClient = new SSHClient()) {
            sshClient.loadKnownHosts();
            sshClient.connect(APP_HOST);
            sshClient.authPassword(USER_NAME, USER_PASSWORD);
            try (Session session = sshClient.startSession()) {
                final Session.Command commandLine = session.exec(command);
                await().atMost(COMMAND_TIMEOUT, TimeUnit.SECONDS).until(() -> commandLine.getExitStatus() != null);
                commandLine.close();
                commandOutput = IOUtils.readFully(commandLine.getInputStream()).toString();
            } catch (IOException e) {
                Logger.out.error("Unable not connect to remote host " + APP_HOST, e);
            }
        }
        return commandOutput;
    }
}
{% endhighlight %}

**NOTE**  
Do not forget to use your own app_host parameter, as well as user_password parameter for SSH connection.  

### Executing scenarios

JBehave scenarios can be run either from maven (mvn integration-test serenity:aggregate) or from Intellij IDEA's run configuration.  

### Conclusions
The usage of service command for verification init scripts is pretty straightforward - just run service status and check the terminal output. The status code of the command can be checked as well. Pay attention that if command was run without errors - the status code will be equal to 0. In other way - status code will be equal some value greater than 0, depends on the type of error which was occurred.   

[init-scripts]: http://www.tutorialspoint.com/unix_commands/service.htm
[syslog-ng]: https://syslog-ng.org/
[sshj]: https://github.com/hierynomus/sshj
[awaitility]: https://github.com/awaitility/awaitility