# Selenium Basics

## 1. What is Selenium?

Selenium is an open-source automation tool used for automating web browsers.
It supports multiple languages (Java, Python, C#, JavaScript) and browsers (Chrome, Firefox, Edge, Safari).

## 2. Selenium Components

- **Selenium WebDriver** – Automates browser actions directly
- **Selenium Grid** – Runs tests on multiple machines/browsers in parallel
- **Selenium IDE** – Record-and-playback browser extension

## 3. Setup

1. Install Java/Python
2. Install an IDE (IntelliJ, VS Code, Eclipse)
3. Add Selenium dependency:

```xml

<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.21.0</version>
</dependency>
Download the browser driver (e.g., ChromeDriver) or use WebDriverManager

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class FirstTest {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
        driver.get("https://www.google.com");
        System.out.println(driver.getTitle());
        driver.quit();
    }
}
Always call driver.quit() to close the browser and release resources
driver.close() closes only the current window/tab
