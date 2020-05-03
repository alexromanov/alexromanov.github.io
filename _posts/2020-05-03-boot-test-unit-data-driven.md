---
layout: post
title:  "Magic of Spring Boot testing: Data-driven unit tests"
date:   2020-05-03 00:00:49 +0300
author: "Oleksandr Romanov"
description: "In this blog post I will explain how to use a data-driven approach for unit testing"
header-img: "img/20200503/input.jpg"

---

## Sample application for the test

For my series of blog posts about Spring Boot testing, I've prepared a sample application.  

One of the app functionality is formatting input values in the form of two sings after decimal points - XX.YY.  

It has a UI part as well as a REST API part.
UI part takes an input:  

![Project Structure]({{ site.baseurl }}/img/20200503/format-ui.png)  

and returns a result:  

![Project Structure]({{ site.baseurl }}/img/20200503/format-result-ui.png)  

For API part the same functionality is implemented for an "/format" endpoint:  

![Project Structure]({{ site.baseurl }}/img/20200503/post-request.png)  

The result object is:  

![Project Structure]({{ site.baseurl }}/img/20200503/post-response.png)

## Formatter service

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
