# Selenium Cookies

## 1. Introduction

A cookie is a small piece of data stored by the browser for a website.

Web applications commonly use cookies for:

* Session management
* Authentication
* User preferences
* Tracking
* Shopping carts
* Remembering login state
* Personalization

Selenium WebDriver provides methods to create, read, and delete browser cookies.

---

# 2. Selenium Cookie Class

Selenium provides the:

```java
org.openqa.selenium.Cookie
```

class for working with cookies.

Import:

```java
import org.openqa.selenium.Cookie;
```

---

# 3. Get Cookies

To retrieve all cookies:

```java
Set<Cookie> cookies = driver.manage().getCookies();
```

Import:

```java
import java.util.Set;
```

Example:

```java
Set<Cookie> cookies =
        driver.manage().getCookies();

for (Cookie cookie : cookies) {
    System.out.println(cookie);
}
```

---

# 4. Print Cookie Information

You can retrieve individual cookie properties.

```java
Set<Cookie> cookies =
        driver.manage().getCookies();

for (Cookie cookie : cookies) {

    System.out.println("Name: " + cookie.getName());
    System.out.println("Value: " + cookie.getValue());
    System.out.println("Domain: " + cookie.getDomain());
    System.out.println("Path: " + cookie.getPath());
    System.out.println("Expiry: " + cookie.getExpiry());
    System.out.println("Secure: " + cookie.isSecure());
    System.out.println("HttpOnly: " + cookie.isHttpOnly());

    System.out.println("----------------------");
}
```

---

# 5. Get Cookie By Name

Use:

```java
getCookieNamed()
```

Example:

```java
Cookie cookie =
        driver.manage().getCookieNamed("sessionId");
```

Print the cookie:

```java
System.out.println(cookie);
```

---

# 6. Check Whether Cookie Exists

```java
Cookie cookie =
        driver.manage().getCookieNamed("sessionId");

if (cookie != null) {
    System.out.println("Cookie exists");
} else {
    System.out.println("Cookie does not exist");
}
```

---

# 7. Get Cookie Name

```java
String name = cookie.getName();

System.out.println(name);
```

---

# 8. Get Cookie Value

```java
String value = cookie.getValue();

System.out.println(value);
```

---

# 9. Get Cookie Domain

```java
String domain = cookie.getDomain();

System.out.println(domain);
```

---

# 10. Get Cookie Path

```java
String path = cookie.getPath();

System.out.println(path);
```

---

# 11. Get Cookie Expiry

```java
Date expiry = cookie.getExpiry();

System.out.println(expiry);
```

Import:

```java
import java.util.Date;
```

Modern Selenium versions may expose expiry using:

```java
Optional<Instant>
```

depending on the Selenium API version.

---

# 12. Check Secure Cookie

```java
boolean secure = cookie.isSecure();

System.out.println(secure);
```

A secure cookie is intended to be transmitted over HTTPS.

---

# 13. Check HttpOnly Cookie

```java
boolean httpOnly = cookie.isHttpOnly();

System.out.println(httpOnly);
```

`HttpOnly` cookies are generally inaccessible to client-side JavaScript.

They are commonly used to reduce exposure to certain client-side attacks.

---

# 14. Add Cookie

To add a cookie:

```java
Cookie cookie =
        new Cookie("username", "Selva");

driver.manage().addCookie(cookie);
```

---

# 15. Complete Add Cookie Example

```java
import org.openqa.selenium.Cookie;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class AddCookieExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        driver.get("https://example.com");

        Cookie cookie =
                new Cookie("username", "Selva");

        driver.manage().addCookie(cookie);

        driver.quit();
    }
}
```

---

# 16. Important: Navigate Before Adding Cookie

Usually, you must first navigate to the target domain.

Correct:

```java
driver.get("https://example.com");

Cookie cookie =
        new Cookie("username", "Selva");

driver.manage().addCookie(cookie);
```

Then refresh:

```java
driver.navigate().refresh();
```

Incorrect:

```java
Cookie cookie =
        new Cookie("username", "Selva");

driver.manage().addCookie(cookie);
```

before the browser has navigated to an appropriate domain can result in an invalid cookie domain error.

---

# 17. Add Cookie With Domain

```java
Cookie cookie =
        new Cookie(
            "username",
            "Selva",
            "example.com",
            "/",
            null
        );

driver.manage().addCookie(cookie);
```

The exact constructor signatures available can vary with Selenium versions.

---

# 18. Cookie Constructor

A cookie can contain:

```text
Name
Value
Domain
Path
Expiry
Secure
HttpOnly
SameSite
```

Example conceptually:

```java
Cookie cookie =
        new Cookie.Builder(
            "username",
            "Selva"
        )
        .domain("example.com")
        .path("/")
        .isSecure(true)
        .build();
```

---

# 19. Cookie Builder

The `Cookie.Builder` approach is useful when you want to specify additional cookie properties.

Example:

```java
Cookie cookie =
        new Cookie.Builder(
            "username",
            "Selva"
        )
        .domain("example.com")
        .path("/")
        .isSecure(true)
        .build();

driver.manage().addCookie(cookie);
```

---

# 20. Cookie With Expiry

Example:

```java
Calendar calendar =
        Calendar.getInstance();

calendar.add(
    Calendar.DAY_OF_MONTH,
    7
);

Cookie cookie =
        new Cookie.Builder(
            "testCookie",
            "testValue"
        )
        .expiresOn(calendar.getTime())
        .build();

driver.manage().addCookie(cookie);
```

Import:

```java
import java.util.Calendar;
```

---

# 21. Cookie With Secure Flag

```java
Cookie cookie =
        new Cookie.Builder(
            "secureCookie",
            "secureValue"
        )
        .isSecure(true)
        .build();

driver.manage().addCookie(cookie);
```

This is appropriate for HTTPS contexts.

---

# 22. Delete Cookie By Name

Use:

```java
deleteCookieNamed()
```

Example:

```java
driver.manage().deleteCookieNamed("username");
```

---

# 23. Delete Cookie Object

First retrieve the cookie:

```java
Cookie cookie =
        driver.manage().getCookieNamed("username");
```

Then:

```java
if (cookie != null) {
    driver.manage().deleteCookie(cookie);
}
```

---

# 24. Delete All Cookies

To remove all cookies:

```java
driver.manage().deleteAllCookies();
```

This is commonly used during test cleanup.

---

# 25. Verify Cookie After Deletion

```java
driver.manage().deleteCookieNamed("username");

Cookie cookie =
        driver.manage().getCookieNamed("username");

if (cookie == null) {
    System.out.println("Cookie deleted successfully");
}
```

---

# 26. Count Cookies

```java
Set<Cookie> cookies =
        driver.manage().getCookies();

System.out.println(
    "Cookie count: " + cookies.size()
);
```

---

# 27. Check Cookie Using Loop

```java
Set<Cookie> cookies =
        driver.manage().getCookies();

boolean found = false;

for (Cookie cookie : cookies) {

    if (cookie.getName().equals("sessionId")) {
        found = true;
        break;
    }
}

System.out.println("Cookie found: " + found);
```

---

# 28. Cookie Authentication

Cookies can sometimes be used to establish an authenticated browser state.

Typical flow:

```text
Login
   ↓
Application creates authentication cookie
   ↓
Cookie is stored in browser
   ↓
Cookie is reused
   ↓
Application recognizes session
```

Example:

```java
driver.get("https://example.com");

Cookie authCookie =
        new Cookie(
            "auth",
            "test-authentication-value"
        );

driver.manage().addCookie(authCookie);

driver.navigate().refresh();
```

The actual cookie name and value depend on the application.

---

# 29. Cookie-Based Login Test

A test may follow this pattern:

```java
@Test
public void cookieLoginTest() {

    driver.get("https://example.com");

    Cookie authCookie =
        new Cookie(
            "auth",
            "authentication-value"
        );

    driver.manage().addCookie(authCookie);

    driver.navigate().refresh();

    // Verify authenticated page
}
```

Do not hardcode real production authentication tokens.

Use test credentials or test environment values.

---

# 30. Save Cookies

Selenium can retrieve cookies, but it does not automatically provide a complete "save cookies to file" workflow.

You can serialize the cookie properties yourself.

Example:

```java
Set<Cookie> cookies =
        driver.manage().getCookies();

for (Cookie cookie : cookies) {

    System.out.println(
        cookie.getName()
        + "="
        + cookie.getValue()
    );
}
```

You can then store the required information in a file or test data source.

---

# 31. Reuse Cookies

A common test approach is:

```text
Test 1
  ↓
Login
  ↓
Get cookies
  ↓
Store cookies
  ↓
Test 2
  ↓
Open domain
  ↓
Add cookies
  ↓
Refresh
  ↓
Continue authenticated testing
```

Example:

```java
driver.get("https://example.com");

for (Cookie cookie : savedCookies) {
    driver.manage().addCookie(cookie);
}

driver.navigate().refresh();
```

---

# 32. Cookie-Based Session Reuse

This can reduce repeated login operations in large test suites.

Example:

```java
public void addCookies(
        WebDriver driver,
        Set<Cookie> cookies) {

    driver.get("https://example.com");

    for (Cookie cookie : cookies) {
        driver.manage().addCookie(cookie);
    }

    driver.navigate().refresh();
}
```

---

# 33. Cookie Utility Class

A reusable cookie utility can be created.

```java
import java.util.Set;

import org.openqa.selenium.Cookie;
import org.openqa.selenium.WebDriver;

public class CookieUtils {

    public static void addCookie(
            WebDriver driver,
            String name,
            String value) {

        Cookie cookie =
                new Cookie(name, value);

        driver.manage().addCookie(cookie);
    }

    public static Cookie getCookie(
            WebDriver driver,
            String name) {

        return driver.manage()
                     .getCookieNamed(name);
    }

    public static Set<Cookie> getAllCookies(
            WebDriver driver) {

        return driver.manage()
                     .getCookies();
    }

    public static void deleteCookie(
            WebDriver driver,
            String name) {

        driver.manage()
             .deleteCookieNamed(name);
    }

    public static void deleteAllCookies(
            WebDriver driver) {

        driver.manage()
             .deleteAllCookies();
    }
}
```

---

# 34. Using Cookie Utility

Add cookie:

```java
CookieUtils.addCookie(
    driver,
    "username",
    "Selva"
);
```

Get cookie:

```java
Cookie cookie =
    CookieUtils.getCookie(
        driver,
        "username"
    );
```

Get all cookies:

```java
Set<Cookie> cookies =
    CookieUtils.getAllCookies(driver);
```

Delete cookie:

```java
CookieUtils.deleteCookie(
    driver,
    "username"
);
```

Delete all:

```java
CookieUtils.deleteAllCookies(driver);
```

---

# 35. Cookie Validation With TestNG

Example:

```java
@Test
public void verifyCookie() {

    driver.get("https://example.com");

    Cookie cookie =
        new Cookie(
            "username",
            "Selva"
        );

    driver.manage().addCookie(cookie);

    Cookie actual =
        driver.manage()
              .getCookieNamed("username");

    Assert.assertNotNull(actual);

    Assert.assertEquals(
        actual.getValue(),
        "Selva"
    );
}
```

Import:

```java
import org.testng.Assert;
import org.testng.annotations.Test;
```

---

# 36. Verify Cookie Name and Value

```java
Cookie cookie =
    driver.manage()
          .getCookieNamed("username");

Assert.assertEquals(
    cookie.getName(),
    "username"
);

Assert.assertEquals(
    cookie.getValue(),
    "Selva"
);
```

---

# 37. Verify Domain

```java
Assert.assertEquals(
    cookie.getDomain(),
    "example.com"
);
```

Be aware that the exact domain representation depends on how the cookie was created and returned by the browser.

---

# 38. Verify Path

```java
Assert.assertEquals(
    cookie.getPath(),
    "/"
);
```

---

# 39. Verify Secure Flag

```java
Assert.assertTrue(
    cookie.isSecure()
);
```

---

# 40. Verify HttpOnly Flag

```java
Assert.assertTrue(
    cookie.isHttpOnly()
);
```

---

# 41. Cookie Attributes

Important cookie attributes include:

| Attribute | Purpose                             |
| --------- | ----------------------------------- |
| Name      | Identifies the cookie               |
| Value     | Stores the cookie data              |
| Domain    | Defines the domain                  |
| Path      | Defines where the cookie applies    |
| Expiry    | Defines when it expires             |
| Secure    | Sends cookie over HTTPS             |
| HttpOnly  | Restricts JavaScript access         |
| SameSite  | Controls cross-site cookie behavior |

---

# 42. SameSite

Modern browsers support the `SameSite` attribute.

Common values:

```text
Strict
Lax
None
```

### Strict

Cookie is restricted more strongly to same-site requests.

### Lax

Allows some cross-site navigation scenarios.

### None

Allows cross-site cookie usage, generally requiring:

```text
Secure=true
```

Selenium's support for specific cookie attributes depends on the Selenium/browser version.

---

# 43. Session Cookie vs Persistent Cookie

## Session Cookie

A session cookie generally exists only for the browser session.

It may not have a persistent expiry date.

## Persistent Cookie

A persistent cookie has an expiry time.

Example:

```java
Cookie cookie =
    new Cookie.Builder(
        "test",
        "value"
    )
    .expiresOn(expiryDate)
    .build();
```

---

# 44. Cookie Domain

A cookie can be associated with a domain.

Example:

```text
example.com
```

The browser uses the domain rules to determine when the cookie should be sent.

You generally need to be on the appropriate domain before adding a cookie.

---

# 45. Cookie Path

A cookie can have a path:

```text
/
```

or a more specific path:

```text
/account
```

Example:

```java
Cookie cookie =
    new Cookie.Builder(
        "user",
        "Selva"
    )
    .path("/")
    .build();
```

---

# 46. Cookie Expiration

Example:

```java
Calendar calendar =
        Calendar.getInstance();

calendar.add(
    Calendar.DAY_OF_MONTH,
    7
);

Cookie cookie =
    new Cookie.Builder(
        "testCookie",
        "testValue"
    )
    .expiresOn(calendar.getTime())
    .build();
```

The cookie expires after the specified date/time.

---

# 47. Cookies and Browser Sessions

Cookies are associated with the browser session/profile.

When a new WebDriver instance starts, it generally starts with a new browser session/profile unless a persistent browser profile is deliberately configured.

Therefore:

```java
WebDriver driver1 =
    new ChromeDriver();
```

and:

```java
WebDriver driver2 =
    new ChromeDriver();
```

should not be assumed to share cookies.

---

# 48. Clear Cookies Before Test

A common cleanup approach:

```java
@BeforeMethod
public void setup() {

    driver.get("https://example.com");

    driver.manage().deleteAllCookies();
}
```

This helps prevent one test's browser state from affecting another test.

---

# 49. Delete Cookies After Test

```java
@AfterMethod
public void cleanup() {

    driver.manage().deleteAllCookies();

    driver.quit();
}
```

---

# 50. Cookies With TestNG

Example:

```java
public class CookieTest {

    WebDriver driver;

    @BeforeMethod
    public void setup() {

        driver = new ChromeDriver();

        driver.get("https://example.com");
    }

    @Test
    public void cookieTest() {

        Cookie cookie =
            new Cookie(
                "username",
                "Selva"
            );

        driver.manage().addCookie(cookie);

        Cookie actual =
            driver.manage()
                  .getCookieNamed("username");

        Assert.assertNotNull(actual);

        Assert.assertEquals(
            actual.getValue(),
            "Selva"
        );
    }

    @AfterMethod
    public void tearDown() {

        driver.manage()
              .deleteAllCookies();

        driver.quit();
    }
}
```

---

# 51. Cookies With Page Object Model

Cookies are generally browser/session-level functionality, so they are often better handled in:

* Driver utilities
* Base test
* Authentication utilities
* Test setup/teardown

rather than placing cookie management directly in every page object.

Example utility:

```java
public class SessionManager {

    private WebDriver driver;

    public SessionManager(WebDriver driver) {
        this.driver = driver;
    }

    public void addAuthCookie(
            String name,
            String value) {

        driver.manage().addCookie(
            new Cookie(name, value)
        );
    }

    public void refresh() {
        driver.navigate().refresh();
    }
}
```

---

# 52. Cookie Authentication Flow

A framework can use:

```text
Start Browser
      ↓
Open Application Domain
      ↓
Check Authentication Cookie
      ↓
Cookie Exists?
    /       \
  Yes        No
   ↓          ↓
Refresh      Login
   ↓          ↓
Authenticated ←
```

Example:

```java
Cookie auth =
    driver.manage()
          .getCookieNamed("auth");

if (auth != null) {

    driver.navigate().refresh();

} else {

    // Perform login
}
```

---

# 53. Cookie vs Local Storage vs Session Storage

Cookies are not the same as browser storage.

| Feature                 | Cookies                      | Local Storage       | Session Storage     |
| ----------------------- | ---------------------------- | ------------------- | ------------------- |
| Storage                 | Browser cookie store         | Browser storage     | Browser storage     |
| Expiration              | Can expire                   | Persistent          | Session-based       |
| Sent with HTTP requests | Yes, subject to cookie rules | No                  | No                  |
| JavaScript access       | Usually yes unless HttpOnly  | Yes                 | Yes                 |
| Selenium API            | `manage()`                   | JavaScript/Web APIs | JavaScript/Web APIs |

---

# 54. Local Storage Example

Although Local Storage is different from cookies, JavaScriptExecutor can access it.

```java
JavascriptExecutor js =
    (JavascriptExecutor) driver;

js.executeScript(
    "localStorage.setItem('username','Selva');"
);
```

Get value:

```java
String value =
    (String) js.executeScript(
        "return localStorage.getItem('username');"
    );
```

---

# 55. Session Storage Example

```java
js.executeScript(
    "sessionStorage.setItem('username','Selva');"
);
```

Get value:

```java
String value =
    (String) js.executeScript(
        "return sessionStorage.getItem('username');"
    );
```

Cookies, Local Storage, and Session Storage should be handled according to how the application actually stores its state.

---

# 56. Common Cookie Exceptions

## InvalidCookieDomainException

This can occur when attempting to add a cookie for a domain that does not match the current page.

Example problem:

```java
driver.get("https://example.com");

Cookie cookie =
    new Cookie.Builder(
        "test",
        "value"
    )
    .domain("different-domain.com")
    .build();

driver.manage().addCookie(cookie);
```

Solution:

* Navigate to the correct domain.
* Ensure the cookie domain matches the current site.

---

# 57. InvalidArgumentException

Can occur when invalid cookie data or unsupported attributes are supplied.

Check:

* Cookie name
* Cookie value
* Domain
* Path
* Expiry
* Browser compatibility

---

# 58. NoSuchCookieException

If a cookie is expected but does not exist, operations involving that cookie may fail.

Instead of assuming it exists:

```java
Cookie cookie =
    driver.manage()
          .getCookieNamed("sessionId");

if (cookie != null) {
    System.out.println("Cookie exists");
}
```

---

# 59. Best Practices

## 1. Navigate to the domain first

```java
driver.get("https://example.com");
```

Then add the cookie.

---

## 2. Do not hardcode real authentication tokens

Never commit real session tokens or production authentication cookies to Git.

---

## 3. Use test environment values

Use:

```text
Test environment
Test users
Test tokens
```

instead of production authentication data.

---

## 4. Clean browser state

Use:

```java
driver.manage().deleteAllCookies();
```

when test isolation requires it.

---

## 5. Do not assume cookies equal authentication

Modern applications may use:

* Cookies
* Local Storage
* Session Storage
* Tokens
* Server-side sessions
* Multiple authentication mechanisms

Understand the application's authentication design first.

---

## 6. Prefer application-level authentication when appropriate

Cookie injection can speed up tests, but it should not replace end-to-end login tests completely.

Maintain separate tests for:

```text
Login functionality
Session/authentication behavior
Authenticated application functionality
```

---

# 60. Complete Cookie Example

```java
import java.util.Set;

import org.openqa.selenium.Cookie;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class CookieExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        driver.get("https://example.com");

        // Add cookie
        Cookie cookie =
                new Cookie(
                    "username",
                    "Selva"
                );

        driver.manage().addCookie(cookie);

        // Get cookie
        Cookie actual =
                driver.manage()
                      .getCookieNamed("username");

        System.out.println(
            "Cookie Name: "
            + actual.getName()
        );

        System.out.println(
            "Cookie Value: "
            + actual.getValue()
        );

        // Get all cookies
        Set<Cookie> cookies =
                driver.manage().getCookies();

        for (Cookie c : cookies) {
            System.out.println(c);
        }

        // Refresh browser
        driver.navigate().refresh();

        // Delete cookie
        driver.manage()
              .deleteCookieNamed("username");

        // Delete all cookies
        driver.manage()
              .deleteAllCookies();

        driver.quit();
    }
}
```

---

# 61. Most Important Cookie Methods

```text
driver.manage().getCookies()
```

Returns all cookies.

```text
driver.manage().getCookieNamed("name")
```

Returns a specific cookie.

```text
driver.manage().addCookie(cookie)
```

Adds a cookie.

```text
driver.manage().deleteCookie(cookie)
```

Deletes a specific cookie.

```text
driver.manage().deleteCookieNamed("name")
```

Deletes a cookie by name.

```text
driver.manage().deleteAllCookies()
```

Deletes all cookies.

---

# 62. Quick Revision

```text
WebDriver
   |
   +-- manage()
          |
          +-- getCookies()
          |
          +-- getCookieNamed()
          |
          +-- addCookie()
          |
          +-- deleteCookie()
          |
          +-- deleteCookieNamed()
          |
          +-- deleteAllCookies()
```

Cookie object:

```text
Cookie
  |
  +-- Name
  +-- Value
  +-- Domain
  +-- Path
  +-- Expiry
  +-- Secure
  +-- HttpOnly
  +-- SameSite
```

---

# 63. Interview Questions

## Q1. How do you get all cookies in Selenium?

```java
Set<Cookie> cookies =
    driver.manage().getCookies();
```

---

## Q2. How do you get a cookie by name?

```java
Cookie cookie =
    driver.manage()
          .getCookieNamed("sessionId");
```

---

## Q3. How do you add a cookie?

```java
Cookie cookie =
    new Cookie("username", "Selva");

driver.manage().addCookie(cookie);
```

---

## Q4. How do you delete a cookie?

```java
driver.manage()
      .deleteCookieNamed("username");
```

---

## Q5. How do you delete all cookies?

```java
driver.manage()
      .deleteAllCookies();
```

---

## Q6. Why should you navigate to a domain before adding a cookie?

Because the browser needs the current page/domain context to validate whether the cookie can be added.

---

## Q7. What is the difference between getCookies() and getCookieNamed()?

`getCookies()` returns all available cookies.

```java
driver.manage().getCookies();
```

`getCookieNamed()` returns one specific cookie.

```java
driver.manage().getCookieNamed("sessionId");
```

---

## Q8. How can cookies help with test execution speed?

Cookies can sometimes be reused to establish an already-authenticated session instead of performing the UI login for every test.

---

## Q9. Can cookies be used to bypass login?

They can sometimes restore an already authenticated session, depending on the application's authentication architecture.

However, authentication cookies should be treated as sensitive credentials and should never be hardcoded or committed to source control.

---

## Q10. What is HttpOnly?

`HttpOnly` is a cookie attribute that prevents normal client-side JavaScript from reading the cookie.

It is commonly used for sensitive session cookies.

---

## Q11. What is Secure?

A cookie marked `Secure` is intended to be transmitted only over secure HTTPS connections.

---

## Q12. What is SameSite?

`SameSite` controls how cookies are sent in cross-site contexts.

Common values are:

```text
Strict
Lax
None
```

---

## Q13. What is the difference between session and persistent cookies?

A session cookie generally lasts for the browser session.

A persistent cookie has an expiry time and can remain available beyond the current session.

---

## Q14. Can Selenium directly access HttpOnly cookies?

Yes.

Selenium's WebDriver cookie API can retrieve cookies that are marked `HttpOnly`, whereas normal page JavaScript cannot access them.

---

## Q15. What exception can occur when adding a cookie for the wrong domain?

Typically:

```text
InvalidCookieDomainException
```

---

# 64. Key Takeaways

* Selenium manages cookies through `driver.manage()`.
* Use the `Cookie` class to create and inspect cookies.
* `getCookies()` returns all cookies.
* `getCookieNamed()` returns a specific cookie.
* `addCookie()` adds a cookie.
* `deleteCookie()` deletes a cookie object.
* `deleteCookieNamed()` deletes a cookie by name.
* `deleteAllCookies()` removes all cookies.
* Navigate to the correct domain before adding cookies.
* Cookies can sometimes be used to reuse authenticated sessions.
* Never store real authentication tokens in source code.
* `HttpOnly`, `Secure`, and `SameSite` are important cookie attributes.
* Cookies are different from Local Storage and Session Storage.
* Cookie management is useful for test setup, session management, and test isolation.
* Do not rely exclusively on cookie injection for authentication testing.
