# Selenium WebElements

## 1. What is a WebElement?

`WebElement` is an interface in Selenium that represents an HTML element on a web page.

Examples of web elements:

- Text box
- Button
- Link
- Checkbox
- Radio button
- Dropdown
- Image
- Label
- Table
- Form
- Text area

Example HTML:

    <input id="username" type="text">

Selenium:

    WebElement username =
        driver.findElement(
            By.id("username")
        );

Here:

- `WebElement` is the interface.
- `username` is the reference variable.
- `driver.findElement()` locates the element.

---

# 2. Import WebElement

Use:

    import org.openqa.selenium.WebElement;

Example:

    WebElement element =
        driver.findElement(
            By.id("username")
        );

---

# 3. Finding a WebElement

The most common way to find an element is:

    driver.findElement(By.id("username"));

Example:

    WebElement username =
        driver.findElement(
            By.id("username")
        );

---

# 4. findElement()

`findElement()` returns the first matching WebElement.

Example:

    WebElement loginButton =
        driver.findElement(
            By.id("login")
        );

    loginButton.click();

If Selenium cannot find the element, it throws:

    NoSuchElementException

---

# 5. findElements()

`findElements()` returns a list of WebElements.

Example:

    List<WebElement> links =
        driver.findElements(
            By.tagName("a")
        );

You can loop through the elements:

    for (WebElement link : links) {

        System.out.println(
            link.getText()
        );
    }

If no elements are found, `findElements()` returns an empty list.

---

# 6. findElement() vs findElements()

| findElement() | findElements() |
|---|---|
| Returns one WebElement | Returns List<WebElement> |
| Returns first matching element | Returns all matching elements |
| Throws NoSuchElementException if not found | Returns empty list if not found |
| Used for individual elements | Used for multiple elements |

Example:

    WebElement element =
        driver.findElement(By.id("username"));

Multiple:

    List<WebElement> elements =
        driver.findElements(
            By.className("product")
        );

---

# 7. click()

`click()` is used to click a WebElement.

Example:

    WebElement loginButton =
        driver.findElement(
            By.id("login")
        );

    loginButton.click();

Shortcut:

    driver.findElement(
        By.id("login")
    ).click();

---

# 8. sendKeys()

`sendKeys()` is used to enter text into an input field.

Example:

    WebElement username =
        driver.findElement(
            By.id("username")
        );

    username.sendKeys("Selva");

---

# 9. clear()

`clear()` removes existing text from an input field.

Example:

    WebElement username =
        driver.findElement(
            By.id("username")
        );

    username.clear();

    username.sendKeys("Selva");

---

# 10. getText()

`getText()` returns the visible text of a WebElement.

HTML:

    <h1>Welcome Selva</h1>

Selenium:

    WebElement heading =
        driver.findElement(
            By.tagName("h1")
        );

    String text = heading.getText();

    System.out.println(text);

Output:

    Welcome Selva

---

# 11. getText() and Input Fields

For an input element:

    <input id="username" value="Selva">

`getText()` may not return the value typed into the input.

Use:

    getAttribute("value")

Example:

    String value =
        driver.findElement(
            By.id("username")
        ).getAttribute("value");

---

# 12. getAttribute()

`getAttribute()` returns the value of an HTML attribute.

HTML:

    <input
        id="username"
        name="user"
        value="Selva">

Examples:

    String id =
        element.getAttribute("id");

    String name =
        element.getAttribute("name");

    String value =
        element.getAttribute("value");

---

# 13. getDomAttribute()

Selenium also provides:

    getDomAttribute()

It retrieves the value of an attribute from the DOM.

Example:

    String value =
        element.getDomAttribute("value");

Use this when you specifically want the DOM attribute value.

---

# 14. getDomProperty()

`getDomProperty()` retrieves the current DOM property value.

Example:

    String value =
        element.getDomProperty("value");

This can be useful when the current property differs from the original HTML attribute.

---

# 15. getAttribute() vs getDomAttribute() vs getDomProperty()

These methods can behave differently.

### getAttribute()

Returns the value associated with an attribute/property according to Selenium's behavior.

### getDomAttribute()

Returns the DOM attribute value.

### getDomProperty()

Returns the current DOM property value.

For input fields, understanding the difference can be important.

Example:

    element.getDomProperty("value");

is useful when you want the current value of the input.

---

# 16. isDisplayed()

`isDisplayed()` checks whether an element is visible.

Example:

    WebElement loginButton =
        driver.findElement(
            By.id("login")
        );

    boolean visible =
        loginButton.isDisplayed();

    System.out.println(visible);

Returns:

    true

or:

    false

---

# 17. isEnabled()

`isEnabled()` checks whether an element is enabled.

Example:

    boolean enabled =
        driver.findElement(
            By.id("login")
        ).isEnabled();

If the button is disabled:

    false

If enabled:

    true

---

# 18. isSelected()

`isSelected()` checks whether an element is selected.

It is commonly used for:

- Checkbox
- Radio button
- Select option

Example:

    WebElement checkbox =
        driver.findElement(
            By.id("remember")
        );

    boolean selected =
        checkbox.isSelected();

---

# 19. Checkbox Example

HTML:

    <input
        type="checkbox"
        id="remember">

Selenium:

    WebElement checkbox =
        driver.findElement(
            By.id("remember")
        );

    if (!checkbox.isSelected()) {

        checkbox.click();
    }

This ensures the checkbox becomes selected.

---

# 20. Unselect Checkbox

Example:

    if (checkbox.isSelected()) {

        checkbox.click();
    }

This ensures the checkbox becomes unselected.

---

# 21. Radio Button Example

HTML:

    <input
        type="radio"
        id="male"
        name="gender">

Selenium:

    WebElement male =
        driver.findElement(
            By.id("male")
        );

    if (!male.isSelected()) {

        male.click();
    }

---

# 22. Checking Whether Button Is Enabled

Example:

    WebElement submit =
        driver.findElement(
            By.id("submit")
        );

    if (submit.isEnabled()) {

        submit.click();

    } else {

        System.out.println(
            "Submit button is disabled"
        );
    }

---

# 23. Checking Whether Element Is Displayed

Example:

    WebElement message =
        driver.findElement(
            By.id("successMessage")
        );

    if (message.isDisplayed()) {

        System.out.println(
            message.getText()
        );
    }

---

# 24. getTagName()

`getTagName()` returns the HTML tag name.

HTML:

    <input id="username">

Example:

    String tag =
        driver.findElement(
            By.id("username")
        ).getTagName();

Output:

    input

---

# 25. getCssValue()

`getCssValue()` returns the value of a CSS property.

Example:

    String color =
        element.getCssValue("color");

Another example:

    String fontSize =
        element.getCssValue("font-size");

---

# 26. getRect()

`getRect()` returns the element's location and dimensions.

Example:

    Rectangle rect =
        element.getRect();

    System.out.println(
        rect.getX()
    );

    System.out.println(
        rect.getY()
    );

    System.out.println(
        rect.getWidth()
    );

    System.out.println(
        rect.getHeight()
    );

---

# 27. getLocation()

Returns the location of the element.

Example:

    Point location =
        element.getLocation();

    System.out.println(
        location.getX()
    );

    System.out.println(
        location.getY()
    );

---

# 28. getSize()

Returns the size of the element.

Example:

    Dimension size =
        element.getSize();

    System.out.println(
        size.getWidth()
    );

    System.out.println(
        size.getHeight()
    );

---

# 29. submit()

`submit()` can submit a form associated with an element.

Example:

    WebElement form =
        driver.findElement(
            By.id("loginForm")
        );

    form.submit();

However, in modern Selenium automation, clicking the appropriate submit button is often clearer:

    driver.findElement(
        By.id("submit")
    ).click();

---

# 30. sendKeys() with Keyboard Keys

You can use Selenium's `Keys` class.

Import:

    import org.openqa.selenium.Keys;

Example:

    element.sendKeys(
        Keys.ENTER
    );

---

# 31. Ctrl + A

Example:

    element.sendKeys(
        Keys.CONTROL,
        "a"
    );

This selects all text.

---

# 32. Ctrl + C

Example:

    element.sendKeys(
        Keys.CONTROL,
        "c"
    );

---

# 33. Ctrl + V

Example:

    element.sendKeys(
        Keys.CONTROL,
        "v"
    );

---

# 34. Clearing and Entering Text

Example:

    WebElement search =
        driver.findElement(
            By.id("search")
        );

    search.clear();

    search.sendKeys(
        "Toyota"
    );

    search.sendKeys(
        Keys.ENTER
    );

---

# 35. Handling Text Areas

HTML:

    <textarea
        id="comments">
    </textarea>

Selenium:

    WebElement comments =
        driver.findElement(
            By.id("comments")
        );

    comments.sendKeys(
        "This is my comment."
    );

---

# 36. Handling Read-Only Elements

HTML:

    <input
        id="username"
        readonly>

A read-only field may be displayed but cannot normally be edited.

Check:

    boolean enabled =
        element.isEnabled();

However, `isEnabled()` alone does not necessarily tell you whether an input is editable.

You can inspect:

    element.getAttribute("readonly");

or:

    element.getDomAttribute("readonly");

---

# 37. Handling Disabled Elements

HTML:

    <button
        id="submit"
        disabled>
        Submit
    </button>

Check:

    boolean enabled =
        element.isEnabled();

Expected:

    false

---

# 38. WebElement and Explicit Wait

It is common to locate an element after waiting for the required condition.

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement loginButton =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("login")
                )
        );

    loginButton.click();

---

# 39. Presence vs Visibility

### Presence

The element exists in the DOM.

Example:

    ExpectedConditions
        .presenceOfElementLocated(
            By.id("username")
        );

### Visibility

The element exists and is visible.

Example:

    ExpectedConditions
        .visibilityOfElementLocated(
            By.id("username")
        );

### Clickable

The element is visible and enabled.

Example:

    ExpectedConditions
        .elementToBeClickable(
            By.id("login")
        );

---

# 40. WebElement Reference

A WebElement reference represents an element at a particular point in the browser's DOM.

Example:

    WebElement username =
        driver.findElement(
            By.id("username")
        );

If the page changes and the element is recreated, the old reference may become invalid.

This can cause:

    StaleElementReferenceException

---

# 41. StaleElementReferenceException

Example:

    WebElement element =
        driver.findElement(
            By.id("username")
        );

    driver.navigate().refresh();

    element.click();

The old element reference may no longer be valid.

Better approach:

    driver.navigate().refresh();

    WebElement element =
        driver.findElement(
            By.id("username")
        );

    element.click();

---

# 42. Why WebElement Becomes Stale

Common reasons:

- Page refresh
- Navigation
- AJAX update
- DOM re-render
- React component re-render
- Angular component update
- Element removed and recreated

---

# 43. WebElement Inside an iframe

If the element is inside an iframe, you must first switch into the frame.

Example:

    driver.switchTo().frame(
        "paymentFrame"
    );

Then:

    WebElement cardNumber =
        driver.findElement(
            By.id("cardNumber")
        );

    cardNumber.sendKeys(
        "1234"
    );

Return to main document:

    driver.switchTo().defaultContent();

---

# 44. WebElement Inside Shadow DOM

Some modern applications use Shadow DOM.

Example:

    WebElement host =
        driver.findElement(
            By.cssSelector(
                "my-component"
            )
        );

    SearchContext shadowRoot =
        host.getShadowRoot();

    WebElement element =
        shadowRoot.findElement(
            By.cssSelector(
                "input"
            )
        );

Shadow DOM handling is covered separately in:

    ShadowDOM/Selenium-ShadowDOM.md

---

# 45. WebElement and JavaScript

Sometimes JavaScript can be used to interact with an element.

Example:

    JavascriptExecutor js =
        (JavascriptExecutor) driver;

    js.executeScript(
        "arguments[0].click();",
        element
    );

Normal Selenium interaction should generally be preferred when it works correctly.

---

# 46. Scrolling to a WebElement

Example:

    JavascriptExecutor js =
        (JavascriptExecutor) driver;

    js.executeScript(
        "arguments[0].scrollIntoView(true);",
        element
    );

This scrolls the page until the element is in view.

---

# 47. WebElement Screenshot

Modern Selenium supports taking a screenshot of an individual WebElement.

Example:

    File screenshot =
        element.getScreenshotAs(
            OutputType.FILE
        );

This can be useful when you only need evidence for one element instead of the entire page.

---

# 48. WebElement Collections

Example HTML:

    <div class="product">
        Product 1
    </div>

    <div class="product">
        Product 2
    </div>

    <div class="product">
        Product 3
    </div>

Selenium:

    List<WebElement> products =
        driver.findElements(
            By.className("product")
        );

Loop:

    for (WebElement product : products) {

        System.out.println(
            product.getText()
        );
    }

---

# 49. Get Text from Multiple Elements

Example:

    List<WebElement> products =
        driver.findElements(
            By.cssSelector(".product")
        );

    for (WebElement product : products) {

        String text =
            product.getText();

        System.out.println(text);
    }

---

# 50. Get Attribute from Multiple Elements

Example:

    List<WebElement> links =
        driver.findElements(
            By.tagName("a")
        );

    for (WebElement link : links) {

        String href =
            link.getAttribute("href");

        System.out.println(href);
    }

---

# 51. Count WebElements

Example:

    List<WebElement> products =
        driver.findElements(
            By.cssSelector(".product")
        );

    int count = products.size();

    System.out.println(
        "Product count: " + count
    );

---

# 52. Verify Minimum Number of Elements

Example:

    List<WebElement> products =
        driver.findElements(
            By.cssSelector(".product")
        );

    if (products.size() >= 3) {

        System.out.println(
            "At least 3 products displayed"
        );
    }

---

# 53. Verify Exact Number of Elements

Example:

    List<WebElement> products =
        driver.findElements(
            By.cssSelector(".product")
        );

    if (products.size() == 5) {

        System.out.println(
            "Exactly 5 products found"
        );
    }

---

# 54. Find an Element Within Another WebElement

You can search inside a WebElement.

Example:

    WebElement product =
        driver.findElement(
            By.cssSelector(".product")
        );

    WebElement title =
        product.findElement(
            By.cssSelector(".title")
        );

This is useful for structured components.

---

# 55. Nested WebElement Example

HTML:

    <div class="product">
        <h2 class="title">
            Toyota Camry
        </h2>

        <button class="details">
            Details
        </button>
    </div>

Selenium:

    WebElement product =
        driver.findElement(
            By.cssSelector(".product")
        );

    WebElement title =
        product.findElement(
            By.cssSelector(".title")
        );

    WebElement details =
        product.findElement(
            By.cssSelector(".details")
        );

---

# 56. WebElement vs By

`By` represents a locator.

`WebElement` represents the actual element.

Example:

    By usernameLocator =
        By.id("username");

    WebElement username =
        driver.findElement(
            usernameLocator
        );

Relationship:

    By
    ↓
    Locator

    WebElement
    ↓
    Located HTML element

---

# 57. By vs WebElement in Page Objects

Recommended:

    private By username =
        By.id("username");

Then:

    driver.findElement(username)
          .sendKeys("Selva");

This can be preferable to storing a WebElement reference for dynamic pages because the element can be located when the action is performed.

---

# 58. @FindBy and WebElement

With PageFactory, you can define:

    @FindBy(id = "username")
    private WebElement username;

Then:

    username.sendKeys("Selva");

Example:

    public class LoginPage {

        @FindBy(id = "username")
        private WebElement username;

        @FindBy(id = "password")
        private WebElement password;

        @FindBy(id = "login")
        private WebElement loginButton;

        public void login(
                String user,
                String pass) {

            username.sendKeys(user);

            password.sendKeys(pass);

            loginButton.click();
        }
    }

PageFactory is covered separately in:

    PageFactory/Selenium-PageFactory.md

---

# 59. WebElement and Page Object Model

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

This keeps WebElement interaction inside the page class.

---

# 60. Common WebElement Exceptions

Common exceptions include:

- NoSuchElementException
- StaleElementReferenceException
- ElementNotInteractableException
- ElementClickInterceptedException
- InvalidElementStateException
- ElementNotSelectableException
- TimeoutException

---

# 61. NoSuchElementException

Occurs when Selenium cannot locate the element.

Example:

    driver.findElement(
        By.id("doesNotExist")
    );

Possible causes:

- Wrong locator
- Element not present
- Wrong page
- Wrong frame
- Wrong window
- Element loaded later

---

# 62. ElementNotInteractableException

Occurs when Selenium finds an element but cannot interact with it in the current state.

Possible causes:

- Hidden element
- Disabled element
- Element not ready
- Incorrect interaction

Possible solutions:

- Wait for visibility
- Wait for clickability
- Verify element state
- Scroll if necessary
- Check the DOM

---

# 63. ElementClickInterceptedException

Occurs when another element prevents the click.

Possible causes:

- Popup
- Overlay
- Loading spinner
- Sticky header
- Modal
- Animation

Use appropriate waits and handle the blocking element.

---

# 64. InvalidElementStateException

Can occur when an element is in a state that does not allow the requested operation.

Example:

Trying to clear an element that cannot be edited.

Check:

    isEnabled()

and the element's attributes/state before interacting.

---

# 65. WebElement Best Practices

1. Use meaningful variable names.
2. Use stable locators.
3. Prefer Page Object Model.
4. Avoid unnecessary global WebElement references.
5. Use explicit waits for dynamic elements.
6. Re-locate elements after major DOM changes.
7. Avoid unnecessary JavaScript clicks.
8. Verify element state before interaction when necessary.
9. Keep locators separate from test logic.
10. Avoid `Thread.sleep()` for normal synchronization.
11. Use `findElements()` when working with collections.
12. Handle stale elements appropriately.
13. Keep page-specific WebElement interactions in page classes.
14. Use screenshots when debugging difficult UI failures.

---

# 66. WebElement Interaction Pattern

A common pattern is:

    Locate
       ↓
    Wait
       ↓
    Verify State
       ↓
    Interact
       ↓
    Validate

Example:

    By loginButton =
        By.id("login");

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement button =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    loginButton
                )
        );

    button.click();

---

# 67. Example: Complete WebElement Test

    import java.time.Duration;

    import org.openqa.selenium.By;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.chrome.ChromeDriver;
    import org.openqa.selenium.support.ui.ExpectedConditions;
    import org.openqa.selenium.support.ui.WebDriverWait;

    public class WebElementExample {

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

                WebElement username =
                    wait.until(
                        ExpectedConditions
                            .visibilityOfElementLocated(
                                By.id("username")
                            )
                    );

                username.clear();

                username.sendKeys(
                    "Selva"
                );

                WebElement login =
                    wait.until(
                        ExpectedConditions
                            .elementToBeClickable(
                                By.id("login")
                            )
                    );

                login.click();

            } finally {

                driver.quit();
            }
        }
    }

---

# 68. WebElement Quick Reference

| Method | Purpose |
|---|---|
| `click()` | Click element |
| `sendKeys()` | Enter text / keyboard input |
| `clear()` | Clear editable field |
| `getText()` | Get visible text |
| `getAttribute()` | Get attribute/property-related value |
| `getDomAttribute()` | Get DOM attribute |
| `getDomProperty()` | Get DOM property |
| `isDisplayed()` | Check visibility |
| `isEnabled()` | Check enabled state |
| `isSelected()` | Check selection |
| `getTagName()` | Get HTML tag |
| `getCssValue()` | Get CSS property |
| `getRect()` | Get position and dimensions |
| `getLocation()` | Get location |
| `getSize()` | Get dimensions |
| `submit()` | Submit associated form |
| `getScreenshotAs()` | Capture element screenshot |

---

# 69. Most Important WebElement Methods for Interviews

Focus especially on:

    click()
    sendKeys()
    clear()
    getText()
    getAttribute()
    getDomAttribute()
    getDomProperty()
    isDisplayed()
    isEnabled()
    isSelected()

Also understand:

    findElement()
    findElements()

---

# 70. Common WebElement Interview Questions

1. What is WebElement in Selenium?
2. What is the difference between WebDriver and WebElement?
3. What is the difference between `findElement()` and `findElements()`?
4. What does `click()` do?
5. What does `sendKeys()` do?
6. What does `clear()` do?
7. What is the difference between `getText()` and `getAttribute()`?
8. How do you get the value of an input field?
9. What is `isDisplayed()`?
10. What is `isEnabled()`?
11. What is `isSelected()`?
12. How do you check whether a checkbox is selected?
13. How do you check whether a button is enabled?
14. How do you get the tag name?
15. How do you get CSS properties?
16. How do you get the location of an element?
17. How do you get the size of an element?
18. What is StaleElementReferenceException?
19. Why does a WebElement become stale?
20. How do you handle stale elements?
21. What is ElementNotInteractableException?
22. What is ElementClickInterceptedException?
23. How do you wait for a WebElement?
24. How do you find multiple WebElements?
25. How do you loop through WebElements?
26. How do you find an element inside another WebElement?
27. How do you handle elements inside an iframe?
28. How do you handle elements inside Shadow DOM?
29. What is the difference between `By` and `WebElement`?
30. Should WebElement objects be stored globally in a framework?
31. How does Page Object Model use WebElements?
32. What is `@FindBy`?
33. What is PageFactory?
34. When should you re-locate a WebElement?
35. How do you handle dynamic WebElements?

---

# 71. Quick Revision

Remember:

    By
    ↓
    Locator

    findElement()
    ↓
    WebElement

    WebElement
    ↓
    HTML Element

    click()
    ↓
    Click

    sendKeys()
    ↓
    Enter Text

    clear()
    ↓
    Clear Text

    getText()
    ↓
    Visible Text

    getAttribute()
    ↓
    Attribute Value

    isDisplayed()
    ↓
    Visible?

    isEnabled()
    ↓
    Enabled?

    isSelected()
    ↓
    Selected?

---

# 72. Final Summary

`WebElement` is one of the most important Selenium concepts.

The typical Selenium interaction is:

    WebDriver
        ↓
    By Locator
        ↓
    findElement()
        ↓
    WebElement
        ↓
    Wait / Verify State
        ↓
    click() / sendKeys() / clear()
        ↓
    Validate Result

For professional automation frameworks:

    Locators
        +
    WebElements
        +
    Explicit Waits
        +
    Page Object Model
        +
    TestNG
        =
    Maintainable Selenium Automation

The most important WebElement methods to remember are:

    click()
    sendKeys()
    clear()
    getText()
    getAttribute()
    getDomAttribute()
    getDomProperty()
    isDisplayed()
    isEnabled()
    isSelected()
    getTagName()
    getCssValue()
    getRect()
    getLocation()
    getSize()
    submit()
    getScreenshotAs()

A strong Selenium engineer should also understand how WebElements behave when the DOM changes, especially `StaleElementReferenceException`, dynamic elements, iframes, Shadow DOM, waits, and Page Object Model.
