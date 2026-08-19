# Selenium JavaScriptExecutor

## 1. Introduction

`JavaScriptExecutor` is an interface in Selenium WebDriver that allows us to execute JavaScript code directly in the browser.

It is useful when normal Selenium WebDriver operations are not sufficient or when we need to interact with the browser using JavaScript.

Common uses include:

* Clicking an element using JavaScript
* Entering text using JavaScript
* Scrolling the page
* Scrolling to an element
* Highlighting an element
* Getting page information
* Refreshing the page
* Working with hidden or difficult elements
* Executing JavaScript in the browser context

---

# 2. JavaScriptExecutor Interface

Selenium provides:

```java
JavascriptExecutor
```

Import:

```java
import org.openqa.selenium.JavascriptExecutor;
```

JavaScriptExecutor can be obtained from the WebDriver:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;
```

Now JavaScript can be executed using:

```java
js.executeScript();
```

---

# 3. Basic Example

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript("alert('Hello Selenium');");
```

This executes JavaScript inside the browser.

---

# 4. executeScript()

The primary method is:

```java
executeScript()
```

Example:

```java
js.executeScript("document.title");
```

Example with return value:

```java
String title =
        (String) js.executeScript("return document.title;");

System.out.println(title);
```

---

# 5. Complete Basic Example

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.JavascriptExecutor;

public class JavaScriptExecutorExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://example.com");

        JavascriptExecutor js =
                (JavascriptExecutor) driver;

        String title =
                (String) js.executeScript(
                        "return document.title;"
                );

        System.out.println("Title: " + title);

        driver.quit();
    }
}
```

---

# 6. Execute JavaScript Without Return Value

```java
js.executeScript(
    "document.body.style.backgroundColor='yellow';"
);
```

This changes the page background.

---

# 7. Execute JavaScript With Return Value

JavaScript:

```java
js.executeScript(
    "return document.title;"
);
```

Java:

```java
String title =
        (String) js.executeScript(
            "return document.title;"
        );
```

The `return` keyword is required when JavaScript needs to return a value.

---

# 8. JavaScriptExecutor With WebElement

A WebElement can be passed as an argument to JavaScript.

```java
WebElement element =
        driver.findElement(By.id("username"));

js.executeScript(
    "arguments[0].click();",
    element
);
```

Here:

```text
arguments[0]
```

represents the first Java argument passed to JavaScript.

---

# 9. arguments[0]

Example:

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

The mapping is:

```text
arguments[0] → element
```

If multiple arguments are passed:

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    element,
    "Selva"
);
```

Then:

```text
arguments[0] → element
arguments[1] → "Selva"
```

---

# 10. JavaScript Click

Normal Selenium click:

```java
driver.findElement(
    By.id("login")
).click();
```

JavaScript click:

```java
WebElement login =
        driver.findElement(By.id("login"));

js.executeScript(
    "arguments[0].click();",
    login
);
```

---

# 11. When to Use JavaScript Click

JavaScript click can be useful when:

* Element is difficult to click normally
* An overlay interferes with the click
* Element is outside the visible viewport
* Normal Selenium click fails
* Application has unusual JavaScript behavior

However, JavaScript click should **not** automatically replace normal Selenium click.

Prefer:

```java
element.click();
```

first.

Use JavaScript when there is a valid reason.

---

# 12. Enter Text Using JavaScript

Normal Selenium:

```java
driver.findElement(
    By.id("username")
).sendKeys("Selva");
```

JavaScript:

```java
WebElement username =
        driver.findElement(By.id("username"));

js.executeScript(
    "arguments[0].value='Selva';",
    username
);
```

---

# 13. JavaScript Send Keys Alternative

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    username,
    "Selva"
);
```

This is useful when `sendKeys()` does not work as expected.

---

# 14. Important Difference: sendKeys vs JavaScript Value

Normal Selenium:

```java
element.sendKeys("Selva");
```

simulates keyboard interaction.

JavaScript:

```java
js.executeScript(
    "arguments[0].value='Selva';",
    element
);
```

directly changes the DOM value.

Some modern applications rely on keyboard/input events.

Therefore, JavaScript value assignment may not trigger the same application events as `sendKeys()`.

Prefer `sendKeys()` whenever it works correctly.

---

# 15. Scroll Down

Scroll down by 500 pixels:

```java
js.executeScript(
    "window.scrollBy(0,500);"
);
```

Format:

```text
window.scrollBy(horizontal, vertical)
```

Example:

```java
js.executeScript(
    "window.scrollBy(0,1000);"
);
```

---

# 16. Scroll Up

```java
js.executeScript(
    "window.scrollBy(0,-500);"
);
```

Negative Y value scrolls upward.

---

# 17. Scroll to Bottom

```java
js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);
```

This moves to the bottom of the page.

---

# 18. Scroll to Top

```java
js.executeScript(
    "window.scrollTo(0, 0);"
);
```

---

# 19. Scroll to an Element

This is one of the most common JavaScriptExecutor operations.

```java
WebElement element =
        driver.findElement(By.id("footer"));

js.executeScript(
    "arguments[0].scrollIntoView(true);",
    element
);
```

The browser scrolls until the element becomes visible.

---

# 20. Smooth Scroll

```java
js.executeScript(
    "arguments[0].scrollIntoView({behavior: 'smooth', block: 'center'});",
    element
);
```

This provides a smoother scrolling experience.

---

# 21. Scroll Element to Center

```java
js.executeScript(
    "arguments[0].scrollIntoView({block: 'center'});",
    element
);
```

This can be useful when fixed headers cover elements after scrolling.

---

# 22. Get Page Title

Using WebDriver:

```java
String title = driver.getTitle();
```

Using JavaScript:

```java
String title =
        (String) js.executeScript(
            "return document.title;"
        );
```

---

# 23. Get Current URL

Using WebDriver:

```java
String url = driver.getCurrentUrl();
```

Using JavaScript:

```java
String url =
        (String) js.executeScript(
            "return window.location.href;"
        );
```

---

# 24. Get Domain

```java
String domain =
        (String) js.executeScript(
            "return document.domain;"
        );

System.out.println(domain);
```

---

# 25. Get Page Ready State

JavaScript can be used to check page loading state.

```java
String state =
        (String) js.executeScript(
            "return document.readyState;"
        );

System.out.println(state);
```

Possible values:

```text
loading
interactive
complete
```

Usually:

```text
complete
```

means the document has finished loading.

---

# 26. Refresh Page Using JavaScript

```java
js.executeScript(
    "location.reload();"
);
```

Normal Selenium alternative:

```java
driver.navigate().refresh();
```

Prefer the normal Selenium method unless JavaScript is specifically required.

---

# 27. Navigate Using JavaScript

```java
js.executeScript(
    "window.location='https://example.com';"
);
```

Normally prefer:

```java
driver.get("https://example.com");
```

---

# 28. Highlight an Element

JavaScript can modify the style of an element.

```java
WebElement element =
        driver.findElement(By.id("username"));

js.executeScript(
    "arguments[0].style.border='3px solid red';",
    element
);
```

This is useful for debugging.

---

# 29. Highlight Multiple Elements

```java
List<WebElement> elements =
        driver.findElements(By.tagName("input"));

for (WebElement element : elements) {

    js.executeScript(
        "arguments[0].style.border='2px solid red';",
        element
    );
}
```

---

# 30. Change Background Color

```java
js.executeScript(
    "arguments[0].style.backgroundColor='yellow';",
    element
);
```

---

# 31. Get Element Attribute Using JavaScript

```java
String value =
        (String) js.executeScript(
            "return arguments[0].getAttribute('value');",
            element
        );
```

---

# 32. Get Text Using JavaScript

```java
String text =
        (String) js.executeScript(
            "return arguments[0].innerText;",
            element
        );

System.out.println(text);
```

Alternative:

```java
String text =
        (String) js.executeScript(
            "return arguments[0].textContent;",
            element
        );
```

---

# 33. innerText vs textContent

`innerText` generally represents visible rendered text.

```java
return arguments[0].innerText;
```

`textContent` returns the text content of the element, including text that may not be visibly rendered.

```java
return arguments[0].textContent;
```

For normal Selenium validation, prefer:

```java
element.getText();
```

when it provides the required result.

---

# 34. Get Element Value

```java
String value =
        (String) js.executeScript(
            "return arguments[0].value;",
            element
        );
```

---

# 35. Set Element Attribute

```java
js.executeScript(
    "arguments[0].setAttribute('value','Selva');",
    element
);
```

Be careful when modifying application DOM attributes because it may not represent a real user interaction.

---

# 36. Remove Attribute

```java
js.executeScript(
    "arguments[0].removeAttribute('disabled');",
    element
);
```

This can technically make a disabled element appear enabled.

### Important

Using JavaScript to remove application restrictions is generally useful for debugging, but it is usually **not valid functional automation** because the real user may not be able to perform the action.

---

# 37. Check Element Visibility Using JavaScript

```java
Boolean visible =
        (Boolean) js.executeScript(
            "return arguments[0].offsetParent !== null;",
            element
        );

System.out.println(visible);
```

---

# 38. Check Element Display Property

```java
String display =
        (String) js.executeScript(
            "return window.getComputedStyle(arguments[0]).display;",
            element
        );
```

---

# 39. Check Element Disabled State

```java
Boolean disabled =
        (Boolean) js.executeScript(
            "return arguments[0].disabled;",
            element
        );

System.out.println(disabled);
```

---

# 40. Get Document Height

```java
Long height =
        (Long) js.executeScript(
            "return document.body.scrollHeight;"
        );

System.out.println(height);
```

---

# 41. Get Window Inner Height

```java
Long height =
        (Long) js.executeScript(
            "return window.innerHeight;"
        );

System.out.println(height);
```

---

# 42. Get Scroll Position

Get current vertical scroll position:

```java
Long position =
        (Long) js.executeScript(
            "return window.pageYOffset;"
        );

System.out.println(position);
```

Modern alternative:

```java
Long position =
        (Long) js.executeScript(
            "return window.scrollY;"
        );
```

---

# 43. Scroll to Bottom and Verify

```java
js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);

Long currentPosition =
        (Long) js.executeScript(
            "return window.scrollY;"
        );

System.out.println(currentPosition);
```

---

# 44. JavaScriptExecutor With Arguments

Multiple arguments can be passed.

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    username,
    "Selva"
);
```

Another example:

```java
js.executeScript(
    "arguments[0].style.border = arguments[1];",
    element,
    "3px solid red"
);
```

---

# 45. JavaScriptExecutor With Multiple WebElements

```java
WebElement first =
        driver.findElement(By.id("first"));

WebElement second =
        driver.findElement(By.id("second"));

js.executeScript(
    "arguments[0].value = 'Java';" +
    "arguments[1].value = 'Selenium';",
    first,
    second
);
```

Mapping:

```text
arguments[0] → first
arguments[1] → second
```

---

# 46. Execute JavaScript From a Test

```java
@Test
public void javascriptTest() {

    JavascriptExecutor js =
            (JavascriptExecutor) driver;

    String title =
            (String) js.executeScript(
                "return document.title;"
            );

    System.out.println(title);
}
```

---

# 47. JavaScriptExecutor in Page Object Model

Page class:

```java
public class LoginPage {

    private WebDriver driver;

    private By username =
            By.id("username");

    private By loginButton =
            By.id("login");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public void enterUsername(String value) {

        WebElement element =
                driver.findElement(username);

        JavascriptExecutor js =
                (JavascriptExecutor) driver;

        js.executeScript(
            "arguments[0].value = arguments[1];",
            element,
            value
        );
    }

    public void clickLogin() {

        WebElement element =
                driver.findElement(loginButton);

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

# 48. JavaScript Utility Class

A reusable utility class can simplify framework code.

```java
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;

public class JavaScriptUtils {

    private WebDriver driver;
    private JavascriptExecutor js;

    public JavaScriptUtils(WebDriver driver) {
        this.driver = driver;
        this.js = (JavascriptExecutor) driver;
    }

    public void click(WebElement element) {

        js.executeScript(
            "arguments[0].click();",
            element
        );
    }

    public void enterText(
            WebElement element,
            String text) {

        js.executeScript(
            "arguments[0].value = arguments[1];",
            element,
            text
        );
    }

    public void scrollToElement(
            WebElement element) {

        js.executeScript(
            "arguments[0].scrollIntoView(true);",
            element
        );
    }

    public void scrollToBottom() {

        js.executeScript(
            "window.scrollTo(0, document.body.scrollHeight);"
        );
    }

    public void scrollToTop() {

        js.executeScript(
            "window.scrollTo(0, 0);"
        );
    }

    public void highlight(
            WebElement element) {

        js.executeScript(
            "arguments[0].style.border='3px solid red';",
            element
        );
    }

    public String getTitle() {

        return (String) js.executeScript(
            "return document.title;"
        );
    }

    public String getCurrentUrl() {

        return (String) js.executeScript(
            "return window.location.href;"
        );
    }
}
```

---

# 49. Using JavaScript Utility

```java
JavaScriptUtils jsUtils =
        new JavaScriptUtils(driver);

jsUtils.enterText(
    username,
    "Selva"
);

jsUtils.scrollToElement(
    footer
);

jsUtils.highlight(
    footer
);
```

---

# 50. JavaScriptExecutor and Explicit Wait

JavaScriptExecutor does not replace explicit waits.

Example:

```java
WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

WebElement element =
        wait.until(
            ExpectedConditions.elementToBeClickable(
                By.id("login")
            )
        );

JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript(
    "arguments[0].click();",
    element
);
```

First wait for the element.

Then use JavaScript if necessary.

---

# 51. JavaScriptExecutor vs Selenium WebDriver

| Operation    | Selenium               | JavaScript                 |
| ------------ | ---------------------- | -------------------------- |
| Click        | `click()`              | `arguments[0].click()`     |
| Enter text   | `sendKeys()`           | `arguments[0].value=`      |
| Scroll       | Actions / JS           | `window.scrollTo()`        |
| Get title    | `getTitle()`           | `document.title`           |
| Get URL      | `getCurrentUrl()`      | `window.location.href`     |
| Refresh      | `navigate().refresh()` | `location.reload()`        |
| Get text     | `getText()`            | `innerText`                |
| Find element | `findElement()`        | `document.querySelector()` |

---

# 52. Selenium Click vs JavaScript Click

Normal Selenium:

```java
element.click();
```

JavaScript:

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

### Prefer Selenium click

Use:

```java
element.click();
```

when possible.

### Use JavaScript click when necessary

For example:

* Element is obscured
* Normal click fails
* Scrolling behavior causes problems
* Application has unusual interaction behavior

---

# 53. Selenium sendKeys vs JavaScript

Normal:

```java
element.sendKeys("Selva");
```

JavaScript:

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    element,
    "Selva"
);
```

### Recommendation

Prefer:

```java
sendKeys()
```

because it behaves more like a real keyboard interaction.

Use JavaScript only when there is a specific reason.

---

# 54. JavaScriptExecutor and Shadow DOM

JavaScript can sometimes be useful when working with Shadow DOM.

Example:

```java
WebElement shadowHost =
        driver.findElement(By.cssSelector("#host"));

SearchContext shadowRoot =
        shadowHost.getShadowRoot();
```

Modern Selenium provides direct Shadow DOM support, so JavaScript should not automatically be the first choice.

---

# 55. JavaScriptExecutor and Hidden Elements

JavaScript can technically interact with hidden elements:

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

But this can bypass real user behavior.

Therefore, don't use JavaScript simply to bypass application restrictions.

Instead, determine why the element is hidden and use the correct user-facing flow.

---

# 56. Common Problems

## Problem 1: JavaScript click does not trigger application logic

Some modern applications listen for specific browser events.

Instead of:

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

try:

```java
element.click();
```

first.

---

## Problem 2: Setting value does not update the application

This may happen with React, Angular, Vue, or other JavaScript frameworks.

Instead of:

```java
js.executeScript(
    "arguments[0].value='Selva';",
    element
);
```

prefer:

```java
element.sendKeys("Selva");
```

If JavaScript is required, the appropriate input/change events may also need to be triggered.

---

# 57. Common Exceptions

## JavascriptException

Occurs when the JavaScript execution fails.

Example:

```java
js.executeScript(
    "invalid javascript code"
);
```

---

## StaleElementReferenceException

Can occur if the WebElement passed to JavaScript is no longer attached to the DOM.

Solution:

Locate the element again.

---

## NoSuchElementException

Occurs if the element cannot be found before JavaScript is executed.

Solution:

Use a correct locator and appropriate wait.

---

# 58. Best Practices

### 1. Prefer normal Selenium first

Use:

```java
element.click();
element.sendKeys();
```

before JavaScript.

### 2. Use JavaScript for specific problems

Good use cases:

```text
Scrolling
Debugging
Browser-level information
Special interaction issues
```

### 3. Use explicit waits

Do not use JavaScript as a replacement for synchronization.

### 4. Avoid bypassing application behavior

Do not remove `disabled`, `readonly`, validation, or other restrictions just to make a test pass.

### 5. Create reusable utility methods

Keep JavaScript code in a utility class rather than duplicating it throughout tests.

---

# 59. Interview Questions

## Q1. What is JavaScriptExecutor?

`JavascriptExecutor` is a Selenium interface that allows JavaScript code to be executed in the browser.

```java
JavascriptExecutor js =
    (JavascriptExecutor) driver;
```

---

## Q2. Which method is used to execute JavaScript?

```java
executeScript()
```

Example:

```java
js.executeScript(
    "return document.title;"
);
```

---

## Q3. How do you click an element using JavaScript?

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

---

## Q4. What is arguments[0]?

`arguments[0]` represents the first argument passed from Java to the JavaScript code.

Example:

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

Here:

```text
arguments[0] = element
```

---

## Q5. How do you scroll to an element?

```java
js.executeScript(
    "arguments[0].scrollIntoView(true);",
    element
);
```

---

## Q6. How do you scroll to the bottom?

```java
js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);
```

---

## Q7. How do you scroll to the top?

```java
js.executeScript(
    "window.scrollTo(0, 0);"
);
```

---

## Q8. How do you get the page title using JavaScript?

```java
String title =
    (String) js.executeScript(
        "return document.title;"
    );
```

---

## Q9. How do you get the current URL using JavaScript?

```java
String url =
    (String) js.executeScript(
        "return window.location.href;"
    );
```

---

## Q10. Can JavaScriptExecutor replace Selenium WebDriver?

No.

JavaScriptExecutor is a supporting capability.

Selenium WebDriver should generally be preferred for normal user interactions.

---

## Q11. Why should JavaScript click not always be used?

Because JavaScript click can bypass some of the normal browser interaction behavior.

A real user interacts through the browser UI, while JavaScript can directly invoke DOM methods.

---

## Q12. How do you enter text using JavaScript?

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    element,
    "Selva"
);
```

---

## Q13. How do you highlight an element?

```java
js.executeScript(
    "arguments[0].style.border='3px solid red';",
    element
);
```

---

## Q14. How do you check page loading state?

```java
String state =
    (String) js.executeScript(
        "return document.readyState;"
    );
```

---

## Q15. What are the possible document.readyState values?

Common values are:

```text
loading
interactive
complete
```

---

# 60. Quick Revision

```text
JavascriptExecutor
        |
        +-- executeScript()
        |
        +-- arguments[0]
        |
        +-- JavaScript Click
        |
        +-- JavaScript Enter Text
        |
        +-- Scroll
        |     |
        |     +-- scrollBy()
        |     +-- scrollTo()
        |     +-- scrollIntoView()
        |
        +-- Get Title
        |
        +-- Get URL
        |
        +-- Get Text
        |
        +-- Get Attributes
        |
        +-- Highlight Element
        |
        +-- Page Ready State
        |
        +-- Browser Information
```

---

# 61. Most Important Code Snippets

### Create JavaScriptExecutor

```java
JavascriptExecutor js =
    (JavascriptExecutor) driver;
```

### Click

```java
js.executeScript(
    "arguments[0].click();",
    element
);
```

### Enter text

```java
js.executeScript(
    "arguments[0].value = arguments[1];",
    element,
    "Selva"
);
```

### Scroll to element

```java
js.executeScript(
    "arguments[0].scrollIntoView(true);",
    element
);
```

### Scroll to bottom

```java
js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);
```

### Scroll to top

```java
js.executeScript(
    "window.scrollTo(0, 0);"
);
```

### Get title

```java
String title =
    (String) js.executeScript(
        "return document.title;"
    );
```

### Get URL

```java
String url =
    (String) js.executeScript(
        "return window.location.href;"
    );
```

### Highlight

```java
js.executeScript(
    "arguments[0].style.border='3px solid red';",
    element
);
```

### Page ready state

```java
String state =
    (String) js.executeScript(
        "return document.readyState;"
    );
```

---

# 62. Key Takeaways

* `JavascriptExecutor` allows Selenium to execute JavaScript in the browser.
* Use `executeScript()` to execute JavaScript.
* `arguments[0]` represents the first Java argument passed to JavaScript.
* JavaScript can click elements, set values, scroll pages, retrieve information, and modify DOM properties.
* Prefer normal Selenium methods whenever possible.
* Use JavaScript when normal WebDriver interaction is insufficient.
* JavaScriptExecutor should not be used to bypass real application restrictions.
* Explicit waits are still important when using JavaScript.
* A reusable JavaScript utility class is useful in a Selenium framework.
* `sendKeys()` is generally preferable to directly setting `.value`.
* `element.click()` is generally preferable to JavaScript click.
* JavaScript is especially useful for scrolling, debugging, and special browser interactions.
