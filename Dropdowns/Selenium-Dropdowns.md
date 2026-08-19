# Selenium Dropdowns

## 1. Introduction

Dropdowns are common UI elements used to allow users to select one or more values from a list.

In Selenium, dropdown handling depends on the type of dropdown used by the application.

There are mainly three types:

1. Standard HTML `<select>` dropdown
2. Custom dropdown
3. Multi-select dropdown

For standard HTML `<select>` dropdowns, Selenium provides the `Select` class.

---

# 2. Import Select Class

```java
import org.openqa.selenium.support.ui.Select;
