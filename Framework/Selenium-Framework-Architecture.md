# Selenium Framework Architecture

## 1. What Is a Selenium Automation Framework?

A Selenium automation framework is a structured collection of:

* Selenium WebDriver
* Java
* TestNG
* Maven
* Page Object Model
* Driver Factory
* Utilities
* Configuration
* Test data
* Listeners
* Reporting
* Selenium Grid
* CI/CD tools

The goal is to create an automation solution that is:

* Maintainable
* Reusable
* Scalable
* Reliable
* Easy to debug
* Easy to execute locally and in CI/CD
* Suitable for parallel execution

---

# 2. Recommended Technology Stack

A professional Selenium framework can use:

```text
Java
  +
Selenium WebDriver
  +
TestNG
  +
Maven
  +
Page Object Model
  +
ThreadLocal
  +
Selenium Grid
  +
Jenkins
```

Additional tools:

```text
Log4j / SLF4J
ExtentReports / Allure
Git
GitHub / GitLab / Bitbucket
Jenkins
Docker
```

---

# 3. High-Level Architecture

```text
                         Jenkins / CI
                              |
                              v
                           Maven
                              |
                              v
                           TestNG
                              |
                    +---------+---------+
                    |                   |
                Test Classes        Listeners
                    |                   |
                    v                   v
                BaseTest            Reporting
                    |
                    v
              DriverManager
                    |
                ThreadLocal
                    |
                    v
              DriverFactory
               /         \
              /           \
          Local             Grid
           |                 |
      WebDriver        RemoteWebDriver
           |                 |
      Chrome/Edge       Selenium Grid
      Firefox               |
                             v
                    Browser Nodes
                    /      |      \
                Chrome   Firefox   Edge
                    |
                    v
               Page Objects
                    |
                    v
              Page Components
                    |
                    v
                Utilities
```

---

# 4. Recommended Project Structure

```text
SeleniumFramework
│
├── pom.xml
├── testng.xml
├── README.md
├── .gitignore
│
├── src
│   │
│   ├── main
│   │   └── java
│   │       │
│   │       ├── base
│   │       │   ├── BasePage.java
│   │       │   └── BaseTest.java
│   │       │
│   │       ├── driver
│   │       │   ├── DriverFactory.java
│   │       │   └── DriverManager.java
│   │       │
│   │       ├── pages
│   │       │   ├── LoginPage.java
│   │       │   ├── HomePage.java
│   │       │   └── AccountPage.java
│   │       │
│   │       ├── components
│   │       │   ├── Header.java
│   │       │   └── NavigationMenu.java
│   │       │
│   │       ├── utils
│   │       │   ├── WaitUtils.java
│   │       │   ├── ScreenshotUtils.java
│   │       │   ├── ConfigReader.java
│   │       │   ├── JavaScriptUtils.java
│   │       │   └── ExcelUtils.java
│   │       │
│   │       └── constants
│   │           └── FrameworkConstants.java
│   │
│   └── test
│       │
│       ├── java
│       │   │
│       │   ├── tests
│       │   │   ├── LoginTest.java
│       │   │   └── AccountTest.java
│       │   │
│       │   ├── dataproviders
│       │   │   └── TestDataProvider.java
│       │   │
│       │   └── listeners
│       │       └── TestListener.java
│       │
│       └── resources
│           │
│           ├── config
│           │   ├── config.properties
│           │   ├── qa.properties
│           │   └── prod.properties
│           │
│           └── testdata
│               ├── loginData.json
│               └── testData.xlsx
│
└── reports
```

---

# 5. Maven

Maven manages:

* Dependencies
* Build
* Test execution
* Plugins
* Project lifecycle

Basic Maven structure:

```text
project
│
├── pom.xml
│
└── src
    ├── main
    └── test
```

---

# 6. Basic pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.seleniumstudy</groupId>

    <artifactId>selenium-framework</artifactId>

    <version>1.0-SNAPSHOT</version>

    <properties>

        <maven.compiler.source>17</maven.compiler.source>

        <maven.compiler.target>17</maven.compiler.target>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

    </properties>

    <dependencies>

        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.XX.X</version>
        </dependency>

        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.XX.X</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

</project>
```

Use the Selenium and TestNG versions appropriate for your project rather than copying an outdated version number.

---

# 7. Maven Commands

Compile:

```bash
mvn compile
```

Run tests:

```bash
mvn test
```

Clean project:

```bash
mvn clean
```

Clean and test:

```bash
mvn clean test
```

Skip tests:

```bash
mvn clean install -DskipTests
```

---

# 8. Configuration File

Do not hard-code environment-specific values throughout the framework.

Example:

```properties
browser=chrome
environment=qa
headless=false
grid.enabled=false
grid.url=http://localhost:4444
qa.url=https://qa.example.com
```

---

# 9. ConfigReader

```java
package utils;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class ConfigReader {

    private static final Properties properties =
            new Properties();

    static {

        try {

            FileInputStream input =
                    new FileInputStream(
                            "src/test/resources/config/config.properties"
                    );

            properties.load(input);

            input.close();

        } catch (IOException e) {

            throw new RuntimeException(
                    "Unable to load configuration",
                    e
            );
        }
    }

    public static String getProperty(
            String key) {

        return properties.getProperty(key);
    }
}
```

Usage:

```java
String browser =
        ConfigReader.getProperty("browser");

String environment =
        ConfigReader.getProperty("environment");
```

---

# 10. Environment-Based Configuration

Example:

```text
config
│
├── config.properties
├── qa.properties
└── prod.properties
```

QA:

```properties
url=https://qa.example.com
```

Production:

```properties
url=https://www.example.com
```

Then select the environment using a Maven parameter:

```bash
mvn test -Denvironment=qa
```

or:

```bash
mvn test -Denvironment=prod
```

---

# 11. Constants

Keep framework-wide constants in one place.

```java
public class FrameworkConstants {

    public static final int EXPLICIT_WAIT = 10;

    public static final String CONFIG_PATH =
            "src/test/resources/config/config.properties";

    public static final String SCREENSHOT_PATH =
            "screenshots/";

    private FrameworkConstants() {
    }
}
```

---

# 12. DriverFactory

The Driver Factory creates the correct WebDriver.

```java
package driver;

import java.net.MalformedURLException;
import java.net.URL;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;

public class DriverFactory {

    public static WebDriver createDriver(
            String browser,
            boolean gridEnabled)
            throws MalformedURLException {

        if (gridEnabled) {

            return createRemoteDriver(browser);
        }

        return createLocalDriver(browser);
    }

    private static WebDriver createLocalDriver(
            String browser) {

        switch (browser.toLowerCase()) {

            case "chrome":
                return new ChromeDriver(
                        new ChromeOptions()
                );

            case "firefox":
                return new FirefoxDriver(
                        new FirefoxOptions()
                );

            case "edge":
                return new EdgeDriver(
                        new EdgeOptions()
                );

            default:
                throw new IllegalArgumentException(
                        "Unsupported browser: " + browser
                );
        }
    }

    private static WebDriver createRemoteDriver(
            String browser)
            throws MalformedURLException {

        String gridUrl =
                ConfigReader.getProperty(
                        "grid.url"
                );

        URL url = new URL(gridUrl);

        switch (browser.toLowerCase()) {

            case "chrome":
                return new RemoteWebDriver(
                        url,
                        new ChromeOptions()
                );

            case "firefox":
                return new RemoteWebDriver(
                        url,
                        new FirefoxOptions()
                );

            case "edge":
                return new RemoteWebDriver(
                        url,
                        new EdgeOptions()
                );

            default:
                throw new IllegalArgumentException(
                        "Unsupported browser: " + browser
                );
        }
    }
}
```

---

# 13. DriverManager

For parallel execution, use ThreadLocal.

```java
package driver;

import org.openqa.selenium.WebDriver;

public class DriverManager {

    private static final ThreadLocal<WebDriver>
            driver = new ThreadLocal<>();

    public static void setDriver(
            WebDriver webDriver) {

        driver.set(webDriver);
    }

    public static WebDriver getDriver() {

        return driver.get();
    }

    public static void quitDriver() {

        WebDriver webDriver =
                driver.get();

        if (webDriver != null) {

            webDriver.quit();

            driver.remove();
        }
    }

    private DriverManager() {
    }
}
```

---

# 14. Why ThreadLocal?

Suppose three tests execute simultaneously:

```text
Thread 1 → Test A → Chrome 1
Thread 2 → Test B → Chrome 2
Thread 3 → Test C → Chrome 3
```

Each thread gets a separate WebDriver.

Without ThreadLocal, parallel tests may accidentally share a driver.

---

# 15. BaseTest

BaseTest contains common test setup and cleanup.

```java
package base;

import java.net.MalformedURLException;

import org.openqa.selenium.WebDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

import driver.DriverFactory;
import driver.DriverManager;
import utils.ConfigReader;

public class BaseTest {

    @BeforeMethod
    public void setUp()
            throws MalformedURLException {

        String browser =
                ConfigReader.getProperty("browser");

        boolean gridEnabled =
                Boolean.parseBoolean(
                        ConfigReader.getProperty(
                                "grid.enabled"
                        )
                );

        WebDriver driver =
                DriverFactory.createDriver(
                        browser,
                        gridEnabled
                );

        DriverManager.setDriver(driver);

        driver.manage()
                .window()
                .maximize();

        String url =
                ConfigReader.getProperty("qa.url");

        driver.get(url);
    }

    @AfterMethod
    public void tearDown() {

        DriverManager.quitDriver();
    }
}
```

---

# 16. BasePage

BasePage contains common page functionality.

```java
package base;

import org.openqa.selenium.WebDriver;

public class BasePage {

    protected WebDriver driver;

    public BasePage(WebDriver driver) {

        this.driver = driver;
    }

    public String getTitle() {

        return driver.getTitle();
    }

    public String getUrl() {

        return driver.getCurrentUrl();
    }

    public void refresh() {

        driver.navigate().refresh();
    }

    public void back() {

        driver.navigate().back();
    }

    public void forward() {

        driver.navigate().forward();
    }
}
```

---

# 17. LoginPage

```java
package pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

import base.BasePage;

public class LoginPage
        extends BasePage {

    private By username =
            By.id("username");

    private By password =
            By.id("password");

    private By loginButton =
            By.id("login");

    public LoginPage(WebDriver driver) {

        super(driver);
    }

    public LoginPage enterUsername(
            String value) {

        driver.findElement(username)
                .sendKeys(value);

        return this;
    }

    public LoginPage enterPassword(
            String value) {

        driver.findElement(password)
                .sendKeys(value);

        return this;
    }

    public HomePage clickLogin() {

        driver.findElement(loginButton)
                .click();

        return new HomePage(driver);
    }

    public HomePage login(
            String usernameValue,
            String passwordValue) {

        return enterUsername(usernameValue)
                .enterPassword(passwordValue)
                .clickLogin();
    }
}
```

---

# 18. HomePage

```java
package pages;

import org.openqa.selenium.WebDriver;

import base.BasePage;

public class HomePage
        extends BasePage {

    public HomePage(WebDriver driver) {

        super(driver);
    }

    public boolean isHomePageDisplayed() {

        return driver.getTitle()
                .contains("Home");
    }
}
```

---

# 19. LoginTest

```java
package tests;

import org.testng.Assert;
import org.testng.annotations.Test;

import base.BaseTest;
import driver.DriverManager;
import pages.HomePage;
import pages.LoginPage;

public class LoginTest
        extends BaseTest {

    @Test
    public void validLoginTest() {

        LoginPage loginPage =
                new LoginPage(
                        DriverManager.getDriver()
                );

        HomePage homePage =
                loginPage.login(
                        "admin",
                        "password"
                );

        Assert.assertTrue(
                homePage.isHomePageDisplayed()
        );
    }
}
```

---

# 20. Utility Classes

Common utilities can include:

```text
utils
│
├── WaitUtils.java
├── ScreenshotUtils.java
├── JavaScriptUtils.java
├── ConfigReader.java
├── ExcelUtils.java
├── JsonUtils.java
├── DateUtils.java
└── WebTableUtils.java
```

The goal is to avoid duplicate code.

---

# 21. WaitUtils

```java
package utils;

import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class WaitUtils {

    private WebDriverWait wait;

    public WaitUtils(WebDriver driver) {

        wait = new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );
    }

    public WebElement waitForVisible(
            By locator) {

        return wait.until(
                ExpectedConditions
                        .visibilityOfElementLocated(
                                locator
                        )
        );
    }

    public WebElement waitForClickable(
            By locator) {

        return wait.until(
                ExpectedConditions
                        .elementToBeClickable(
                                locator
                        )
        );
    }

    public boolean waitForTitle(
            String title) {

        return wait.until(
                ExpectedConditions
                        .titleContains(title)
        );
    }
}
```

---

# 22. Screenshot Utility

```java
package utils;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;

public class ScreenshotUtils {

    public static void takeScreenshot(
            WebDriver driver,
            String fileName) {

        TakesScreenshot screenshot =
                (TakesScreenshot) driver;

        File source =
                screenshot.getScreenshotAs(
                        OutputType.FILE
                );

        File destination =
                new File(
                        "screenshots/"
                        + fileName
                        + ".png"
                );

        try {

            Files.createDirectories(
                    destination.getParentFile()
                            .toPath()
            );

            Files.copy(
                    source.toPath(),
                    destination.toPath()
            );

        } catch (IOException e) {

            throw new RuntimeException(
                    "Unable to save screenshot",
                    e
            );
        }
    }
}
```

---

# 23. JavaScript Utility

```java
package utils;

import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;

public class JavaScriptUtils {

    private WebDriver driver;

    public JavaScriptUtils(WebDriver driver) {

        this.driver = driver;
    }

    public void scrollToElement(
            WebElement element) {

        JavascriptExecutor js =
                (JavascriptExecutor) driver;

        js.executeScript(
                "arguments[0].scrollIntoView(true);",
                element
        );
    }

    public void click(
            WebElement element) {

        JavascriptExecutor js =
                (JavascriptExecutor) driver;

        js.executeScript(
                "arguments[0].click();",
                element
        );
    }
}
```

---

# 24. TestNG DataProvider

DataProvider allows the same test to execute with different test data.

```java
package dataproviders;

import org.testng.annotations.DataProvider;

public class TestDataProvider {

    @DataProvider(name = "loginData")
    public Object[][] loginData() {

        return new Object[][] {

                {"user1", "password1"},
                {"user2", "password2"},
                {"user3", "password3"}

        };
    }
}
```

---

# 25. Using DataProvider

```java
@Test(
        dataProvider = "loginData",
        dataProviderClass = TestDataProvider.class
)
public void loginTest(
        String username,
        String password) {

    LoginPage loginPage =
            new LoginPage(
                    DriverManager.getDriver()
            );

    loginPage.login(
            username,
            password
    );
}
```

---

# 26. Parallel DataProvider

TestNG can run DataProvider iterations in parallel.

```java
@DataProvider(
        name = "loginData",
        parallel = true
)
public Object[][] loginData() {

    return new Object[][] {

            {"user1", "password1"},
            {"user2", "password2"},
            {"user3", "password3"}

    };
}
```

When using parallel execution, ThreadLocal WebDriver is especially important.

---

# 27. TestNG XML

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
        "https://testng.org/testng-1.0.dtd">

<suite
        name="Regression Suite"
        parallel="tests"
        thread-count="3">

    <test name="Login Tests">

        <classes>

            <class name="tests.LoginTest"/>

        </classes>

    </test>

    <test name="Account Tests">

        <classes>

            <class name="tests.AccountTest"/>

        </classes>

    </test>

</suite>
```

---

# 28. TestNG Groups

Tests can be categorized:

```java
@Test(groups = "smoke")
public void loginTest() {
}
```

```java
@Test(groups = "regression")
public void accountTest() {
}
```

Run only smoke tests:

```xml
<groups>

    <run>

        <include name="smoke"/>

    </run>

</groups>
```

---

# 29. Smoke and Regression Structure

A typical framework can have:

```text
Smoke
│
├── Login
├── Search
└── Logout

Regression
│
├── Login
├── Search
├── Account
├── Payment
├── Reports
└── Logout
```

Smoke tests validate critical functionality quickly.

Regression tests provide broader coverage.

---

# 30. Test Listener

TestNG listeners can react to test execution events.

Common interface:

```java
ITestListener
```

Example:

```java
public class TestListener
        implements ITestListener {

    @Override
    public void onTestFailure(
            ITestResult result) {

        System.out.println(
                "Test Failed: "
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
}
```

---

# 31. Register Listener

Using annotation:

```java
@Listeners(TestListener.class)
public class LoginTest
        extends BaseTest {
}
```

Or configure it in the TestNG XML depending on framework requirements.

---

# 32. Screenshot on Failure

A listener can capture a screenshot when a test fails.

```java
@Override
public void onTestFailure(
        ITestResult result) {

    WebDriver driver =
            DriverManager.getDriver();

    ScreenshotUtils.takeScreenshot(
            driver,
            result.getName()
    );
}
```

This makes failures much easier to investigate.

---

# 33. Reporting

A framework can integrate reporting tools such as:

* Allure
* ExtentReports
* TestNG reports
* Jenkins reports

A typical flow:

```text
Test
 |
Listener
 |
Result
 |
Report
```

---

# 34. Logging

Logging is different from reporting.

Logging helps developers understand what happened during execution.

Example:

```java
System.out.println(
        "Opening login page"
);
```

A production framework should generally use a logging framework instead of relying only on `System.out.println()`.

Common choices include:

```text
SLF4J
Logback
Log4j2
```

---

# 35. Example Logging

```java
private static final Logger logger =
        LoggerFactory.getLogger(
                LoginPage.class
        );

public void enterUsername(
        String usernameValue) {

    logger.info(
            "Entering username"
    );

    driver.findElement(username)
            .sendKeys(usernameValue);
}
```

Avoid logging passwords or other sensitive values.

---

# 36. Test Data Management

Test data should be separated from automation logic.

Possible sources:

```text
TestNG DataProvider
JSON
CSV
Excel
Database
Properties
API
```

Example JSON:

```json
{
    "username": "testuser",
    "password": "testpassword"
}
```

---

# 37. Page Components

Large applications often contain reusable components.

Examples:

```text
Header
Footer
Navigation Menu
Search Box
Calendar
Date Picker
Modal
Table
```

Example:

```java
public class Header {

    private WebDriver driver;

    private By logout =
            By.id("logout");

    public Header(WebDriver driver) {

        this.driver = driver;
    }

    public void logout() {

        driver.findElement(logout)
                .click();
    }
}
```

---

# 38. Page Object + Component Object

Example:

```text
HomePage
│
├── Header
├── NavigationMenu
└── SearchComponent
```

This avoids duplicating the same header or navigation logic across many Page Objects.

---

# 39. Selenium Grid Integration

Configuration:

```properties
grid.enabled=true
grid.url=http://localhost:4444
```

DriverFactory:

```java
if (gridEnabled) {

    return new RemoteWebDriver(
            new URL(gridUrl),
            new ChromeOptions()
    );
}
```

Now the same test can run:

```text
Local
   OR
Selenium Grid
```

without changing the test code.

---

# 40. Local vs Grid Configuration

Local:

```properties
browser=chrome
grid.enabled=false
```

Grid:

```properties
browser=chrome
grid.enabled=true
grid.url=http://localhost:4444
```

The test remains unchanged.

---

# 41. Jenkins CI/CD Flow

Typical flow:

```text
Developer
    |
    v
Git Repository
    |
    v
Jenkins
    |
    v
Checkout
    |
    v
Maven
    |
    v
TestNG
    |
    v
Selenium Grid
    |
    v
Browser Nodes
    |
    v
Tests
    |
    v
Reports
```

---

# 42. Jenkins Maven Command

Example:

```bash
mvn clean test
```

Environment:

```bash
mvn clean test -Denvironment=qa
```

Browser:

```bash
mvn clean test -Dbrowser=chrome
```

Grid:

```bash
mvn clean test -Dgrid.enabled=true
```

Multiple parameters:

```bash
mvn clean test -Denvironment=qa -Dbrowser=chrome -Dgrid.enabled=true
```

---

# 43. Maven Profiles

Maven profiles can represent environments.

Example:

```xml
<profiles>

    <profile>

        <id>qa</id>

        <properties>

            <environment>qa</environment>

        </properties>

    </profile>

    <profile>

        <id>prod</id>

        <properties>

            <environment>prod</environment>

        </properties>

    </profile>

</profiles>
```

Run:

```bash
mvn clean test -Pqa
```

or:

```bash
mvn clean test -Pprod
```

---

# 44. Git Integration

Recommended repository structure:

```text
SeleniumFramework
│
├── src
├── pom.xml
├── testng.xml
├── README.md
└── .gitignore
```

Common Git commands:

```bash
git init
```

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "Initial Selenium framework"
```

```bash
git push
```

---

# 45. .gitignore

Do not commit generated files such as:

```text
target/
screenshots/
reports/
logs/
*.log
.idea/
.vscode/
```

Example:

```text
target/
reports/
screenshots/
logs/
*.log
.idea/
```

Keep credentials and secrets out of source control.

---

# 46. Secrets Management

Do not store:

```text
username=realUser
password=realPassword
apiKey=realSecret
```

directly in Git.

Better options:

* Environment variables
* Jenkins Credentials
* Secret managers
* CI/CD protected variables
* External configuration

Example:

```java
String password =
        System.getenv("TEST_PASSWORD");
```

---

# 47. Environment Variables

Example:

```bash
set TEST_BROWSER=chrome
```

Java:

```java
String browser =
        System.getenv("TEST_BROWSER");
```

This is useful for CI/CD.

---

# 48. Complete Execution Flow

```text
1. Developer pushes code
             |
             v
2. Git Repository
             |
             v
3. Jenkins Pipeline
             |
             v
4. Maven Build
             |
             v
5. TestNG
             |
             v
6. BaseTest
             |
             v
7. DriverFactory
             |
             v
8. DriverManager
             |
             v
9. Local Driver / Grid
             |
             v
10. Page Object
             |
             v
11. Test Execution
             |
             v
12. Listener
             |
             +------> Screenshot
             |
             +------> Logs
             |
             +------> Report
```

---

# 49. Complete Example

## BaseTest

```java
public class BaseTest {

    @BeforeMethod
    public void setUp()
            throws MalformedURLException {

        String browser =
                System.getProperty(
                        "browser",
                        "chrome"
                );

        boolean grid =
                Boolean.parseBoolean(
                        System.getProperty(
                                "grid.enabled",
                                "false"
                        )
                );

        WebDriver driver =
                DriverFactory.createDriver(
                        browser,
                        grid
                );

        DriverManager.setDriver(driver);

        driver.manage()
                .window()
                .maximize();

        driver.get(
                System.getProperty(
                        "url",
                        "https://example.com"
                )
        );
    }

    @AfterMethod
    public void tearDown() {

        DriverManager.quitDriver();
    }
}
```

---

# 50. Complete Login Test

```java
public class LoginTest
        extends BaseTest {

    @Test
    public void validLoginTest() {

        LoginPage loginPage =
                new LoginPage(
                        DriverManager.getDriver()
                );

        HomePage homePage =
                loginPage.login(
                        "admin",
                        "password"
                );

        Assert.assertTrue(
                homePage.isHomePageDisplayed()
        );
    }
}
```

Run locally:

```bash
mvn clean test
```

Run with Grid:

```bash
mvn clean test -Dgrid.enabled=true
```

Run Firefox:

```bash
mvn clean test -Dbrowser=firefox
```

Run Chrome on Grid:

```bash
mvn clean test -Dbrowser=chrome -Dgrid.enabled=true
```

---

# 51. Framework Layers

A good framework can be divided into layers.

## Layer 1 — Test Layer

Contains:

```text
TestNG tests
Assertions
Test scenarios
Test groups
```

## Layer 2 — Page Layer

Contains:

```text
Locators
Page actions
Page navigation
Component objects
```

## Layer 3 — Driver Layer

Contains:

```text
DriverFactory
DriverManager
ThreadLocal
Grid
Browser options
```

## Layer 4 — Utility Layer

Contains:

```text
Waits
Screenshots
JavaScript
Excel
JSON
Configuration
Dates
Web tables
```

## Layer 5 — Infrastructure Layer

Contains:

```text
Maven
Git
Jenkins
Selenium Grid
Docker
Reporting
Logging
```

---

# 52. Dependency Direction

Keep dependencies flowing in a controlled direction.

```text
Tests
  |
  v
Pages
  |
  v
Driver / Utilities
```

Avoid making utilities depend on individual test classes.

Avoid circular dependencies.

---

# 53. Framework Design Principles

Follow these principles:

### Single Responsibility

Each class should have a clear purpose.

```text
DriverFactory
    → Creates drivers

DriverManager
    → Manages drivers

LoginPage
    → Handles login page

ConfigReader
    → Reads configuration
```

### DRY

**Don't Repeat Yourself.**

Move duplicated code into reusable methods.

### Encapsulation

Keep implementation details private.

```java
private By username =
        By.id("username");
```

### Reusability

Common functionality belongs in:

```text
Base classes
Utilities
Components
```

### Maintainability

Changing a locator should require updating one Page Object rather than dozens of tests.

---

# 54. Common Framework Mistakes

## Mistake 1 — Everything in one class

```text
Test.java
```

containing all:

* Driver code
* Locators
* Tests
* Utilities
* Data
* Reporting

Avoid this.

---

## Mistake 2 — Static WebDriver

```java
static WebDriver driver;
```

This is unsafe for parallel tests.

Use:

```java
ThreadLocal<WebDriver>
```

---

## Mistake 3 — Hard-coded environment URLs

Avoid:

```java
driver.get(
    "https://qa.example.com"
);
```

Use configuration.

---

## Mistake 4 — Hard-coded browser

Avoid:

```java
new ChromeDriver();
```

throughout the framework.

Use DriverFactory.

---

## Mistake 5 — Hard-coded credentials

Never commit real credentials to Git.

---

## Mistake 6 — Thread.sleep()

Avoid:

```java
Thread.sleep(5000);
```

Prefer explicit waits.

---

## Mistake 7 — Duplicate page locators

Store locators in Page Objects.

---

# 55. Interview Questions

## Q1. Explain your Selenium framework architecture.

A strong answer:

> My Selenium framework is based on Java, Selenium WebDriver, TestNG and Maven. I use Page Object Model for UI abstraction, DriverFactory for browser creation, ThreadLocal DriverManager for parallel execution, BaseTest for setup and teardown, utility classes for reusable functionality, TestNG DataProvider for test data, listeners for screenshots and reporting, Selenium Grid for remote execution, and Jenkins for CI/CD. Configuration and secrets are externalized so the same tests can run across different environments.

---

## Q2. Why do you use DriverFactory?

DriverFactory centralizes WebDriver creation and allows the framework to support:

```text
Chrome
Firefox
Edge
RemoteWebDriver
```

without duplicating browser creation logic.

---

## Q3. Why do you use DriverManager?

DriverManager manages the WebDriver lifecycle and provides thread-safe access to the driver.

---

## Q4. Why ThreadLocal?

Because parallel test threads need independent WebDriver instances.

---

## Q5. How do you run the same framework locally and on Grid?

Use a configuration property:

```properties
grid.enabled=false
```

for local execution and:

```properties
grid.enabled=true
```

for Grid execution.

DriverFactory selects either a local driver or RemoteWebDriver.

---

## Q6. How do you handle multiple environments?

Use external configuration:

```text
qa.properties
stage.properties
prod.properties
```

and select the environment using a system property or Maven profile.

---

## Q7. How do you handle test failures?

Use a TestNG listener to:

1. Detect failure
2. Capture screenshot
3. Log failure
4. Add result to report
5. Preserve useful diagnostic information

---

## Q8. How do you run tests in parallel?

Use TestNG:

```xml
parallel="tests"
thread-count="3"
```

and use ThreadLocal WebDriver.

---

## Q9. How do you integrate Jenkins?

Jenkins checks out the Git repository, executes Maven, runs TestNG tests, connects to Selenium Grid when configured, and publishes reports.

---

## Q10. How do you make a Selenium framework maintainable?

Use:

```text
POM
+
DriverFactory
+
ThreadLocal
+
BaseTest
+
Utilities
+
Configuration
+
Listeners
+
Data-driven testing
```

and keep each class responsible for one major area.

---

# 56. Senior-Level Framework Architecture

A mature automation framework can look like:

```text
                         Git
                          |
                          v
                       Jenkins
                          |
                    Maven / TestNG
                          |
              +-----------+-----------+
              |                       |
          Test Layer             Listener Layer
              |                       |
              v                       v
         BaseTest                 Reporting
              |
              v
        DriverManager
              |
          ThreadLocal
              |
              v
        DriverFactory
          /       \
       Local       Grid
        |            |
    WebDriver    RemoteWebDriver
                     |
                     v
                Selenium Grid
                     |
          +----------+----------+
          |          |          |
       Chrome     Firefox      Edge
          |
          v
     Page Objects
          |
          v
    Page Components
          |
          v
      Utilities
          |
      +---+---+---+
      |   |   |   |
    Wait JS  Data Config
```

---

# 57. Recommended Framework Checklist

* [ ] Java
* [ ] Selenium WebDriver
* [ ] TestNG
* [ ] Maven
* [ ] Page Object Model
* [ ] BasePage
* [ ] BaseTest
* [ ] DriverFactory
* [ ] DriverManager
* [ ] ThreadLocal
* [ ] ConfigReader
* [ ] WaitUtils
* [ ] ScreenshotUtils
* [ ] JavaScriptUtils
* [ ] DataProvider
* [ ] TestNG Groups
* [ ] TestNG Listeners
* [ ] Reporting
* [ ] Logging
* [ ] Selenium Grid
* [ ] Parallel execution
* [ ] Environment configuration
* [ ] Git
* [ ] Jenkins
* [ ] Secure credential management

---

# 58. Final Architecture Summary

The most important architecture to remember is:

```text
                    TESTNG
                       |
                       v
                   BASE TEST
                       |
                       v
                DRIVER MANAGER
                       |
                  THREAD LOCAL
                       |
                       v
                DRIVER FACTORY
                  /          \
              LOCAL           GRID
                |               |
             DRIVER      REMOTE DRIVER
                |               |
                +-------+-------+
                        |
                        v
                   PAGE OBJECTS
                        |
                        v
                  PAGE COMPONENTS
                        |
                        v
                    UTILITIES
                        |
                        v
               REPORTING / LOGGING
                        |
                        v
                     JENKINS
```

A professional Selenium framework should allow you to change the **browser, environment, execution mode, test data, or Grid configuration without changing the actual test scenarios**.

The core principle is:

```text
Test Logic
    ≠
Page Logic
    ≠
Driver Logic
    ≠
Configuration
    ≠
Test Data
    ≠
Reporting
```

Keeping these responsibilities separate is the foundation of a scalable Selenium automation framework.
