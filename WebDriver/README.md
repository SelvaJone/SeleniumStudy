## `WebDriver/README.md`

```markdown
# WebDriver

## 1. What is WebDriver?

`WebDriver` is an interface in Selenium that defines methods for controlling a browser (navigation, finding elements, executing scripts).

## 2. Common WebDriver Implementations

| Browser | Driver Class |
|---|---|
| Chrome | `ChromeDriver` |
| Firefox | `FirefoxDriver` |
| Edge | `EdgeDriver` |
| Safari | `SafariDriver` |

## 3. Creating a WebDriver Instance

```java

WebDriver driver = new ChromeDriver();
WebDriver Lifecycle
Instantiate driver
Navigate to a page
Locate elements
Perform actions
Assert/validate results
Quit driver
6. Best Practices
Use WebDriverManager to avoid manually managing driver binaries
Always quit the driver in a finally block or @AfterMethod/@AfterEach

Common WebDriver Methods
Method

Description

get(String url)

Navigates to a URL

getTitle()

Returns the page title

getCurrentUrl()

Returns the current URL

getPageSource()

Returns the page's HTML source
