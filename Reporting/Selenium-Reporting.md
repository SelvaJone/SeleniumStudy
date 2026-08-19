# Selenium Reporting

## Table of Contents

1. [What is Test Automation Reporting?](#1-what-is-test-automation-reporting)
2. [Why Reporting is Important](#2-why-reporting-is-important)
3. [Types of Selenium Reports](#3-types-of-selenium-reports)
4. [TestNG Default Reports](#4-testng-default-reports)
5. [TestNG HTML Report](#5-testng-html-report)
6. [TestNG XML Results](#6-testng-xml-results)
7. [Extent Reports](#7-extent-reports)
8. [ExtentReports Dependency](#8-extentreports-dependency)
9. [Basic Extent Report Setup](#9-basic-extent-report-setup)
10. [Extent Report with TestNG](#10-extent-report-with-testng)
11. [Extent Report with Listener](#11-extent-report-with-listener)
12. [Screenshot on Failure](#12-screenshot-on-failure)
13. [Attach Screenshot to Extent Report](#13-attach-screenshot-to-extent-report)
14. [Pass, Fail and Skip Status](#14-pass-fail-and-skip-status)
15. [Logging in Reports](#15-logging-in-reports)
16. [TestNG Listener + Extent Report](#16-testng-listener--extent-report)
17. [Allure Reporting](#17-allure-reporting)
18. [Allure Maven Configuration](#18-allure-maven-configuration)
19. [Allure TestNG Integration](#19-allure-testng-integration)
20. [Allure Annotations](#20-allure-annotations)
21. [Screenshots in Allure](#21-screenshots-in-allure)
22. [Reports with DataProvider](#22-reports-with-dataprovider)
23. [Reports with Parallel Execution](#23-reports-with-parallel-execution)
24. [Reports with Selenium Grid](#24-reports-with-selenium-grid)
25. [Reports in Maven](#25-reports-in-maven)
26. [Reports in Jenkins](#26-reports-in-jenkins)
27. [Recommended Framework Reporting Structure](#27-recommended-framework-reporting-structure)
28. [Complete Extent Report Framework](#28-complete-extent-report-framework)
29. [Best Practices](#29-best-practices)
30. [Interview Questions](#30-interview-questions)
31. [Quick Revision](#31-quick-revision)

---

# 1. What is Test Automation Reporting?

Test automation reporting provides a summary of automated test execution.

A report typically shows:

```text
Test Name
Test Description
Execution Time
Status
Browser
Environment
Failure Reason
Screenshots
Logs
Exception Details
```

Example:

```text
Login Test       PASS
Search Test      PASS
Checkout Test    FAIL
Payment Test     SKIP
```

Reports help QA engineers, developers, managers, and CI/CD systems understand the test execution results.

---

# 2. Why Reporting is Important

Good reporting helps with:

* Identifying failed tests
* Understanding failure reasons
* Tracking regression results
* Attaching screenshots
* Debugging automation failures
* Sharing results with the team
* CI/CD integration
* Historical analysis
* Release validation

Without reporting, a large automation suite may only produce console output that is difficult to analyze.

---

# 3. Types of Selenium Reports

Selenium itself does not provide a built-in reporting framework.

Reporting is usually provided by the test framework or third-party reporting libraries.

Common options include:

```text
TestNG Reports
ExtentReports
Allure Reports
Maven Surefire Reports
Custom HTML Reports
CI/CD Reports
```

A common enterprise combination is:

```text
Selenium
+
Java
+
TestNG
+
ExtentReports / Allure
+
Jenkins
```

---

# 4. TestNG Default Reports

When TestNG tests are executed, TestNG generates reports in the `test-output` directory.

Typical structure:

```text
test-output
│
├── index.html
├── emailable-report.html
├── testng-results.xml
├── testng-failed.xml
└── junitreports
```

The exact files can vary depending on the TestNG version and execution configuration.

---

# 5. TestNG HTML Report

One of the most commonly used default reports is:

```text
test-output/index.html
```

It provides information such as:

```text
Passed Tests
Failed Tests
Skipped Tests
Methods
Classes
Groups
Execution Details
```

Another useful report is:

```text
test-output/emailable-report.html
```

This is useful for sharing a concise execution summary.

---

# 6. TestNG XML Results

TestNG also produces XML execution results.

Example:

```text
test-output/testng-results.xml
```

XML results are useful for:

* CI/CD tools
* Report generators
* Result processing
* Custom reporting
* Automated analysis

Example structure:

```xml
<testng-results>
    <suite>
        <test>
            <class>
                <test-method
                    name="loginTest"
                    status="PASS"/>
            </class>
        </test>
    </suite>
</testng-results>
```

---

# 7. Extent Reports

ExtentReports is a popular reporting library for Java automation frameworks.

It can provide:

```text
PASS
FAIL
SKIP
Screenshots
Logs
Test steps
Execution time
Environment information
Author information
Categories
```

Example report:

```text
---------------------------------------
Selenium Automation Report
---------------------------------------

Login Test       PASS
Search Test      PASS
Checkout Test    FAIL

Failure:
Element not found

Screenshot:
checkoutTest.png
```

---

# 8. ExtentReports Dependency

For Maven, add the ExtentReports dependency to `pom.xml`.

Example:

```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.x.x</version>
</dependency>
```

Use a currently supported version compatible with your project.

---

# 9. Basic Extent Report Setup

```java
import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentSparkReporter;

public class ExtentManager {

    private static ExtentReports extent;

    public static ExtentReports getExtentReports() {

        if (extent == null) {

            ExtentSparkReporter sparkReporter =
                new ExtentSparkReporter(
                    "reports/ExtentReport.html"
                );

            extent = new ExtentReports();

            extent.attachReporter(
                sparkReporter
            );

            extent.setSystemInfo(
                "OS",
                System.getProperty("os.name")
            );

            extent.setSystemInfo(
                "Java",
                System.getProperty("java.version")
            );
        }

        return extent;
    }
}
```

---

# 10. Extent Report with TestNG

Basic example:

```java
import org.testng.annotations.Test;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;

public class LoginTest {

    @Test
    public void loginTest() {

        ExtentReports extent =
            ExtentManager.getExtentReports();

        ExtentTest test =
            extent.createTest("Login Test");

        test.info("Opening login page");

        test.info("Entering username");

        test.info("Entering password");

        test.pass("Login successful");

        extent.flush();
    }
}
```

The important method is:

```java
extent.flush();
```

It writes the report data to the configured report file.

---

# 11. Extent Report with Listener

A better approach is to use a TestNG listener.

Instead of writing reporting code in every test:

```java
test.pass();
test.fail();
test.info();
```

the listener automatically handles test lifecycle events.

Example:

```java
import org.testng.ITestListener;
import org.testng.ITestResult;

import com.aventstack.extentreports.*;

public class ExtentListener
        implements ITestListener {

    private ExtentReports extent =
        ExtentManager.getExtentReports();

    private ThreadLocal<ExtentTest> test =
        new ThreadLocal<>();

    @Override
    public void onTestStart(
            ITestResult result) {

        ExtentTest extentTest =
            extent.createTest(
                result.getMethod()
                      .getMethodName()
            );

        test.set(extentTest);
    }

    @Override
    public void onTestSuccess(
            ITestResult result) {

        test.get().pass(
            "Test Passed"
        );
    }

    @Override
    public void onTestFailure(
            ITestResult result) {

        test.get().fail(
            result.getThrowable()
        );
    }

    @Override
    public void onTestSkipped(
            ITestResult result) {

        test.get().skip(
            "Test Skipped"
        );
    }

    @Override
    public void onFinish(
            ITestContext context) {

        extent.flush();
    }
}
```

Register the listener:

```java
import org.testng.annotations.Listeners;

@Listeners(ExtentListener.class)
public class LoginTest {

    @Test
    public void loginTest() {

        System.out.println(
            "Executing login test"
        );
    }
}
```

---

# 12. Screenshot on Failure

A good automation framework captures a screenshot when a test fails.

Basic utility:

```java
import org.openqa.selenium.*;

import java.io.File;
import java.nio.file.*;

public class ScreenshotUtil {

    public static String captureScreenshot(
            WebDriver driver,
            String testName) {

        try {

            TakesScreenshot screenshot =
                (TakesScreenshot) driver;

            File source =
                screenshot.getScreenshotAs(
                    OutputType.FILE
                );

            Path destination =
                Paths.get(
                    "reports/screenshots/"
                    + testName
                    + ".png"
                );

            Files.createDirectories(
                destination.getParent()
            );

            Files.copy(
                source.toPath(),
                destination,
                StandardCopyOption.REPLACE_EXISTING
            );

            return destination.toString();

        } catch (Exception e) {

            e.printStackTrace();

            return null;
        }
    }
}
```

---

# 13. Attach Screenshot to Extent Report

Inside `onTestFailure()`:

```java
@Override
public void onTestFailure(
        ITestResult result) {

    ExtentTest extentTest =
        test.get();

    extentTest.fail(
        result.getThrowable()
    );

    Object instance =
        result.getInstance();

    WebDriver driver =
        ((BaseTest) instance).getDriver();

    String screenshotPath =
        ScreenshotUtil.captureScreenshot(
            driver,
            result.getName()
        );

    try {

        extentTest.addScreenCaptureFromPath(
            screenshotPath
        );

    } catch (Exception e) {

        e.printStackTrace();
    }
}
```

Now the report can contain:

```text
Test Failed
    ↓
Exception
    ↓
Screenshot
```

---

# 14. Pass, Fail and Skip Status

ExtentReports supports different statuses.

## Pass

```java
test.pass("Login successful");
```

## Fail

```java
test.fail("Login failed");
```

## Skip

```java
test.skip("Test skipped");
```

## Information

```java
test.info("Opening browser");
```

## Warning

```java
test.warning("Slow response detected");
```

---

# 15. Logging in Reports

You can record individual test steps.

```java
test.info("Open application");

test.info("Enter username");

test.info("Enter password");

test.info("Click login");

test.info("Verify home page");

test.pass("Login completed");
```

A report might display:

```text
Login Test

INFO    Open application
INFO    Enter username
INFO    Enter password
INFO    Click login
INFO    Verify home page
PASS    Login completed
```

---

# 16. TestNG Listener + Extent Report

Recommended architecture:

```text
                TestNG
                  |
                  v
           ITestListener
                  |
       +----------+----------+
       |          |          |
      PASS       FAIL       SKIP
       |          |          |
       +----------+----------+
                  |
                  v
          ExtentReports
                  |
                  v
          HTML Report
```

The test classes should focus primarily on test behavior.

Reporting logic should remain centralized in the listener.

---

# 17. Allure Reporting

Allure is another popular reporting solution.

It provides rich reports containing:

```text
Test results
Steps
Screenshots
Attachments
Categories
Suites
Timelines
Graphs
Environment information
```

Typical flow:

```text
TestNG
   ↓
Allure TestNG Adapter
   ↓
allure-results
   ↓
Allure Report
```

---

# 18. Allure Maven Configuration

Example dependency:

```xml
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-testng</artifactId>
    <version>2.x.x</version>
</dependency>
```

Allure also requires its command-line/reporting tooling to generate and view the HTML report.

---

# 19. Allure TestNG Integration

Example:

```java
import io.qameta.allure.*;

@Test
@Description("Verify valid user login")
public void loginTest() {

    login();
}
```

You can also use:

```java
@Epic("Authentication")
@Feature("Login")
@Story("Valid Login")
```

Example:

```java
@Epic("User Management")
@Feature("Login")
@Story("Valid Credentials")
@Test
public void validLogin() {

    login();
}
```

---

# 20. Allure Annotations

Common annotations:

```text
@Epic
@Feature
@Story
@Description
@Severity
@Owner
@Link
@Issue
@TmsLink
```

Example:

```java
@Epic("Service Shop")

@Feature("Appointment Booking")

@Story("Create Service Appointment")

@Severity(SeverityLevel.CRITICAL)

@Owner("QA Team")

@Test
public void createAppointment() {

}
```

These annotations help organize reports.

---

# 21. Screenshots in Allure

Attach a Selenium screenshot:

```java
import io.qameta.allure.Attachment;

@Attachment(
    value = "Screenshot",
    type = "image/png"
)
public byte[] attachScreenshot(
        WebDriver driver) {

    return ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.BYTES
        );
}
```

Call it when needed:

```java
attachScreenshot(driver);
```

A screenshot will appear as an attachment in the Allure report.

---

# 22. Reports with DataProvider

A DataProvider test can generate separate test executions.

```java
@DataProvider(name = "loginData")
public Object[][] loginData() {

    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}

@Test(dataProvider = "loginData")
public void loginTest(
        String username,
        String password) {

    // Test logic
}
```

Reporting should identify each data set clearly.

For example:

```text
Login Test - user1    PASS
Login Test - user2    PASS
Login Test - user3    FAIL
```

A useful approach is to include the data values in the test name or report metadata.

---

# 23. Reports with Parallel Execution

Parallel execution introduces thread-safety concerns.

Bad approach:

```java
private ExtentTest test;
```

Multiple threads may overwrite the same object.

Better:

```java
private ThreadLocal<ExtentTest> test =
    new ThreadLocal<>();
```

Then:

```java
test.set(extentTest);
```

Retrieve:

```java
ExtentTest currentTest =
    test.get();
```

Remove after completion:

```java
test.remove();
```

Recommended architecture:

```text
Thread 1 → ExtentTest 1
Thread 2 → ExtentTest 2
Thread 3 → ExtentTest 3
Thread 4 → ExtentTest 4
```

This is especially important when TestNG executes Selenium tests in parallel.

---

# 24. Reports with Selenium Grid

When tests execute on Selenium Grid:

```text
TestNG
   ↓
Parallel Execution
   ↓
RemoteWebDriver
   ↓
Selenium Grid
   ↓
Browser Nodes
   ↓
Test Results
   ↓
Extent / Allure
```

Reports should capture useful environment information:

```text
Browser
Browser Version
Operating System
Environment
Grid Node
Test Name
Execution Time
```

---

# 25. Reports in Maven

Run the tests:

```bash
mvn test
```

TestNG results are typically generated under:

```text
test-output/
```

Maven Surefire results are typically generated under:

```text
target/surefire-reports/
```

These results can be consumed by CI/CD tools and reporting plugins.

---

# 26. Reports in Jenkins

A common CI/CD flow:

```text
Developer
    ↓
Git
    ↓
Jenkins
    ↓
Maven
    ↓
TestNG
    ↓
Selenium
    ↓
Tests
    ↓
Reports
```

Jenkins can publish test results after execution.

Example Maven command:

```bash
mvn clean test
```

Typical artifacts:

```text
test-output/
target/surefire-reports/
reports/
```

Reports can be archived as build artifacts or published through Jenkins reporting plugins.

---

# 27. Recommended Framework Reporting Structure

Example:

```text
SeleniumFramework
│
├── reports
│   ├── ExtentReport.html
│   │
│   └── screenshots
│       ├── loginTest.png
│       └── checkoutTest.png
│
├── test-output
│
├── target
│   └── surefire-reports
│
├── src
│   ├── main
│   │   └── java
│   │       ├── utilities
│   │       │   ├── ExtentManager.java
│   │       │   └── ScreenshotUtil.java
│   │       │
│   │       └── listeners
│   │           └── ExtentListener.java
│   │
│   └── test
│       └── java
│           └── tests
│               ├── LoginTest.java
│               └── SearchTest.java
│
└── testng.xml
```

---

# 28. Complete Extent Report Framework

## ExtentManager.java

```java
import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentSparkReporter;

public class ExtentManager {

    private static ExtentReports extent;

    private ExtentManager() {
    }

    public static synchronized
    ExtentReports getExtentReports() {

        if (extent == null) {

            ExtentSparkReporter reporter =
                new ExtentSparkReporter(
                    "reports/ExtentReport.html"
                );

            extent = new ExtentReports();

            extent.attachReporter(
                reporter
            );

            extent.setSystemInfo(
                "OS",
                System.getProperty("os.name")
            );

            extent.setSystemInfo(
                "Java",
                System.getProperty("java.version")
            );

            extent.setSystemInfo(
                "User",
                System.getProperty("user.name")
            );
        }

        return extent;
    }
}
```

---

## ScreenshotUtil.java

```java
import org.openqa.selenium.*;

import java.io.File;
import java.nio.file.*;

public class ScreenshotUtil {

    public static String capture(
            WebDriver driver,
            String testName) {

        try {

            TakesScreenshot screenshot =
                (TakesScreenshot) driver;

            File source =
                screenshot.getScreenshotAs(
                    OutputType.FILE
                );

            Path destination =
                Paths.get(
                    "reports/screenshots/",
                    testName + ".png"
                );

            Files.createDirectories(
                destination.getParent()
            );

            Files.copy(
                source.toPath(),
                destination,
                StandardCopyOption.REPLACE_EXISTING
            );

            return destination.toString();

        } catch (Exception e) {

            e.printStackTrace();

            return null;
        }
    }
}
```

---

## ExtentListener.java

```java
import com.aventstack.extentreports.*;
import org.testng.*;

public class ExtentListener
        implements ITestListener {

    private static ExtentReports extent =
        ExtentManager.getExtentReports();

    private static ThreadLocal<ExtentTest>
        extentTest =
            new ThreadLocal<>();

    @Override
    public void onTestStart(
            ITestResult result) {

        String testName =
            result.getMethod()
                  .getMethodName();

        ExtentTest test =
            extent.createTest(testName);

        extentTest.set(test);
    }

    @Override
    public void onTestSuccess(
            ITestResult result) {

        extentTest
            .get()
            .pass("Test Passed");
    }

    @Override
    public void onTestFailure(
            ITestResult result) {

        extentTest
            .get()
            .fail(result.getThrowable());

        Object instance =
            result.getInstance();

        if (instance instanceof BaseTest) {

            BaseTest baseTest =
                (BaseTest) instance;

            WebDriver driver =
                baseTest.getDriver();

            String screenshot =
                ScreenshotUtil.capture(
                    driver,
                    result.getName()
                );

            if (screenshot != null) {

                try {

                    extentTest
                        .get()
                        .addScreenCaptureFromPath(
                            screenshot
                        );

                } catch (Exception e) {

                    e.printStackTrace();
                }
            }
        }
    }

    @Override
    public void onTestSkipped(
            ITestResult result) {

        extentTest
            .get()
            .skip("Test Skipped");
    }

    @Override
    public void onFinish(
            ITestContext context) {

        extent.flush();

        extentTest.remove();
    }
}
```

---

## BaseTest.java

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

    public WebDriver getDriver() {
        return driver;
    }
}
```

---

## LoginTest.java

```java
import org.testng.Assert;
import org.testng.annotations.Test;

public class LoginTest
        extends BaseTest {

    @Test(
        groups = {"smoke", "regression"},
        description =
            "Verify valid login"
    )
    public void validLogin() {

        String title =
            driver.getTitle();

        Assert.assertNotNull(title);
    }

    @Test(
        groups = {"regression"},
        description =
            "Verify invalid login"
    )
    public void invalidLogin() {

        Assert.assertTrue(
            true
        );
    }
}
```

---

## Register Listener

Using annotation:

```java
@Listeners(ExtentListener.class)
public class LoginTest
        extends BaseTest {
}
```

Or register the listener in `testng.xml`:

```xml
<listeners>

    <listener
        class-name="listeners.ExtentListener"/>

</listeners>
```

---

# 29. Best Practices

## 1. Centralize reporting

Avoid creating reporting objects in every test class.

Prefer:

```text
ExtentManager
        ↓
ExtentListener
        ↓
TestNG
```

---

## 2. Use listeners

Listeners keep reporting logic separate from business test logic.

---

## 3. Capture screenshots only when useful

A common approach is:

```text
FAIL → Screenshot
PASS → Optional
```

Capturing screenshots for every step can make reports unnecessarily large.

---

## 4. Use ThreadLocal for parallel tests

For parallel execution:

```java
ThreadLocal<ExtentTest>
```

is safer than a shared `ExtentTest`.

---

## 5. Include environment information

Useful metadata:

```text
Environment
Browser
Browser Version
OS
Build Number
Application Version
Execution Date
```

---

## 6. Give meaningful test names

Instead of:

```java
@Test
public void test1() {
}
```

Use:

```java
@Test
public void verifyUserCanCreateServiceAppointment() {
}
```

---

## 7. Include failure details

A good failure report should provide:

```text
Test Name
Status
Exception
Stack Trace
Screenshot
Browser
Environment
```

---

## 8. Keep generated reports out of source control when appropriate

Generated reports can be large and are usually not committed to the main source repository.

Common `.gitignore` entries:

```text
target/
test-output/
reports/
*.html
```

Whether reports should be committed depends on the team's CI/CD and artifact-retention strategy.

---

# 30. Interview Questions

## Beginner

### 1. Does Selenium generate reports?

No.

Selenium is primarily a browser automation library. Reporting is normally provided by TestNG, JUnit, ExtentReports, Allure, Maven, CI tools, or custom reporting solutions.

---

### 2. What report does TestNG generate?

TestNG generates HTML and XML execution results, commonly under:

```text
test-output/
```

---

### 3. What is ExtentReports?

ExtentReports is a reporting library that can generate rich HTML reports containing test status, logs, screenshots, and execution information.

---

### 4. What is Allure?

Allure is a test reporting framework that provides detailed reports with steps, attachments, screenshots, metadata, categories, and execution information.

---

## Intermediate

### 5. How do you capture screenshots on failure?

Implement `ITestListener` and capture the screenshot inside:

```java
onTestFailure()
```

---

### 6. How do you attach a screenshot to ExtentReports?

Use:

```java
extentTest.addScreenCaptureFromPath(
    screenshotPath
);
```

---

### 7. What is `extent.flush()`?

It writes the current ExtentReports data to the configured report output.

---

### 8. Why use ThreadLocal with ExtentTest?

When tests run in parallel, each thread should maintain its own `ExtentTest` instance.

---

### 9. Where are Maven Surefire reports generated?

Typically:

```text
target/surefire-reports/
```

---

### 10. How do you integrate reports with Jenkins?

The normal flow is:

```text
Jenkins
   ↓
Maven
   ↓
TestNG
   ↓
Selenium Tests
   ↓
Test Results
   ↓
Jenkins Report / Archived Artifacts
```

---

## Advanced

### 11. How would you design reporting for a parallel Selenium framework?

Use:

```text
TestNG
   ↓
ITestListener
   ↓
ThreadLocal<ExtentTest>
   ↓
ExtentReports
```

Each execution thread receives its own report test object.

---

### 12. How do you capture screenshots only for failed tests?

Implement:

```java
onTestFailure()
```

and call the screenshot utility there.

---

### 13. How do you add browser and environment information?

Use:

```java
extent.setSystemInfo(
    "Browser",
    browser
);
```

Other examples:

```java
extent.setSystemInfo(
    "Environment",
    "QA"
);

extent.setSystemInfo(
    "OS",
    System.getProperty("os.name")
);
```

---

### 14. How do you prevent reporting code from being duplicated?

Use a centralized:

```text
ExtentManager
+
ITestListener
+
ScreenshotUtil
```

---

### 15. What is the difference between TestNG reports and ExtentReports?

```text
TestNG Report
    Basic execution results
    HTML/XML
    Automatically generated

ExtentReports
    Rich HTML report
    Logs
    Screenshots
    Custom metadata
    Better visualization
```

---

# 31. Quick Revision

## Selenium Reporting Architecture

```text
                 Selenium
                    |
                    v
                  TestNG
                    |
                    v
             ITestListener
                    |
          +---------+---------+
          |         |         |
        PASS       FAIL      SKIP
          |         |         |
          |      Screenshot   |
          |         |         |
          +---------+---------+
                    |
                    v
             ExtentReports
                    |
                    v
              HTML Report
                    |
                    v
                 Jenkins
```

## Important Classes

```text
ExtentReports
ExtentTest
ExtentSparkReporter
ITestListener
ITestResult
ScreenshotUtil
```

## Important Methods

```java
extent.createTest()
extent.flush()

test.info()
test.pass()
test.fail()
test.skip()
test.warning()

test.addScreenCaptureFromPath()
```

## Important TestNG Listener Methods

```java
onTestStart()
onTestSuccess()
onTestFailure()
onTestSkipped()
onStart()
onFinish()
```

## Recommended Enterprise Setup

```text
Java
  ↓
Selenium WebDriver
  ↓
TestNG
  ↓
Page Object Model
  ↓
ThreadLocal WebDriver
  ↓
ITestListener
  ↓
ExtentReports / Allure
  ↓
Screenshots + Logs
  ↓
Maven
  ↓
Jenkins
  ↓
CI/CD Reports
```

## Final Interview Summary

Remember these points:

```text
1. Selenium does not provide built-in reporting.

2. TestNG provides basic HTML/XML execution results.

3. ExtentReports provides rich HTML reports.

4. Allure provides detailed test reports and attachments.

5. ITestListener can capture test lifecycle events.

6. onTestFailure() is commonly used for screenshots.

7. extent.flush() writes the report.

8. ThreadLocal<ExtentTest> is important for parallel execution.

9. Maven Surefire generates machine-readable test results.

10. Jenkins can publish or archive automation results.

11. Reporting should be centralized in the framework.

12. Screenshots, logs, environment, browser, and exceptions
    should be available for failed tests.

13. Generated reports should generally not be committed
    to the source repository unless the team's workflow
    specifically requires it.
```
