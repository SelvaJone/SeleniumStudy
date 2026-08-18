## `Browser/README.md`

```markdown
# Browser Handling

## 1. Navigation Commands

```java

driver.navigate().to("https://example.com");
driver.navigate().back();
driver.navigate().forward();
driver.navigate().refresh();

2. Window Management

driver.manage().window().maximize();
driver.manage().window().minimize();
driver.manage().window().fullscreen();

3. Handling Multiple Windows/Tabs
String parentWindow = driver.getWindowHandle();

for (String windowHandle : driver.getWindowHandles()) {
    driver.switchTo().window(windowHandle);
}

driver.switchTo().window(parentWindow);

4. Handling Frames

driver.switchTo().frame("frameName");
driver.switchTo().frame(0); // by index
driver.switchTo().defaultContent(); // exit frame
