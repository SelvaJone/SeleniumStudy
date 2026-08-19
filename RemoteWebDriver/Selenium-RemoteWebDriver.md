# Selenium RemoteWebDriver

## 1. What is RemoteWebDriver?

`RemoteWebDriver` is a Selenium WebDriver implementation used to execute browser automation tests on a **remote machine**.

Instead of running the browser directly on the same machine where the test code is executed, the test sends WebDriver commands to a remote Selenium server or Selenium Grid.

### Basic flow

```text
Test Code
   |
   v
RemoteWebDriver
   |
   v
Selenium Grid / Remote Selenium Server
   |
   v
Browser on Remote Machine
WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444"),
    new ChromeOptions()
);
2. Why Use RemoteWebDriver?

RemoteWebDriver is commonly used when:

Tests need to run on different machines
Tests need different browsers
Tests need different operating systems
Selenium Grid is used
CI/CD pipelines execute tests
Parallel execution is required
Browser execution needs to be distributed
Docker containers are used
Cloud Selenium providers are used
3. Local WebDriver vs RemoteWebDriver
Local WebDriver
WebDriver driver = new ChromeDriver();

The browser runs on the same machine as the test.

Test Machine
   |
   +-- Java Test
   |
   +-- ChromeDriver
   |
   +-- Chrome Browser
RemoteWebDriver
WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444"),
    new ChromeOptions()
);

The test sends commands to a remote Selenium server.

Test Machine
   |
   +-- Java Test
   |
   +-- RemoteWebDriver
          |
          v
    Selenium Grid
          |
          v
    Remote Machine
          |
          v
       Chrome
4. Import Statements
import java.net.MalformedURLException;
import java.net.URL;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
5. Basic RemoteWebDriver Example
import java.net.MalformedURLException;
import java.net.URL;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;


public class RemoteWebDriverExample {


    public static void main(String[] args) throws MalformedURLException {


        URL gridUrl = new URL("http://localhost:4444");


        ChromeOptions options = new ChromeOptions();


        WebDriver driver = new RemoteWebDriver(gridUrl, options);


        driver.get("https://www.google.com");


        System.out.println(driver.getTitle());


        driver.quit();
    }
}
6. How RemoteWebDriver Works

The following happens when this code executes:

WebDriver driver =
    new RemoteWebDriver(gridUrl, options);
Step 1

The test creates a request.

Test Code
Step 2

RemoteWebDriver sends the request to Selenium Grid.

RemoteWebDriver
       |
       v
Selenium Grid
Step 3

Grid finds a matching browser/node.

Grid
 |
 +-- Chrome
 +-- Firefox
 +-- Edge
Step 4

Grid creates a browser session.

Step 5

Commands are sent to the selected browser.

driver.get("https://example.com");
driver.findElement(...).click();
Step 6

The browser performs the requested actions.

7. Selenium Grid and RemoteWebDriver

Selenium Grid allows Selenium tests to run on remote machines.

A typical architecture is:

                 Test Machine
                      |
                      v
              RemoteWebDriver
                      |
                      v
                Selenium Grid
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
       Chrome      Firefox       Edge
        Node        Node         Node

RemoteWebDriver is the client-side mechanism used to communicate with Grid.

8. Selenium Grid 4

Selenium Grid 4 introduced a redesigned architecture.

Grid 4 can manage:

Router
Distributor
Session Queue
Session Map
Nodes
Event Bus

For most test automation projects, you do not need to manually interact with each component.

Your test generally connects to the Grid URL:

URL gridUrl = new URL("http://localhost:4444");

and creates a RemoteWebDriver.

9. Starting Selenium Grid 4

If Selenium Server is downloaded as a JAR, Grid can be started with:

java -jar selenium-server-4.x.x.jar standalone

The default Grid URL is generally:

http://localhost:4444

You can then open the Grid console in a browser.

http://localhost:4444
10. RemoteWebDriver with Chrome
ChromeOptions options = new ChromeOptions();


WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444"),
    options
);
11. RemoteWebDriver with Firefox
FirefoxOptions options = new FirefoxOptions();


WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444"),
    options
);

Imports:

import org.openqa.selenium.firefox.FirefoxOptions;
12. RemoteWebDriver with Edge
EdgeOptions options = new EdgeOptions();


WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444"),
    options
);

Import:

import org.openqa.selenium.edge.EdgeOptions;
13. Browser Selection

The browser can be selected using browser options.

Chrome
ChromeOptions options = new ChromeOptions();
Firefox
FirefoxOptions options = new FirefoxOptions();
Edge
EdgeOptions options = new EdgeOptions();

Then:

RemoteWebDriver driver =
    new RemoteWebDriver(gridUrl, options);

Grid looks for a node capable of supporting that browser.

14. ChromeOptions

Modern Selenium code should use browser-specific Options classes.

Example:

ChromeOptions options = new ChromeOptions();


options.addArguments("--headless=new");
options.addArguments("--window-size=1920,1080");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );
15. FirefoxOptions
FirefoxOptions options = new FirefoxOptions();


options.addArguments("-headless");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );
16. EdgeOptions
EdgeOptions options = new EdgeOptions();


options.addArguments("--headless=new");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );
17. DesiredCapabilities vs Options

Older Selenium code commonly used:

DesiredCapabilities capabilities =
    new DesiredCapabilities();


capabilities.setBrowserName("chrome");

Modern Selenium uses browser-specific Options:

ChromeOptions options = new ChromeOptions();

Recommended:

RemoteWebDriver driver =
    new RemoteWebDriver(gridUrl, options);
Modern approach
ChromeOptions
FirefoxOptions
EdgeOptions
SafariOptions
        |
        v
RemoteWebDriver
        |
        v
Selenium Grid
18. Using Browser Version

You can request a specific browser version using capabilities/options.

Example:

ChromeOptions options = new ChromeOptions();


options.setBrowserVersion("stable");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );

Depending on the Grid/node configuration, Grid will select a compatible node.

19. Platform Selection

You can request a platform.

Example:

ChromeOptions options = new ChromeOptions();


options.setPlatformName("Windows 11");


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );

Grid uses the requested capabilities to find a matching node.

20. Complete RemoteWebDriver Example
import java.net.MalformedURLException;
import java.net.URL;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;


public class RemoteChromeTest {


    public static void main(String[] args)
            throws MalformedURLException {


        URL gridUrl = new URL("http://localhost:4444");


        ChromeOptions options = new ChromeOptions();


        options.addArguments("--start-maximized");


        WebDriver driver =
                new RemoteWebDriver(gridUrl, options);


        try {


            driver.get("https://www.google.com");


            System.out.println(
                "Title: " + driver.getTitle()
            );


        } finally {


            driver.quit();
        }
    }
}
21. RemoteWebDriver with TestNG

RemoteWebDriver is frequently used with TestNG.

import java.net.MalformedURLException;
import java.net.URL;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;


public class RemoteTest {


    WebDriver driver;


    @BeforeMethod
    public void setup() throws MalformedURLException {


        ChromeOptions options = new ChromeOptions();


        driver = new RemoteWebDriver(
            new URL("http://localhost:4444"),
            options
        );
    }


    @Test
    public void testGoogle() {


        driver.get("https://www.google.com");


        System.out.println(driver.getTitle());
    }


    @AfterMethod
    public void tearDown() {


        if (driver != null) {
            driver.quit();
        }
    }
}
22. RemoteWebDriver with TestNG Parallel Execution

RemoteWebDriver is especially useful when combined with TestNG parallel execution.

Example:

@Test
public void testChrome() throws Exception {


    ChromeOptions options = new ChromeOptions();


    WebDriver driver =
        new RemoteWebDriver(
            new URL("http://localhost:4444"),
            options
        );


    try {


        driver.get("https://www.google.com");


    } finally {


        driver.quit();
    }
}

Multiple tests can create separate remote sessions.

Test 1 ---> Grid ---> Chrome Node 1
Test 2 ---> Grid ---> Chrome Node 2
Test 3 ---> Grid ---> Firefox Node
Test 4 ---> Grid ---> Edge Node
23. TestNG XML Parallel Example
<!DOCTYPE suite SYSTEM
    "https://testng.org/testng-1.0.dtd">


<suite name="RemoteSuite" parallel="tests" thread-count="3">


    <test name="ChromeTest">
        <classes>
            <class name="tests.ChromeTest"/>
        </classes>
    </test>


    <test name="FirefoxTest">
        <classes>
            <class name="tests.FirefoxTest"/>
        </classes>
    </test>


    <test name="EdgeTest">
        <classes>
            <class name="tests.EdgeTest"/>
        </classes>
    </test>


</suite>
24. Parameterizing Browser

A framework can dynamically select the browser.

Example:

public WebDriver createDriver(
        String browser) throws Exception {


    URL gridUrl =
        new URL("http://localhost:4444");


    if (browser.equalsIgnoreCase("chrome")) {


        return new RemoteWebDriver(
            gridUrl,
            new ChromeOptions()
        );


    } else if (browser.equalsIgnoreCase("firefox")) {


        return new RemoteWebDriver(
            gridUrl,
            new FirefoxOptions()
        );


    } else if (browser.equalsIgnoreCase("edge")) {


        return new RemoteWebDriver(
            gridUrl,
            new EdgeOptions()
        );


    } else {


        throw new IllegalArgumentException(
            "Unsupported browser: " + browser
        );
    }
}
25. Driver Factory with RemoteWebDriver

A framework commonly uses a DriverFactory.

public class DriverFactory {


    public static WebDriver createDriver(
            String browser) throws Exception {


        URL gridUrl =
            new URL("http://localhost:4444");


        switch (browser.toLowerCase()) {


            case "chrome":


                return new RemoteWebDriver(
                    gridUrl,
                    new ChromeOptions()
                );


            case "firefox":


                return new RemoteWebDriver(
                    gridUrl,
                    new FirefoxOptions()
                );


            case "edge":


                return new RemoteWebDriver(
                    gridUrl,
                    new EdgeOptions()
                );


            default:


                throw new IllegalArgumentException(
                    "Invalid browser: " + browser
                );
        }
    }
}

Usage:

WebDriver driver =
    DriverFactory.createDriver("chrome");
26. RemoteWebDriver in a Selenium Framework

A typical framework may look like:

selenium-framework
│
├── src/test/java
│   ├── tests
│   ├── pages
│   ├── utilities
│   └── factory
│
├── src/test/resources
│   ├── config.properties
│   └── testng.xml
│
└── pom.xml

Driver creation:

Test
 |
 v
DriverFactory
 |
 +-- Local WebDriver
 |
 +-- RemoteWebDriver
 |
 v
Selenium Grid
27. Configurable Local/Remote Execution

A framework can support both local and remote execution.

Example configuration:

execution=remote
browser=chrome
gridUrl=http://localhost:4444

Driver factory:

public static WebDriver createDriver(
        String execution,
        String browser,
        String gridUrl) throws Exception {


    if (execution.equalsIgnoreCase("local")) {


        if (browser.equalsIgnoreCase("chrome")) {
            return new ChromeDriver();
        }


        if (browser.equalsIgnoreCase("firefox")) {
            return new FirefoxDriver();
        }


    } else if (execution.equalsIgnoreCase("remote")) {


        URL url = new URL(gridUrl);


        if (browser.equalsIgnoreCase("chrome")) {
            return new RemoteWebDriver(
                url,
                new ChromeOptions()
            );
        }


        if (browser.equalsIgnoreCase("firefox")) {
            return new RemoteWebDriver(
                url,
                new FirefoxOptions()
            );
        }
    }


    throw new IllegalArgumentException(
        "Invalid configuration"
    );
}
28. RemoteWebDriver and Docker

Selenium Grid can also run using Docker.

Example architecture:

Test Machine
     |
     v
RemoteWebDriver
     |
     v
Selenium Grid Container
     |
     +---- Chrome Container
     |
     +---- Firefox Container
     |
     +---- Edge Container

The test still uses:

WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        new ChromeOptions()
    );

The difference is that the Selenium infrastructure runs inside containers.

29. RemoteWebDriver in CI/CD

RemoteWebDriver is very useful in CI/CD environments such as:

GitHub Actions
Jenkins
Azure DevOps
GitLab CI

Typical flow:

Developer
   |
   v
Git Push
   |
   v
CI/CD Pipeline
   |
   v
Maven
   |
   v
TestNG
   |
   v
RemoteWebDriver
   |
   v
Selenium Grid
   |
   v
Browser

Example Maven command:

mvn clean test
30. Grid URL Configuration

Do not hard-code the Grid URL throughout the framework.

Bad:

new URL("http://localhost:4444");

in many classes.

Better:

grid.url=http://localhost:4444

Then load it from configuration.

Example:

String gridUrl =
    properties.getProperty("grid.url");
31. RemoteWebDriver Session

Every RemoteWebDriver creates a browser session.

Example:

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        new ChromeOptions()
    );

Conceptually:

RemoteWebDriver
       |
       v
Create Session
       |
       v
Grid
       |
       v
Node
       |
       v
Chrome

The session remains active until:

driver.quit();
32. driver.close() vs driver.quit()

With remote execution, quit() is especially important.

close()
driver.close();

Closes the current browser window.

quit()
driver.quit();

Ends the WebDriver session and closes all associated browser windows.

Recommended:

@AfterMethod
public void tearDown() {


    if (driver != null) {
        driver.quit();
    }
}
33. Common RemoteWebDriver Exceptions
1. SessionNotCreatedException

Possible causes:

Browser version mismatch
Unsupported browser
No matching Grid node
Invalid capabilities
Grid unavailable
2. UnreachableBrowserException

Possible causes:

Browser crashed
Node unavailable
Network issue
Remote machine disconnected
3. WebDriverException

Possible causes:

Incorrect Grid URL
Selenium server not running
Network connection issue
Invalid configuration
4. TimeoutException

Possible causes:

Slow remote machine
Application response delay
Network latency
Incorrect wait configuration
34. Grid URL Connection Problem

If this fails:

new RemoteWebDriver(
    new URL("http://localhost:4444"),
    options
);

check whether Grid is running.

java -jar selenium-server.jar standalone

Also verify:

http://localhost:4444
35. RemoteWebDriver and Network Latency

Remote execution introduces network communication.

Local:

Test -> Browser

Remote:

Test
 |
 v
Network
 |
 v
Grid
 |
 v
Node
 |
 v
Browser

Therefore remote execution may be slightly slower than local execution.

Good automation practices include:

Use explicit waits
Avoid unnecessary commands
Avoid excessive screenshots
Reuse framework utilities
Run tests in parallel
Keep test data efficient
36. RemoteWebDriver Best Practices
1. Use browser Options
ChromeOptions options =
    new ChromeOptions();
2. Always quit the driver
driver.quit();
3. Keep Grid URL configurable
grid.url=http://localhost:4444
4. Use DriverFactory

Centralize driver creation.

5. Support local and remote execution
execution=local
execution=remote
6. Use TestNG parallel execution

Useful for reducing execution time.

7. Use ThreadLocal for parallel tests

Each thread should have its own driver instance.

37. RemoteWebDriver with ThreadLocal

For parallel execution, a common framework design is:

public class DriverManager {


    private static ThreadLocal<WebDriver> driver =
            new ThreadLocal<>();


    public static void setDriver(WebDriver webDriver) {
        driver.set(webDriver);
    }


    public static WebDriver getDriver() {
        return driver.get();
    }


    public static void unload() {


        driver.remove();
    }
}

Setup:

@BeforeMethod
public void setup() throws Exception {


    WebDriver webDriver =
        new RemoteWebDriver(
            new URL("http://localhost:4444"),
            new ChromeOptions()
        );


    DriverManager.setDriver(webDriver);
}

Usage:

DriverManager.getDriver()
    .get("https://example.com");

Cleanup:

@AfterMethod
public void tearDown() {


    if (DriverManager.getDriver() != null) {


        DriverManager.getDriver().quit();


        DriverManager.unload();
    }
}
38. Why ThreadLocal Is Important

Without ThreadLocal:

Thread 1 ---> Driver
Thread 2 ---> Same Driver
Thread 3 ---> Same Driver

This can cause:

Test interference
Wrong browser actions
Race conditions
Incorrect screenshots
Session conflicts

With ThreadLocal:

Thread 1 ---> Driver 1 ---> Browser 1
Thread 2 ---> Driver 2 ---> Browser 2
Thread 3 ---> Driver 3 ---> Browser 3

This is a common pattern for Selenium parallel frameworks.

39. RemoteWebDriver vs Selenium Grid

These are not the same thing.

RemoteWebDriver

A Selenium WebDriver implementation used by the test to communicate with a remote browser.

Selenium Grid

Infrastructure that manages remote browser execution.

RemoteWebDriver
      |
      v
Selenium Grid
      |
      v
Remote Browser
40. RemoteWebDriver vs WebDriver

WebDriver is an interface:

WebDriver driver;

RemoteWebDriver is a concrete implementation:

RemoteWebDriver driver;

Usually framework code should depend on the interface:

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );

This makes the framework more flexible.

41. RemoteWebDriver and Page Object Model

RemoteWebDriver works normally with Page Object Model.

Example:

public class LoginPage {


    private WebDriver driver;


    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }


    public void enterUsername(String username) {


        driver.findElement(
            By.id("username")
        ).sendKeys(username);
    }
}

Test:

LoginPage loginPage =
    new LoginPage(driver);


loginPage.enterUsername("testuser");

The page object does not need to know whether the driver is:

ChromeDriver
FirefoxDriver
EdgeDriver
RemoteWebDriver

It only needs:

WebDriver
42. RemoteWebDriver and PageFactory

PageFactory can also be used:

public class LoginPage {


    WebDriver driver;


    @FindBy(id = "username")
    WebElement username;


    public LoginPage(WebDriver driver) {


        this.driver = driver;


        PageFactory.initElements(
            driver,
            this
        );
    }


    public void enterUsername(String value) {


        username.sendKeys(value);
    }
}

RemoteWebDriver works with this approach because it implements the WebDriver interface.

43. Remote Screenshots

RemoteWebDriver supports screenshots.

TakesScreenshot screenshot =
    (TakesScreenshot) driver;


File source =
    screenshot.getScreenshotAs(
        OutputType.FILE
    );

Import:

import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.OutputType;

In a remote environment, screenshot handling should be integrated into the framework's reporting mechanism.

44. RemoteWebDriver Capabilities Example
ChromeOptions options =
    new ChromeOptions();


options.setBrowserVersion("stable");


options.setPlatformName("Windows 11");


options.addArguments(
    "--window-size=1920,1080"
);


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );

The requested capabilities help Grid determine which node should receive the session.

45. Complete DriverFactory Example
import java.net.URL;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;


public class DriverFactory {


    public static WebDriver createRemoteDriver(
            String browser,
            String gridUrl) throws Exception {


        URL url = new URL(gridUrl);


        switch (browser.toLowerCase()) {


            case "chrome":


                return new RemoteWebDriver(
                    url,
                    new ChromeOptions()
                );


            case "firefox":


                return new RemoteWebDriver(
                    url,
                    new FirefoxOptions()
                );


            case "edge":


                return new RemoteWebDriver(
                    url,
                    new EdgeOptions()
                );


            default:


                throw new IllegalArgumentException(
                    "Unsupported browser: " + browser
                );
        }
    }
}

Usage:

WebDriver driver =
    DriverFactory.createRemoteDriver(
        "chrome",
        "http://localhost:4444"
    );
46. RemoteWebDriver Framework Flow

A professional Selenium framework can follow:

TestNG
  |
  v
BaseTest
  |
  v
DriverFactory
  |
  v
RemoteWebDriver
  |
  v
Selenium Grid
  |
  +----------------+
  |                |
  v                v
Chrome            Firefox
Node              Node
  |                |
  v                v
Browser           Browser

With parallel execution:

                 Selenium Grid
                      |
        +-------------+-------------+
        |             |             |
        v             v             v
     Chrome        Chrome        Firefox
     Session       Session       Session
        |             |             |
     Thread 1      Thread 2      Thread 3
47. Interview Questions
Beginner
1. What is RemoteWebDriver?

RemoteWebDriver allows Selenium tests to execute browser commands on a remote machine or Selenium Grid.

2. Why do we use RemoteWebDriver?

To execute tests remotely across different browsers, machines, operating systems, containers, or cloud environments.

3. What is the difference between ChromeDriver and RemoteWebDriver?

ChromeDriver directly controls Chrome locally.

RemoteWebDriver sends commands to a remote Selenium server/Grid.

4. What is Selenium Grid?

Selenium Grid is a distributed execution infrastructure for running Selenium tests across different browsers and machines.

5. What is the default Selenium Grid URL?

Typically:

http://localhost:4444
48. Intermediate Interview Questions
6. How do you create a RemoteWebDriver?
WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        new ChromeOptions()
    );
7. What is the role of ChromeOptions?

It defines Chrome-specific capabilities and configuration requested for the remote session.

8. Can RemoteWebDriver work without Selenium Grid?

Yes.

It can communicate with a compatible remote Selenium server or another remote WebDriver endpoint. Grid is one common infrastructure for remote execution.

9. Why should we use WebDriver as the variable type?

Because it allows the framework to work with different WebDriver implementations.

WebDriver driver =
    new RemoteWebDriver(...);
10. What happens when driver.quit() is called?

The WebDriver session is terminated and the associated browser windows are closed.

49. Advanced Interview Questions
11. How does RemoteWebDriver select a Grid node?

Grid uses the requested session capabilities to find a compatible node.

12. How do you run Chrome and Firefox tests in parallel?

Use separate RemoteWebDriver sessions and configure TestNG parallel execution.

13. Why is ThreadLocal useful with RemoteWebDriver?

It provides each parallel test thread with its own WebDriver instance.

14. How do you configure the Grid URL?

Prefer external configuration:

grid.url=http://localhost:4444

rather than hard-coding it throughout the framework.

15. What causes SessionNotCreatedException?

Common causes include:

No matching node
Unsupported browser
Invalid capabilities
Browser version mismatch
Grid/node configuration problems
16. How would you design a framework supporting both local and remote execution?

Use a DriverFactory:

Configuration
     |
     v
DriverFactory
     |
 +---+---+
 |       |
Local   Remote
 |       |
Driver  Grid
17. How does RemoteWebDriver work in CI/CD?

The CI server runs the test suite and connects to a Selenium Grid or remote browser environment.

Jenkins
   |
   v
Maven
   |
   v
TestNG
   |
   v
RemoteWebDriver
   |
   v
Selenium Grid
18. How would you troubleshoot a remote test that cannot create a session?

Check:

Grid is running
Grid URL is correct
Node is registered
Requested browser is available
Browser version is compatible
Capabilities are valid
Network connectivity is available
Grid/node logs for errors
50. Key Takeaways
RemoteWebDriver
       |
       +-- Remote browser execution
       |
       +-- Selenium Grid
       |
       +-- Cross-browser testing
       |
       +-- Distributed testing
       |
       +-- Parallel execution
       |
       +-- CI/CD
       |
       +-- Docker
       |
       +-- Cloud Selenium providers
Most important code
ChromeOptions options =
    new ChromeOptions();


WebDriver driver =
    new RemoteWebDriver(
        new URL("http://localhost:4444"),
        options
    );
Remember
WebDriver
   ↓
RemoteWebDriver
   ↓
Selenium Grid
   ↓
Remote Node
   ↓
Browser

RemoteWebDriver is the key Selenium component that allows your test code to communicate with a browser running outside the test machine.


