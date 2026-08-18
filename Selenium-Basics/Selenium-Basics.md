# Selenium Basics

## 1. What is Selenium?

Selenium is an open-source automation framework used to automate web applications.

Selenium supports multiple programming languages, including:

- Java
- Python
- JavaScript
- C#
- Ruby

Selenium is primarily used for:

- Web UI automation
- Functional testing
- Regression testing
- Smoke testing
- End-to-end testing
- Cross-browser testing
- Data-driven testing
- Parallel testing

Selenium is not used to automate desktop applications directly.

---

# 2. Selenium Components

The Selenium ecosystem includes several important components:

1. Selenium WebDriver
2. Selenium IDE
3. Selenium Grid

For professional automation frameworks, **Selenium WebDriver** is the most commonly used component.

---

# 3. Selenium WebDriver

Selenium WebDriver provides APIs to communicate with web browsers.

It allows automation code to:

- Open browsers
- Navigate to URLs
- Find web elements
- Enter text
- Click buttons
- Select dropdown values
- Handle alerts
- Handle frames
- Switch windows
- Capture screenshots
- Execute JavaScript
- Read page information
- Close browsers

Example:

    WebDriver driver = new ChromeDriver();

    driver.get("https://www.google.com");

    driver.quit();

---

# 4. How Selenium WebDriver Works

The basic flow is:

    Java Test Code
          |
          ↓
    Selenium WebDriver API
          |
          ↓
    Browser Driver
          |
          ↓
    Browser
          |
          ↓
    Web Application

For example:

    TestNG Test
        ↓
    Selenium WebDriver
        ↓
    ChromeDriver
        ↓
    Chrome Browser
        ↓
    Web Application

---

# 5. Selenium WebDriver Architecture

Your Java code uses the Selenium WebDriver API.

Example:

    WebDriver driver = new ChromeDriver();

The `WebDriver` interface defines browser operations.

`ChromeDriver` provides the implementation for Chrome.

Similarly:

    WebDriver driver = new FirefoxDriver();

    WebDriver driver = new EdgeDriver();

This demonstrates abstraction and runtime polymorphism.

---

# 6. Selenium WebDriver Interface

`WebDriver` is an interface.

Example:

    WebDriver driver;

The actual implementation can be:

    driver = new ChromeDriver();

or:

    driver = new FirefoxDriver();

or:

    driver = new EdgeDriver();

This is one reason Selenium makes extensive use of Java OOP concepts.

---

# 7. Browser Drivers

Browser-specific driver implementations allow Selenium to communicate with browsers.

Common examples:

| Browser | Driver |
|---|---|
| Chrome | ChromeDriver |
| Firefox | FirefoxDriver |
| Edge | EdgeDriver |
| Safari | SafariDriver |

Modern Selenium versions can often manage the required browser driver automatically through Selenium Manager.

---

# 8. First Selenium Program

Example:

    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.chrome.ChromeDriver;

    public class FirstSeleniumTest {

        public static void main(String[] args) {

            WebDriver driver = new ChromeDriver();

            driver.get("https://www.google.com");

            System.out.println(driver.getTitle());

            driver.quit();
        }
    }

---

# 9. Creating WebDriver Object

Example:

    WebDriver driver = new ChromeDriver();

Explanation:

    WebDriver
    ↓
    Interface

    driver
    ↓
    Reference variable

    new ChromeDriver()
    ↓
    Object

This is runtime polymorphism.

---

# 10. `get()` Method

The `get()` method opens a URL.

Example:

    driver.get("https://www.google.com");

It navigates the browser to the specified URL.

---

# 11. `getTitle()`

Returns the title of the current web page.

Example:

    String title = driver.getTitle();

    System.out.println(title);

---

# 12. `getCurrentUrl()`

Returns the current URL.

Example:

    String url = driver.getCurrentUrl();

    System.out.println(url);

---

# 13. `getPageSource()`

Returns the HTML source of the current page.

Example:

    String source = driver.getPageSource();

    System.out.println(source);

This can be useful for debugging, but it should not normally be used as the primary way to locate elements.

---

# 14. `close()` vs `quit()`

## close()

Closes the current browser window.

Example:

    driver.close();

## quit()

Closes all browser windows opened by the WebDriver session and ends the WebDriver session.

Example:

    driver.quit();

### Difference

| `close()` | `quit()` |
|---|---|
| Closes current window | Ends the entire WebDriver session |
| Useful for closing one window | Usually preferred for test cleanup |
| Session may remain if other windows exist | All windows are closed |

In automation frameworks, `driver.quit()` is generally preferred during teardown.

---

# 15. Browser Navigation Commands

Selenium provides navigation methods through `driver.navigate()`.

## navigate().to()

    driver.navigate().to("https://www.google.com");

## navigate().back()

    driver.navigate().back();

## navigate().forward()

    driver.navigate().forward();

## navigate().refresh()

    driver.navigate().refresh();

Example:

    driver.get("https://www.google.com");

    driver.navigate().to("https://www.amazon.com");

    driver.navigate().back();

    driver.navigate().forward();

    driver.navigate().refresh();

---

# 16. Browser Window Management

Get the current window handle:

    String windowHandle =
        driver.getWindowHandle();

Get all window handles:

    Set<String> windowHandles =
        driver.getWindowHandles();

Example:

    System.out.println(driver.getWindowHandle());

    System.out.println(driver.getWindowHandles());

---

# 17. Window Size

Get window size:

    Dimension size =
        driver.manage().window().getSize();

Set window size:

    driver.manage().window()
          .setSize(new Dimension(1200, 800));

---

# 18. Maximize Browser

Example:

    driver.manage().window().maximize();

This maximizes the browser window.

---

# 19. Minimize Browser

Example:

    driver.manage().window().minimize();

---

# 20. Full Screen

Example:

    driver.manage().window().fullscreen();

---

# 21. Finding Web Elements

Selenium provides several locator strategies.

Common locators are:

1. ID
2. Name
3. Class Name
4. Tag Name
5. Link Text
6. Partial Link Text
7. CSS Selector
8. XPath

Modern Selenium uses the `By` class.

Example:

    driver.findElement(By.id("username"));

---

# 22. `findElement()`

`findElement()` returns the first matching element.

Example:

    WebElement username =
        driver.findElement(By.id("username"));

If no matching element is found, Selenium throws `NoSuchElementException`.

---

# 23. `findElements()`

`findElements()` returns a list of matching elements.

Example:

    List<WebElement> links =
        driver.findElements(By.tagName("a"));

If no matching elements are found, `findElements()` returns an empty list rather than throwing `NoSuchElementException`.

---

# 24. `findElement()` vs `findElements()`

| findElement() | findElements() |
|---|---|
| Returns one WebElement | Returns List<WebElement> |
| Returns first matching element | Returns all matching elements |
| Throws NoSuchElementException if not found | Returns empty list if not found |

---

# 25. WebElement

`WebElement` represents an HTML element on a web page.

Example:

    WebElement username =
        driver.findElement(By.id("username"));

You can perform operations such as:

- click
- sendKeys
- clear
- getText
- getAttribute
- isDisplayed
- isEnabled
- isSelected

---

# 26. `click()`

Used to click an element.

Example:

    WebElement loginButton =
        driver.findElement(By.id("login"));

    loginButton.click();

---

# 27. `sendKeys()`

Used to enter text.

Example:

    WebElement username =
        driver.findElement(By.id("username"));

    username.sendKeys("Selva");

---

# 28. `clear()`

Clears text from an input field.

Example:

    WebElement username =
        driver.findElement(By.id("username"));

    username.clear();

---

# 29. `getText()`

Returns visible text from an element.

Example:

    String text =
        driver.findElement(By.id("message"))
              .getText();

    System.out.println(text);

---

# 30. `getAttribute()`

Returns the value of an HTML attribute.

Example:

    String value =
        driver.findElement(By.id("username"))
              .getAttribute("value");

Another example:

    String className =
        element.getAttribute("class");

---

# 31. `isDisplayed()`

Checks whether an element is displayed.

Example:

    boolean displayed =
        driver.findElement(By.id("login"))
              .isDisplayed();

---

# 32. `isEnabled()`

Checks whether an element is enabled.

Example:

    boolean enabled =
        driver.findElement(By.id("login"))
              .isEnabled();

---

# 33. `isSelected()`

Checks whether an element is selected.

Useful for:

- Checkbox
- Radio button
- Option elements

Example:

    boolean selected =
        driver.findElement(By.id("remember"))
              .isSelected();

---

# 34. Basic Selenium Test Example

    import org.openqa.selenium.By;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.chrome.ChromeDriver;

    public class LoginTest {

        public static void main(String[] args) {

            WebDriver driver = new ChromeDriver();

            driver.manage().window().maximize();

            driver.get("https://example.com");

            driver.findElement(By.id("username"))
                  .sendKeys("Selva");

            driver.findElement(By.id("password"))
                  .sendKeys("password");

            driver.findElement(By.id("login"))
                  .click();

            System.out.println(driver.getTitle());

            driver.quit();
        }
    }

---

# 35. Selenium Locators

Locators identify elements on a web page.

Common locator types:

    By.id()
    By.name()
    By.className()
    By.tagName()
    By.linkText()
    By.partialLinkText()
    By.cssSelector()
    By.xpath()

Example:

    driver.findElement(By.id("username"));

---

# 36. ID Locator

HTML:

    <input id="username">

Selenium:

    driver.findElement(By.id("username"));

ID is generally preferred when it is unique and stable.

---

# 37. Name Locator

HTML:

    <input name="username">

Selenium:

    driver.findElement(By.name("username"));

---

# 38. Class Name Locator

HTML:

    <button class="login-button">
        Login
    </button>

Selenium:

    driver.findElement(By.className("login-button"));

Important:

If an element has multiple classes, `By.className()` should not be given a string containing spaces.

For example:

    class="btn primary login"

Do not use:

    By.className("btn primary")

Use CSS Selector instead:

    By.cssSelector(".btn.primary")

---

# 39. Tag Name Locator

HTML:

    <input>

Selenium:

    driver.findElement(By.tagName("input"));

Useful when working with groups of elements.

Example:

    List<WebElement> inputs =
        driver.findElements(By.tagName("input"));

---

# 40. Link Text

HTML:

    <a href="/login">Login</a>

Selenium:

    driver.findElement(
        By.linkText("Login")
    ).click();

---

# 41. Partial Link Text

HTML:

    <a href="/login">
        Login to Application
    </a>

Selenium:

    driver.findElement(
        By.partialLinkText("Login")
    ).click();

---

# 42. CSS Selector

CSS Selector is a powerful locator strategy.

Example:

    driver.findElement(
        By.cssSelector("#username")
    );

Class:

    driver.findElement(
        By.cssSelector(".login-button")
    );

Attribute:

    driver.findElement(
        By.cssSelector("input[name='username']")
    );

---

# 43. XPath

XPath is used to locate elements using the HTML document structure.

Example:

    driver.findElement(
        By.xpath("//input[@id='username']")
    );

Text:

    driver.findElement(
        By.xpath("//button[text()='Login']")
    );

---

# 44. Absolute XPath vs Relative XPath

Absolute XPath:

    /html/body/div[1]/div[2]/form/input[1]

Relative XPath:

    //input[@id='username']

Relative XPath is generally preferred because it is less dependent on the complete page hierarchy.

---

# 45. Explicit Wait

Explicit wait waits for a specific condition.

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement element =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("username")
                )
        );

---

# 46. Common ExpectedConditions

Common conditions include:

    visibilityOfElementLocated()

    elementToBeClickable()

    presenceOfElementLocated()

    alertIsPresent()

    titleContains()

    titleIs()

    urlContains()

    frameToBeAvailableAndSwitchToIt()

---

# 47. Implicit Wait

Implicit wait tells WebDriver to wait for a specified amount of time when trying to find elements.

Example:

    driver.manage().timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

Implicit wait applies to element lookup operations for that driver session.

---

# 48. Explicit Wait vs Implicit Wait

| Implicit Wait | Explicit Wait |
|---|---|
| Applies to element lookup | Applied to specific conditions |
| Global for the WebDriver session | Targeted |
| Simple | More flexible |
| Less precise | More precise |

In modern automation frameworks, explicit waits are generally preferred for synchronization.

---

# 49. Thread.sleep()

Example:

    Thread.sleep(5000);

This pauses the current thread for 5 seconds.

It is generally not recommended as the primary synchronization mechanism because it waits a fixed amount of time regardless of whether the application is ready.

Better approach:

    WebDriverWait

---

# 50. Why Synchronization Is Important

Modern web applications may load elements asynchronously.

Examples:

- AJAX
- API calls
- Dynamic content
- Animations
- React applications
- Angular applications
- Loading spinners

Selenium may try to interact with an element before it is ready.

Proper waits help synchronize the test with the application.

---

# 51. Handling Alerts

JavaScript alerts can be handled using:

    driver.switchTo().alert();

Example:

    Alert alert =
        driver.switchTo().alert();

    alert.accept();

Other methods:

    alert.dismiss();

    alert.getText();

    alert.sendKeys("text");

---

# 52. Handling Frames

Switch into a frame:

    driver.switchTo().frame("frameName");

Switch back to the main page:

    driver.switchTo().defaultContent();

Switch to parent frame:

    driver.switchTo().parentFrame();

---

# 53. Handling Multiple Windows

Get all window handles:

    Set<String> handles =
        driver.getWindowHandles();

Switch to another window:

    for (String handle : handles) {

        driver.switchTo().window(handle);
    }

Always identify the target window when possible instead of relying on arbitrary Set iteration order.

---

# 54. Dropdown Handling

For a standard HTML `<select>` element, Selenium provides the `Select` class.

Example:

    Select select =
        new Select(
            driver.findElement(
                By.id("country")
            )
        );

Select by visible text:

    select.selectByVisibleText("USA");

Select by value:

    select.selectByValue("US");

Select by index:

    select.selectByIndex(1);

---

# 55. Mouse Actions

Selenium provides the `Actions` class.

Example:

    Actions actions =
        new Actions(driver);

Move to an element:

    actions.moveToElement(element)
           .perform();

Right click:

    actions.contextClick(element)
           .perform();

Double click:

    actions.doubleClick(element)
           .perform();

Click and hold:

    actions.clickAndHold(element)
           .perform();

---

# 56. Keyboard Actions

Example:

    actions.sendKeys(Keys.ENTER)
           .perform();

Another example:

    actions.keyDown(Keys.CONTROL)
           .sendKeys("a")
           .keyUp(Keys.CONTROL)
           .perform();

---

# 57. JavaScriptExecutor

JavaScript can be executed through Selenium.

Example:

    JavascriptExecutor js =
        (JavascriptExecutor) driver;

    js.executeScript(
        "arguments[0].click();",
        element
    );

Scroll:

    js.executeScript(
        "window.scrollBy(0,500);"
    );

Scroll to element:

    js.executeScript(
        "arguments[0].scrollIntoView(true);",
        element
    );

JavaScript should generally be used when normal Selenium interactions are insufficient or when there is a specific reason to execute JavaScript.

---

# 58. Screenshots

Selenium supports screenshots using `TakesScreenshot`.

Example:

    TakesScreenshot screenshot =
        (TakesScreenshot) driver;

    File source =
        screenshot.getScreenshotAs(
            OutputType.FILE
        );

Screenshots are commonly used for:

- Failed tests
- Debugging
- Test evidence
- Reports

---

# 59. Selenium Exceptions

Common Selenium exceptions include:

- NoSuchElementException
- StaleElementReferenceException
- TimeoutException
- ElementNotInteractableException
- ElementClickInterceptedException
- InvalidSelectorException
- NoSuchWindowException
- NoSuchFrameException
- NoAlertPresentException
- SessionNotCreatedException

Example:

    try {

        driver.findElement(
            By.id("username")
        ).click();

    } catch (NoSuchElementException e) {

        System.out.println(
            "Element not found"
        );
    }

---

# 60. Common Reasons for NoSuchElementException

Possible causes:

1. Incorrect locator
2. Element does not exist
3. Element is inside an iframe
4. Page has not loaded
5. Wrong window
6. Dynamic element
7. Incorrect page
8. Application state is different from expected

Possible solution:

- Verify locator
- Use explicit wait
- Switch to the correct frame
- Switch to the correct window
- Verify URL
- Verify page state

---

# 61. StaleElementReferenceException

This occurs when the previously located element is no longer attached to the current DOM.

Example situation:

    WebElement element =
        driver.findElement(By.id("username"));

    // Page refresh or DOM update

    element.click();

The reference may now be stale.

Possible solution:

Locate the element again after the DOM update.

---

# 62. ElementClickInterceptedException

This can occur when another element is blocking the target element.

Possible causes:

- Popup
- Overlay
- Loading spinner
- Sticky header
- Animation

Possible solutions:

- Wait for the blocking element to disappear
- Wait for the target to become clickable
- Scroll to the element
- Close the popup
- Use JavaScript only when appropriate

---

# 63. TimeoutException

Occurs when a wait condition is not satisfied within the specified timeout.

Example:

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

If the condition is not met, Selenium can throw `TimeoutException`.

---

# 64. Selenium with Java

A common Selenium automation stack is:

    Java
      +
    Selenium WebDriver
      +
    TestNG
      +
    Maven
      +
    Git
      +
    Jenkins

Additional tools may include:

    Rest Assured
    Cucumber
    Allure
    Extent Reports
    Selenium Grid

---

# 65. Selenium with TestNG

TestNG is commonly used as the test execution framework.

Example:

    import org.testng.annotations.Test;

    public class LoginTest {

        @Test
        public void loginTest() {

            System.out.println(
                "Login test"
            );
        }
    }

TestNG provides features such as:

- `@Test`
- `@BeforeMethod`
- `@AfterMethod`
- `@BeforeClass`
- `@AfterClass`
- `@BeforeSuite`
- `@AfterSuite`
- DataProvider
- Groups
- Assertions
- Listeners
- Parallel execution

---

# 66. Selenium Framework Structure

A typical Selenium framework may look like:

    SeleniumStudy
    |
    ├── src/test/java
    │   |
    │   ├── tests
    │   ├── pages
    │   ├── utilities
    │   ├── listeners
    │   └── base
    |
    ├── src/test/resources
    │   |
    │   ├── testdata
    │   └── config
    |
    ├── pom.xml
    └── testng.xml

---

# 67. Page Object Model

Page Object Model is a design pattern commonly used in Selenium frameworks.

Each application page is represented by a class.

Example:

    LoginPage
    HomePage
    SearchPage
    CheckoutPage

A page class contains:

- Locators
- Page actions
- Page-specific methods

Example:

    public class LoginPage {

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

        public void login(
                String user,
                String pass) {

            driver.findElement(username)
                  .sendKeys(user);

            driver.findElement(password)
                  .sendKeys(pass);

            driver.findElement(loginButton)
                  .click();
        }
    }

---

# 68. Why Use Page Object Model?

Benefits:

- Reusable code
- Maintainable locators
- Reduced duplication
- Better readability
- Easier maintenance
- Separation of test logic and page logic

Instead of writing:

    driver.findElement(...)
          .sendKeys(...);

in every test, the test can use:

    loginPage.login(
        "Selva",
        "password"
    );

---

# 69. Selenium and OOP

Selenium automation uses several OOP concepts.

## Encapsulation

Page classes hide locators and implementation details.

## Inheritance

Base test classes can be extended by test classes.

## Abstraction

`WebDriver` is an interface.

## Polymorphism

Different browser driver implementations can be assigned to a `WebDriver` reference.

Example:

    WebDriver driver =
        new ChromeDriver();

---

# 70. Selenium Best Practices

1. Use meaningful locators.
2. Prefer stable attributes.
3. Avoid unnecessary XPath.
4. Avoid absolute XPath.
5. Prefer explicit waits for synchronization.
6. Avoid unnecessary `Thread.sleep()`.
7. Use Page Object Model.
8. Keep test logic separate from page logic.
9. Reuse utility methods.
10. Use `driver.quit()` during cleanup.
11. Keep test data separate from test code.
12. Use meaningful test names.
13. Capture screenshots for failures.
14. Use logging.
15. Use version control.
16. Use CI/CD.
17. Run tests in parallel when appropriate.
18. Avoid hard-coded credentials.
19. Keep locators maintainable.
20. Make tests independent whenever possible.

---

# 71. Common Selenium Interview Questions

## Basic Questions

1. What is Selenium?
2. What are the components of Selenium?
3. What is Selenium WebDriver?
4. What is the difference between Selenium IDE and WebDriver?
5. What browsers does Selenium support?
6. What programming languages does Selenium support?
7. What is WebDriver?
8. What is ChromeDriver?
9. How does Selenium communicate with a browser?
10. What is Selenium Manager?

## WebDriver Questions

11. What is the difference between `close()` and `quit()`?
12. Difference between `get()` and `navigate().to()`?
13. How do you maximize a browser?
14. How do you get the current URL?
15. How do you get the page title?
16. How do you refresh a page?
17. How do you navigate back?
18. How do you navigate forward?
19. What is a window handle?
20. How do you handle multiple windows?

## Locator Questions

21. What are Selenium locators?
22. Which locator is preferred?
23. ID vs XPath?
24. CSS Selector vs XPath?
25. Absolute XPath vs Relative XPath?
26. How do you handle dynamic XPath?
27. Difference between `findElement()` and `findElements()`?

## Wait Questions

28. What is implicit wait?
29. What is explicit wait?
30. What is FluentWait?
31. Implicit wait vs explicit wait?
32. Why should `Thread.sleep()` generally be avoided?
33. How do you wait for an element to be clickable?

## WebElement Questions

34. What is WebElement?
35. Difference between `getText()` and `getAttribute()`?
36. Difference between `isDisplayed()` and `isEnabled()`?
37. How do you clear a textbox?
38. How do you enter text?
39. How do you click an element?

## Advanced Questions

40. How do you handle alerts?
41. How do you handle frames?
42. How do you handle multiple windows?
43. How do you handle dropdowns?
44. How do you perform mouse actions?
45. How do you perform keyboard actions?
46. How do you execute JavaScript?
47. How do you take screenshots?
48. What is Page Object Model?
49. What is PageFactory?
50. How do you handle dynamic elements?
51. What is StaleElementReferenceException?
52. What is ElementClickInterceptedException?
53. How do you handle synchronization issues?
54. How do you run Selenium tests in parallel?
55. What is Selenium Grid?
56. How do you integrate Selenium with Jenkins?
57. How do you integrate Selenium with Maven?
58. How do you create a Selenium framework?
59. How do you handle test data?
60. How do you design a maintainable Selenium framework?

---

# 72. Selenium Quick Revision

    Selenium
    ↓
    Web Automation

    WebDriver
    ↓
    Browser Automation API

    WebElement
    ↓
    HTML Element

    By
    ↓
    Locator

    WebDriverWait
    ↓
    Synchronization

    Actions
    ↓
    Mouse + Keyboard

    Alert
    ↓
    JavaScript Alert

    Select
    ↓
    HTML Select Dropdown

    JavascriptExecutor
    ↓
    Execute JavaScript

    TakesScreenshot
    ↓
    Capture Screenshot

    Page Object Model
    ↓
    Maintainable Framework

---

# 73. Selenium Automation Flow

A typical automation test follows:

    Start Test
       ↓
    Launch Browser
       ↓
    Open Application
       ↓
    Locate Element
       ↓
    Wait for Element
       ↓
    Perform Action
       ↓
    Validate Result
       ↓
    Capture Evidence
       ↓
    Close Browser
       ↓
    Test Complete

---

# 74. Selenium Framework Flow

A professional automation framework may follow:

    TestNG
       ↓
    Test Class
       ↓
    Page Object
       ↓
    Utility Class
       ↓
    Selenium WebDriver
       ↓
    Browser
       ↓
    Application

Supporting tools:

    Maven
       ↓
    Dependency Management

    Git
       ↓
    Version Control

    Jenkins
       ↓
    CI/CD

    Selenium Grid
       ↓
    Parallel / Cross-Browser Execution

---

# 75. Final Selenium Basics Summary

Important Selenium concepts to remember:

1. Selenium is used for web automation.
2. WebDriver is the primary Selenium API for browser automation.
3. `WebDriver` is an interface.
4. `ChromeDriver`, `FirefoxDriver`, and `EdgeDriver` provide browser-specific implementations.
5. `WebElement` represents a web element.
6. `By` provides locator strategies.
7. `findElement()` returns one matching element.
8. `findElements()` returns a list.
9. Explicit waits are important for synchronization.
10. `Thread.sleep()` should not be the primary synchronization mechanism.
11. `close()` closes the current window.
12. `quit()` ends the WebDriver session.
13. `Actions` handles advanced mouse and keyboard interactions.
14. `Alert` handles JavaScript alerts.
15. `Select` handles standard HTML select dropdowns.
16. `JavascriptExecutor` executes JavaScript.
17. `TakesScreenshot` captures screenshots.
18. Page Object Model improves maintainability.
19. TestNG provides test execution features.
20. Maven manages project dependencies and builds.
21. Git provides version control.
22. Jenkins provides CI/CD automation.
23. Selenium Grid supports distributed and parallel execution.

---

# 76. Key Selenium + Java Interview Concept

Remember this relationship:

    Java
      ↓
    Selenium WebDriver
      ↓
    WebDriver Interface
      ↓
    ChromeDriver / FirefoxDriver / EdgeDriver
      ↓
    Browser
      ↓
    Web Application

And for framework design:

    Java OOP
       ↓
    Page Object Model
       ↓
    TestNG
       ↓
    Utilities
       ↓
    DataProvider
       ↓
    Listeners
       ↓
    Maven
       ↓
    GitHub
       ↓
    Jenkins
       ↓
    Selenium Grid

This forms the foundation of a professional Java + Selenium automation framework.
