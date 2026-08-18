# Selenium Locators

## 1. What is a Locator?

A locator is a mechanism used by Selenium WebDriver to identify an HTML element on a web page.

For example, suppose the HTML is:

    <input id="username" name="userName" type="text">

Selenium can locate this element using:

    driver.findElement(By.id("username"));

Locators are one of the most important concepts in Selenium automation.

---

# 2. Why Are Locators Important?

Selenium must identify an element before it can interact with it.

For example:

    Enter username
        ↓
    Find username textbox
        ↓
    Send text
        ↓
    Click Login

Example:

    driver.findElement(By.id("username"))
          .sendKeys("Selva");

If the locator is incorrect, Selenium cannot find the element.

---

# 3. Selenium Locator Strategies

Selenium provides the following commonly used locator strategies:

1. ID
2. Name
3. Class Name
4. Tag Name
5. Link Text
6. Partial Link Text
7. CSS Selector
8. XPath

The first six are provided directly through `By`.

Example:

    By.id()
    By.name()
    By.className()
    By.tagName()
    By.linkText()
    By.partialLinkText()
    By.cssSelector()
    By.xpath()

---

# 4. By Class

Selenium provides the `By` class for locating elements.

Example:

    import org.openqa.selenium.By;

    driver.findElement(
        By.id("username")
    );

The `By` class provides locator strategies.

---

# 5. ID Locator

The ID locator identifies an element using its `id` attribute.

HTML:

    <input id="username" type="text">

Selenium:

    driver.findElement(
        By.id("username")
    );

This is usually one of the preferred locator strategies when the ID is:

- Unique
- Stable
- Not dynamically generated

---

# 6. ID Locator Example

HTML:

    <button id="loginButton">
        Login
    </button>

Selenium:

    driver.findElement(
        By.id("loginButton")
    ).click();

---

# 7. Advantages of ID Locator

ID is generally:

- Easy to read
- Easy to maintain
- Fast
- Simple
- Usually unique

Example:

    By.id("username")

is easier to understand than a long XPath.

---

# 8. When Not to Use ID

Do not blindly use an ID if it is dynamic.

Example:

    <input id="user_12345">

The next execution may generate:

    <input id="user_67890">

In this case, the ID is dynamic.

A better locator may use:

- Stable attributes
- CSS Selector
- XPath
- Other unique attributes

---

# 9. Name Locator

The `name` locator uses the HTML `name` attribute.

HTML:

    <input
        name="username"
        type="text">

Selenium:

    driver.findElement(
        By.name("username")
    );

---

# 10. Name Locator Example

HTML:

    <input name="email">

Selenium:

    driver.findElement(
        By.name("email")
    ).sendKeys("test@example.com");

---

# 11. Class Name Locator

The class name locator uses the `class` attribute.

HTML:

    <button class="loginButton">
        Login
    </button>

Selenium:

    driver.findElement(
        By.className("loginButton")
    ).click();

---

# 12. Important Class Name Rule

Suppose HTML contains:

    <button class="btn primary login">
        Login
    </button>

Do not use:

    By.className("btn primary")

`By.className()` expects a single class name.

Use CSS instead:

    By.cssSelector(".btn.primary")

---

# 13. Multiple Classes

HTML:

    <button class="btn primary login">
        Login
    </button>

Possible CSS Selector:

    By.cssSelector(".btn.primary.login")

You can combine multiple classes in a CSS selector.

---

# 14. Tag Name Locator

The tag name locator uses the HTML tag.

HTML:

    <input type="text">

Selenium:

    driver.findElement(
        By.tagName("input")
    );

This is useful when finding multiple elements of the same type.

---

# 15. Find All Input Elements

Example:

    List<WebElement> inputs =
        driver.findElements(
            By.tagName("input")
        );

System.out.println(
    "Input count: " + inputs.size()
);

---

# 16. Link Text Locator

`linkText()` locates an anchor element using its complete visible text.

HTML:

    <a href="/login">
        Login
    </a>

Selenium:

    driver.findElement(
        By.linkText("Login")
    ).click();

---

# 17. Link Text Example

HTML:

    <a href="/products">
        Products
    </a>

Selenium:

    driver.findElement(
        By.linkText("Products")
    ).click();

---

# 18. Partial Link Text

`partialLinkText()` locates a link using part of its visible text.

HTML:

    <a href="/products">
        View All Products
    </a>

Selenium:

    driver.findElement(
        By.partialLinkText("Products")
    ).click();

---

# 19. Link Text vs Partial Link Text

| Link Text | Partial Link Text |
|---|---|
| Matches complete link text | Matches part of link text |
| More specific | Less specific |
| `linkText("Login")` | `partialLinkText("Log")` |

Example:

    <a>Login to Application</a>

Full:

    By.linkText("Login to Application")

Partial:

    By.partialLinkText("Login")

---

# 20. CSS Selector

CSS Selector is one of the most powerful locator strategies in Selenium.

Example:

    driver.findElement(
        By.cssSelector("#username")
    );

CSS selectors are often concise and readable.

---

# 21. CSS ID Selector

HTML:

    <input id="username">

CSS:

    #username

Selenium:

    driver.findElement(
        By.cssSelector("#username")
    );

---

# 22. CSS Class Selector

HTML:

    <button class="loginButton">
        Login
    </button>

CSS:

    .loginButton

Selenium:

    driver.findElement(
        By.cssSelector(".loginButton")
    );

---

# 23. CSS Multiple Classes

HTML:

    <button class="btn primary login">
        Login
    </button>

CSS:

    .btn.primary.login

Selenium:

    driver.findElement(
        By.cssSelector(".btn.primary.login")
    );

---

# 24. CSS Attribute Selector

HTML:

    <input
        name="username"
        type="text">

CSS:

    input[name='username']

Selenium:

    driver.findElement(
        By.cssSelector(
            "input[name='username']"
        )
    );

---

# 25. CSS Attribute Equals

HTML:

    <input
        name="username">

Selector:

    [name='username']

Example:

    By.cssSelector(
        "input[name='username']"
    );

---

# 26. CSS Attribute Contains

HTML:

    <input
        id="user_12345">

CSS:

    input[id*='user']

Selenium:

    driver.findElement(
        By.cssSelector(
            "input[id*='user']"
        )
    );

`*=` means:

    Contains

---

# 27. CSS Attribute Starts With

CSS:

    input[id^='user']

`^=` means:

    Starts with

Example:

    id="user_12345"

matches:

    input[id^='user']

---

# 28. CSS Attribute Ends With

CSS:

    input[id$='name']

`$=` means:

    Ends with

Example:

    id="username"

matches:

    input[id$='name']

---

# 29. CSS Descendant Selector

HTML:

    <div id="login">
        <input name="username">
    </div>

CSS:

    #login input

Selenium:

    driver.findElement(
        By.cssSelector(
            "#login input"
        )
    );

This means:

Find an `input` inside the element with ID `login`.

---

# 30. CSS Child Selector

HTML:

    <div id="login">
        <input name="username">
    </div>

CSS:

    #login > input

The `>` represents a direct child.

---

# 31. CSS Attribute Combination

HTML:

    <input
        type="text"
        name="username"
        class="form-control">

CSS:

    input[type='text'][name='username']

Selenium:

    driver.findElement(
        By.cssSelector(
            "input[type='text'][name='username']"
        )
    );

---

# 32. XPath

XPath is another powerful locator strategy.

Example:

    driver.findElement(
        By.xpath(
            "//input[@id='username']"
        )
    );

XPath is especially useful for:

- Complex DOM structures
- Dynamic elements
- Relationships between elements
- Text-based identification
- Parent-child relationships
- Sibling relationships

---

# 33. Basic XPath Syntax

General syntax:

    //tag[@attribute='value']

Example:

    //input[@id='username']

Selenium:

    driver.findElement(
        By.xpath(
            "//input[@id='username']"
        )
    );

---

# 34. XPath Using Name

HTML:

    <input name="username">

XPath:

    //input[@name='username']

Selenium:

    driver.findElement(
        By.xpath(
            "//input[@name='username']"
        )
    );

---

# 35. XPath Using Class

HTML:

    <input class="username">

XPath:

    //input[@class='username']

However, exact class matching can be fragile when multiple classes are present.

For example:

    class="form-control username"

A better XPath may use `contains()`.

---

# 36. XPath Contains

Syntax:

    //tag[contains(@attribute,'value')]

Example:

    //input[contains(@id,'user')]

Selenium:

    driver.findElement(
        By.xpath(
            "//input[contains(@id,'user')]"
        )
    );

---

# 37. XPath Starts With

Syntax:

    //tag[starts-with(@attribute,'value')]

Example:

    //input[
        starts-with(@id,'user')
    ]

This is useful for dynamic IDs.

Example:

    user_12345
    user_67890

---

# 38. XPath Text

XPath can locate an element using visible text.

Example:

    //button[text()='Login']

Selenium:

    driver.findElement(
        By.xpath(
            "//button[text()='Login']"
        )
    ).click();

---

# 39. XPath Contains Text

Example:

    //button[
        contains(text(),'Login')
    ]

This can match:

    Login

or:

    Login Now

depending on the DOM structure.

---

# 40. XPath Normalize Space

`normalize-space()` can help when text contains extra whitespace.

Example:

    //button[
        normalize-space()='Login'
    ]

This can be useful when the HTML contains formatting whitespace.

---

# 41. XPath AND

XPath conditions can be combined using `and`.

Example:

    //input[
        @type='text'
        and @name='username'
    ]

Selenium:

    driver.findElement(
        By.xpath(
            "//input[@type='text' and @name='username']"
        )
    );

---

# 42. XPath OR

XPath conditions can be combined using `or`.

Example:

    //input[
        @id='username'
        or @name='username'
    ]

This matches an input satisfying either condition.

---

# 43. XPath Parent

Example:

    //input[@id='username']/parent::div

This finds the parent `div` of the username input.

---

# 44. XPath Child

Example:

    //div[@id='login']/child::input

This finds an input that is a child of the specified div.

---

# 45. XPath Ancestor

Example:

    //input[@id='username']
        /ancestor::form

This finds an ancestor form element.

---

# 46. XPath Descendant

Example:

    //div[@id='login']
        /descendant::input

This finds descendant input elements.

---

# 47. XPath Following Sibling

Example:

    //label[text()='Username']
        /following-sibling::input

This can locate an input that appears after a label.

---

# 48. XPath Preceding Sibling

Example:

    //input[@id='username']
        /preceding-sibling::label

This locates a label that appears before the input.

---

# 49. XPath Following

Example:

    //label[text()='Username']
        /following::input

`following::` can locate elements appearing later in the document.

---

# 50. XPath Preceding

Example:

    //input[@id='username']
        /preceding::label

This can locate elements appearing earlier in the document.

---

# 51. XPath Index

Example:

    (//input)[1]

This selects the first matching input.

Second:

    (//input)[2]

Third:

    (//input)[3]

Be careful with indexes because DOM changes can make index-based locators fragile.

---

# 52. XPath Position

Example:

    //input[position()=1]

Another example:

    //input[position()=2]

---

# 53. XPath Last

Example:

    //input[last()]

This selects the last matching input.

---

# 54. XPath Dynamic Elements

Suppose the HTML is:

    <input id="user_12345">

The number changes every execution.

Instead of:

    //input[@id='user_12345']

Use:

    //input[
        starts-with(@id,'user_')
    ]

or:

    //input[
        contains(@id,'user')
    ]

---

# 55. XPath Using Multiple Attributes

Example:

    //input[
        @type='text'
        and @placeholder='Username'
    ]

This is often more reliable than using a single unstable attribute.

---

# 56. XPath Using Class Contains

For:

    class="form-control username active"

Avoid:

    //input[@class='username']

because the complete class attribute is different.

Use:

    //input[
        contains(
            @class,
            'username'
        )
    ]

For more robust class-token matching, consider:

    //input[
        contains(
            concat(' ', normalize-space(@class), ' '),
            ' username '
        )
    ]

This avoids accidentally matching a class such as `notusername`.

---

# 57. XPath Text with Parent

Example HTML:

    <div>
        <span>Username</span>
        <input>
    </div>

XPath:

    //span[text()='Username']
        /parent::div/input

This can locate the input relative to the label.

---

# 58. Relative XPath

Relative XPath normally begins with:

    //

Example:

    //input[@id='username']

Relative XPath is generally preferred over absolute XPath.

---

# 59. Absolute XPath

Absolute XPath starts from the root HTML element.

Example:

    /html/body/div[1]/div[2]/form/input[1]

Absolute XPath is generally fragile.

Why?

Because changes to the DOM hierarchy can break the locator.

---

# 60. Relative vs Absolute XPath

| Relative XPath | Absolute XPath |
|---|---|
| Starts with `//` | Starts with `/html` |
| More maintainable | Fragile |
| Less dependent on DOM hierarchy | Highly dependent on DOM hierarchy |
| Preferred | Generally avoided |

---

# 61. ID vs XPath

Example ID:

    By.id("username")

Example XPath:

    By.xpath("//input[@id='username']")

If the ID is unique and stable, ID is usually simpler.

Prefer:

    By.id("username")

over:

    By.xpath("//input[@id='username']")

when possible.

---

# 62. CSS vs XPath

Both are powerful locator strategies.

CSS example:

    input[name='username']

XPath example:

    //input[@name='username']

CSS is often:

- Shorter
- Readable
- Fast
- Good for attributes and hierarchy

XPath is often more flexible for:

- Text
- Parent relationships
- Sibling relationships
- Ancestors
- Complex DOM relationships

---

# 63. CSS vs XPath Example

HTML:

    <button
        id="login"
        class="btn primary">
        Login
    </button>

CSS:

    #login

XPath:

    //button[@id='login']

If ID is stable, both can work.

The best locator is usually the one that is:

- Stable
- Unique
- Readable
- Maintainable

---

# 64. Locator Priority

A practical preference is:

    1. Stable unique ID
    2. Stable unique data attribute
    3. Stable name
    4. CSS Selector
    5. Relative XPath
    6. Link Text / Partial Link Text when appropriate
    7. Other strategies based on the DOM

This is not an absolute rule.

The best locator depends on the application's HTML.

---

# 65. Data Attributes

Modern applications often provide attributes specifically useful for automation.

Example:

    <button
        data-testid="login-button">
        Login
    </button>

CSS:

    By.cssSelector(
        "[data-testid='login-button']"
    );

XPath:

    By.xpath(
        "//button[@data-testid='login-button']"
    );

Stable test-specific attributes are often excellent locators.

---

# 66. Avoid Fragile Locators

Avoid locators that depend heavily on:

- Random IDs
- Generated class names
- DOM indexes
- Deep DOM hierarchy
- CSS generated by frameworks
- Frequently changing text
- Styling-only classes

Bad:

    /html/body/div[2]/div[1]/div[4]/button[2]

Better:

    //button[@data-testid='login']

---

# 67. Locator Uniqueness

A good locator should ideally identify exactly one element.

Example:

    By.id("username")

If multiple elements match:

    driver.findElements(
        By.cssSelector(".login")
    )

can return multiple elements.

You should understand whether the locator is intended to identify one element or a collection.

---

# 68. Checking Locator Count

Example:

    List<WebElement> elements =
        driver.findElements(
            By.cssSelector(".login")
        );

    System.out.println(
        "Matching elements: "
        + elements.size()
    );

If you expect exactly one element, you can assert the count in a test.

---

# 69. Dynamic XPath Example

HTML:

    <input id="user_12345">

Possible XPath:

    //input[
        starts-with(@id,'user_')
    ]

Another:

    //input[
        contains(@id,'user')
    ]

---

# 70. Dynamic CSS Example

HTML:

    <input id="user_12345">

CSS:

    input[id^='user_']

`^=` means starts with.

Another:

    input[id*='user']

`*=` means contains.

---

# 71. Locating Elements by Placeholder

HTML:

    <input
        placeholder="Enter username">

CSS:

    input[placeholder='Enter username']

XPath:

    //input[
        @placeholder='Enter username'
    ]

---

# 72. Locating Elements by Role

HTML:

    <button
        role="button">
        Login
    </button>

CSS:

    [role='button']

XPath:

    //*[@role='button']

Use semantic and stable attributes when they are reliable in the application.

---

# 73. Locating Elements by Aria Label

HTML:

    <input
        aria-label="Username">

CSS:

    input[aria-label='Username']

XPath:

    //input[
        @aria-label='Username'
    ]

Accessibility attributes can sometimes provide useful and meaningful locators.

---

# 74. Locator Reuse

Instead of repeating locators throughout the test, define them in page classes.

Example:

    private By username =
        By.id("username");

    private By password =
        By.id("password");

    private By loginButton =
        By.id("login");

Then:

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

This is commonly used with Page Object Model.

---

# 75. Page Object Model and Locators

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

        public void enterUsername(
                String value) {

            driver.findElement(username)
                  .sendKeys(value);
        }

        public void enterPassword(
                String value) {

            driver.findElement(password)
                  .sendKeys(value);
        }

        public void clickLogin() {

            driver.findElement(loginButton)
                  .click();
        }
    }

---

# 76. Locator Best Practices

1. Prefer stable locators.
2. Prefer unique locators.
3. Use IDs when they are stable and unique.
4. Use stable `data-*` attributes when available.
5. Prefer relative XPath over absolute XPath.
6. Avoid unnecessary indexes.
7. Avoid deeply nested XPath.
8. Avoid random generated attributes.
9. Avoid styling-dependent locators.
10. Keep locators inside page classes.
11. Give locator variables meaningful names.
12. Do not duplicate locators unnecessarily.
13. Use CSS selectors when they are concise and stable.
14. Use XPath when DOM relationships make it more suitable.
15. Verify that locators work reliably across repeated executions.

---

# 77. Common Locator Mistakes

## Mistake 1: Absolute XPath

Bad:

    /html/body/div[1]/div[2]/form/input[1]

Better:

    //input[@id='username']

---

## Mistake 2: Dynamic ID

Bad:

    //input[@id='user_12345']

Better:

    //input[
        starts-with(@id,'user_')
    ]

---

## Mistake 3: Multiple Classes with className

Bad:

    By.className(
        "btn primary"
    )

Better:

    By.cssSelector(
        ".btn.primary"
    )

---

## Mistake 4: Using Index Everywhere

Bad:

    (//button)[7]

Better:

    //button[@data-testid='login']

---

## Mistake 5: Long XPath

Bad:

    /html/body/div[1]/div[2]/div[3]
        /form/div[2]/div[1]/input

Better:

    //input[@name='username']

---

# 78. Locator Troubleshooting

If Selenium cannot find an element, check:

1. Is the locator correct?
2. Is the element present in the DOM?
3. Is the page fully loaded?
4. Is the element inside an iframe?
5. Are you on the correct window?
6. Is the element dynamically generated?
7. Is there a shadow DOM?
8. Is the locator unique?
9. Is there an overlay?
10. Is the element visible?
11. Is the application state correct?
12. Is the element inside a different browsing context?

---

# 79. Locator and Wait Combination

Finding an element:

    By username =
        By.id("username");

Waiting for it:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement element =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    username
                )
        );

Then interact:

    element.sendKeys("Selva");

This approach is commonly used in robust automation frameworks.

---

# 80. Common Selenium Locator Interview Questions

1. What are Selenium locators?
2. How many locator strategies does Selenium provide?
3. Which locator is fastest?
4. Which locator do you prefer?
5. ID vs XPath?
6. CSS Selector vs XPath?
7. Absolute XPath vs Relative XPath?
8. How do you handle dynamic IDs?
9. How do you write dynamic XPath?
10. What is `contains()` in XPath?
11. What is `starts-with()` in XPath?
12. What is `normalize-space()`?
13. How do you locate an element by text?
14. How do you locate a parent element?
15. How do you locate a child element?
16. How do you locate a sibling?
17. How do you locate the first element?
18. How do you locate the last element?
19. What is the difference between `findElement()` and `findElements()`?
20. How do you handle multiple matching elements?
21. What is a stable locator?
22. What is a dynamic locator?
23. Why should absolute XPath generally be avoided?
24. Why is `By.className()` not suitable for multiple class names?
25. How do you locate elements using `data-testid`?
26. How do you locate elements using `aria-label`?
27. How do you handle dynamic web elements?
28. How do you create maintainable locators in Page Object Model?
29. How do you debug a failing locator?
30. What locator strategy would you choose for a dynamic application?

---

# 81. Quick Locator Cheat Sheet

## ID

    By.id("username")

## Name

    By.name("username")

## Class

    By.className("loginButton")

## Tag

    By.tagName("input")

## Link Text

    By.linkText("Login")

## Partial Link Text

    By.partialLinkText("Log")

## CSS ID

    By.cssSelector("#username")

## CSS Class

    By.cssSelector(".loginButton")

## CSS Attribute

    By.cssSelector(
        "input[name='username']"
    )

## CSS Contains

    By.cssSelector(
        "input[id*='user']"
    )

## CSS Starts With

    By.cssSelector(
        "input[id^='user']"
    )

## CSS Ends With

    By.cssSelector(
        "input[id$='name']"
    )

## XPath Attribute

    By.xpath(
        "//input[@id='username']"
    )

## XPath Contains

    By.xpath(
        "//input[contains(@id,'user')]"
    )

## XPath Starts With

    By.xpath(
        "//input[starts-with(@id,'user')]"
    )

## XPath Text

    By.xpath(
        "//button[text()='Login']"
    )

## XPath Multiple Conditions

    By.xpath(
        "//input[@type='text' and @name='username']"
    )

---

# 82. Final Locator Strategy

When creating a Selenium locator, think in this order:

    1. Is there a stable unique ID?
             ↓
            YES
             ↓
        Use By.id()

    NO
    ↓
    Is there a stable data-* attribute?
             ↓
            YES
             ↓
        Use CSS Selector

    NO
    ↓
    Is there a stable name/class/attribute?
             ↓
            YES
             ↓
        Use CSS or By.name()

    NO
    ↓
    Do I need text or DOM relationships?
             ↓
            YES
             ↓
        Use Relative XPath

    Avoid:
        Absolute XPath
        Random IDs
        Fragile indexes
        Deep DOM paths
        Unstable CSS classes

---

# 83. Final Summary

Selenium locators are the foundation of reliable web automation.

The most important strategies are:

    ID
    Name
    Class Name
    Tag Name
    Link Text
    Partial Link Text
    CSS Selector
    XPath

For professional automation:

    Stable Locator
        +
    Unique Locator
        +
    Readable Locator
        +
    Maintainable Locator
        =
    Reliable Selenium Test

Remember:

    Good locator
        ↓
    Reliable element identification
        ↓
    Stable automation
        ↓
    Maintainable framework

For Selenium interviews, make sure you can explain:

    ID
    CSS Selector
    XPath
    Dynamic XPath
    contains()
    starts-with()
    normalize-space()
    Parent/Child
    Sibling
    Absolute vs Relative XPath
    CSS vs XPath
    findElement()
    findElements()
    Stable vs Dynamic Locators
