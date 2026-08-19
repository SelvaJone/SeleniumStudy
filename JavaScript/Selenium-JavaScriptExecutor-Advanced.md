# Selenium JavaScriptExecutor – Advanced

## 1. Introduction

`JavaScriptExecutor` is a Selenium interface that allows you to execute JavaScript code directly inside the browser.

It is useful when normal Selenium WebDriver methods are not sufficient for a particular interaction.

```java
JavascriptExecutor js = (JavascriptExecutor) driver;
js.executeScript("alert('Hello Selenium');");
Why Use JavaScriptExecutor?

Normally Selenium interacts with elements using:

driver.findElement(By.id("username")).click();

Sometimes an element may be:

Covered by another element
Outside the visible viewport
Difficult to click normally
Hidden or dynamically rendered
Controlled by custom JavaScript
Difficult to inspect through normal WebDriver APIs

In such cases, JavaScriptExecutor can be useful.

However, JavaScriptExecutor should not be the first choice.

Prefer normal Selenium APIs whenever possible.

3. Creating JavaScriptExecutor
JavascriptExecutor js = (JavascriptExecutor) driver;

You can also write:

JavascriptExecutor js = (JavascriptExecutor) driver;

The driver must implement the JavaScriptExecutor interface.

4. executeScript()

executeScript() executes JavaScript synchronously.

Syntax:

js.executeScript("JavaScript code");

Example:

js.executeScript("console.log('Hello Selenium');");
5. JavaScript Alert
js.executeScript("alert('Welcome to Selenium');");

Handle the alert:

driver.switchTo().alert().accept();

Complete example:

JavascriptExecutor js = (JavascriptExecutor) driver;


js.executeScript("alert('Test Alert');");


driver.switchTo().alert().accept();
6. Get Page Title Using JavaScript
String title = (String) js.executeScript("return document.title;");


System.out.println(title);

Equivalent Selenium method:

String title = driver.getTitle();

Prefer:

driver.getTitle();

for normal Selenium automation.

7. Get Current URL Using JavaScript
String url = (String) js.executeScript("return window.location.href;");


System.out.println(url);

Normal Selenium alternative:

String url = driver.getCurrentUrl();
8. Refresh Page Using JavaScript
js.executeScript("location.reload();");

Normal Selenium:

driver.navigate().refresh();

Prefer the Selenium method unless JavaScript execution is specifically required.

9. Scroll Down

Scroll down by a specific number of pixels:

js.executeScript("window.scrollBy(0,500);");

Scroll down 1000 pixels:

js.executeScript("window.scrollBy(0,1000);");
10. Scroll Up
js.executeScript("window.scrollBy(0,-500);");
11. Scroll to Bottom of Page
js.executeScript("window.scrollTo(0, document.body.scrollHeight);");

This moves the browser to the bottom of the page.

12. Scroll to Top of Page
js.executeScript("window.scrollTo(0,0);");
13. Scroll Element Into View

A very useful technique is:

WebElement element = driver.findElement(By.id("submit"));


js.executeScript(
    "arguments[0].scrollIntoView(true);",
    element
);

arguments[0] represents the first Java object passed to JavaScript.

14. Scroll Element to Center of Screen
WebElement element = driver.findElement(By.id("submit"));


js.executeScript(
    "arguments[0].scrollIntoView({block: 'center'});",
    element
);

This is often better than simply scrolling the element to the top.

15. Click an Element Using JavaScript
WebElement button = driver.findElement(By.id("submit"));


js.executeScript(
    "arguments[0].click();",
    button
);

This bypasses Selenium's normal click mechanism.

16. When JavaScript Click Can Be Useful

JavaScript click may help when:

Element is covered by another element
Element is outside viewport
Normal click throws ElementClickInterceptedException
UI has unusual event handling
Custom controls interfere with Selenium click

Example:

try {
    driver.findElement(By.id("submit")).click();
} catch (ElementClickInterceptedException e) {
    js.executeScript(
        "arguments[0].click();",
        driver.findElement(By.id("submit"))
    );
}

However, JavaScript click does not reproduce all browser-level behavior of a real user click.

Therefore, use it as a fallback rather than the default.

17. Enter Text Using JavaScript
WebElement username = driver.findElement(By.id("username"));


js.executeScript(
    "arguments[0].value='Selva';",
    username
);

This directly changes the DOM value.

18. JavaScript Text Entry vs sendKeys()

Normal Selenium:

driver.findElement(By.id("username"))
      .sendKeys("Selva");

JavaScript:

js.executeScript(
    "arguments[0].value='Selva';",
    driver.findElement(By.id("username"))
);

Important:

JavaScript changes the DOM value directly.

It may not trigger the same keyboard/input events that a real user action or sendKeys() would trigger.

Therefore:

sendKeys()

should normally be preferred.

19. Trigger Input Event

If the application depends on JavaScript input events, simply setting .value may not be enough.

Example:

js.executeScript(
    "arguments[0].value='Selva';" +
    "arguments[0].dispatchEvent(new Event('input', {bubbles:true}));",
    username
);

For change events:

js.executeScript(
    "arguments[0].value='Selva';" +
    "arguments[0].dispatchEvent(new Event('change', {bubbles:true}));",
    username
);
20. Highlight an Element

Highlight an element temporarily:

WebElement element = driver.findElement(By.id("username"));


js.executeScript(
    "arguments[0].style.border='3px solid red';",
    element
);

This is useful for debugging.

21. Change Background Color
js.executeScript(
    "arguments[0].style.backgroundColor='yellow';",
    element
);
22. Change Text Color
js.executeScript(
    "arguments[0].style.color='blue';",
    element
);
23. Add Multiple CSS Properties
js.executeScript(
    "arguments[0].style.border='3px solid red';" +
    "arguments[0].style.backgroundColor='yellow';",
    element
);
24. Remove an Attribute

Example:

js.executeScript(
    "arguments[0].removeAttribute('disabled');",
    element
);

This can technically enable a disabled element.

However, modifying application state this way should be avoided in normal functional tests unless it is specifically the behavior being tested.

25. Add an Attribute
js.executeScript(
    "arguments[0].setAttribute('data-test','selenium');",
    element
);
26. Get an Attribute Using JavaScript
String value = (String) js.executeScript(
    "return arguments[0].getAttribute('value');",
    element
);


System.out.println(value);

Normally prefer:

String value = element.getAttribute("value");
27. Get Element Text Using JavaScript
String text = (String) js.executeScript(
    "return arguments[0].innerText;",
    element
);


System.out.println(text);

Other DOM properties:

String text = (String) js.executeScript(
    "return arguments[0].textContent;",
    element
);

Difference:

innerText generally reflects rendered text.
textContent returns text content from the DOM.
28. Get HTML of an Element
String html = (String) js.executeScript(
    "return arguments[0].innerHTML;",
    element
);


System.out.println(html);

To get the entire element:

String html = (String) js.executeScript(
    "return arguments[0].outerHTML;",
    element
);
29. Get Element Tag Name
String tagName = (String) js.executeScript(
    "return arguments[0].tagName;",
    element
);


System.out.println(tagName);

Example output:

BUTTON
30. Check Element Visibility Using JavaScript
Boolean visible = (Boolean) js.executeScript(
    "return arguments[0].offsetParent !== null;",
    element
);


System.out.println(visible);

However, Selenium's:

element.isDisplayed();

is generally preferable for WebDriver automation.

31. Check Element Enabled State

JavaScript:

Boolean disabled = (Boolean) js.executeScript(
    "return arguments[0].disabled;",
    element
);


System.out.println(!disabled);

Normally:

element.isEnabled();

should be preferred.

32. Check Checkbox State
Boolean checked = (Boolean) js.executeScript(
    "return arguments[0].checked;",
    checkbox
);


System.out.println(checked);

Normal Selenium:

boolean checked = checkbox.isSelected();
33. Select Checkbox Using JavaScript
js.executeScript(
    "arguments[0].checked=true;",
    checkbox
);

But setting the property may not trigger application events.

If needed:

js.executeScript(
    "arguments[0].checked=true;" +
    "arguments[0].dispatchEvent(new Event('change', {bubbles:true}));",
    checkbox
);
34. Unselect Checkbox
js.executeScript(
    "arguments[0].checked=false;",
    checkbox
);

Again, prefer:

checkbox.click();

when normal Selenium interaction works.

35. Open a New Tab Using JavaScript
js.executeScript("window.open('https://www.google.com','_blank');");

Then retrieve the window handles:

Set<String> windows = driver.getWindowHandles();


System.out.println(windows);
36. Navigate Using JavaScript
js.executeScript(
    "window.location.href='https://www.google.com';"
);

Normal Selenium:

driver.get("https://www.google.com");

Prefer WebDriver navigation for normal test automation.

37. Execute JavaScript Function

Suppose the page contains:

function displayMessage() {
    alert("Hello");
}

You can execute:

js.executeScript("displayMessage();");
38. Execute JavaScript With Arguments

Java:

String name = "Selva";


js.executeScript(
    "alert(arguments[0]);",
    name
);

Multiple arguments:

String firstName = "Selva";
String lastName = "Rajam";


js.executeScript(
    "alert(arguments[0] + ' ' + arguments[1]);",
    firstName,
    lastName
);
39. Using WebElement as JavaScript Argument
WebElement button = driver.findElement(By.id("submit"));


js.executeScript(
    "arguments[0].click();",
    button
);

This is one of the most important JavaScriptExecutor techniques in Selenium.

40. Multiple WebElements as Arguments
WebElement first = driver.findElement(By.id("first"));
WebElement second = driver.findElement(By.id("second"));


js.executeScript(
    "arguments[0].value='Selva';" +
    "arguments[1].value='Xavier';",
    first,
    second
);
41. Return Value From JavaScript

JavaScript:

String title = (String) js.executeScript(
    "return document.title;"
);

JavaScriptExecutor can return:

String
Boolean
Long
Double
WebElement
List
Map
null
42. Return WebElement
WebElement element = (WebElement) js.executeScript(
    "return document.getElementById('username');"
);


element.sendKeys("Selva");
43. Return Multiple Elements
List<WebElement> elements = (List<WebElement>) js.executeScript(
    "return document.querySelectorAll('input');"
);

Depending on browser/driver behavior, returned DOM collections are converted into Selenium-supported collections.

For maintainability, normal Selenium locators are usually preferable.

44. Get Page Height
Long height = (Long) js.executeScript(
    "return document.body.scrollHeight;"
);


System.out.println("Page Height: " + height);
45. Get Current Scroll Position
Long scrollPosition = (Long) js.executeScript(
    "return window.pageYOffset;"
);


System.out.println(scrollPosition);
46. Scroll Until Element Is Visible
WebElement element = driver.findElement(By.id("footer"));


js.executeScript(
    "arguments[0].scrollIntoView({block:'center'});",
    element
);

Then:

element.click();

This is generally better than using JavaScript click directly.

47. Smooth Scrolling
js.executeScript(
    "window.scrollTo({top: document.body.scrollHeight, behavior: 'smooth'});"
);
48. Scroll Inside a Div

Suppose a page has a scrollable container:

WebElement container = driver.findElement(By.id("results"));


js.executeScript(
    "arguments[0].scrollTop = arguments[0].scrollHeight;",
    container
);

This is different from scrolling the entire page.

49. Find an Element Using querySelector()

JavaScript:

WebElement element = (WebElement) js.executeScript(
    "return document.querySelector('#username');"
);

Equivalent Selenium:

WebElement element = driver.findElement(By.cssSelector("#username"));
50. Find Multiple Elements Using querySelectorAll()
List<WebElement> elements = (List<WebElement>) js.executeScript(
    "return document.querySelectorAll('input');"
);
51. Access DOM Parent
WebElement parent = (WebElement) js.executeScript(
    "return arguments[0].parentElement;",
    element
);
52. Access Child Elements
List<WebElement> children = (List<WebElement>) js.executeScript(
    "return arguments[0].children;",
    element
);
53. Access Sibling Element

Next sibling:

WebElement sibling = (WebElement) js.executeScript(
    "return arguments[0].nextElementSibling;",
    element
);

Previous sibling:

WebElement sibling = (WebElement) js.executeScript(
    "return arguments[0].previousElementSibling;",
    element
);
54. Execute Async JavaScript

Synchronous:

js.executeScript("return document.title;");

Asynchronous:

js.executeAsyncScript(
    "var callback = arguments[arguments.length - 1];" +
    "setTimeout(function() {" +
    "callback('Completed');" +
    "}, 2000);"
);

Configure the script timeout:

driver.manage().timeouts()
      .scriptTimeout(Duration.ofSeconds(10));
55. executeScript() vs executeAsyncScript()
executeScript()

Used for synchronous JavaScript.

js.executeScript("return document.title;");

The script finishes before execution continues.

executeAsyncScript()

Used for asynchronous JavaScript.

js.executeAsyncScript(
    "var callback = arguments[arguments.length - 1];" +
    "setTimeout(function() {" +
    "callback('Done');" +
    "}, 2000);"
);

The callback tells Selenium that the asynchronous script has completed.

56. JavaScriptExecutor With PageFactory

Example Page Object:

public class LoginPage {


    private WebDriver driver;


    private JavascriptExecutor js;


    @FindBy(id = "username")
    private WebElement username;


    @FindBy(id = "password")
    private WebElement password;


    @FindBy(id = "login")
    private WebElement loginButton;


    public LoginPage(WebDriver driver) {
        this.driver = driver;
        this.js = (JavascriptExecutor) driver;


        PageFactory.initElements(driver, this);
    }


    public void enterUsername(String value) {
        username.sendKeys(value);
    }


    public void enterPassword(String value) {
        password.sendKeys(value);
    }


    public void clickLoginUsingJavaScript() {
        js.executeScript(
            "arguments[0].click();",
            loginButton
        );
    }
}
57. JavaScriptExecutor Utility Class

A utility class can centralize JavaScript operations.

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


    public void scrollToElement(WebElement element) {
        js.executeScript(
            "arguments[0].scrollIntoView({block:'center'});",
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
            "window.scrollTo(0,0);"
        );
    }


    public void highlight(WebElement element) {
        js.executeScript(
            "arguments[0].style.border='3px solid red';",
            element
        );
    }


    public String getPageTitle() {
        return (String) js.executeScript(
            "return document.title;"
        );
    }


    public String getPageUrl() {
        return (String) js.executeScript(
            "return window.location.href;"
        );
    }
}
58. Using the JavaScript Utility
JavaScriptUtils jsUtils =
    new JavaScriptUtils(driver);


jsUtils.scrollToElement(loginButton);


jsUtils.highlight(loginButton);


jsUtils.click(loginButton);
59. JavaScriptExecutor and Shadow DOM

For open Shadow DOM, JavaScript can sometimes be used to access the shadow root.

Example:

WebElement host = driver.findElement(By.cssSelector("#shadow-host"));


SearchContext shadowRoot = (SearchContext) js.executeScript(
    "return arguments[0].shadowRoot;",
    host
);

Then:

WebElement element =
    shadowRoot.findElement(By.cssSelector("#username"));

Modern Selenium also provides native Shadow DOM support, so prefer Selenium's native APIs when available.

60. JavaScriptExecutor vs Selenium WebDriver
Task	Preferred Approach
Click button	element.click()
Enter text	sendKeys()
Get title	driver.getTitle()
Get URL	driver.getCurrentUrl()
Navigate	driver.get()
Refresh	driver.navigate().refresh()
Scroll element	scrollIntoView() if needed
Special DOM manipulation	JavaScriptExecutor
Debug/highlight	JavaScriptExecutor
Custom browser JavaScript	JavaScriptExecutor
61. Important Rule

Do not replace all Selenium operations with JavaScript.

Bad approach:

js.executeScript("arguments[0].click();", button);
js.executeScript("arguments[0].value='Selva';", username);
js.executeScript("window.location.href='https://example.com';");

Better:

button.click();


username.sendKeys("Selva");


driver.get("https://example.com");

Use JavaScript only when there is a valid reason.

62. Common JavaScriptExecutor Problems
Problem 1: JavaScript Click Does Not Trigger Application Logic

Example:

js.executeScript(
    "arguments[0].click();",
    element
);

Some frameworks rely on specific browser events.

Solution:

Prefer:

element.click();

If JavaScript is required, trigger the appropriate events.

63. Problem 2: Setting value Does Not Update React/Angular Application

This may fail:

js.executeScript(
    "arguments[0].value='Selva';",
    input
);

The DOM value changes, but the framework may not recognize it.

Prefer:

input.sendKeys("Selva");

If JavaScript is unavoidable, dispatch the required events.

64. Problem 3: Element Is Null

Example:

WebElement element = (WebElement) js.executeScript(
    "return document.querySelector('#missing');"
);

If the selector does not match, the result may be null.

Always make sure the element exists.

65. Problem 4: JavaScript Syntax Error

Incorrect:

js.executeScript(
    "document.getElementById('username').value='Selva'"
);

Missing semicolon is generally tolerated by JavaScript, but malformed JavaScript will cause an exception.

Example:

js.executeScript(
    "document.getElementById('username').value='Selva';"
);

Keep JavaScript simple and testable.

66. Problem 5: Stale Element

JavaScriptExecutor does not eliminate stale element problems.

Example:

WebElement button = driver.findElement(By.id("submit"));


js.executeScript(
    "arguments[0].click();",
    button
);

If the DOM has changed and the reference is stale:

StaleElementReferenceException

may still occur.

Re-locate the element when necessary.

67. Best Practices
Prefer Selenium WebDriver APIs.
Use JavaScript as a fallback.
Keep JavaScript code short.
Avoid unnecessary DOM manipulation.
Do not bypass application behavior just to make a test pass.
Use arguments[0] instead of building element-specific JavaScript strings.
Prefer scrollIntoView() before using JavaScript click.
Trigger required events when modifying values.
Put reusable JavaScript operations in a utility class.
Keep JavaScriptExecutor usage documented.
68. Interview Question
What is JavaScriptExecutor?

JavaScriptExecutor is a Selenium interface used to execute JavaScript code inside the browser.

Example:

JavascriptExecutor js =
    (JavascriptExecutor) driver;


js.executeScript(
    "arguments[0].click();",
    element
);
69. Interview Question
Why would you use JavaScriptExecutor instead of click()?

Normally:

element.click();

should be preferred.

JavaScriptExecutor can be used when normal Selenium interaction is blocked by issues such as:

Overlapping elements
Difficult scrolling behavior
Custom DOM behavior
Special JavaScript-based controls

Example:

js.executeScript(
    "arguments[0].click();",
    element
);
70. Interview Question
How do you scroll to an element?
js.executeScript(
    "arguments[0].scrollIntoView({block:'center'});",
    element
);
71. Interview Question
How do you scroll to the bottom of a page?
js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);
72. Interview Question
How do you enter text using JavaScript?
js.executeScript(
    "arguments[0].value='Selva';",
    element
);

But normally:

element.sendKeys("Selva");

is preferred.

73. Interview Question
How do you execute asynchronous JavaScript?
js.executeAsyncScript(
    "var callback = arguments[arguments.length - 1];" +
    "setTimeout(function() {" +
    "callback('Done');" +
    "}, 2000);"
);

Configure:

driver.manage().timeouts()
      .scriptTimeout(Duration.ofSeconds(10));
74. Interview Question
What is arguments[0]?

arguments[0] represents the first argument passed from Java to JavaScript.

Example:

js.executeScript(
    "arguments[0].click();",
    button
);

Here:

arguments[0] = button
75. Interview Question
Can JavaScriptExecutor return a value?

Yes.

Example:

String title = (String) js.executeScript(
    "return document.title;"
);
76. Interview Question
Can JavaScriptExecutor return a WebElement?

Yes.

WebElement element = (WebElement) js.executeScript(
    "return document.getElementById('username');"
);
77. Interview Question
What is the difference between executeScript() and executeAsyncScript()?
executeScript()	executeAsyncScript()
Synchronous	Asynchronous
Waits for script completion	Uses callback
Simpler JavaScript	Useful for async operations
Commonly used	Used for asynchronous browser scripts
78. Complete Example
import java.time.Duration;


import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;


public class JavaScriptExecutorDemo {


    public static void main(String[] args) {


        WebDriver driver = new ChromeDriver();


        driver.manage().window().maximize();


        driver.manage().timeouts()
              .implicitlyWait(Duration.ofSeconds(10));


        driver.get("https://example.com");


        JavascriptExecutor js =
            (JavascriptExecutor) driver;


        // Get title
        String title = (String) js.executeScript(
            "return document.title;"
        );


        System.out.println("Title: " + title);


        // Get URL
        String url = (String) js.executeScript(
            "return window.location.href;"
        );


        System.out.println("URL: " + url);


        // Scroll to bottom
        js.executeScript(
            "window.scrollTo(0, document.body.scrollHeight);"
        );


        // Scroll to top
        js.executeScript(
            "window.scrollTo(0,0);"
        );


        // Find element
        WebElement element =
            driver.findElement(By.tagName("h1"));


        // Scroll to element
        js.executeScript(
            "arguments[0].scrollIntoView({block:'center'});",
            element
        );


        // Highlight element
        js.executeScript(
            "arguments[0].style.border='3px solid red';",
            element
        );


        driver.quit();
    }
}
79. Key Takeaways
JavascriptExecutor
        |
        +-- executeScript()
        |
        +-- executeAsyncScript()
        |
        +-- Scroll
        |
        +-- Click
        |
        +-- DOM manipulation
        |
        +-- Read DOM properties
        |
        +-- Execute custom JavaScript
        |
        +-- Return JavaScript values
        |
        +-- Debugging / highlighting

Most important syntax:

JavascriptExecutor js =
    (JavascriptExecutor) driver;

Execute JavaScript:

js.executeScript("return document.title;");

Pass WebElement:

js.executeScript(
    "arguments[0].click();",
    element
);

Scroll:

js.executeScript(
    "arguments[0].scrollIntoView({block:'center'});",
    element
);

Async JavaScript:

js.executeAsyncScript("...");
80. Final Recommendation

Use Selenium WebDriver as the primary automation API.

Use JavaScriptExecutor when:

Normal Selenium
      |
      v
Can Selenium perform the action?
      |
   YES ---> Use Selenium
      |
      NO
      |
      v
Can JavaScript safely solve the problem?
      |
   YES ---> Use JavaScriptExecutor
      |
      NO
      |
      v
Investigate the application / DOM / synchronization issue

JavaScriptExecutor is a powerful fallback tool, not a replacement for Selenium WebDriver.


