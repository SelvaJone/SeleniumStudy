# Selenium + TestNG

## Table of Contents

1. [What is TestNG?](#1-what-is-testng)
2. [Why Use TestNG with Selenium?](#2-why-use-testng-with-selenium)
3. [TestNG Features](#3-testng-features)
4. [TestNG Installation](#4-testng-installation)
5. [Basic TestNG Test](#5-basic-testng-test)
6. [TestNG Annotations](#6-testng-annotations)
7. [Annotation Execution Order](#7-annotation-execution-order)
8. [Priority](#8-priority)
9. [Enabled and Disabled Tests](#9-enabled-and-disabled-tests)
10. [Description](#10-description)
11. [TestNG Assertions](#11-testng-assertions)
12. [Hard Assertions](#12-hard-assertions)
13. [Soft Assertions](#13-soft-assertions)
14. [Grouping Tests](#14-grouping-tests)
15. [dependsOnMethods](#15-dependsonmethods)
16. [dependsOnGroups](#16-dependsongroups)
17. [DataProvider](#17-dataprovider)
18. [DataProvider with Selenium](#18-dataprovider-with-selenium)
19. [Parameters](#19-parameters)
20. [TestNG XML](#20-testng-xml)
21. [Running Multiple Classes](#21-running-multiple-classes)
22. [Parallel Execution](#22-parallel-execution)
23. [Parallel DataProvider](#23-parallel-dataprovider)
24. [InvocationCount](#24-invocationcount)
25. [ThreadPoolSize](#25-threadpoolsize)
26. [Timeout](#26-timeout)
27. [Expected Exceptions](#27-expected-exceptions)
28. [Retry Failed Tests](#28-retry-failed-tests)
29. [ITestResult](#29-itestresult)
30. [Listeners](#30-listeners)
31. [Common TestNG Listeners](#31-common-testng-listeners)
32. [ITestListener Example](#32-itestlistener-example)
33. [Screenshots on Failure](#33-screenshots-on-failure)
34. [TestNG with Page Object Model](#34-testng-with-page-object-model)
35. [TestNG with PageFactory](#35-testng-with-pagefactory)
36. [Base Test Class](#36-base-test-class)
37. [TestNG with Selenium Grid](#37-testng-with-selenium-grid)
38. [TestNG + Maven](#38-testng--maven)
39. [Maven Surefire Plugin](#39-maven-surefire-plugin)
40. [TestNG + Cucumber](#40-testng--cucumber)
41. [TestNG Framework Structure](#41-testng-framework-structure)
42. [Complete Framework Example](#42-complete-framework-example)
43. [Important Interview Questions](#43-important-interview-questions)
44. [Best Practices](#44-best-practices)
45. [Quick Revision](#45-quick-revision)

---

# 1. What is TestNG?

**TestNG** stands for **Test Next Generation**.

It is a testing framework for Java inspired by JUnit and designed to provide additional features such as:

* Annotations
* Assertions
* Test grouping
* Test dependencies
* Data-driven testing
* Parallel execution
* Parameterization
* Test listeners
* Retry mechanisms
* Reporting

TestNG is widely used with Selenium WebDriver for automation testing.

---

# 2. Why Use TestNG with Selenium?

Selenium is responsible for browser automation.

TestNG is responsible for organizing and executing tests.

### Selenium

Handles:

* Browser launching
* Locating elements
* Clicking
* Sending text
* Navigation
* Browser interactions

### TestNG

Handles:

* Test execution
* Assertions
* Test ordering
* Test grouping
* Test data
* Parallel execution
* Reporting
* Test dependencies
* Retry logic

### Example

```java
@Test
public void loginTest() {

    driver.get("https://example.com");

    driver.findElement(By.id("username")).sendKeys("admin");
    driver.findElement(By.id("password")).sendKeys("password");
    driver.findElement(By.id("login")).click();

    Assert.assertEquals(driver.getTitle(), "Home");
}
```

---

# 3. TestNG Features

Important TestNG features:

```text
Annotations
Assertions
Groups
Priority
Dependencies
DataProvider
Parameters
Listeners
Parallel execution
Retry failed tests
Timeouts
Expected exceptions
TestNG XML
Reporting
```

---

# 4. TestNG Installation

## Eclipse

Install TestNG through:

```text
Help
→ Eclipse Marketplace
→ Search "TestNG"
→ Install
```

## Maven

Add TestNG dependency to `pom.xml`:

```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.11.0</version>
    <scope>test</scope>
</dependency>
```

Always verify the current compatible version for your project.

---

# 5. Basic TestNG Test

```java
import org.testng.annotations.Test;

public class LoginTest {

    @Test
    public void loginTest() {

        System.out.println("Login Test");
    }
}
```

The `@Test` annotation tells TestNG that the method is a test method.

---

# 6. TestNG Annotations

Important TestNG annotations:

```text
@BeforeSuite
@AfterSuite

@BeforeTest
@AfterTest

@BeforeClass
@AfterClass

@BeforeMethod
@AfterMethod

@Test
```

---

## @BeforeSuite

Runs once before all tests in the suite.

```java
@BeforeSuite
public void beforeSuite() {
    System.out.println("Before Suite");
}
```

---

## @AfterSuite

Runs once after all tests in the suite.

```java
@AfterSuite
public void afterSuite() {
    System.out.println("After Suite");
}
```

---

## @BeforeTest

Runs before the `<test>` section in TestNG XML.

```java
@BeforeTest
public void beforeTest() {
    System.out.println("Before Test");
}
```

---

## @AfterTest

Runs after the `<test>` section.

```java
@AfterTest
public void afterTest() {
    System.out.println("After Test");
}
```

---

## @BeforeClass

Runs once before the first test method in a class.

```java
@BeforeClass
public void setupClass() {
    System.out.println("Before Class");
}
```

---

## @AfterClass

Runs once after all test methods in the class.

```java
@AfterClass
public void tearDownClass() {
    System.out.println("After Class");
}
```

---

## @BeforeMethod

Runs before every `@Test` method.

```java
@BeforeMethod
public void setup() {
    System.out.println("Setup");
}
```

---

## @AfterMethod

Runs after every `@Test` method.

```java
@AfterMethod
public void cleanup() {
    System.out.println("Cleanup");
}
```

---

## @Test

Marks a method as a test.

```java
@Test
public void verifyLogin() {
    System.out.println("Login");
}
```

---

# 7. Annotation Execution Order

Typical order:

```text
@BeforeSuite

@BeforeTest

@BeforeClass

@BeforeMethod

@Test

@AfterMethod

@BeforeMethod

@Test

@AfterMethod

@AfterClass

@AfterTest

@AfterSuite
```

Example:

```java
@BeforeSuite
public void beforeSuite() {}

@BeforeTest
public void beforeTest() {}

@BeforeClass
public void beforeClass() {}

@BeforeMethod
public void beforeMethod() {}

@Test
public void test1() {}

@AfterMethod
public void afterMethod() {}

@AfterClass
public void afterClass() {}

@AfterTest
public void afterTest() {}

@AfterSuite
public void afterSuite() {}
```

---

# 8. Priority

TestNG normally does not guarantee execution order based on source-code order.

Use `priority` when a specific order is required.

```java
@Test(priority = 1)
public void login() {
}

@Test(priority = 2)
public void search() {
}

@Test(priority = 3)
public void logout() {
}
```

Execution:

```text
login
search
logout
```

Lower priority values execute first.

Example:

```java
@Test(priority = 0)
public void testA() {}

@Test(priority = 1)
public void testB() {}

@Test(priority = 2)
public void testC() {}
```

---

# 9. Enabled and Disabled Tests

## Disable a Test

```java
@Test(enabled = false)
public void testLogin() {
}
```

This test will not execute.

## Enable a Test

```java
@Test(enabled = true)
public void testSearch() {
}
```

`enabled = true` is the default.

---

# 10. Description

You can provide a description.

```java
@Test(
    description = "Verify user can login with valid credentials"
)
public void loginTest() {
}
```

Descriptions are useful in reports.

---

# 11. TestNG Assertions

Assertions validate expected versus actual results.

Common assertions:

```text
assertEquals()
assertNotEquals()
assertTrue()
assertFalse()
assertNull()
assertNotNull()
```

Import:

```java
import org.testng.Assert;
```

Example:

```java
Assert.assertEquals(actual, expected);
```

---

# 12. Hard Assertions

Hard assertion stops the current test when the assertion fails.

```java
Assert.assertEquals(actualTitle, expectedTitle);

System.out.println("This line executes only if assertion passes");
```

Example:

```java
@Test
public void verifyTitle() {

    String actualTitle = driver.getTitle();

    Assert.assertEquals(
        actualTitle,
        "Expected Title"
    );
}
```

If the assertion fails, the remaining statements in that test method are not executed.

---

# 13. Soft Assertions

Soft assertions allow multiple validations before marking the test as failed.

```java
import org.testng.asserts.SoftAssert;

@Test
public void verifyPage() {

    SoftAssert softAssert = new SoftAssert();

    softAssert.assertEquals(
        driver.getTitle(),
        "Expected Title"
    );

    softAssert.assertTrue(
        driver.findElement(By.id("logo")).isDisplayed()
    );

    softAssert.assertTrue(
        driver.findElement(By.id("menu")).isDisplayed()
    );

    softAssert.assertAll();
}
```

### Important

Always call:

```java
softAssert.assertAll();
```

Otherwise TestNG may not report the soft assertion failures.

---

# 14. Grouping Tests

Groups allow you to categorize tests.

```java
@Test(groups = "smoke")
public void loginTest() {
}

@Test(groups = "regression")
public void searchTest() {
}

@Test(groups = "regression")
public void paymentTest() {
}
```

A test can belong to multiple groups.

```java
@Test(groups = {"smoke", "regression"})
public void loginTest() {
}
```

---

## Run Groups Through XML

```xml
<groups>
    <run>
        <include name="smoke"/>
    </run>
</groups>
```

Exclude:

```xml
<groups>
    <run>
        <exclude name="regression"/>
    </run>
</groups>
```

---

# 15. dependsOnMethods

One test can depend on another.

```java
@Test
public void login() {
}

@Test(dependsOnMethods = "login")
public void searchProduct() {
}
```

Execution:

```text
login
searchProduct
```

If `login` fails, `searchProduct` may be skipped.

Multiple dependencies:

```java
@Test(
    dependsOnMethods = {
        "login",
        "search"
    }
)
public void checkout() {
}
```

---

# 16. dependsOnGroups

A test can depend on a group.

```java
@Test(groups = "login")
public void login() {
}
```

```java
@Test(dependsOnGroups = "login")
public void checkout() {
}
```

---

# 17. DataProvider

`@DataProvider` is used for data-driven testing.

Example:

```java
@DataProvider(name = "loginData")
public Object[][] loginData() {

    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}
```

Use it:

```java
@Test(dataProvider = "loginData")
public void loginTest(String username, String password) {

    System.out.println(username);
    System.out.println(password);
}
```

Execution:

```text
user1 / pass1
user2 / pass2
user3 / pass3
```

---

# 18. DataProvider with Selenium

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.*;

public class LoginTest {

    WebDriver driver;

    @BeforeMethod
    public void setup() {
        driver = new ChromeDriver();
        driver.get("https://example.com/login");
    }

    @DataProvider(name = "loginData")
    public Object[][] loginData() {

        return new Object[][] {
            {"user1", "pass1"},
            {"user2", "pass2"},
            {"user3", "pass3"}
        };
    }

    @Test(dataProvider = "loginData")
    public void loginTest(String username, String password) {

        driver.findElement(By.id("username"))
              .sendKeys(username);

        driver.findElement(By.id("password"))
              .sendKeys(password);

        driver.findElement(By.id("login"))
              .click();
    }

    @AfterMethod
    public void tearDown() {

        if (driver != null) {
            driver.quit();
        }
    }
}
```

---

# 19. Parameters

TestNG parameters can be passed through `testng.xml`.

Java:

```java
@Parameters("browser")
@Test
public void launchBrowser(String browser) {

    System.out.println(browser);
}
```

XML:

```xml
<parameter name="browser" value="chrome"/>
```

Complete example:

```xml
<suite name="Automation Suite">

    <test name="Chrome Test">

        <parameter
            name="browser"
            value="chrome"/>

        <classes>
            <class name="tests.LoginTest"/>
        </classes>

    </test>

</suite>
```

---

# 20. TestNG XML

`testng.xml` is used to control test execution.

Basic example:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
    "https://testng.org/testng-1.0.dtd">

<suite name="Selenium Suite">

    <test name="Smoke Tests">

        <classes>

            <class name="tests.LoginTest"/>
            <class name="tests.SearchTest"/>

        </classes>

    </test>

</suite>
```

---

# 21. Running Multiple Classes

```xml
<suite name="Regression Suite">

    <test name="Regression">

        <classes>

            <class name="tests.LoginTest"/>
            <class name="tests.SearchTest"/>
            <class name="tests.CartTest"/>
            <class name="tests.PaymentTest"/>

        </classes>

    </test>

</suite>
```

---

# 22. Parallel Execution

TestNG supports parallel execution.

Example:

```xml
<suite
    name="Parallel Suite"
    parallel="tests"
    thread-count="3">

    <test name="Chrome">
        <classes>
            <class name="tests.ChromeTest"/>
        </classes>
    </test>

    <test name="Firefox">
        <classes>
            <class name="tests.FirefoxTest"/>
        </classes>
    </test>

    <test name="Edge">
        <classes>
            <class name="tests.EdgeTest"/>
        </classes>
    </test>

</suite>
```

Other options:

```text
parallel="tests"
parallel="classes"
parallel="methods"
```

### Important

When Selenium tests run in parallel, each thread should have its own WebDriver instance.

A common approach is `ThreadLocal<WebDriver>`.

Example:

```java
private static ThreadLocal<WebDriver> driver =
        new ThreadLocal<>();

public static WebDriver getDriver() {
    return driver.get();
}

public static void setDriver(WebDriver webDriver) {
    driver.set(webDriver);
}

public static void unload() {
    driver.remove();
}
```

---

# 23. Parallel DataProvider

DataProvider can also execute tests in parallel.

```java
@DataProvider(
    name = "loginData",
    parallel = true
)
public Object[][] loginData() {

    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}
```

This is useful for large data-driven test suites.

---

# 24. InvocationCount

Execute the same test multiple times.

```java
@Test(invocationCount = 5)
public void testLogin() {

    System.out.println("Login Test");
}
```

The test runs five times.

---

# 25. ThreadPoolSize

`threadPoolSize` controls the number of threads used when invoking a test multiple times.

```java
@Test(
    invocationCount = 10,
    threadPoolSize = 3
)
public void testSearch() {

    System.out.println(
        Thread.currentThread().getId()
    );
}
```

Ten invocations are executed using three threads.

---

# 26. Timeout

TestNG can fail a test if it takes longer than the specified time.

```java
@Test(timeOut = 5000)
public void testPageLoad() {

}
```

`5000` means 5000 milliseconds.

Equivalent:

```text
5 seconds
```

---

# 27. Expected Exceptions

TestNG can verify that a specific exception is thrown.

```java
@Test(
    expectedExceptions = ArithmeticException.class
)
public void divisionTest() {

    int result = 10 / 0;
}
```

The test passes because `ArithmeticException` is expected.

Multiple exceptions:

```java
@Test(
    expectedExceptions = {
        NullPointerException.class,
        ArithmeticException.class
    }
)
public void exceptionTest() {
}
```

Use this carefully. For most Selenium validations, explicit assertions and controlled exception handling are easier to understand.

---

# 28. Retry Failed Tests

Sometimes a test fails because of:

* Temporary network issue
* Browser instability
* Application timing issue
* Environment issue

TestNG allows retrying failed tests.

Create a retry analyzer:

```java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer
        implements IRetryAnalyzer {

    private int count = 0;

    private final int maxRetryCount = 2;

    @Override
    public boolean retry(ITestResult result) {

        if (count < maxRetryCount) {
            count++;
            return true;
        }

        return false;
    }
}
```

Use it:

```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void loginTest() {

}
```

### Best Practice

Do not use retries to hide genuine application defects.

---

# 29. ITestResult

`ITestResult` provides information about test execution.

Example:

```java
public void getResult(ITestResult result) {

    String testName = result.getName();

    System.out.println(testName);

    if (result.getStatus() == ITestResult.FAILURE) {
        System.out.println("Test Failed");
    }
}
```

Useful status values:

```java
ITestResult.SUCCESS
ITestResult.FAILURE
ITestResult.SKIP
```

---

# 30. Listeners

Listeners allow you to monitor test execution.

Common use cases:

* Screenshot on failure
* Logging
* Reporting
* Test start/end events
* Failure handling
* Retry handling

Important listener interfaces include:

```text
ITestListener
ISuiteListener
IInvokedMethodListener
IAnnotationTransformer
IRetryAnalyzer
```

---

# 31. Common TestNG Listeners

## ITestListener

Main methods:

```java
onStart()
onTestStart()
onTestSuccess()
onTestFailure()
onTestSkipped()
onFinish()
```

## ISuiteListener

```java
onStart()
onFinish()
```

Useful for suite-level setup and cleanup.

## IInvokedMethodListener

Useful for actions before and after methods are invoked.

```java
beforeInvocation()
afterInvocation()
```

---

# 32. ITestListener Example

```java
import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener
        implements ITestListener {

    @Override
    public void onTestStart(
            ITestResult result) {

        System.out.println(
            "Test Started: "
            + result.getName()
        );
    }

    @Override
    public void onTestSuccess(
            ITestResult result) {

        System.out.println(
            "Test Passed: "
            + result.getName()
        );
    }

    @Override
    public void onTestFailure(
            ITestResult result) {

        System.out.println(
            "Test Failed: "
            + result.getName()
        );
    }

    @Override
    public void onTestSkipped(
            ITestResult result) {

        System.out.println(
            "Test Skipped: "
            + result.getName()
        );
    }

    @Override
    public void onStart(
            ITestContext context) {

        System.out.println(
            "Suite Started"
        );
    }

    @Override
    public void onFinish(
            ITestContext context) {

        System.out.println(
            "Suite Finished"
        );
    }
}
```

Register the listener:

```java
@Listeners(TestListener.class)
public class LoginTest {

    @Test
    public void loginTest() {
    }
}
```

---

# 33. Screenshots on Failure

A listener can capture screenshots automatically.

Example:

```java
@Override
public void onTestFailure(ITestResult result) {

    Object currentClass =
        result.getInstance();

    WebDriver driver =
        ((BaseTest) currentClass).getDriver();

    TakesScreenshot screenshot =
        (TakesScreenshot) driver;

    File source =
        screenshot.getScreenshotAs(
            OutputType.FILE
        );

    File destination =
        new File(
            "screenshots/"
            + result.getName()
            + ".png"
        );

    try {
        Files.copy(
            source.toPath(),
            destination.toPath(),
            StandardCopyOption.REPLACE_EXISTING
        );
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

This is commonly implemented in a reusable framework utility.

---

# 34. TestNG with Page Object Model

A clean Selenium framework separates:

```text
Test Classes
Page Classes
Utilities
Driver Management
Test Data
Configuration
Listeners
```

Example:

```text
src
└── test
    └── java
        ├── pages
        │   ├── LoginPage.java
        │   └── HomePage.java
        │
        ├── tests
        │   └── LoginTest.java
        │
        └── base
            └── BaseTest.java
```

---

# 35. TestNG with PageFactory

Page class:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {

    private WebDriver driver;

    @FindBy(id = "username")
    private WebElement username;

    @FindBy(id = "password")
    private WebElement password;

    @FindBy(id = "login")
    private WebElement loginButton;

    public LoginPage(WebDriver driver) {

        this.driver = driver;

        PageFactory.initElements(
            driver,
            this
        );
    }

    public void login(
            String user,
            String pass) {

        username.sendKeys(user);
        password.sendKeys(pass);
        loginButton.click();
    }
}
```

Test class:

```java
public class LoginTest extends BaseTest {

    @Test
    public void loginTest() {

        LoginPage loginPage =
            new LoginPage(driver);

        loginPage.login(
            "admin",
            "password"
        );
    }
}
```

---

# 36. Base Test Class

A base class can centralize WebDriver setup and cleanup.

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.*;

public class BaseTest {

    protected WebDriver driver;

    @BeforeMethod
    public void setup() {

        driver = new ChromeDriver();

        driver.manage()
              .window()
              .maximize();

        driver.get(
            "https://example.com"
        );
    }

    @AfterMethod
    public void tearDown() {

        if (driver != null) {
            driver.quit();
        }
    }
}
```

Test:

```java
public class LoginTest extends BaseTest {

    @Test
    public void loginTest() {

        System.out.println(
            driver.getTitle()
        );
    }
}
```

---

# 37. TestNG with Selenium Grid

TestNG can execute Selenium tests on Grid.

Example:

```java
URL gridUrl =
    new URL("http://localhost:4444");

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        new ChromeOptions()
    );
```

TestNG XML can define different browser tests.

```xml
<suite
    name="Grid Suite"
    parallel="tests"
    thread-count="3">

    <test name="Chrome">
        <parameter
            name="browser"
            value="chrome"/>

        <classes>
            <class name="tests.LoginTest"/>
        </classes>
    </test>

    <test name="Firefox">
        <parameter
            name="browser"
            value="firefox"/>

        <classes>
            <class name="tests.LoginTest"/>
        </classes>
    </test>

</suite>
```

This allows the same tests to execute against multiple browser configurations.

---

# 38. TestNG + Maven

Typical project structure:

```text
SeleniumStudy
│
├── pom.xml
│
└── src
    ├── main
    │   └── java
    │
    └── test
        └── java
            ├── tests
            ├── pages
            ├── utilities
            └── base
```

Example dependencies:

```xml
<dependencies>

    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.x.x</version>
    </dependency>

    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.x.x</version>
        <scope>test</scope>
    </dependency>

</dependencies>
```

Use the current compatible versions in your project rather than copying version placeholders blindly.

---

# 39. Maven Surefire Plugin

The Maven Surefire Plugin is commonly used to execute TestNG tests.

Example:

```xml
<build>

    <plugins>

        <plugin>
            <groupId>
                org.apache.maven.plugins
            </groupId>

            <artifactId>
                maven-surefire-plugin
            </artifactId>

            <version>3.5.3</version>

            <configuration>

                <suiteXmlFiles>

                    <suiteXmlFile>
                        testng.xml
                    </suiteXmlFile>

                </suiteXmlFiles>

            </configuration>

        </plugin>

    </plugins>

</build>
```

Run:

```bash
mvn test
```

---

# 40. TestNG + Cucumber

TestNG can also be used as a Cucumber test runner.

Example:

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = "stepdefinitions",
    plugin = {
        "pretty",
        "html:target/cucumber-report.html"
    }
)
public class TestRunner
        extends AbstractTestNGCucumberTests {
}
```

This allows Cucumber scenarios to be executed through TestNG.

---

# 41. TestNG Framework Structure

A production-style Selenium + Java + TestNG framework can look like:

```text
SeleniumFramework
│
├── pom.xml
├── testng.xml
├── README.md
│
├── src
│   ├── main
│   │   └── java
│   │       ├── base
│   │       │   └── DriverFactory.java
│   │       │
│   │       ├── pages
│   │       │   ├── LoginPage.java
│   │       │   └── HomePage.java
│   │       │
│   │       └── utilities
│   │           ├── ConfigReader.java
│   │           ├── ExcelReader.java
│   │           └── ScreenshotUtil.java
│   │
│   └── test
│       └── java
│           ├── tests
│           │   ├── LoginTest.java
│           │   └── SearchTest.java
│           │
│           ├── listeners
│           │   └── TestListener.java
│           │
│           └── data
│               └── TestData.java
│
└── reports
```

---

# 42. Complete Framework Example

## DriverFactory.java

```java
public class DriverFactory {

    private static ThreadLocal<WebDriver> driver =
        new ThreadLocal<>();

    public static void initializeDriver(
            String browser) {

        if (browser.equalsIgnoreCase("chrome")) {

            driver.set(new ChromeDriver());

        } else if (
            browser.equalsIgnoreCase("firefox")) {

            driver.set(new FirefoxDriver());

        } else {

            throw new IllegalArgumentException(
                "Unsupported browser: "
                + browser
            );
        }

        getDriver()
            .manage()
            .window()
            .maximize();
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void quitDriver() {

        if (driver.get() != null) {

            driver.get().quit();

            driver.remove();
        }
    }
}
```

---

## BaseTest.java

```java
public class BaseTest {

    @Parameters("browser")
    @BeforeMethod
    public void setup(String browser) {

        DriverFactory.initializeDriver(
            browser
        );

        DriverFactory.getDriver().get(
            "https://example.com"
        );
    }

    @AfterMethod
    public void tearDown() {

        DriverFactory.quitDriver();
    }
}
```

---

## LoginPage.java

```java
public class LoginPage {

    private WebDriver driver;

    @FindBy(id = "username")
    private WebElement username;

    @FindBy(id = "password")
    private WebElement password;

    @FindBy(id = "login")
    private WebElement loginButton;

    public LoginPage(WebDriver driver) {

        this.driver = driver;

        PageFactory.initElements(
            driver,
            this
        );
    }

    public void login(
            String user,
            String password) {

        username.sendKeys(user);

        this.password.sendKeys(password);

        loginButton.click();
    }
}
```

---

## LoginTest.java

```java
public class LoginTest extends BaseTest {

    @Test(
        groups = {"smoke", "regression"},
        description =
            "Verify valid user login"
    )
    public void loginTest() {

        LoginPage loginPage =
            new LoginPage(
                DriverFactory.getDriver()
            );

        loginPage.login(
            "admin",
            "password"
        );

        Assert.assertTrue(
            DriverFactory.getDriver()
                .getTitle()
                .contains("Home")
        );
    }
}
```

---

## testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
    "https://testng.org/testng-1.0.dtd">

<suite
    name="Selenium Suite"
    parallel="tests"
    thread-count="2">

    <test name="Chrome Tests">

        <parameter
            name="browser"
            value="chrome"/>

        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>

        <classes>
            <class name="tests.LoginTest"/>
        </classes>

    </test>

    <test name="Firefox Tests">

        <parameter
            name="browser"
            value="firefox"/>

        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>

        <classes>
            <class name="tests.LoginTest"/>
        </classes>

    </test>

</suite>
```

---

# 43. Important Interview Questions

## Beginner

### 1. What is TestNG?

TestNG is a Java testing framework used to organize and execute automated tests.

### 2. Why use TestNG with Selenium?

Selenium performs browser automation while TestNG provides test execution, assertions, grouping, data-driven testing, dependencies, parallel execution, listeners, and reporting capabilities.

### 3. What is `@Test`?

It marks a method as a TestNG test method.

### 4. What is `@BeforeMethod`?

It executes before every `@Test` method.

### 5. What is `@AfterMethod`?

It executes after every `@Test` method.

### 6. What is `testng.xml`?

It is a configuration file used to define and control TestNG test execution.

---

## Intermediate

### 7. Difference between `@BeforeClass` and `@BeforeMethod`

```text
@BeforeClass
    Runs once before the first test method in a class.

@BeforeMethod
    Runs before every test method.
```

### 8. What is DataProvider?

DataProvider supplies multiple sets of data to a test method.

### 9. What is the difference between parameters and DataProvider?

```text
Parameters
    Usually pass configuration values through XML.

DataProvider
    Supplies multiple test-data sets to a test method.
```

### 10. What is `dependsOnMethods`?

It establishes a dependency between test methods.

### 11. What is `priority`?

It controls the execution order of test methods when explicit ordering is needed.

### 12. What are groups?

Groups categorize tests such as:

```text
smoke
sanity
regression
integration
```

---

## Advanced

### 13. How do you execute TestNG tests in parallel?

Configure `parallel` and `thread-count` in TestNG XML.

```xml
<suite
    parallel="tests"
    thread-count="4">
```

### 14. How do you make WebDriver thread-safe?

Use `ThreadLocal<WebDriver>` so each execution thread gets its own driver.

### 15. How do you capture screenshots after failures?

Implement `ITestListener` and capture a screenshot inside `onTestFailure()`.

### 16. How do you retry failed tests?

Implement `IRetryAnalyzer`.

### 17. How do you execute only smoke tests?

Use TestNG groups.

```xml
<groups>
    <run>
        <include name="smoke"/>
    </run>
</groups>
```

### 18. How do you execute tests on multiple browsers?

Pass browser values using TestNG parameters or a DataProvider.

### 19. How do you run TestNG tests through Maven?

Configure the Maven Surefire Plugin and run:

```bash
mvn test
```

### 20. How do you execute Selenium tests in parallel on Grid?

Use:

```text
TestNG
+
parallel execution
+
RemoteWebDriver
+
Selenium Grid
```

---

# 44. Best Practices

## 1. Keep test methods small

Prefer:

```java
@Test
public void verifyLogin() {
}
```

Instead of one huge method containing every application flow.

---

## 2. Use Page Objects

Keep locators and page interactions inside page classes.

```text
Test → Page → WebDriver
```

---

## 3. Avoid Thread.sleep()

Do not rely on:

```java
Thread.sleep(5000);
```

Prefer explicit waits:

```java
WebDriverWait wait =
    new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
    );

wait.until(
    ExpectedConditions
        .visibilityOfElementLocated(
            By.id("username")
        )
);
```

---

## 4. Use meaningful test names

Good:

```java
verifyUserCanLoginWithValidCredentials()
```

Avoid:

```java
test1()
```

---

## 5. Use groups

Organize tests into:

```text
smoke
sanity
regression
integration
```

---

## 6. Use DataProvider for test data

Avoid hard-coding many test cases.

```java
@Test(dataProvider = "loginData")
```

---

## 7. Use listeners for common functionality

Listeners are useful for:

```text
Screenshots
Logging
Reporting
Failure handling
Test lifecycle events
```

---

## 8. Do not create one WebDriver for the entire parallel suite

For parallel execution, use separate WebDriver instances per thread.

---

## 9. Do not use retries to hide defects

A retry should handle temporary instability, not mask application failures.

---

## 10. Keep configuration separate

Store:

```text
Browser
Environment
URL
Timeouts
Credentials/configuration
```

outside the test classes whenever practical.

---

# 45. Quick Revision

## TestNG Annotations

```text
@BeforeSuite
@AfterSuite

@BeforeTest
@AfterTest

@BeforeClass
@AfterClass

@BeforeMethod
@AfterMethod

@Test
```

## Assertions

```java
Assert.assertEquals()
Assert.assertNotEquals()
Assert.assertTrue()
Assert.assertFalse()
Assert.assertNull()
Assert.assertNotNull()
```

## Data Driven Testing

```java
@DataProvider
```

## Test Organization

```java
priority
groups
dependsOnMethods
dependsOnGroups
```

## Execution

```text
testng.xml
parallel
thread-count
invocationCount
threadPoolSize
```

## Failure Handling

```text
ITestListener
IRetryAnalyzer
ITestResult
```

## Selenium Framework

```text
TestNG
   ↓
BaseTest
   ↓
DriverFactory
   ↓
Page Objects
   ↓
Selenium WebDriver
```

## Senior-Level Selenium + TestNG Stack

```text
Java
 ↓
Selenium WebDriver
 ↓
TestNG
 ↓
Page Object Model
 ↓
PageFactory / component objects
 ↓
DataProvider
 ↓
Listeners
 ↓
Retry Analyzer
 ↓
ThreadLocal WebDriver
 ↓
Selenium Grid
 ↓
Maven
 ↓
Jenkins / CI
 ↓
Reports
```

---

# Key Takeaways

```text
TestNG = Test execution framework

Selenium = Browser automation tool

@DataProvider = Data-driven testing

@Parameters = Configuration/input parameters

@BeforeMethod = Before every test

@AfterMethod = After every test

@BeforeClass = Once before class tests

@AfterClass = Once after class tests

priority = Test ordering

groups = Test categorization

dependsOnMethods = Test dependency

parallel = Parallel execution

ITestListener = Test lifecycle/listening

IRetryAnalyzer = Retry failed tests

testng.xml = Test execution configuration

ThreadLocal<WebDriver> = Thread-safe driver management
```

A strong Selenium automation framework commonly combines:

```text
Selenium
+
Java
+
TestNG
+
Page Object Model
+
DataProvider
+
Listeners
+
ThreadLocal
+
Selenium Grid
+
Maven
+
CI/CD
```
