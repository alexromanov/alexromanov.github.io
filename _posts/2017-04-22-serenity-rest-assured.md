---
layout: post
title:  "Assuring REST with Serenity"
date:   2017-04-22 08:00:47 +0300
author: "Oleksandr Romanov"
description: "Is it possible to test REST services with Serenity framework? Let's try to find it out!"

---

_In this short blog post I will share the tips on how to start writing API tests with Serenity framework and Rest Assured library_ 

**Target audience:** QA Automation engineers  

### Origins of Serenity  

In one of my previous [posts][previous], I wrote couple of words about Serenity and specifically about one of issues, which my team have faced not so long ago. It was tricky investigation, but, fortunately, issue was resolved.  

[Serenity][serenity-base] framework (also known as Thucydides) - it is open - source framework for test automation. Framework has a great integration with Selenium WebDriver; it gives the choice whether to use BDD approach (with Cucumber of JBehave tools) or pure JUnit tests. One of the framework benefits - it is full - featured test report, which can be generated after the test run and used even as "living documentation" for the functionalities.  

**If you want to learn about Serenity more - please start with this great [reference][serenity-reference] manual. It is definitely helplful.**  

### Here comes the REST  

REST architecture approach is widely used in modern software development now. A lot of API's created using this approach and they should be carefully tested and automated too. One of the most used library in Java world for test automation of REST API is [Rest Assured][rest-assured]. It makes REST testing a lot easier by providing good abilities including different types of authentication, powerfull response validation both for XML and JSON based messages and more.

Thankfully, Serenity has an integration with Rest Assured library just 'from the box'.

**Personally, I have used [Rest Assured official examples on GitHub][rest-github-reference] for reference and learning library features. Also, I can highly recommend to use [Bas Dijkstra's blog post series][ontestautomation-rest] on REST test automation (particularly, his Rest Assured training materials also on [GitHub][rest-demo]).**  

### Enough of theory - let's do some practice!  
There are lot of ways of starting new project with Serenity and Rest Assured. I will describe only the simplest one.  
In my current example I will use [JBehave BDD tool][jbehave-reference] with Serenity. For Cucumber tool the steps will be pretty similar.  

 - Create new Java project in Intellij IDEA. While creating - choose to use serenity-jbehave-archetype for generating new Maven - based project;

![Project Structure]({{ site.baseurl }}/img/20170417/jbehave_archetype.png)  

 - Add serenity-rest-assured dependency to the pom.xml;

![Project Structure]({{ site.baseurl }}/img/20170417/add_rest_dependency.png)  

 - Rebuild the project;

### Turn BDD scenarios into real code.  

Our practice example will be based on automating the Google Map API (particularly base and reverse geo coding functionality). 
JBehave BDD scenarios are placed at test/resources/stories/GeoCoding.story file:  

{% highlight gherkin %}

Scenario: Get geocode for Amphitheatre Parkway
Given I have an address 1600 Amphitheatre Parkway, Mountain View, CA
When I request google api for geocode with address
Then I should get response status code 200
And I should get longitude -122.08427 and latitude 37.422348

Scenario: Get geocode for Winnetka
Given I have an address Winnetka
When I request google api for geocode with address
Then I should get response status code 200
And I should get longitude -87.735895 and latitude 42.10808340000001

Scenario: Get reversed geocode for Bedford Avenue
Given I have an longitude and latitude 40.714224,-73.961452
When I request google api for geocode with lontitude and latitude
Then I should get response status code 200
And I should get address 277 Bedford Ave, Brooklyn, NY 11211, USA
{% endhighlight %}

Scenario is set of steps. Each JBehave step has own implementation in Java code. In our case JBehave step implementations will be placed at GeoApiStepDefinitions.class: 

{% highlight java %}
public class GeoApiStepDefinitions {
    @Steps
    private GeoApiActions geoApiActions;

    @Given("I have an address $adr")
    public void givenIHaveAnAddress(String adr) {
        geoApiActions.saveAddress(adr);
    }

    @Given("I have an longitude and latitude $lnglat")
    public void givenIHaveLngLat(String lnglat){
        geoApiActions.saveLongitudeLatitude(lnglat);
    }

    @When("I request google api for geocode with address")
    public void whenIRequestGeoCodeApiWithAddress(){
        geoApiActions.requestGeoCodeWithAddress();
    }

    @When("I request google api for geocode with lontitude and latitude")
    public void whenIRequestGeoCodeWithLatLng(){
        geoApiActions.requestGeoCodeWithLngLat();
    }

    @Then("I should get longitude $lng and latitude $lat")
    public void thenIShouldHaveLongAndLat(float lng, float lat){
        assertThat("Wrong longitude in response", geoApiActions.getResponseLongitude(), equalTo(lng));
        assertThat("Wrong latitude in response", geoApiActions.getResponseLatitude(), equalTo(lat));
    }

    @Then("I should get address $address")
    public void thenIShouldHaveAddress(String address){
        assertThat("Wrong address in response", geoApiActions.getAddress(), equalTo(address));
    }

    @Then("I should get response status code $expectedStatusCode")
    public void verifyStatusCode(int expectedStatusCode){
        assertThat("Wrong response status code", geoApiActions.getStatusCode() ,equalTo(expectedStatusCode));
    }
}
{% endhighlight %}

Serenity framework just adds additional layer of abstraction: now one BDD step can include one or more different Serenity steps. Serenity steps are just methods, but with special annotation @Step. 
We are implementing steps by adding an additional Steps class for our API. Here we will use Serenity-Rest-Assured lib for sending requests and validating responses:  

{% highlight java %}
public class GeoApiActions {

    @Step
    public void saveAddress(String adr){
        Serenity.getCurrentSession().put("address", adr);
    }

    @Step
    public void requestGeoCodeWithAddress(){
        Map<String, String> params = new HashMap<>();
        params.put("address", Serenity.getCurrentSession().get("address").toString());
        params.put("key", "AIzaSyA2fREIe1D2Y48aZ1QQiIcokpgrAHIZ8e0");
        SerenityRest.given().contentType("application/json")
                .and().params(params)
                .when().get("https://maps.googleapis.com/maps/api/geocode/json");
    }

    @Step
    public float getResponseLongitude(){
        return SerenityRest.then().extract().body().jsonPath().get("results.geometry.location.lng[0]");
    }

    @Step
    public float getResponseLatitude(){
        return SerenityRest.then().extract().body().jsonPath().get("results.geometry.location.lat[0]");
    }

    @Step
    public void saveLongitudeLatitude(String lngLat){
        Serenity.getCurrentSession().put("latlng", lngLat);
    }

    @Step
    public String getAddress(){
        return SerenityRest.then().extract().body().jsonPath().get("results.formatted_address[0]");
    }

    @Step
    public void requestGeoCodeWithLngLat(){
        Map<String, String> params = new HashMap<>();
        params.put("latlng", Serenity.getCurrentSession().get("latlng").toString());
        params.put("key", "AIzaSyB4PSrUP4QOqUmfHllvisriD1kntiNiExE");
        SerenityRest.given().contentType("application/json")
                .and().params(params)
                .when().get("https://maps.googleapis.com/maps/api/geocode/json");
    }

    @Step
    public int getStatusCode(){
        return SerenityRest.then().extract().statusCode();
    }
}
{% endhighlight %}

As all parts of our test automation puzzle are revealed - we can run our test scenarios in order to make sure that scenarios are correct and return the expected results.
For this purpose just run AcceptanceTestSuite.class for running all JBehave stories in scope.

![Project Structure]({{ site.baseurl }}/img/20170417/rest_tests_passed.png)

The final step is to generate serenity report by running Maven command “serenity:aggregate”. Report will be generated and placed in target/site/serenity directory of the project. In order to open the report - open index.html file.

### Conclusions
As you can see, it is easy to integrate and use RestAssured library with Serenity framework and BDD approach. Of course, it is separate question -  whether to use RestAssured with an additional level of abstraction as BDD scenarios. But in some cases it can add value - specifically if the project needs a living and understandable documentation for all features, including REST services part.

All code examples available on Github: [serenity-rest][serenity-rest]

[previous]: https://alexromanov.github.io/2017/02/06/serenity-browser-restart/
[serenity-base]: http://www.thucydides.info/#/
[rest-assured]: http://rest-assured.io/
[rest-github-reference]: https://github.com/rest-assured/rest-assured/wiki/Usage
[ontestautomation-rest]: http://www.ontestautomation.com/category/api-testing/
[rest-demo]: https://github.com/basdijkstra/workshops/tree/master/rest-assured
[jbehave-reference]: http://jbehave.org/
[serenity-reference]: http://thucydides.info/docs/serenity-staging/
[serenity-rest]: https://github.com/alexromanov/serenityrest