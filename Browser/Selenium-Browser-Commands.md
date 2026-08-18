# Selenium Browser Commands

## 1. Introduction

Selenium WebDriver provides several commands to control and interact with the browser.

These commands are used for:

- Opening a URL
- Getting the current URL
- Getting the page title
- Getting page source
- Navigating backward
- Navigating forward
- Refreshing the page
- Managing browser windows
- Managing cookies
- Maximizing the browser
- Setting browser size
- Closing windows
- Closing the browser

---

# 2. WebDriver

`WebDriver` is the main Selenium interface used to communicate with the browser.

Example:

    WebDriver driver =
        new ChromeDriver();

Import:

    import org.openqa.selenium.WebDriver;

    import org.openqa.selenium.chrome.ChromeDriver;

---

# 3. Opening a URL

Use:

    driver.get("https://example.com");

Example:

    WebDriver driver =
        new ChromeDriver();

    driver.get(
        "https://www.google.com"
    );

This opens the specified URL.

---

# 4. get() vs navigate().to()

You can also open a URL using:

    driver.navigate().to(
        "https://example.com"
    );

Both can navigate to a URL.

Example:

    driver.get(
        "https://example.com"
    );

or:

    driver.navigate().to(
        "https://example.com"
    );

---

# 5. getCurrentUrl()

Returns the current URL.

Example:

    String currentUrl =
        driver.getCurrentUrl();

    System.out.println(
        currentUrl
    );

Example output:

    https://example.com/login

---

# 6. getTitle()

Returns the title of the current page.

Example:

    String title =
        driver.getTitle();

    System.out.println(
        title
    );

---

# 7. getPageSource()

Returns the current page source.

Example:

    String source =
        driver.getPageSource();

    System.out.println(
        source
    );

This can be useful for debugging and inspecting the current DOM source.

---

# 8. Page Source Example

    driver.get(
        "https://example.com"
    );

    String source =
        driver.getPageSource();

    System.out.println(
        source
    );

---

# 9. navigate().back()

Moves the browser back one page.

Example:

    driver.navigate().back();

Example flow:

    Google
      ↓
    Amazon
      ↓
    Back
      ↓
    Google

---

# 10. navigate().forward()

Moves the browser forward one page.

Example:

    driver.navigate().forward();

Example flow:

    Google
      ↓
    Amazon
      ↓
    Back
      ↓
    Google
      ↓
    Forward
      ↓
    Amazon

---

# 11. navigate().refresh()

Refreshes the current page.

Example:

    driver.navigate().refresh();

---

# 12. Complete Navigation Example

    driver.get(
        "https://example.com"
    );

    driver.navigate().to(
        "https://example.com/login"
    );

    driver.navigate().back();

    driver.navigate().forward();

    driver.navigate().refresh();

---

# 13. close()

`close()` closes the current browser window or tab.

Example:

    driver.close();

If multiple windows are open, `close()` closes only the currently selected window.

---

# 14. quit()

`quit()` closes all browser windows associated with the WebDriver session and ends the WebDriver session.

Example:

    driver.quit();

---

# 15. close() vs quit()

| close() | quit() |
|---|---|
| Closes current window/tab | Closes all browser windows |
| WebDriver session may remain | Ends WebDriver session |
| Useful for individual windows | Usually used at test completion |

---

# 16. Recommended Browser Cleanup

Use:

    try {

        // Test execution

    } finally {

        driver.quit();
    }

This ensures the browser is closed even if the test fails.

---

# 17. Browser Window Management

Selenium provides:

    driver.manage().window()

This can be used to:

- Maximize
- Minimize
- Fullscreen
- Set size
- Set position
- Get position
- Get size

---

# 18. Maximize Browser

Example:

    driver.manage()
          .window()
          .maximize();

This maximizes the current browser window.

---

# 19. Fullscreen Browser

Example:

    driver.manage()
          .window()
          .fullscreen();

This requests fullscreen mode.

---

# 20. Minimize Browser

Example:

    driver.manage()
          .window()
          .minimize();

This minimizes the browser window.

---

# 21. Browser Window Size

You can set a specific window size.

Example:

    Dimension size =
        new Dimension(
            1280,
            720
        );

    driver.manage()
          .window()
          .setSize(size);

Import:

    import org.openqa.selenium.Dimension;

---

# 22. Get Window Size

Example:

    Dimension size =
        driver.manage()
              .window()
              .getSize();

    System.out.println(
        "Width: "
        + size.getWidth()
    );

    System.out.println(
        "Height: "
        + size.getHeight()
    );

---

# 23. Browser Window Position

You can set the browser window position.

Example:

    Point position =
        new Point(
            100,
            100
        );

    driver.manage()
          .window()
          .setPosition(
              position
          );

Imports:

    import org.openqa.selenium.Point;

---

# 24. Get Window Position

Example:

    Point position =
        driver.manage()
              .window()
              .getPosition();

    System.out.println(
        "X: "
        + position.getX()
    );

    System.out.println(
        "Y: "
        + position.getY()
    );

---

# 25. Browser Window Example

    driver.manage()
          .window()
          .maximize();

    Dimension size =
        driver.manage()
              .window()
              .getSize();

    System.out.println(
        size.getWidth()
    );

    System.out.println(
        size.getHeight()
    );

---

# 26. Browser Timeouts

Selenium allows you to configure different timeout settings.

Common timeouts:

- Implicit wait
- Page load timeout
- Script timeout

---

# 27. Page Load Timeout

Example:

    driver.manage()
          .timeouts()
          .pageLoadTimeout(
              Duration.ofSeconds(30)
          );

This defines how long WebDriver waits for a page load operation.

Import:

    import java.time.Duration;

---

# 28. Script Timeout

Used for asynchronous JavaScript execution.

Example:

    driver.manage()
          .timeouts()
          .scriptTimeout(
              Duration.ofSeconds(30)
          );

---

# 29. Implicit Wait

Example:

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

This affects element location operations.

For detailed information, see:

    Waits/Selenium-Waits.md

---

# 30. Browser Cookies

Selenium allows you to manage browser cookies.

Main operations include:

    addCookie()
    getCookieNamed()
    getCookies()
    deleteCookieNamed()
    deleteCookie()
    deleteAllCookies()

---

# 31. Add Cookie

Example:

    Cookie cookie =
        new Cookie(
            "username",
            "Selva"
        );

    driver.manage()
          .addCookie(cookie);

Import:

    import org.openqa.selenium.Cookie;

---

# 32. Get Cookie by Name

Example:

    Cookie cookie =
        driver.manage()
              .getCookieNamed(
                  "username"
              );

    if (cookie != null) {

        System.out.println(
            cookie.getValue()
        );
    }

---

# 33. Get All Cookies

Example:

    Set<Cookie> cookies =
        driver.manage()
              .getCookies();

    for (Cookie cookie : cookies) {

        System.out.println(
            cookie.getName()
            + " = "
            + cookie.getValue()
        );
    }

---

# 34. Delete Cookie by Name

Example:

    driver.manage()
          .deleteCookieNamed(
              "username"
          );

---

# 35. Delete Specific Cookie

Example:

    Cookie cookie =
        driver.manage()
              .getCookieNamed(
                  "username"
              );

    if (cookie != null) {

        driver.manage()
              .deleteCookie(cookie);
    }

---

# 36. Delete All Cookies

Example:

    driver.manage()
          .deleteAllCookies();

This removes cookies accessible to the current browser session.

---

# 37. Cookie Example

    driver.get(
        "https://example.com"
    );

    Cookie cookie =
        new Cookie(
            "test",
            "12345"
        );

    driver.manage()
          .addCookie(cookie);

    Cookie result =
        driver.manage()
              .getCookieNamed("test");

    System.out.println(
        result.getValue()
    );

    driver.manage()
          .deleteCookieNamed("test");

---

# 38. Browser Session

A WebDriver session is created when you instantiate a driver.

Example:

    WebDriver driver =
        new ChromeDriver();

The browser session continues until:

    driver.quit();

---

# 39. Session ID

WebDriver maintains a session identifier.

Depending on Selenium version and implementation, you can access session information through the WebDriver API where supported.

Example:

    SessionId sessionId =
        ((RemoteWebDriver) driver)
            .getSessionId();

Import:

    import org.openqa.selenium.remote.SessionId;

This can be useful for debugging remote executions.

---

# 40. RemoteWebDriver

`RemoteWebDriver` allows Selenium to communicate with a remote browser.

Example:

    WebDriver driver =
        new RemoteWebDriver(
            new URL(
                "http://localhost:4444"
            ),
            new ChromeOptions()
        );

This is commonly used with:

- Selenium Grid
- Remote machines
- Docker
- Cloud browser platforms

---

# 41. Browser Capabilities

Browser options/capabilities configure browser behavior.

For Chrome:

    ChromeOptions options =
        new ChromeOptions();

    WebDriver driver =
        new ChromeDriver(options);

Import:

    import org.openqa.selenium.chrome.ChromeOptions;

---

# 42. Headless Chrome

Chrome can run without displaying the browser UI.

Example:

    ChromeOptions options =
        new ChromeOptions();

    options.addArguments(
        "--headless=new"
    );

    WebDriver driver =
        new ChromeDriver(options);

Headless execution is commonly used in CI/CD environments.

---

# 43. Headless vs Normal Browser

### Normal

    Chrome
        ↓
    Browser UI visible

### Headless

    Chrome
        ↓
    No visible UI

Headless execution can be useful for:

- Jenkins
- CI/CD
- Docker
- Server environments
- Faster automated execution in some cases

---

# 44. Browser Arguments

Example:

    ChromeOptions options =
        new ChromeOptions();

    options.addArguments(
        "--start-maximized"
    );

    options.addArguments(
        "--disable-notifications"
    );

    WebDriver driver =
        new ChromeDriver(options);

Only use browser arguments when they are appropriate for the test environment.

---

# 45. ChromeOptions Example

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

---

# 46. Browser Automation Flow

Typical Selenium browser flow:

    Create WebDriver
          ↓
    Configure browser
          ↓
    Open URL
          ↓
    Maximize / configure window
          ↓
    Locate elements
          ↓
    Interact
          ↓
    Navigate
          ↓
    Validate
          ↓
    Quit browser

---

# 47. Basic Browser Test

    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.chrome.ChromeDriver;

    public class BrowserExample {

        public static void main(
                String[] args) {

            WebDriver driver =
                new ChromeDriver();

            try {

                driver.manage()
                      .window()
                      .maximize();

                driver.get(
                    "https://example.com"
                );

                System.out.println(
                    "Title: "
                    + driver.getTitle()
                );

                System.out.println(
                    "URL: "
                    + driver.getCurrentUrl()
                );

            } finally {

                driver.quit();
            }
        }
    }

---

# 48. Navigation Test

    driver.get(
        "https://example.com"
    );

    driver.navigate().to(
        "https://example.com/login"
    );

    System.out.println(
        driver.getCurrentUrl()
    );

    driver.navigate().back();

    driver.navigate().forward();

    driver.navigate().refresh();

---

# 49. Browser Commands Cheat Sheet

## Open URL

    driver.get(url);

## Navigate to URL

    driver.navigate().to(url);

## Back

    driver.navigate().back();

## Forward

    driver.navigate().forward();

## Refresh

    driver.navigate().refresh();

## Current URL

    driver.getCurrentUrl();

## Page Title

    driver.getTitle();

## Page Source

    driver.getPageSource();

## Close Current Window

    driver.close();

## Quit Browser

    driver.quit();

## Maximize

    driver.manage()
          .window()
          .maximize();

## Minimize

    driver.manage()
          .window()
          .minimize();

## Fullscreen

    driver.manage()
          .window()
          .fullscreen();

---

# 50. Browser Window Cheat Sheet

## Set Size

    driver.manage()
          .window()
          .setSize(
              new Dimension(1280, 720)
          );

## Get Size

    driver.manage()
          .window()
          .getSize();

## Set Position

    driver.manage()
          .window()
          .setPosition(
              new Point(100, 100)
          );

## Get Position

    driver.manage()
          .window()
          .getPosition();

---

# 51. Timeout Cheat Sheet

## Implicit Wait

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

## Page Load Timeout

    driver.manage()
          .timeouts()
          .pageLoadTimeout(
              Duration.ofSeconds(30)
          );

## Script Timeout

    driver.manage()
          .timeouts()
          .scriptTimeout(
              Duration.ofSeconds(30)
          );

---

# 52. Cookie Cheat Sheet

## Add

    driver.manage()
          .addCookie(cookie);

## Get One

    driver.manage()
          .getCookieNamed(
              "username"
          );

## Get All

    driver.manage()
          .getCookies();

## Delete One by Name

    driver.manage()
          .deleteCookieNamed(
              "username"
          );

## Delete One

    driver.manage()
          .deleteCookie(cookie);

## Delete All

    driver.manage()
          .deleteAllCookies();

---

# 53. get() vs navigate().to()

Both can navigate to a URL.

Example:

    driver.get(url);

    driver.navigate().to(url);

`navigate()` also provides:

    back()
    forward()
    refresh()

A common interview answer:

"`get()` and `navigate().to()` can both open a URL. The `navigate()` interface additionally provides browser navigation commands such as back, forward, and refresh."

---

# 54. close() vs quit()

Interview answer:

"`close()` closes the currently selected browser window or tab. `quit()` closes all windows associated with the WebDriver session and terminates the session. I normally use `quit()` during test cleanup."

---

# 55. getText() vs getPageSource()

`getText()` belongs to WebElement and returns visible text from an element.

Example:

    element.getText();

`getPageSource()` belongs to WebDriver and returns the current page source.

Example:

    driver.getPageSource();

---

# 56. Browser Commands in Page Objects

Browser-level operations usually belong in framework/base classes rather than individual page classes.

Example:

    public class BaseTest {

        protected WebDriver driver;

        public void openApplication(
                String url) {

            driver.get(url);
        }

        public void maximizeBrowser() {

            driver.manage()
                  .window()
                  .maximize();
        }

        public void closeBrowser() {

            if (driver != null) {

                driver.quit();
            }
        }
    }

---

# 57. Browser Setup in TestNG

Example:

    @BeforeMethod
    public void setUp() {

        driver =
            new ChromeDriver();

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

---

# 58. Browser Commands and Parallel Testing

In parallel execution, every test should have its own WebDriver session.

Example concept:

    Test 1
       ↓
    Thread 1
       ↓
    Chrome Session 1

    Test 2
       ↓
    Thread 2
       ↓
    Chrome Session 2

This is commonly implemented using:

    ThreadLocal<WebDriver>

---

# 59. Browser Commands and Selenium Grid

When running on Selenium Grid:

    TestNG
       ↓
    WebDriver
       ↓
    RemoteWebDriver
       ↓
    Selenium Grid
       ↓
    Remote Browser

Example:

    WebDriver driver =
        new RemoteWebDriver(
            gridUrl,
            options
        );

---

# 60. Common Browser Command Mistakes

## Mistake 1

Using:

    driver.close();

when you actually need to terminate the entire session.

Use:

    driver.quit();

---

## Mistake 2

Not closing the browser after the test.

Use:

    @AfterMethod

or:

    @AfterTest

depending on the framework design.

---

## Mistake 3

Hardcoding browser configuration everywhere.

Centralize browser setup in a factory or BaseTest.

---

## Mistake 4

Using browser-specific options incorrectly.

Keep browser configuration centralized.

---

## Mistake 5

Using fixed sleeps instead of synchronization.

Use explicit waits where appropriate.

---

# 61. Best Practices

1. Create the WebDriver in a controlled setup method.
2. Keep browser configuration centralized.
3. Use `driver.quit()` for cleanup.
4. Use `try/finally` for standalone scripts.
5. Use TestNG lifecycle methods in a TestNG framework.
6. Keep URLs in configuration files.
7. Avoid hardcoding environment-specific URLs.
8. Configure browser options centrally.
9. Use explicit waits for dynamic elements.
10. Use ThreadLocal for parallel WebDriver sessions.
11. Use RemoteWebDriver for Grid execution.
12. Keep browser setup separate from page objects.
13. Avoid unnecessary browser commands in test methods.
14. Capture logs and screenshots when failures occur.

---

# 62. Complete Browser Management Example

    import java.time.Duration;

    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.chrome.ChromeDriver;

    public class BrowserManagementExample {

        public static void main(
                String[] args) {

            WebDriver driver =
                new ChromeDriver();

            try {

                driver.manage()
                      .timeouts()
                      .pageLoadTimeout(
                          Duration.ofSeconds(30)
                      );

                driver.manage()
                      .window()
                      .maximize();

                driver.get(
                    "https://example.com"
                );

                System.out.println(
                    "Title: "
                    + driver.getTitle()
                );

                System.out.println(
                    "URL: "
                    + driver.getCurrentUrl()
                );

                driver.navigate().refresh();

                driver.navigate().back();

                driver.navigate().forward();

            } finally {

                driver.quit();
            }
        }
    }

---

# 63. Interview Questions

1. What is WebDriver?
2. What is the difference between `get()` and `navigate().to()`?
3. What is `getCurrentUrl()`?
4. What is `getTitle()`?
5. What is `getPageSource()`?
6. What is the difference between `close()` and `quit()`?
7. How do you maximize the browser?
8. How do you minimize the browser?
9. How do you run Chrome in headless mode?
10. How do you set browser window size?
11. How do you get browser window size?
12. How do you set browser window position?
13. What are Selenium timeouts?
14. What is page load timeout?
15. What is script timeout?
16. How do you manage cookies?
17. How do you add a cookie?
18. How do you delete cookies?
19. What is ChromeOptions?
20. What is RemoteWebDriver?
21. What is Selenium Grid?
22. How do you execute Selenium tests remotely?
23. How do you run Selenium tests in Jenkins?
24. How do you run Selenium tests in headless mode?
25. How do you handle browser setup in a framework?
26. How do you manage browsers in parallel execution?
27. Why should browser setup be centralized?
28. Where should `driver.quit()` be called?
29. How do you configure different browsers?
30. How do you design browser management for a Selenium framework?

---

# 64. Final Summary

The most important Selenium browser commands are:

    get()
    navigate().to()
    navigate().back()
    navigate().forward()
    navigate().refresh()
    getCurrentUrl()
    getTitle()
    getPageSource()
    close()
    quit()

Window management:

    maximize()
    minimize()
    fullscreen()
    setSize()
    getSize()
    setPosition()
    getPosition()

Timeout management:

    implicitlyWait()
    pageLoadTimeout()
    scriptTimeout()

Cookie management:

    addCookie()
    getCookieNamed()
    getCookies()
    deleteCookieNamed()
    deleteCookie()
    deleteAllCookies()

Browser configuration:

    ChromeOptions
    FirefoxOptions
    EdgeOptions
    RemoteWebDriver

Professional Selenium flow:

    Browser Configuration
            ↓
    WebDriver Creation
            ↓
    Open Application
            ↓
    Window Management
            ↓
    Element Interaction
            ↓
    Navigation
            ↓
    Validation
            ↓
    Cleanup
            ↓
    driver.quit()

A strong Selenium framework should keep browser creation, configuration, and cleanup centralized rather than duplicating browser-management code in every test.
