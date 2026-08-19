# Selenium Headless Browser

## 1. What is Headless Browser Testing?

A **headless browser** is a browser that runs without opening a visible graphical user interface (GUI).

Selenium can execute automation tests in headless mode using browsers such as:

- Google Chrome
- Mozilla Firefox
- Microsoft Edge

Headless execution is especially useful for:

- CI/CD pipelines
- Jenkins
- Docker containers
- Remote execution
- Faster automated test execution
- Server environments without a GUI

---

# 2. Headed vs Headless Browser

## Headed Mode

The browser opens normally and you can see the UI.

```java
WebDriver driver = new ChromeDriver();

driver.get("https://www.google.com");
+----------------------------------+
| Google Chrome                    |
+----------------------------------+
| https://www.google.com           |
|                                  |
|          Google                  |
|                                  |
+----------------------------------+
Headless Mode

The browser runs in the background without displaying a browser window.

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");


WebDriver driver = new ChromeDriver(options);

There is no visible browser window, but Selenium still interacts with the web page.

3. Why Use Headless Selenium?
Advantages
1. Useful in CI/CD

Servers running Jenkins or GitHub Actions may not have a graphical desktop.

Headless execution allows Selenium tests to run without a GUI.

2. Faster Execution

Headless browsers generally consume fewer resources because they don't need to render a visible browser window.

3. Lower Resource Usage

Headless execution can reduce:

CPU usage
Memory usage
Display-related overhead
4. Useful in Docker

Docker containers commonly run without a graphical desktop environment.

Headless Chrome or Firefox is therefore frequently used.

5. Parallel Execution

Headless mode is useful when running many browser instances simultaneously.

Example:

Test 1 → Chrome Headless
Test 2 → Chrome Headless
Test 3 → Chrome Headless
Test 4 → Chrome Headless
4. Chrome Headless Mode

Modern Selenium versions can run Chrome in headless mode using:

--headless=new

Example:

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;


public class ChromeHeadlessDemo {


    public static void main(String[] args) {


        ChromeOptions options = new ChromeOptions();


        options.addArguments("--headless=new");


        WebDriver driver = new ChromeDriver(options);


        driver.get("https://www.google.com");


        System.out.println(driver.getTitle());


        driver.quit();
    }
}
5. Chrome Headless with Window Size

One important difference with headless execution is that the browser does not have a normal visible window.

It is therefore a good practice to specify a window size.

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");
options.addArguments("--window-size=1920,1080");


WebDriver driver = new ChromeDriver(options);

Complete example:

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;


public class ChromeHeadlessWindowSize {


    public static void main(String[] args) {


        ChromeOptions options = new ChromeOptions();


        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");


        WebDriver driver = new ChromeDriver(options);


        try {
            driver.get("https://www.google.com");


            System.out.println("Title: " + driver.getTitle());


        } finally {
            driver.quit();
        }
    }
}
6. Setting Window Size Using Selenium

You can also set the window size using Selenium:

driver.manage().window().setSize(new Dimension(1920, 1080));

Import:

import org.openqa.selenium.Dimension;

Example:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");


WebDriver driver = new ChromeDriver(options);


driver.manage().window().setSize(new Dimension(1920, 1080));

However, setting the size directly through the Chrome option is often convenient for headless execution:

options.addArguments("--window-size=1920,1080");
7. Firefox Headless Mode

Firefox can also run in headless mode.

Use:

FirefoxOptions options = new FirefoxOptions();


options.addArguments("-headless");


WebDriver driver = new FirefoxDriver(options);

Example:

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;


public class FirefoxHeadlessDemo {


    public static void main(String[] args) {


        FirefoxOptions options = new FirefoxOptions();


        options.addArguments("-headless");


        WebDriver driver = new FirefoxDriver(options);


        try {
            driver.get("https://www.google.com");


            System.out.println(driver.getTitle());


        } finally {
            driver.quit();
        }
    }
}
8. Firefox Headless with Window Size
FirefoxOptions options = new FirefoxOptions();


options.addArguments("-headless");


WebDriver driver = new FirefoxDriver(options);


driver.manage().window().setSize(
    new Dimension(1920, 1080)
);

Import:

import org.openqa.selenium.Dimension;
9. Microsoft Edge Headless Mode

Microsoft Edge is Chromium-based and supports headless execution.

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.edge.EdgeOptions;


public class EdgeHeadlessDemo {


    public static void main(String[] args) {


        EdgeOptions options = new EdgeOptions();


        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");


        WebDriver driver = new EdgeDriver(options);


        try {
            driver.get("https://www.google.com");


            System.out.println(driver.getTitle());


        } finally {
            driver.quit();
        }
    }
}
10. Headless Chrome with Common Options

A real-world test may use multiple browser options.

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");
options.addArguments("--window-size=1920,1080");
options.addArguments("--disable-gpu");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");


WebDriver driver = new ChromeDriver(options);
Important

Options such as:

--no-sandbox
--disable-dev-shm-usage

are especially common in Linux/Docker environments.

Do not automatically add every Chrome argument to every project. Use options based on the execution environment and the problem you are solving.

11. Headless Browser with Selenium Manager

Modern Selenium versions can automatically manage browser drivers through Selenium Manager.

Example:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");


WebDriver driver = new ChromeDriver(options);

You normally do not need:

System.setProperty(
    "webdriver.chrome.driver",
    "path/to/chromedriver"
);

when Selenium Manager can resolve the appropriate driver.

12. Headless Browser with TestNG

Example TestNG test:

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;


public class HeadlessTest {


    WebDriver driver;


    @BeforeMethod
    public void setup() {


        ChromeOptions options = new ChromeOptions();


        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");


        driver = new ChromeDriver(options);
    }


    @Test
    public void verifyGoogleTitle() {


        driver.get("https://www.google.com");


        System.out.println(
            "Title: " + driver.getTitle()
        );
    }


    @AfterMethod
    public void tearDown() {


        if (driver != null) {
            driver.quit();
        }
    }
}
13. Headless Browser with Page Object Model

A framework should normally keep browser configuration separate from test classes.

Example:

src
 └── test
     └── java
         ├── base
         │   └── BaseTest.java
         │
         ├── pages
         │   └── LoginPage.java
         │
         └── tests
             └── LoginTest.java
14. BaseTest with Headless Configuration
package base;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;


public class BaseTest {


    protected WebDriver driver;


    @BeforeMethod
    public void setup() {


        ChromeOptions options = new ChromeOptions();


        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");


        driver = new ChromeDriver(options);


        driver.manage().deleteAllCookies();
    }


    @AfterMethod
    public void tearDown() {


        if (driver != null) {
            driver.quit();
        }
    }
}
15. Test Class
package tests;


import base.BaseTest;
import org.testng.Assert;
import org.testng.annotations.Test;


public class GoogleTest extends BaseTest {


    @Test
    public void verifyGoogleTitle() {


        driver.get("https://www.google.com");


        String title = driver.getTitle();


        System.out.println("Title: " + title);


        Assert.assertTrue(
            title.toLowerCase().contains("google")
        );
    }
}
16. Running Headless Based on Configuration

In a framework, it is better not to hard-code headless mode.

For example:

boolean headless = true;

Then:

ChromeOptions options = new ChromeOptions();


if (headless) {
    options.addArguments("--headless=new");
}


driver = new ChromeDriver(options);

This allows:

Local Development → Headed
CI/CD              → Headless
Debugging          → Headed
Docker             → Headless
17. Using System Property

You can control headless execution from the command line.

Example:

boolean headless =
    Boolean.parseBoolean(
        System.getProperty("headless", "false")
    );

Then:

ChromeOptions options = new ChromeOptions();


if (headless) {
    options.addArguments("--headless=new");
}


driver = new ChromeDriver(options);

Run normally:

mvn test

Headless:

mvn test -Dheadless=true

Headed:

mvn test -Dheadless=false

This approach is very useful in CI/CD.

18. Headless Execution in Jenkins

A typical Jenkins pipeline can run:

mvn clean test -Dheadless=true

Your framework reads:

System.getProperty("headless", "false")

and enables:

options.addArguments("--headless=new");

Architecture:

Jenkins
   |
   v
mvn clean test -Dheadless=true
   |
   v
TestNG
   |
   v
Selenium
   |
   v
Chrome Headless
   |
   v
Application
19. Headless Execution in Docker

A Docker-based Selenium execution commonly looks like:

Docker Container
       |
       v
Java + Maven
       |
       v
TestNG
       |
       v
Selenium
       |
       v
Chrome Headless

Typical Chrome options may include:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
options.addArguments("--window-size=1920,1080");
20. Taking Screenshots in Headless Mode

Screenshots work in headless mode.

Example:

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;


File screenshot =
    ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);

Using Java NIO:

import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;


File screenshot =
    ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);


Files.copy(
    screenshot.toPath(),
    Path.of("screenshots/home.png"),
    StandardCopyOption.REPLACE_EXISTING
);

This is especially important in CI/CD because you cannot visually watch the browser.

21. Debugging Headless Failures

One common problem is:

Test passes in headed mode
Test fails in headless mode

Possible causes include:

Different viewport size
Responsive UI behavior
Timing differences
Element outside the viewport
JavaScript behavior
Browser-specific behavior
Incorrect window dimensions
Download behavior
Authentication or popup differences

First try:

options.addArguments("--window-size=1920,1080");

Also capture:

Screenshot
Page source
Browser console logs
TestNG report
Exception stack trace
22. Headless and Responsive Layout

A website may display different content depending on viewport size.

For example:

1920 x 1080
      |
      v
Desktop Layout

while:

375 x 812
      |
      v
Mobile Layout

Therefore, specifying the viewport is important for consistent automation.

options.addArguments("--window-size=1920,1080");
23. Headless Does Not Mean Mobile

A common misconception is:

Headless = Mobile

This is incorrect.

Headless means:

No visible browser UI

Mobile emulation means:

Browser behaves like a mobile device

These are separate concepts.

You can combine them if required.

24. Chrome Mobile Emulation + Headless

Example:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");


Map<String, Object> mobileEmulation =
    new HashMap<>();


mobileEmulation.put(
    "deviceName",
    "iPhone X"
);


options.setExperimentalOption(
    "mobileEmulation",
    mobileEmulation
);


WebDriver driver =
    new ChromeDriver(options);

Required imports:

import java.util.HashMap;
import java.util.Map;
25. Headless Browser and Downloads

Downloads may require additional browser configuration.

Example:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");


Map<String, Object> prefs =
    new HashMap<>();


prefs.put(
    "download.default_directory",
    "/tmp/downloads"
);


prefs.put(
    "download.prompt_for_download",
    false
);


options.setExperimentalOption(
    "prefs",
    prefs
);


WebDriver driver =
    new ChromeDriver(options);

The exact download setup may vary by browser and environment.

26. Headless Browser and Alerts

JavaScript alerts can still be handled in headless mode.

Alert alert = driver.switchTo().alert();


System.out.println(alert.getText());


alert.accept();

Headless mode does not remove Selenium's ability to interact with browser dialogs.

27. Headless Browser and JavaScript

JavaScript continues to execute normally.

Example:

JavascriptExecutor js =
    (JavascriptExecutor) driver;


js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);

Headless does not mean JavaScript is disabled.

28. Headless Browser and WebDriverWait

Explicit waits should still be used.

WebDriverWait wait =
    new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
    );


WebElement element =
    wait.until(
        ExpectedConditions.visibilityOfElementLocated(
            By.id("username")
        )
    );

Do not replace proper synchronization with:

Thread.sleep(5000);

Headless execution can sometimes expose timing problems that were hidden in headed execution.

29. Headless Browser and Parallel Execution

Headless browsers are useful for parallel testing.

Example:

Thread 1 → Chrome Headless
Thread 2 → Chrome Headless
Thread 3 → Chrome Headless
Thread 4 → Chrome Headless

With TestNG:

<suite
    name="ParallelSuite"
    parallel="tests"
    thread-count="4">

Each thread should have its own WebDriver instance.

A common framework approach is:

ThreadLocal<WebDriver>

Example:

private static ThreadLocal<WebDriver>
    driver = new ThreadLocal<>();

This prevents different parallel tests from sharing the same browser instance.

30. Headless vs Selenium Grid

Headless and Selenium Grid solve different problems.

Headless

Controls whether the browser UI is visible.

Chrome
 |
 +-- Headless
Selenium Grid

Controls where the browser runs.

Test
 |
 v
Selenium Grid
 |
 +-- Machine 1 → Chrome
 +-- Machine 2 → Firefox
 +-- Machine 3 → Edge

They can also be combined:

Test
 |
 v
Selenium Grid
 |
 v
Chrome Headless
31. RemoteWebDriver + Headless Chrome

When using Selenium Grid, browser options are passed to the remote browser.

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");
options.addArguments("--window-size=1920,1080");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );

This allows the remote Chrome instance to run headlessly.

32. Example Framework Configuration

A framework can define:

browser=chrome
headless=true

Java:

String browser =
    config.getProperty("browser");


boolean headless =
    Boolean.parseBoolean(
        config.getProperty("headless")
    );

Then:

if (browser.equalsIgnoreCase("chrome")) {


    ChromeOptions options =
        new ChromeOptions();


    if (headless) {
        options.addArguments("--headless=new");
    }


    options.addArguments(
        "--window-size=1920,1080"
    );


    driver = new ChromeDriver(options);
}
33. Recommended Framework Design

A scalable Selenium framework can look like:

seleniumStudy
│
├── config
│   └── config.properties
│
├── base
│   └── BaseTest.java
│
├── driver
│   └── DriverFactory.java
│
├── pages
│   └── LoginPage.java
│
├── tests
│   └── LoginTest.java
│
├── utilities
│   └── ScreenshotUtil.java
│
└── testng.xml

DriverFactory can control:

Browser
Headless
Window Size
Local/Remote
Chrome Options
Firefox Options
Edge Options
34. Recommended DriverFactory Example
public class DriverFactory {


    public static WebDriver createDriver(
        String browser,
        boolean headless
    ) {


        if (browser.equalsIgnoreCase("chrome")) {


            ChromeOptions options =
                new ChromeOptions();


            if (headless) {
                options.addArguments(
                    "--headless=new"
                );
            }


            options.addArguments(
                "--window-size=1920,1080"
            );


            return new ChromeDriver(options);
        }


        if (browser.equalsIgnoreCase("firefox")) {


            FirefoxOptions options =
                new FirefoxOptions();


            if (headless) {
                options.addArguments(
                    "-headless"
                );
            }


            return new FirefoxDriver(options);
        }


        throw new IllegalArgumentException(
            "Unsupported browser: " + browser
        );
    }
}
35. Complete Headless Selenium Example
import java.time.Duration;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;


public class HeadlessCompleteExample {


    public static void main(String[] args) {


        ChromeOptions options =
            new ChromeOptions();


        options.addArguments("--headless=new");
        options.addArguments(
            "--window-size=1920,1080"
        );


        WebDriver driver =
            new ChromeDriver(options);


        try {


            driver.get("https://www.google.com");


            WebDriverWait wait =
                new WebDriverWait(
                    driver,
                    Duration.ofSeconds(10)
                );


            WebElement searchBox =
                wait.until(
                    ExpectedConditions.visibilityOfElementLocated(
                        By.name("q")
                    )
                );


            searchBox.sendKeys(
                "Selenium WebDriver"
            );


            System.out.println(
                "Page Title: " +
                driver.getTitle()
            );


        } finally {


            driver.quit();
        }
    }
}
36. Common Headless Chrome Arguments
Argument	Purpose
--headless=new	Run Chrome without visible UI
--window-size=1920,1080	Set viewport/window size
--no-sandbox	Often used in container/Linux environments
--disable-dev-shm-usage	Helps with limited /dev/shm environments
--disable-gpu	Sometimes used for compatibility in specific environments

Do not blindly use all options. Add only the options required by your environment.

37. Common Problems
Problem 1: Element Not Found

Possible reason:

Headless viewport differs from headed viewport

Solution:

options.addArguments(
    "--window-size=1920,1080"
);
Problem 2: Test Works Locally but Fails in Jenkins

Check:

Browser version
Driver compatibility
OS
Browser options
File permissions
Download paths
Display requirements
Environment variables

Headless mode is often appropriate for Jenkins.

Problem 3: Chrome Crashes in Docker

Common environment-specific options include:

options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");

Also verify the container has compatible browser and driver components.

Problem 4: Screenshots Look Different

Check:

--window-size=1920,1080

Responsive layouts can change depending on viewport dimensions.

38. Headless Best Practices
1. Use headless for CI/CD
Local       → Headed
CI/CD       → Headless
2. Always define a predictable viewport
options.addArguments(
    "--window-size=1920,1080"
);
3. Capture screenshots on failure

This is especially important when there is no visible browser.

4. Use explicit waits
WebDriverWait

instead of excessive:

Thread.sleep()
5. Keep browser configuration centralized

Use:

DriverFactory

or:

BrowserFactory

rather than configuring Chrome separately in every test.

6. Make headless configurable

For example:

mvn test -Dheadless=true
7. Test important flows in both modes

Headless should not completely replace headed validation.

39. Interview Questions
Beginner
1. What is a headless browser?

A browser that runs without displaying a graphical user interface.

2. Why is headless mode used?

Common reasons include:

CI/CD execution
Docker execution
Faster execution
Reduced resource consumption
Server environments without GUI
3. How do you run Chrome headlessly?
ChromeOptions options =
    new ChromeOptions();


options.addArguments(
    "--headless=new"
);


WebDriver driver =
    new ChromeDriver(options);
4. Can Selenium take screenshots in headless mode?

Yes.

TakesScreenshot screenshot =
    (TakesScreenshot) driver;
5. Can JavaScript execute in headless mode?

Yes.

40. Senior-Level Interview Questions
1. Why might a test pass in headed mode but fail in headless mode?

Possible causes include:

Different viewport dimensions
Responsive UI
Timing issues
Element visibility
Browser-specific behavior
Different rendering behavior
File/download configuration
2. How would you debug a headless test failure in Jenkins?

I would capture:

Screenshot
Page source
Logs
Exception stack trace
Browser logs
TestNG report

Then compare the Jenkins environment with local execution.

3. How would you make headless configurable?

Use a system property:

boolean headless =
    Boolean.parseBoolean(
        System.getProperty(
            "headless",
            "false"
        )
    );

Then:

if (headless) {
    options.addArguments("--headless=new");
}

Run:

mvn test -Dheadless=true
4. How would you design headless execution in a Selenium framework?

Centralize browser creation in a DriverFactory.

Test
 |
 v
DriverFactory
 |
 +-- Browser
 +-- Headless
 +-- Window Size
 +-- Local/Remote
 +-- Browser Options
 |
 v
WebDriver

This avoids duplicating browser configuration throughout the framework.

5. Can headless Chrome be used with Selenium Grid?

Yes.

ChromeOptions options =
    new ChromeOptions();


options.addArguments(
    "--headless=new"
);


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );
6. Is headless mode the same as mobile emulation?

No.

Headless controls browser visibility.

Mobile emulation changes browser/device characteristics.

They are independent capabilities and can be combined.

41. Key Takeaways
Headless Browser
       |
       +-- No visible browser UI
       |
       +-- Useful for CI/CD
       |
       +-- Useful for Docker
       |
       +-- Lower resource usage
       |
       +-- Useful for parallel execution
       |
       +-- Supports screenshots
       |
       +-- Supports JavaScript
       |
       +-- Supports WebDriverWait
       |
       +-- Works with Selenium Grid

For Chrome:

ChromeOptions options =
    new ChromeOptions();


options.addArguments(
    "--headless=new"
);


options.addArguments(
    "--window-size=1920,1080"
);


WebDriver driver =
    new ChromeDriver(options);

For Firefox:

FirefoxOptions options =
    new FirefoxOptions();


options.addArguments("-headless");


WebDriver driver =
    new FirefoxDriver(options);

For CI/CD:

mvn clean test -Dheadless=true

Best practice: keep headless configuration centralized and configurable rather than hard-coding it in every Selenium test.


