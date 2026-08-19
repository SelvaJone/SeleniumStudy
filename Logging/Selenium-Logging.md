# Selenium Logging

## 1. Introduction

Logging is an important part of a Selenium automation framework.

Logs help us understand:

- What the test is doing
- Which test step was executed
- Where a test failed
- What exception occurred
- Which browser and environment were used
- How long an operation took
- What data was passed to a method
- What happened before and after a failure

A good logging strategy makes debugging automation failures much easier.

---

# 2. Why Logging Is Important in Selenium

Without logging:

```text
Test Failed
2026-08-19 10:30:01 INFO  - Starting Login Test
2026-08-19 10:30:02 INFO  - Opening application
2026-08-19 10:30:04 INFO  - Entering username
2026-08-19 10:30:05 INFO  - Entering password
2026-08-19 10:30:06 INFO  - Clicking Login button
2026-08-19 10:30:10 ERROR - Login button was not clickable
2026-08-19 10:30:10 ERROR - ElementClickInterceptedException
3. Common Logging Frameworks in Java

Common Java logging technologies include:

Framework	Description
java.util.logging	Built into Java
Log4j	Popular logging framework
Log4j2	Modern version of Log4j
SLF4J	Logging abstraction/API
Logback	Logging implementation commonly used with SLF4J

For Selenium automation frameworks, Log4j2 + SLF4J is a common combination.

4. Logging Levels

The most commonly used logging levels are:

TRACE
DEBUG
INFO
WARN
ERROR
FATAL

The general order is:

TRACE
  ↓
DEBUG
  ↓
INFO
  ↓
WARN
  ↓
ERROR
  ↓
FATAL
TRACE

Very detailed diagnostic information.

logger.trace("Entering login method");

Usually used only when detailed debugging is required.

DEBUG

Useful for developer-level debugging.

logger.debug("Username value received");
INFO

Used for normal application/test execution information.

logger.info("Opening application");

This is commonly used in Selenium frameworks.

WARN

Used for potentially problematic situations.

logger.warn("Element took longer than expected to load");
ERROR

Used when an operation fails.

logger.error("Unable to click Login button");
FATAL

Represents a very serious failure.

logger.fatal("Unable to initialize WebDriver");

It is less commonly required in Selenium test frameworks.

5. Java Built-in Logger

Java provides a built-in logging API.

import java.util.logging.Logger;


public class LoginTest {


    private static final Logger logger =
            Logger.getLogger(LoginTest.class.getName());


    public void login() {


        logger.info("Starting login test");


        logger.info("Entering username");


        logger.info("Entering password");


        logger.info("Clicking login button");
    }
}
6. Log4j2

Log4j2 is widely used in Java automation frameworks.

Typical architecture:

Selenium Test
      |
      v
Logging Utility
      |
      v
SLF4J / Log4j2
      |
      v
Console / File

Logs can be written to:

Console
File
Rolling File
Database
External logging systems
7. Maven Dependencies for Log4j2

Example Maven dependencies:

<dependencies>


    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.25.1</version>
    </dependency>


    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.25.1</version>
    </dependency>


</dependencies>

Always use the current approved version in your organization/project rather than blindly copying an old version.

8. Basic Log4j2 Example
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;


public class LoginTest {


    private static final Logger logger =
            LogManager.getLogger(LoginTest.class);


    public void login() {


        logger.info("Starting login test");


        logger.debug("Preparing login page");


        logger.warn("Login page is taking longer than expected");


        logger.error("Login failed");
    }
}
9. Log4j2 Configuration

Create:

src
 └── test
     └── resources
         └── log4j2.xml

Example:

<?xml version="1.0" encoding="UTF-8"?>


<Configuration status="WARN">


    <Appenders>


        <Console name="Console" target="SYSTEM_OUT">


            <PatternLayout
                pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n"/>


        </Console>


        <File name="File"
              fileName="logs/automation.log">


            <PatternLayout
                pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n"/>


        </File>


    </Appenders>


    <Loggers>


        <Root level="INFO">


            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>


        </Root>


    </Loggers>


</Configuration>
10. Example Log Output

The configuration above can produce:

2026-08-19 10:15:01 INFO  LoginTest - Starting login test
2026-08-19 10:15:02 INFO  LoginTest - Opening application
2026-08-19 10:15:03 INFO  LoginTest - Entering username
2026-08-19 10:15:04 INFO  LoginTest - Entering password
2026-08-19 10:15:05 INFO  LoginTest - Clicking login button
2026-08-19 10:15:07 INFO  LoginTest - Login successful
11. Logging Selenium WebDriver Actions

Logging should be added around important Selenium operations.

logger.info("Opening application");


driver.get("https://example.com");


logger.info("Application opened successfully");

Element interaction:

logger.info("Entering username");


username.sendKeys("testuser");


logger.info("Username entered successfully");

Click:

logger.info("Clicking Login button");


loginButton.click();


logger.info("Login button clicked successfully");
12. Logging Navigation
logger.info("Navigating to login page");


driver.get("https://example.com/login");


logger.info("Current URL: {}", driver.getCurrentUrl());
13. Logging Browser Information
logger.info("Browser: {}", browserName);


logger.info("Current URL: {}", driver.getCurrentUrl());


logger.info("Page title: {}", driver.getTitle());

Example:

INFO - Browser: Chrome
INFO - Current URL: https://example.com/login
INFO - Page title: Login
14. Logging Exceptions

Use try-catch when you need to log additional context.

try {


    loginButton.click();


    logger.info("Login button clicked successfully");


} catch (Exception e) {


    logger.error("Failed to click Login button", e);


    throw e;
}

Passing the exception object is important:

logger.error("Login failed", e);

This allows the stack trace to be captured.

15. Logging in Page Object Classes

Example:

public class LoginPage {


    private static final Logger logger =
            LogManager.getLogger(LoginPage.class);


    private WebDriver driver;


    private By username =
            By.id("username");


    private By password =
            By.id("password");


    private By loginButton =
            By.id("login");


    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }


    public void enterUsername(String value) {


        logger.info("Entering username");


        driver.findElement(username).sendKeys(value);
    }


    public void enterPassword(String value) {


        logger.info("Entering password");


        driver.findElement(password).sendKeys(value);
    }


    public void clickLogin() {


        logger.info("Clicking Login button");


        driver.findElement(loginButton).click();
    }
}
16. Do Not Log Passwords

Never do this:

logger.info("Password: {}", password);

This can expose sensitive information in logs.

Instead:

logger.info("Entering password");

Similarly, avoid logging:

Passwords
Authentication tokens
API keys
Session tokens
Credit card information
Personally sensitive information
17. Logging TestNG Tests

Example:

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.testng.annotations.Test;


public class LoginTest {


    private static final Logger logger =
            LogManager.getLogger(LoginTest.class);


    @Test
    public void validLoginTest() {


        logger.info("===== Test Started =====");


        logger.info("Launching application");


        logger.info("Performing login");


        logger.info("Validating home page");


        logger.info("===== Test Completed =====");
    }
}
18. Logging @BeforeMethod and @AfterMethod
@BeforeMethod
public void setUp() {


    logger.info("Starting browser setup");


    driver = new ChromeDriver();


    logger.info("Chrome browser started");
}
@AfterMethod
public void tearDown() {


    logger.info("Closing browser");


    if (driver != null) {
        driver.quit();
    }


    logger.info("Browser closed");
}
19. Logging Test Name

TestNG provides ITestResult.

import org.testng.ITestResult;


@AfterMethod
public void tearDown(ITestResult result) {


    logger.info("Test Name: {}", result.getName());


    if (result.getStatus() == ITestResult.SUCCESS) {


        logger.info("Test Passed");


    } else if (result.getStatus() == ITestResult.FAILURE) {


        logger.error("Test Failed");


    } else {


        logger.warn("Test Skipped");
    }


    driver.quit();
}
20. Logging Test Status
switch (result.getStatus()) {


    case ITestResult.SUCCESS:
        logger.info("Test PASSED");
        break;


    case ITestResult.FAILURE:
        logger.error("Test FAILED");
        break;


    case ITestResult.SKIP:
        logger.warn("Test SKIPPED");
        break;


    default:
        logger.warn("Unknown test status");
}
21. Logging with TestNG Listeners

Listeners can centralize logging.

public class TestListener implements ITestListener {


    private static final Logger logger =
            LogManager.getLogger(TestListener.class);


    @Override
    public void onTestStart(ITestResult result) {


        logger.info(
            "Test Started: {}",
            result.getName()
        );
    }


    @Override
    public void onTestSuccess(ITestResult result) {


        logger.info(
            "Test Passed: {}",
            result.getName()
        );
    }


    @Override
    public void onTestFailure(ITestResult result) {


        logger.error(
            "Test Failed: {}",
            result.getName(),
            result.getThrowable()
        );
    }


    @Override
    public void onTestSkipped(ITestResult result) {


        logger.warn(
            "Test Skipped: {}",
            result.getName()
        );
    }
}

Register:

@Listeners(TestListener.class)
public class LoginTest {


}
22. Logging Screenshots on Failure

Logging becomes more useful when combined with screenshots.

Example:

@Override
public void onTestFailure(ITestResult result) {


    logger.error(
        "Test Failed: {}",
        result.getName(),
        result.getThrowable()
    );


    ScreenshotUtil.captureScreenshot(
        result.getName()
    );
}

A typical failure flow:

Test Failure
     |
     +----> Log failure
     |
     +----> Capture screenshot
     |
     +----> Capture URL
     |
     +----> Capture page title
     |
     +----> Add result to report
23. Logging Current URL on Failure
try {


    loginButton.click();


} catch (Exception e) {


    logger.error(
        "Login button click failed. URL: {}",
        driver.getCurrentUrl(),
        e
    );


    throw e;
}
24. Logging Page Title on Failure
logger.error(
    "Test failed. Page title: {}",
    driver.getTitle()
);
25. Logging Element Information
logger.info(
    "Clicking element: {}",
    loginButton
);

Example output:

INFO - Clicking element: By.id: login
26. Logging Dynamic Test Data

You can log non-sensitive test data.

logger.info(
    "Testing vehicle with model: {}",
    model
);

or:

logger.info(
    "Testing region: {}",
    region
);

Avoid logging sensitive values.

27. Logging DataProvider Tests

Example:

@DataProvider(name = "loginData")
public Object[][] loginData() {


    return new Object[][] {
        {"user1", "password1"},
        {"user2", "password2"}
    };
}

Do not log passwords.

@Test(dataProvider = "loginData")
public void loginTest(String username, String password) {


    logger.info(
        "Executing login test for user: {}",
        username
    );


    // Test steps
}
28. Logging Wait Operations
logger.info(
    "Waiting for Login button to be clickable"
);


wait.until(
    ExpectedConditions.elementToBeClickable(loginButton)
);


logger.info(
    "Login button is clickable"
);

This is very useful when debugging synchronization issues.

29. Logging Retry Operations

If a framework retries failed tests:

logger.warn(
    "Retrying test. Attempt: {}",
    retryCount
);

Example:

WARN - Test failed. Retrying. Attempt: 1
WARN - Test failed. Retrying. Attempt: 2
INFO - Test passed on attempt: 3
30. Logging Parallel Execution

When tests execute in parallel, include the thread ID.

logger.info(
    "Thread ID: {} - Starting test",
    Thread.currentThread().getId()
);

Example:

INFO - Thread ID: 15 - Starting LoginTest
INFO - Thread ID: 16 - Starting SearchTest
INFO - Thread ID: 17 - Starting AppointmentTest

This is extremely useful when debugging parallel execution.

31. Thread Name in Log4j2

You can include the thread name in the pattern.

<PatternLayout
    pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>

Example:

2026-08-19 10:30:01 [TestNG-test-1] INFO  LoginTest - Test started
2026-08-19 10:30:01 [TestNG-test-2] INFO  SearchTest - Test started
32. Logging Utility Class

Instead of creating logging logic everywhere, you can create a utility class.

public class LogUtil {


    private static final Logger logger =
            LogManager.getLogger(LogUtil.class);


    public static void info(String message) {


        logger.info(message);
    }


    public static void warn(String message) {


        logger.warn(message);
    }


    public static void error(String message) {


        logger.error(message);
    }


    public static void debug(String message) {


        logger.debug(message);
    }
}

Usage:

LogUtil.info("Opening application");


LogUtil.info("Entering username");


LogUtil.warn("Page loading slowly");


LogUtil.error("Login failed");

However, in larger frameworks, directly using a logger associated with the class is usually more useful because the originating class is preserved.

33. Better Logging Utility

A utility can accept parameters.

public class LogUtil {


    private static final Logger logger =
            LogManager.getLogger(LogUtil.class);


    public static void info(String message, Object... params) {


        logger.info(message, params);
    }


    public static void warn(String message, Object... params) {


        logger.warn(message, params);
    }


    public static void error(String message, Object... params) {


        logger.error(message, params);
    }
}

Usage:

LogUtil.info(
    "Current browser: {}",
    browser
);


LogUtil.info(
    "Current region: {}",
    region
);
34. Log File Organization

A framework can maintain:

logs/
│
├── automation.log
├── error.log
└── archive/
    ├── automation-2026-08-18.log
    └── automation-2026-08-19.log

Rolling logs prevent a single log file from becoming too large.

35. Rolling File Logging

Example:

<RollingFile
    name="RollingFile"
    fileName="logs/automation.log"
    filePattern="logs/archive/automation-%d{yyyy-MM-dd}-%i.log.gz">


    <PatternLayout
        pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>


    <Policies>


        <TimeBasedTriggeringPolicy interval="1"/>


        <SizeBasedTriggeringPolicy size="10 MB"/>


    </Policies>


</RollingFile>

This allows logs to rotate based on:

Date
File size
36. Separate Error Log

You can maintain a separate error log.

<RollingFile
    name="ErrorFile"
    fileName="logs/error.log"
    filePattern="logs/archive/error-%d{yyyy-MM-dd}.log">


    <PatternLayout
        pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>


    <ThresholdFilter
        level="ERROR"
        onMatch="ACCEPT"
        onMismatch="DENY"/>


</RollingFile>

This makes troubleshooting failures easier.

37. Selenium Framework Logging Architecture

A mature framework may look like:

Selenium Automation Framework
│
├── Base
│   └── BaseTest.java
│
├── Pages
│   ├── LoginPage.java
│   ├── HomePage.java
│   └── SearchPage.java
│
├── Tests
│   ├── LoginTest.java
│   └── SearchTest.java
│
├── Utilities
│   ├── DriverFactory.java
│   ├── WaitUtil.java
│   ├── ScreenshotUtil.java
│   └── LogUtil.java
│
├── Listeners
│   └── TestListener.java
│
├── resources
│   └── log4j2.xml
│
└── logs
    └── automation.log
38. Complete Selenium Logging Example
BaseTest
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;


public class BaseTest {


    protected WebDriver driver;


    protected final Logger logger =
            LogManager.getLogger(getClass());


    @BeforeMethod
    public void setUp() {


        logger.info("Starting WebDriver");


        driver = new ChromeDriver();


        driver.manage().window().maximize();


        logger.info("Chrome browser started");
    }


    @AfterMethod
    public void tearDown() {


        logger.info("Closing browser");


        if (driver != null) {
            driver.quit();
        }


        logger.info("Browser closed");
    }
}
39. Login Page
import org.openqa.selenium.By;


public class LoginPage {


    private WebDriver driver;


    private final Logger logger =
            LogManager.getLogger(LoginPage.class);


    private By username =
            By.id("username");


    private By password =
            By.id("password");


    private By loginButton =
            By.id("login");


    public LoginPage(WebDriver driver) {


        this.driver = driver;


        logger.info("LoginPage initialized");
    }


    public void enterUsername(String value) {


        logger.info("Entering username");


        driver.findElement(username)
              .sendKeys(value);
    }


    public void enterPassword(String value) {


        logger.info("Entering password");


        driver.findElement(password)
              .sendKeys(value);
    }


    public void clickLogin() {


        logger.info("Clicking Login button");


        driver.findElement(loginButton)
              .click();


        logger.info("Login button clicked");
    }
}
40. Login Test
import org.testng.annotations.Test;


public class LoginTest extends BaseTest {


    @Test
    public void validLoginTest() {


        logger.info("===== Login Test Started =====");


        logger.info("Opening application");


        driver.get("https://example.com");


        logger.info(
            "Application opened. URL: {}",
            driver.getCurrentUrl()
        );


        LoginPage loginPage =
                new LoginPage(driver);


        loginPage.enterUsername("testuser");


        loginPage.enterPassword("secret");


        loginPage.clickLogin();


        logger.info("Validating login");


        logger.info("===== Login Test Completed =====");
    }
}

Notice that the password itself should not be logged.

41. Logging with Assertions
Assert.assertTrue(
    homePage.isDisplayed(),
    "Home page is not displayed"
);


logger.info("Home page validation successful");

Better:

logger.info("Validating Home page");


Assert.assertTrue(
    homePage.isDisplayed(),
    "Home page is not displayed"
);


logger.info("Home page validation passed");
42. Logging Failed Assertions
try {


    logger.info("Validating page title");


    Assert.assertEquals(
        driver.getTitle(),
        "Home"
    );


    logger.info("Page title validation passed");


} catch (AssertionError e) {


    logger.error(
        "Page title validation failed",
        e
    );


    throw e;
}
43. Logging API and Selenium Together

If the automation framework contains API testing as well:

logger.info("Creating customer through API");


Response response =
        given()
        .body(requestBody)
        .post("/customers");


logger.info(
    "API response status: {}",
    response.statusCode()
);


logger.info(
    "Opening UI to validate customer"
);

This is useful for end-to-end test flows.

44. Logging Environment

Always log the environment when tests start.

logger.info(
    "Environment: {}",
    environment
);


logger.info(
    "Browser: {}",
    browser
);


logger.info(
    "Execution mode: {}",
    executionMode
);

Example:

INFO - Environment: QA
INFO - Browser: Chrome
INFO - Execution mode: Parallel
45. Logging Test Metadata

Useful metadata includes:

Test Name
Environment
Browser
Operating System
Thread ID
Execution Mode
Build Number
Application Version
Region
Language

Example:

logger.info(
    "Test: {}, Browser: {}, Environment: {}, Thread: {}",
    testName,
    browser,
    environment,
    Thread.currentThread().getId()
);
46. Logging Selenium Version

For framework troubleshooting, you may also capture dependency/browser information.

logger.info(
    "Browser session created: {}",
    driver
);

For detailed environment diagnostics, capture the versions from your build and runtime environment rather than relying on log messages alone.

47. What Should Be Logged?

Good candidates:

✓ Test start
✓ Test end
✓ Browser startup
✓ Environment
✓ Navigation
✓ Important element interactions
✓ Wait operations
✓ API calls
✓ Response status
✓ Assertions
✓ Exceptions
✓ Screenshots
✓ Current URL on failure
✓ Thread information
✓ Retry attempts
48. What Should NOT Be Logged?

Avoid:

✗ Passwords
✗ Access tokens
✗ API keys
✗ Session tokens
✗ Credit card numbers
✗ Security answers
✗ Sensitive personal information

Logging should help debugging without creating a security problem.

49. Logging Best Practices
1. Use meaningful messages

Bad:

logger.info("Step 1");

Better:

logger.info("Entering username");
2. Log important actions

Do not log every line of code.

Bad:

logger.info("Creating variable");
logger.info("Reading variable");
logger.info("Calling method");

Better:

logger.info("Creating new customer");
3. Use appropriate log levels
INFO  → normal execution
DEBUG → detailed troubleshooting
WARN  → unexpected but recoverable
ERROR → failure
4. Include context

Instead of:

logger.error("Failed");

Use:

logger.error(
    "Failed to click Login button on URL: {}",
    driver.getCurrentUrl()
);
5. Do not expose sensitive information

Never log passwords or authentication secrets.

6. Include thread information for parallel execution

This makes parallel test failures much easier to trace.

7. Use rolling logs

Avoid unlimited log-file growth.

8. Centralize configuration

Keep logging configuration in:

log4j2.xml

rather than hardcoding logging behavior throughout the framework.

50. Logging vs Reporting

Logging and reporting are different.

Logging

Used primarily for debugging and execution tracing.

INFO - Opening application
INFO - Entering username
ERROR - Login failed
Reporting

Used to communicate test results.

LoginTest       PASSED
SearchTest      PASSED
BookingTest     FAILED

Typical frameworks use both:

Selenium
   |
   +---- Logging
   |       |
   |       +---- Console
   |       +---- File
   |
   +---- Reporting
           |
           +---- Extent Report
           +---- Allure
           +---- TestNG Report
51. Logging + Screenshot + Reporting

A professional framework can combine all three:

                    Test Execution
                          |
             +------------+------------+
             |            |            |
             v            v            v
          Logging     Screenshot    Reporting
             |            |            |
             v            v            v
          log file      PNG/JPG     HTML Report

On failure:

Test Failure
     |
     +---- Log exception
     |
     +---- Capture screenshot
     |
     +---- Capture URL
     |
     +---- Capture browser information
     |
     +---- Mark test failed
     |
     +---- Add details to report
52. Recommended Selenium Framework Structure
SeleniumFramework
│
├── src
│   ├── main
│   │   └── java
│   │       ├── pages
│   │       ├── utils
│   │       ├── factory
│   │       └── listeners
│   │
│   └── test
│       ├── java
│       │   └── tests
│       │
│       └── resources
│           └── log4j2.xml
│
├── logs
│   ├── automation.log
│   └── error.log
│
├── screenshots
│
├── reports
│
├── pom.xml
└── testng.xml
53. Interview Questions
Beginner
1. Why is logging important in Selenium?

Logging provides execution information and helps identify the location and cause of failures.

2. What are common Java logging frameworks?

Examples:

java.util.logging
Log4j2
SLF4J
Logback
3. What is Log4j2?

Log4j2 is a Java logging framework used to generate and manage application/test logs.

4. What is SLF4J?

SLF4J is a logging abstraction that allows application code to work with different logging implementations.

5. What are common log levels?
TRACE
DEBUG
INFO
WARN
ERROR
FATAL
54. Intermediate Interview Questions
6. How do you configure Log4j2?

Typically using:

log4j2.xml

or:

log4j2.properties
7. Where should the Log4j2 configuration file be placed?

Usually somewhere on the test/runtime classpath, commonly:

src/test/resources/log4j2.xml

or:

src/main/resources/log4j2.xml

depending on project structure.

8. How do you log exceptions?
logger.error(
    "Login failed",
    exception
);
9. Should passwords be logged?

No.

Sensitive information should never be written to logs.

10. How can logging help with parallel execution?

Include thread information:

logger.info(
    "Thread ID: {}",
    Thread.currentThread().getId()
);

This allows you to identify which parallel test produced a log entry.

55. Advanced Interview Questions
11. How would you design logging for a Selenium framework?

A good design would include:

Log4j2
   |
   +---- Console Appender
   |
   +---- Rolling File Appender
   |
   +---- Error File

Each major framework component should have a class-specific logger.

12. How would you capture useful information when a Selenium test fails?

Capture:

Exception
Screenshot
Current URL
Page title
Browser
Environment
Test name
Thread ID

Then attach the information to the test report.

13. How do you avoid huge log files?

Use rolling file appenders based on:

File size
Date
14. What is the difference between logging and reporting?

Logging provides detailed execution/debugging information.

Reporting provides summarized test results and evidence.

15. How would you implement logging in a parallel Selenium framework?

Use:

Class-level logger
+
Thread information
+
Thread-safe WebDriver management
+
Rolling log files

For parallel tests, combine logging with ThreadLocal<WebDriver> or another thread-safe driver-management approach.

56. Senior-Level Framework Design

A strong Selenium framework can use this architecture:

                    TestNG
                      |
                      v
                Test Listener
                      |
          +-----------+-----------+
          |                       |
          v                       v
     Test Execution            Logging
          |                       |
          |                  +----+----+
          |                  |         |
          |                  v         v
          |               Console    File
          |
          +----> Screenshot
          |
          +----> Reporting
          |
          +----> WebDriver
57. Recommended Logging Strategy

For a production-quality Selenium framework:

INFO
 ↓
Normal test execution


DEBUG
 ↓
Detailed troubleshooting


WARN
 ↓
Unexpected but recoverable condition


ERROR
 ↓
Failure/exception


FATAL
 ↓
Critical framework failure

A typical automation run should produce logs such as:

INFO  - Test execution started
INFO  - Environment: QA
INFO  - Browser: Chrome
INFO  - Starting WebDriver
INFO  - Opening application
INFO  - Login page displayed
INFO  - Entering username
INFO  - Entering password
INFO  - Clicking Login button
INFO  - Login successful
INFO  - Validating home page
INFO  - Test PASSED
INFO  - Closing browser
INFO  - Test execution completed
58. Key Takeaways

Remember these points for Selenium interviews:

1. Logging helps debug automation failures.


2. Log4j2 is commonly used for Java automation logging.


3. SLF4J provides a logging abstraction.


4. INFO is commonly used for normal test execution.


5. DEBUG provides detailed troubleshooting information.


6. WARN indicates potentially problematic conditions.


7. ERROR is used for failures and exceptions.


8. Never log passwords, tokens, API keys, or sensitive data.


9. Use log4j2.xml for centralized configuration.


10. Use rolling files to prevent unlimited log growth.


11. Include thread information for parallel execution.


12. Combine logging with screenshots and reports.


13. Log meaningful business/test actions rather than every line of code.


14. Capture URL, browser, environment, and exception details on failure.


15. Logging and reporting serve different purposes.
59. Final Selenium Logging Checklist
Logging
├── Understand why logging is needed
├── Understand Java Logger
├── Understand Log4j2
├── Understand SLF4J
├── Understand logging levels
├── Configure log4j2.xml
├── Log Selenium actions
├── Log TestNG execution
├── Log exceptions
├── Log assertions
├── Log environment information
├── Log browser information
├── Log thread information
├── Avoid sensitive data
├── Configure rolling logs
├── Create logging utility
├── Integrate logging with listeners
├── Capture screenshots on failure
├── Integrate logging with reports
└── Apply logging best practices
End of Selenium Logging

