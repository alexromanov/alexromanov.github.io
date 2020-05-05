---
layout: post
title:  "Changing Git history in case "
date:   2020-05-05 00:00:49 +0300
author: "Oleksandr Romanov"
description: "In this blog post I will show a quick hint on how to keep commit history clean by using squash"
header-img: "img/20200503/input.jpg"

---

## Sample application for the test

GIT is version control system which one of the most widespread among the developers around the world.

While using it day by day, bad things can happen:  

- you can commit something not useful (by accident)

- you may want to change commit name (due to mistake)

You may also want to keep commit history clean and have one meaningful commit per change  

instead of filling up the history with a bunch or redundant information  

## Changing commit if it hasn't been pushed to the remote

In case if there is only one commit made and it has not been pushed to a remote repository - it is possible to modify multiple things for a given commit:

- Change commit message:

    ``` console
    git commit --amend
    ```

- Revert last commit (number after ~ states number of commits to get back):  

    ``` console
    git reset --soft HEAD~1
    ```

## Turn multiple commits at one

Making multiple commits as one called Squashing.
Here is how to do it via command line (Git Bash or any other unix terminal):

1. First of all you need to have either default text editor installed on working machine or install new one.  
It can be e.g. Notepad++, Sublime Text, vim, etc.  
Before changing the history, we need to tell Git, which editor to use (for example Notepad++):  

    ``` console
    git config core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
    ```

2. Start the squash process by executing interactive rebase process in a working branch:  

    ``` console
    git rebase -i
## There is an IDEA! Keep it even simpler

##

The feature code for format operation is in FormatterService class.

It has one method "formatMoney()" which takes a String as an input and returns the value as a formatted String.

``` java
public String formatMoney(String inputMoney) {
        log.info("Received value for formatting: {}", inputMoney);
        if (inputMoney == null || inputMoney.isEmpty()) {
            return inputMoney;
        }
        inputMoney = inputMoney.replace(',', '.');
        DecimalFormatSymbols symbols = DecimalFormatSymbols.getInstance();
        symbols.setGroupingSeparator(' ');
        symbols.setDecimalSeparator('.');

        DecimalFormat formatter = new DecimalFormat("###,##0.00", symbols);
        String formatted = "";
        try {
            formatted = formatter.format(Double.parseDouble(inputMoney));
        } catch (NumberFormatException ex) {
            log.error(ex.getMessage());
        }
        log.info("Formatted result: {}", formatted);
        return formatted;
    }
```

In order to cover the test on the unit test level, we can write a lot of tests even for a single method, like:

``` java
  public class FormatterTest {
    private FormatterService formatterService;

    @Before
    public void beforeTest() {
        formatterService = new FormatterService();
    }

    @Test
    public void shouldFormatValue(){
        String input = "123.234";
        String result = "123.23";
        assertThat(formatterService.formatMoney(input)).isEqualTo(result);
    }

    @Test
    public void shouldFormatNegativeValue(){
        String input = "-123.236";
        String result = "-123.24";
        assertThat(formatterService.formatMoney(input)).isEqualTo(result);
    }

    // and so on ...
}
```

But in all cases, it will be testing the behavior of the method from an input data point of view.  

Maybe it is a better way how to deal with data-driven unit tests?

## Parameterized tests with JUnit

JUNit offers an ability to test method with multiple test data using [Parameterized][PARA] runner.  

![Project Structure]({{ site.baseurl }}/img/20200503/parameterized-result.png)  

The steps for adding a parameterized test for formatter are the following:

1. Declare test data
  
   ``` java
   @Parameterized.Parameters
    public static Collection<String[]> data() {
        return Arrays.asList(new String[][]{
                {"2310000.159897", "2 310 000.16"},
                {"1600", "1 600.00"},
                {"0", "0.00"},
                {"-123456.456", "-123 456.46"},
                {"123234,456", "123 234.46"},
                {".", ""},
                {",", ""},
                {"111.", "111.00"},
                {".222", "0.22"},
                {null, null}
        });
    }
    ```
  
2. Declare input and output parameters
  
      ``` java
    @Parameterized.Parameter
    public String input;

    @Parameterized.Parameter(1)
    public String expected;
    ```

3. Initialize service and implement the test

      ``` java
    private FormatterService formatterService;

    @Before
    public void beforeTest() {
        formatterService = new FormatterService();
    }

    @Test
    public void shouldConvertValue() {
        assertThat(expected)
                .as("Invalid result of money conversion")
                .isEqualTo(formatterService.formatMoney(input));
    }
    ```

4. Mark test class to be executed with Parameterized.class from JUnit
  
    ``` java
    @RunWith(Parameterized.class)
    public class FormatterServiceTest {}
    ```  

Full source code of the test available [here][Code]

## Benefits of the data-driven approach

Parameterized testing provides the following benefits:

- Increased focus on test data and corner cases

- Better code reusability and less code duplication

- Can be applicable at any test level: unit, component, integration, end-to-end

## Conclusion

The data-driven approach in unit testing can potentially decrease the number of copy-paste code for unit tests and focus developer more on the test data and corner cases.  

But as with any other testing approaches - it is not a silver bullet and should be used appropriately.  

Service code, as well as the test code, can be found in the [Boot-testing-examples][BTE] repository.  

[BTE]: https://github.com/alexromanov/boot-testing-examples
[PARA]: https://github.com/junit-team/junit4/wiki/Parameterized-tests
[Code]: https://github.com/alexromanov/boot-testing-examples/blob/master/src/test/java/alexromanov/boottestingexamples/service/FormatterServiceTest.java
