---
layout: post
title:  "Magic of Spring Boot testing: UI and API tests"
date:   2021-05-03 00:00:49 +0300
author: "Oleksandr Romanov"
description: "Few samples of UI and API tests for Spring Boot application"
header-img: "img/20210503/simple.jpg"

---

## The case

As a software developer working with Spring Boot, you need to make sure that the application [works as expected][Pyramid].  

That's why, you need to implement unit tests (with [Mockito][Mockito]) and integration tests (with Spring Boot [testing toolkit][Spring Test]).  

But in most cases it is not enough. You need to include at least a few positive end-to-end tests which will check the system as a whole.  

In this blog post, I will show you how to use Spring Boot testing abilities to perform API tests. Also, I will demonstrate an example of UI tests.  

Both of such tests can be executed during the standard building process and provide quick feedback if something is broken.  

## API: MockMvc instead of Rest Assured

For this blog post we will use a sample application - [Money Converter][code].  

If you need to verify some simple scenarios, there is no need to add [Rest Assured][Rest Assured] dependency. Spring Boot's testing toolkit has its own internal library for making HTTP API calls and validating results.

First, you need to autowire MockMvc in the test.

``` java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class MockMvcApiTest {
    @Autowired
    private MockMvc mvc;

    private Gson gson;
```

Then, the test will be as following:

``` java
    @Before
    public void beforeTest() {
        gson = new Gson();
    }

    @Test
    public void shouldReturnConvertedResult() throws Exception {
        FormatRequest request = FormatRequest.builder()
                .inputValue(1600)
                .build();

        String value = gson.toJson(request);

        mvc.perform(post("/format")
                .contentType("application/json")
                .content(value))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.status").value("success"))
                .andExpect(jsonPath("$.initialValue").value("1600.0"))
                .andExpect(jsonPath("$.convertedValue").value("1 600.00"));
    }
```

Inside the test, a new request object is created, converted to JSON format and then passed as a content to the HTTP POST request.  

MockMvc provides a fluent API for checking status codes as well as the response content using [JsonPath][JsonPath].  

For more information regarding MockMvc capabilities, please refer to [official examples][MockMvc].

## Selenium Webdriver with Spring Boot

Writing UI tests inside a Spring Boot application is not hard.  

First, you need to add dependencies to the [Selenium WebDriver][Selenium WebDriver] and [WebDriver Manager][WebDriver Manager] in pom.xml.  

``` xml
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-chrome-driver</artifactId>
            <version>3.141.59</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-support</artifactId>
            <version>3.141.59</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
```

Then the tests should be executed using **@SpringBootTest** annotation. Using such annotation, Spring Boot will initialize the full application context, configure all of the API and UI parts - and as a result - application will be available on localhost.

If you set property **webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)** - then the application port will be randomly defined only during test execution.

In case when you need to find out the exact port - autowire the port using **@LocalServerPort** annotation.

``` java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class WebDriverUiTest {
    private static final String BASE_URL = "http://127.0.0.1";

    @LocalServerPort
    private int port;

    private WebDriver driver;
```

Do not forget to include WebDriver configuration before and after tests:  

``` java
@BeforeClass
    public static void beforeAll() {
        WebDriverManager.chromedriver().setup();
    }

    @Before
    public void beforeTest() {
        driver = new ChromeDriver();
        driver.get(BASE_URL + ":" + port);
    }

    @After
    public void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
```

Selenium tests are the same as for any web-based application: instantiating the driver and pages, making actions and asserting the results.

``` java
      @Test
    public void shouldConvertValue() {
        MainPage mainPage = new MainPage(driver);
        assertThat(mainPage.getPageTitle()).as("Page Title is not equal to expected")
                                           .isEqualTo(driver.getTitle());
        ResultPage resultPage = mainPage.convertValue("1600");
        assertThat(resultPage.getConvertedValue()).contains("1 600.00");
    }
```

## Conclusion

As you can see, it is not complicated to implement basic API and UI tests for a Spring Boot based application.  

The main question is - how many end-to-end tests do you plan to have. In case if number of such tests grows bigger, than a few dozens - it is more suitable to move UI tests into separate projects.  

But end-to-end API tests are better to keep with the source code repository.  

Code examples are accessible in the [boot-testing-examples][code] repository.  

[MockMvc]: https://spring.io/guides/gs/testing-web/
[Mockito]: https://site.mockito.org/
[Spring Test]: https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html
[Selenium WebDriver]: https://www.selenium.dev/documentation/en/webdriver/
[WebDriver Manager]: https://github.com/bonigarcia/webdrivermanager
[JsonPath]: https://github.com/json-path/JsonPath
[code]: https://github.com/alexromanov/boot-testing-examples
[Pyramid]: https://alexromanov.github.io/2018/09/10/microservices-automation-approach/
[Rest Assured]: https://rest-assured.io/