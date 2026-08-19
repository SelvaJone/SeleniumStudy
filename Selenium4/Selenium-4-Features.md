# Selenium 4 Features

## 1. Introduction

Selenium 4 is a major version of Selenium WebDriver that introduced several improvements to the WebDriver API, browser automation, developer tooling, and standards support.

Selenium 4 provides features such as:

* Selenium Manager
* Relative Locators
* New tab and window APIs
* Improved Actions API
* Full-page screenshots
* Element screenshots
* `getRect()`
* Chrome DevTools integration
* WebDriver BiDi support
* Improved Selenium Grid
* W3C WebDriver standardization
* Better browser management

---

# 2. Selenium 3 vs Selenium 4

| Feature              | Selenium 3                        | Selenium 4             |
| -------------------- | --------------------------------- | ---------------------- |
| Protocol             | JSON Wire Protocol                | W3C WebDriver          |
| Driver Management    | Usually manual                    | Selenium Manager       |
| Relative Locators    | Not available                     | Available              |
| New Tab API          | Limited                           | Supported              |
| New Window API       | Limited                           | Supported              |
| Full Page Screenshot | Not standardized                  | Supported              |
| Element Screenshot   | Supported                         | Improved               |
| `getRect()`          | Available in some implementations | Standardized API       |
| DevTools Integration | Limited                           | Improved               |
| WebDriver BiDi       | No                                | Supported              |
| Grid                 | Selenium Grid 3                   | Selenium Grid 4        |
| Actions API          | Available                         | Improved               |
| Browser Options      | Older APIs                        | Modern browser options |

---

# 3. W3C WebDriver Standard

Selenium 4 uses the W3C WebDriver protocol as the standard communication mechanism between Selenium and browsers.

The basic architecture is:

```text
Java Test
    ↓
Selenium WebDriver API
    ↓
W3C WebDriver Protocol
    ↓
Browser Driver
    ↓
Browser
```

Examples:

```text
Java
 ↓
ChromeDriver
 ↓
Chrome
```

```text
Java
 ↓
FirefoxDriver
 ↓
Firefox
```

The W3C standard improves interoperability and removes much of the protocol-specific behavior that existed in Selenium 3.

---

# 4. Selenium Manager

One of the most useful Selenium 4 features is Selenium Manager.

Previously, users commonly had to manually download browser drivers.

Example older approach:

```java
System.setProperty(
    "webdriver.chrome.driver",
    "C:\\drivers\\chromedriver.exe"
);

WebDriver driver = new ChromeDriver();
```

With modern Selenium:

```java
WebDriver driver = new ChromeDriver();
```

Selenium Manager can automatically discover and manage the appropriate driver.

This simplifies:

* Driver setup
* Driver version management
* Local development
* CI environments

---

# 5. ChromeDriver Example

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class SeleniumManagerDemo {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://example.com");

        System.out.println(driver.getTitle());

        driver.quit();
    }
}
```

No explicit `chromedriver.exe` path is required in the normal Selenium Manager workflow.

---

# 6. Relative Locators

Relative Locators allow elements to be located based on their position relative to another element.

Supported relationships include:

* Above
* Below
* Left
* Right
* Near

Import:

```java
import static org.openqa.selenium.support.locators.RelativeLocator.with;
```

---

# 7. `above()`

Suppose the page contains:

```html
<input id="email">
<input id="password">
```

You can locate an element above another element.

```java
WebElement password =
        driver.findElement(By.id("password"));

WebElement email =
        driver.findElement(
            with(By.tagName("input"))
            .above(password)
        );
```

---

# 8. `below()`

```java
WebElement email =
        driver.findElement(By.id("email"));

WebElement password =
        driver.findElement(
            with(By.tagName("input"))
            .below(email)
        );
```

---

# 9. `leftOf()`

```java
WebElement password =
        driver.findElement(By.id("password"));

WebElement email =
        driver.findElement(
            with(By.tagName("input"))
            .leftOf(password)
        );
```

---

# 10. `rightOf()`

```java
WebElement email =
        driver.findElement(By.id("email"));

WebElement password =
        driver.findElement(
            with(By.tagName("input"))
            .rightOf(email)
        );
```

---

# 11. `near()`

`near()` finds an element close to another element.

```java
WebElement loginButton =
        driver.findElement(By.id("login"));

WebElement nearbyElement =
        driver.findElement(
            with(By.tagName("input"))
            .near(loginButton)
        );
```

---

# 12. Combining Relative Locators

Relative locators can be chained.

Example:

```java
WebElement element =
        driver.findElement(
            with(By.tagName("input"))
            .above(password)
            .near(loginButton)
        );
```

Use relative locators when the visual relationship between elements is meaningful and stable.

---

# 13. New Window API

Selenium 4 introduced a convenient API for opening a new window or tab.

### New Window

```java
driver.switchTo().newWindow(WindowType.WINDOW);
```

### New Tab

```java
driver.switchTo().newWindow(WindowType.TAB);
```

Import:

```java
import org.openqa.selenium.WindowType;
```

---

# 14. New Tab Example

```java
WebDriver driver = new ChromeDriver();

driver.get("https://example.com");

driver.switchTo().newWindow(WindowType.TAB);

driver.get("https://google.com");

System.out.println(driver.getTitle());

driver.quit();
```

---

# 15. New Window Example

```java
driver.get("https://example.com");

driver.switchTo().newWindow(WindowType.WINDOW);

driver.get("https://google.com");

System.out.println(driver.getTitle());
```

The new tab/window is automatically selected.

---

# 16. Selenium 3 Approach vs Selenium 4

Older approaches often used:

```java
((JavascriptExecutor) driver)
        .executeScript(
            "window.open('https://google.com','_blank');"
        );
```

Selenium 4 provides:

```java
driver.switchTo()
      .newWindow(WindowType.TAB);
```

This is cleaner and uses the WebDriver API directly.

---

# 17. Full Page Screenshot

Selenium 4 provides support for full-page screenshots in browsers that implement the relevant capability.

Example:

```java
File screenshot =
        ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);
```

For browser-specific full-page functionality, Selenium 4 also provides screenshot APIs through the `HasFullPageScreenshot` interface where supported.

Example:

```java
import org.openqa.selenium.HasFullPageScreenshot;
import org.openqa.selenium.OutputType;

File screenshot =
        ((HasFullPageScreenshot) driver)
        .getFullPageScreenshotAs(OutputType.FILE);
```

---

# 18. Element Screenshot

Selenium can capture an individual element.

```java
WebElement logo =
        driver.findElement(By.id("logo"));

File screenshot =
        logo.getScreenshotAs(OutputType.FILE);
```

This is useful for:

* UI validation
* Visual debugging
* Failure analysis
* Element-level evidence

---

# 19. `getRect()`

Selenium 4 provides the `getRect()` API to obtain an element's:

* X position
* Y position
* Width
* Height

Example:

```java
WebElement element =
        driver.findElement(By.id("login"));

Rectangle rect =
        element.getRect();

System.out.println("X: " + rect.getX());
System.out.println("Y: " + rect.getY());
System.out.println("Width: " + rect.getWidth());
System.out.println("Height: " + rect.getHeight());
```

Import:

```java
import org.openqa.selenium.Rectangle;
```

---

# 20. Why `getRect()` Is Useful

You can use it for UI validation.

Example:

```java
Rectangle rect =
        element.getRect();

Assert.assertTrue(rect.getWidth() > 100);
Assert.assertTrue(rect.getHeight() > 20);
```

It can help verify that an element:

* Exists in the expected location
* Has an expected size
* Is rendered with reasonable dimensions

---

# 21. Improved Actions API

Selenium 4 provides an improved W3C Actions API.

It supports:

* Mouse actions
* Keyboard actions
* Pointer actions
* Drag and drop
* Click and hold
* Move
* Double click
* Context click
* Key combinations

Example:

```java
Actions actions =
        new Actions(driver);

actions.moveToElement(element)
       .click()
       .perform();
```

---

# 22. Keyboard Actions

```java
Actions actions =
        new Actions(driver);

actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

For example:

```text
Ctrl + A
```

---

# 23. Mouse Actions

### Move

```java
actions.moveToElement(element)
       .perform();
```

### Click

```java
actions.click(element)
       .perform();
```

### Double click

```java
actions.doubleClick(element)
       .perform();
```

### Right click

```java
actions.contextClick(element)
       .perform();
```

---

# 24. Drag and Drop

```java
actions.dragAndDrop(
        source,
        target
).perform();
```

Or:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 25. Pause in Actions

Selenium 4 Actions API supports pauses.

```java
actions.moveToElement(element)
       .pause(Duration.ofSeconds(1))
       .click()
       .perform();
```

Import:

```java
import java.time.Duration;
```

---

# 26. Chrome DevTools Protocol

Selenium 4 introduced improved support for interacting with browser developer tools through Chrome DevTools Protocol (CDP).

CDP can be used for capabilities such as:

* Network interception
* Browser logs
* Geolocation
* Authentication
* Performance information
* Network throttling
* JavaScript execution
* Device emulation

Selenium's CDP APIs are primarily useful for Chromium-based browsers.

---

# 27. Network Throttling Example

Selenium can use DevTools commands to emulate network conditions.

Example:

```java
DevTools devTools =
        ((HasDevTools) driver).getDevTools();

devTools.createSession();
```

Depending on the Selenium version, the available DevTools APIs are mapped to the supported browser protocol version.

---

# 28. Geolocation

DevTools can be used to emulate a geographic location.

Conceptually:

```text
Test
 ↓
Selenium
 ↓
DevTools
 ↓
Browser
 ↓
Simulated Location
```

This is useful for testing:

* Location-based applications
* Dealer searches
* Maps
* Regional content
* Geo-dependent features

---

# 29. WebDriver BiDi

WebDriver BiDi means:

**WebDriver Bidirectional**

Traditional WebDriver communication is primarily command-oriented:

```text
Test
  ↓
Browser
  ↓
Response
```

BiDi enables communication in both directions:

```text
Test
  ↔
Browser
```

This allows automation frameworks to listen to browser events more effectively.

Potential use cases include:

* Console events
* Network events
* Log events
* Browser events
* Real-time monitoring

---

# 30. CDP vs WebDriver BiDi

| Feature              | CDP                      | WebDriver BiDi                     |
| -------------------- | ------------------------ | ---------------------------------- |
| Standard             | Chrome/Chromium protocol | Web standard                       |
| Browser support      | Primarily Chromium       | Designed for cross-browser support |
| Network events       | Yes                      | Yes                                |
| Console events       | Yes                      | Yes                                |
| Selenium integration | Yes                      | Yes                                |
| Vendor-specific      | More vendor-specific     | More standardized                  |

For new cross-browser automation, WebDriver BiDi is the strategic direction, while CDP remains useful for Chromium-specific capabilities.

---

# 31. Selenium Grid 4

Selenium Grid was significantly improved in Selenium 4.

Grid 4 supports:

* Standalone mode
* Hub/node mode
* Distributed mode
* Docker environments
* Better observability
* Improved scalability
* Modern WebDriver support

---

# 32. Selenium Grid 4 Modes

### Standalone

```text
Test
 ↓
Selenium Grid
 ↓
Browser
```

### Hub and Node

```text
             ┌── Chrome Node
Test → Hub ──┼── Firefox Node
             └── Edge Node
```

### Distributed

```text
Router
  ↓
Distributor
  ↓
Session Queue
  ↓
Nodes
```

---

# 33. Selenium Grid + Parallel Execution

Selenium 4 Grid is particularly useful with TestNG parallel execution.

```text
TestNG
  │
  ├── Thread 1 → Chrome
  ├── Thread 2 → Firefox
  ├── Thread 3 → Edge
  └── Thread 4 → Chrome
             ↓
        Selenium Grid
```

This allows large regression suites to run across multiple browser environments.

---

# 34. Browser Options

Selenium 4 uses modern browser-specific Options classes.

### Chrome

```java
ChromeOptions options =
        new ChromeOptions();

WebDriver driver =
        new ChromeDriver(options);
```

### Firefox

```java
FirefoxOptions options =
        new FirefoxOptions();

WebDriver driver =
        new FirefoxDriver(options);
```

### Edge

```java
EdgeOptions options =
        new EdgeOptions();

WebDriver driver =
        new EdgeDriver(options);
```

---

# 35. Headless Execution

Chrome:

```java
ChromeOptions options =
        new ChromeOptions();

options.addArguments("--headless=new");

WebDriver driver =
        new ChromeDriver(options);
```

Headless execution is useful for:

* CI/CD
* Jenkins
* Docker
* Cloud execution
* Server environments

---

# 36. Window and Tab Handles

Selenium 4 still supports traditional window handles.

```java
String currentWindow =
        driver.getWindowHandle();

Set<String> windows =
        driver.getWindowHandles();
```

Example:

```java
for (String window : driver.getWindowHandles()) {

    driver.switchTo().window(window);

    System.out.println(
            driver.getTitle());
}
```

---

# 37. New Tab with Existing Page

```java
String originalWindow =
        driver.getWindowHandle();

driver.switchTo()
      .newWindow(WindowType.TAB);

driver.get("https://google.com");

System.out.println(
        driver.getTitle());

driver.switchTo()
      .window(originalWindow);
```

---

# 38. Selenium 4 Waits

Modern Selenium Java code should use:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );
```

Example:

```java
WebElement login =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("login")
                )
        );

login.click();
```

Import:

```java
import java.time.Duration;
```

---

# 39. Selenium 4 and Java Time API

Older Selenium code commonly used:

```java
new WebDriverWait(driver, 10);
```

Modern Selenium uses:

```java
new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
);
```

This uses Java's `Duration` API.

---

# 40. Selenium 4 Page Factory

Page Factory continues to work with Selenium 4.

Example:

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
            String pass) {

        username.sendKeys(user);
        password.sendKeys(pass);
        loginButton.click();
    }
}
```

---

# 41. Selenium 4 and JavaScript

JavaScriptExecutor continues to be available.

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript(
        "arguments[0].click();",
        element
);
```

However, JavaScript should generally not replace normal WebDriver interactions unless there is a specific reason.

Prefer:

```java
element.click();
```

before:

```java
js.executeScript(
        "arguments[0].click();",
        element
);
```

---

# 42. Selenium 4 Framework Example

A modern framework can look like:

```text
Selenium Framework
│
├── DriverManager
│   └── ThreadLocal<WebDriver>
│
├── DriverFactory
│
├── BaseTest
│
├── Pages
│   ├── LoginPage
│   ├── HomePage
│   └── SearchPage
│
├── Tests
│
├── Utilities
│
├── Listeners
│
├── Reports
│
├── testng.xml
│
└── pom.xml
```

Technologies:

```text
Java
Selenium 4
TestNG
Maven
ThreadLocal
Selenium Grid
Jenkins
Docker
```

---

# 43. Selenium 4 Example

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WindowType;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class Selenium4Demo {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        try {

            driver.manage()
                  .window()
                  .maximize();

            driver.get(
                    "https://example.com"
            );

            WebDriverWait wait =
                    new WebDriverWait(
                            driver,
                            Duration.ofSeconds(10)
                    );

            wait.until(
                    ExpectedConditions
                        .presenceOfElementLocated(
                            By.tagName("body")
                        )
            );

            System.out.println(
                    driver.getTitle()
            );

            // Open a new tab
            driver.switchTo()
                  .newWindow(
                          WindowType.TAB
                  );

            driver.get(
                    "https://google.com"
            );

            System.out.println(
                    driver.getTitle()
            );

        } finally {

            driver.quit();
        }
    }
}
```

---

# 44. Important Selenium 4 APIs

### New Tab

```java
driver.switchTo()
      .newWindow(WindowType.TAB);
```

### New Window

```java
driver.switchTo()
      .newWindow(WindowType.WINDOW);
```

### Element Rectangle

```java
element.getRect();
```

### Element Screenshot

```java
element.getScreenshotAs(
        OutputType.FILE
);
```

### Thread-Safe Driver

```java
ThreadLocal<WebDriver>
```

### Modern Wait

```java
new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
);
```

### Relative Locator

```java
with(By.tagName("input"))
        .above(element);
```

### Selenium Manager

```java
new ChromeDriver();
```

---

# 45. Common Selenium 4 Interview Questions

## Q1. What are the major features of Selenium 4?

Important features include:

* W3C WebDriver protocol
* Selenium Manager
* Relative Locators
* New tab/window APIs
* Selenium Grid 4
* Improved Actions API
* DevTools integration
* WebDriver BiDi
* Improved screenshots
* `getRect()`

---

## Q2. What is Selenium Manager?

Selenium Manager is Selenium's built-in driver/browser management mechanism that reduces the need to manually configure browser driver executables.

---

## Q3. What are Relative Locators?

Relative Locators locate elements based on their position relative to another element.

Examples:

```java
above()
below()
leftOf()
rightOf()
near()
```

---

## Q4. How do you open a new tab in Selenium 4?

```java
driver.switchTo()
      .newWindow(WindowType.TAB);
```

---

## Q5. How do you open a new browser window?

```java
driver.switchTo()
      .newWindow(WindowType.WINDOW);
```

---

## Q6. What is WebDriver BiDi?

WebDriver BiDi is a bidirectional WebDriver protocol that enables automation clients to receive browser events in addition to sending commands.

---

## Q7. What is the difference between CDP and WebDriver BiDi?

CDP is primarily a browser-vendor protocol associated with Chromium, while WebDriver BiDi is designed as a standardized cross-browser automation protocol.

---

## Q8. What is Selenium Grid 4?

Selenium Grid 4 is Selenium's modern distributed execution platform for running WebDriver sessions across different machines, browsers, and environments.

---

## Q9. What is `getRect()`?

It returns an element's position and dimensions:

```text
X
Y
Width
Height
```

---

## Q10. How do you take an element screenshot?

```java
element.getScreenshotAs(
        OutputType.FILE
);
```

---

# 46. Selenium 4 Best Practices

1. Use the latest stable Selenium version compatible with your project.
2. Let Selenium Manager handle drivers when appropriate.
3. Prefer stable locators.
4. Use explicit waits instead of unnecessary sleeps.
5. Use `Duration` with Selenium 4 waits.
6. Use ThreadLocal for parallel WebDriver sessions.
7. Use Selenium Grid for distributed execution.
8. Use browser Options instead of deprecated configuration patterns.
9. Use WebDriver APIs before resorting to JavaScript.
10. Keep test data independent for parallel execution.
11. Use WebDriver BiDi where supported and appropriate for event-driven automation.
12. Keep CDP usage isolated when browser-specific functionality is required.

---

# 47. Quick Revision

```text
Selenium 4
│
├── W3C WebDriver
│
├── Selenium Manager
│
├── Relative Locators
│   ├── above()
│   ├── below()
│   ├── leftOf()
│   ├── rightOf()
│   └── near()
│
├── New Window / Tab
│
├── Improved Actions
│
├── Screenshots
│
├── getRect()
│
├── Chrome DevTools Protocol
│
├── WebDriver BiDi
│
├── Selenium Grid 4
│
└── Better browser management
```

---

# 48. Key Takeaways

The most important Selenium 4 concepts for interviews and real-world automation are:

```text
1. W3C WebDriver
2. Selenium Manager
3. Relative Locators
4. newWindow()
5. WindowType.TAB
6. WindowType.WINDOW
7. getRect()
8. Element screenshots
9. Actions API
10. Selenium Grid 4
11. CDP
12. WebDriver BiDi
13. ThreadLocal parallel execution
14. Duration-based waits
15. Browser Options
```

Selenium 4 is particularly important for a modern **Java + TestNG + Maven + Selenium Grid + Jenkins + Docker** automation framework because it provides the APIs and infrastructure needed for scalable browser automation.
