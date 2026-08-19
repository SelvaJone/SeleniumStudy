# Selenium File Upload and Download

## 1. Introduction

File upload and download are common requirements in web applications.

Selenium can handle file uploads directly when the application uses:

```html
<input type="file">
File downloads are usually handled by configuring browser preferences and then validating the downloaded file using Java.

This topic covers:

File upload
sendKeys()
Absolute file paths
Relative file paths
Path and Paths
Multiple file upload
File download
Chrome download preferences
Download directory configuration
Download validation
Waiting for downloads
File utilities
TestNG examples
Common exceptions
Interview questions
2. File Upload

The easiest way to upload a file in Selenium is:

WebElement upload =
    driver.findElement(By.id("fileUpload"));


upload.sendKeys(
    "C:\\Users\\Selva\\Documents\\test.pdf"
);

Selenium sends the file path directly to the file input.

3. Important: Use sendKeys() for File Upload

For a normal HTML file input:

<input type="file" id="fileUpload">

use:

driver.findElement(
    By.id("fileUpload")
).sendKeys(
    "C:\\Files\\test.pdf"
);

You generally do not need:

Robot
Clipboard automation
OS-level file chooser automation

when a normal <input type="file"> is available.

4. Complete File Upload Example
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;


public class FileUploadExample {


    public static void main(String[] args) {


        WebDriver driver =
            new ChromeDriver();


        driver.get(
            "https://example.com/upload"
        );


        driver.findElement(
            By.id("fileUpload")
        ).sendKeys(
            "C:\\Files\\test.pdf"
        );


        driver.quit();
    }
}
5. Windows File Path

On Windows:

C:\Users\Selva\Documents\test.pdf

In Java, backslash is an escape character.

Therefore:

"C:\\Users\\Selva\\Documents\\test.pdf"

is required.

6. Forward Slash on Windows

Java also supports forward slashes in many file APIs:

"C:/Users/Selva/Documents/test.pdf"

This is often easier to read.

Example:

driver.findElement(
    By.id("fileUpload")
).sendKeys(
    "C:/Files/test.pdf"
);
7. Linux/Mac File Path

Linux:

/home/selva/files/test.pdf

Mac:

/Users/Selva/Documents/test.pdf

Example:

driver.findElement(
    By.id("fileUpload")
).sendKeys(
    "/home/selva/files/test.pdf"
);
8. Use System.getProperty("user.dir")

Hardcoding absolute paths is not recommended for automation frameworks.

Instead:

String projectPath =
    System.getProperty("user.dir");

This returns the current project directory.

Example:

String filePath =
    System.getProperty("user.dir")
    + "/test-data/test.pdf";


driver.findElement(
    By.id("fileUpload")
).sendKeys(filePath);
9. Use Path

Java NIO provides a cleaner approach.

Path filePath =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );

Import:

import java.nio.file.Path;

Then:

driver.findElement(
    By.id("fileUpload")
).sendKeys(
    filePath.toAbsolutePath().toString()
);
10. Recommended Upload Code
Path filePath =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    filePath.toAbsolutePath().toString()
);

This is preferable to hardcoding:

"C:\\Users\\Selva\\Documents\\test.pdf"

because the test can run on another machine.

11. Verify File Exists Before Upload

Before uploading:

Path filePath =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );


if (!Files.exists(filePath)) {


    throw new RuntimeException(
        "File does not exist: "
        + filePath
    );
}

Imports:

import java.nio.file.Files;
import java.nio.file.Path;
12. Complete Upload With Validation
import java.nio.file.Files;
import java.nio.file.Path;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;


public class FileUploadExample {


    public static void main(String[] args) {


        WebDriver driver =
            new ChromeDriver();


        try {


            driver.get(
                "https://example.com/upload"
            );


            Path filePath =
                Path.of(
                    System.getProperty("user.dir"),
                    "test-data",
                    "test.pdf"
                );


            if (!Files.exists(filePath)) {


                throw new RuntimeException(
                    "File not found: "
                    + filePath
                );
            }


            driver.findElement(
                By.id("fileUpload")
            ).sendKeys(
                filePath.toAbsolutePath()
                       .toString()
            );


        } finally {


            driver.quit();
        }
    }
}
13. Upload Multiple Files

Some applications allow multiple file selection.

HTML:

<input
    type="file"
    id="files"
    multiple
>

Depending on the WebDriver/browser implementation, multiple paths can be supplied separated by newline characters.

Example:

String file1 =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "file1.pdf"
    ).toAbsolutePath().toString();


String file2 =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "file2.pdf"
    ).toAbsolutePath().toString();


driver.findElement(
    By.id("files")
).sendKeys(
    file1 + "\n" + file2
);
14. Multiple File Upload Using List

Example:

List<String> files = List.of(
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "file1.pdf"
    ).toAbsolutePath().toString(),


    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "file2.pdf"
    ).toAbsolutePath().toString()
);


String filePaths =
    String.join("\n", files);


driver.findElement(
    By.id("files")
).sendKeys(filePaths);

Import:

import java.util.List;
15. Upload Image

Example:

Path image =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "profile.jpg"
    );


driver.findElement(
    By.id("profileImage")
).sendKeys(
    image.toAbsolutePath().toString()
);
16. Upload Excel File
Path excel =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "employees.xlsx"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    excel.toAbsolutePath().toString()
);
17. Upload CSV File
Path csv =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "users.csv"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    csv.toAbsolutePath().toString()
);
18. Upload Word Document
Path document =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "document.docx"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    document.toAbsolutePath().toString()
);
19. File Upload With Page Object Model

Page class:

public class UploadPage {


    private WebDriver driver;


    private By fileUpload =
        By.id("fileUpload");


    private By uploadButton =
        By.id("uploadButton");


    public UploadPage(WebDriver driver) {
        this.driver = driver;
    }


    public void uploadFile(
            String filePath) {


        driver.findElement(
            fileUpload
        ).sendKeys(filePath);
    }


    public void clickUpload() {


        driver.findElement(
            uploadButton
        ).click();
    }
}
20. Test Using Page Object
@Test
public void uploadTest() {


    UploadPage uploadPage =
        new UploadPage(driver);


    String filePath =
        Path.of(
            System.getProperty("user.dir"),
            "test-data",
            "test.pdf"
        ).toAbsolutePath().toString();


    uploadPage.uploadFile(filePath);


    uploadPage.clickUpload();
}
21. Upload Utility Class

A reusable utility:

import java.nio.file.Files;
import java.nio.file.Path;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;


public class FileUploadUtils {


    public static void uploadFile(
            WebDriver driver,
            By locator,
            Path filePath) {


        if (!Files.exists(filePath)) {


            throw new RuntimeException(
                "File not found: "
                + filePath
            );
        }


        driver.findElement(
            locator
        ).sendKeys(
            filePath
                .toAbsolutePath()
                .toString()
        );
    }
}
22. Use Upload Utility
Path file =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );


FileUploadUtils.uploadFile(
    driver,
    By.id("fileUpload"),
    file
);
23. File Download

Selenium does not provide a universal high-level "download file" API.

The common approach is:

Configure browser download directory
        ↓
Click Download
        ↓
Wait for file
        ↓
Validate file
24. Configure Chrome Download Directory

Using ChromeOptions:

ChromeOptions options =
    new ChromeOptions();


Map<String, Object> preferences =
    new HashMap<>();


preferences.put(
    "download.default_directory",
    downloadPath
);


preferences.put(
    "download.prompt_for_download",
    false
);


preferences.put(
    "download.directory_upgrade",
    true
);


preferences.put(
    "safebrowsing.enabled",
    true
);


options.setExperimentalOption(
    "prefs",
    preferences
);

Imports:

import java.util.HashMap;
import java.util.Map;


import org.openqa.selenium.chrome.ChromeOptions;
25. Complete Chrome Download Example
import java.nio.file.Path;
import java.util.HashMap;
import java.util.Map;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;


public class FileDownloadExample {


    public static void main(String[] args) {


        Path downloadDirectory =
            Path.of(
                System.getProperty("user.dir"),
                "downloads"
            );


        Map<String, Object> preferences =
            new HashMap<>();


        preferences.put(
            "download.default_directory",
            downloadDirectory
                .toAbsolutePath()
                .toString()
        );


        preferences.put(
            "download.prompt_for_download",
            false
        );


        preferences.put(
            "download.directory_upgrade",
            true
        );


        preferences.put(
            "safebrowsing.enabled",
            true
        );


        ChromeOptions options =
            new ChromeOptions();


        options.setExperimentalOption(
            "prefs",
            preferences
        );


        WebDriver driver =
            new ChromeDriver(options);


        try {


            driver.get(
                "https://example.com/download"
            );


            driver.findElement(
                By.id("download")
            ).click();


        } finally {


            driver.quit();
        }
    }
}
26. Create Download Directory

Before configuring the browser:

Path downloadDirectory =
    Path.of(
        System.getProperty("user.dir"),
        "downloads"
    );


Files.createDirectories(
    downloadDirectory
);
27. Recommended Download Setup
Path downloadDirectory =
    Path.of(
        System.getProperty("user.dir"),
        "downloads"
    );


Files.createDirectories(
    downloadDirectory
);


ChromeOptions options =
    new ChromeOptions();


Map<String, Object> prefs =
    new HashMap<>();


prefs.put(
    "download.default_directory",
    downloadDirectory
        .toAbsolutePath()
        .toString()
);


prefs.put(
    "download.prompt_for_download",
    false
);


prefs.put(
    "download.directory_upgrade",
    true
);


options.setExperimentalOption(
    "prefs",
    prefs
);
28. Firefox Download Configuration

Firefox uses FirefoxOptions and preferences.

Example:

FirefoxOptions options =
    new FirefoxOptions();


options.addPreference(
    "browser.download.folderList",
    2
);


options.addPreference(
    "browser.download.dir",
    downloadDirectory
        .toAbsolutePath()
        .toString()
);


options.addPreference(
    "browser.helperApps.neverAsk.saveToDisk",
    "application/pdf"
);


options.addPreference(
    "pdfjs.disabled",
    true
);

Import:

import org.openqa.selenium.firefox.FirefoxOptions;

The MIME type must match the files your application downloads.

29. Download Directory

Recommended project structure:

project
│
├── src
│
├── test-data
│   ├── test.pdf
│   ├── test.jpg
│   └── users.csv
│
├── downloads
│
├── screenshots
│
└── pom.xml
30. Verify Downloaded File Exists

After clicking Download:

Path file =
    downloadDirectory.resolve(
        "test.pdf"
    );


if (Files.exists(file)) {


    System.out.println(
        "Download successful"
    );


} else {


    System.out.println(
        "Download failed"
    );
}
31. Assert Downloaded File Exists

TestNG:

Assert.assertTrue(
    Files.exists(file),
    "Downloaded file was not found"
);

Import:

import org.testng.Assert;
32. Verify File Size

A zero-byte file may indicate that the download has not completed.

long fileSize =
    Files.size(file);


Assert.assertTrue(
    fileSize > 0,
    "Downloaded file is empty"
);
33. Verify File Extension
String fileName =
    file.getFileName().toString();


Assert.assertTrue(
    fileName.endsWith(".pdf"),
    "Unexpected file extension"
);
34. Verify File Name
Assert.assertEquals(
    file.getFileName().toString(),
    "test.pdf"
);
35. Verify File Type

You can use Java NIO:

String contentType =
    Files.probeContentType(file);


System.out.println(
    "Content type: "
    + contentType
);

For example:

application/pdf

or:

text/csv

The detected MIME type can vary by operating system.

36. Wait for Download

Do not immediately assume the file is ready after clicking Download.

Bad:

driver.findElement(
    By.id("download")
).click();


Assert.assertTrue(
    Files.exists(file)
);

The browser may still be downloading.

Use a wait.

37. Polling Download Wait

A simple approach:

public static boolean waitForFile(
        Path file,
        int timeoutSeconds)
        throws InterruptedException {


    long endTime =
        System.currentTimeMillis()
        + timeoutSeconds * 1000L;


    while (
        System.currentTimeMillis()
        < endTime
    ) {


        if (Files.exists(file)
                && Files.size(file) > 0) {


            return true;
        }


        Thread.sleep(500);
    }


    return false;
}
38. Better Download Wait

The previous method should also consider temporary download files.

Browsers may create temporary files such as:

.crdownload
.part

while a download is in progress.

Therefore, checking only whether the final filename exists may not always be enough.

39. Detect .crdownload

Chrome may use:

test.pdf.crdownload

while the download is in progress.

After completion:

test.pdf

Example:

Path temporaryFile =
    downloadDirectory.resolve(
        "test.pdf.crdownload"
    );


Path finalFile =
    downloadDirectory.resolve(
        "test.pdf"
    );

Wait until:

temporary file does not exist
AND
final file exists
AND
file size > 0
40. Download Wait Utility
public static boolean waitForDownload(
        Path directory,
        String fileName,
        int timeoutSeconds)
        throws InterruptedException {


    Path finalFile =
        directory.resolve(fileName);


    Path tempFile =
        directory.resolve(
            fileName + ".crdownload"
        );


    long endTime =
        System.currentTimeMillis()
        + timeoutSeconds * 1000L;


    while (
        System.currentTimeMillis()
        < endTime
    ) {


        if (Files.exists(finalFile)
                && !Files.exists(tempFile)
                && Files.size(finalFile) > 0) {


            return true;
        }


        Thread.sleep(500);
    }


    return false;
}
41. Download Utility Class
import java.nio.file.Files;
import java.nio.file.Path;


public class FileDownloadUtils {


    public static boolean waitForDownload(
            Path directory,
            String fileName,
            int timeoutSeconds)
            throws InterruptedException {


        Path finalFile =
            directory.resolve(fileName);


        long endTime =
            System.currentTimeMillis()
            + timeoutSeconds * 1000L;


        while (
            System.currentTimeMillis()
            < endTime
        ) {


            if (Files.exists(finalFile)
                    && Files.size(finalFile) > 0) {


                return true;
            }


            Thread.sleep(500);
        }


        return false;
    }


    public static boolean fileExists(
            Path file) {


        return Files.exists(file);
    }


    public static long fileSize(
            Path file)
            throws Exception {


        return Files.size(file);
    }
}
42. Using Download Utility
driver.findElement(
    By.id("download")
).click();


boolean downloaded =
    FileDownloadUtils.waitForDownload(
        downloadDirectory,
        "test.pdf",
        30
    );


Assert.assertTrue(
    downloaded,
    "File download failed"
);
43. Clean Download Directory Before Test

Old files can cause false positives.

Example:

Files.deleteIfExists(
    downloadDirectory.resolve(
        "test.pdf"
    )
);

Do this before starting the download test.

44. Delete All Files From Download Directory
try (Stream<Path> files =
        Files.list(downloadDirectory)) {


    files.forEach(
        path -> {


            try {


                Files.deleteIfExists(path);


            } catch (Exception e) {


                e.printStackTrace();
            }
        }
    );
}

Import:

import java.util.stream.Stream;

Use this carefully if the directory contains files needed by other tests.

45. Complete Download Test
import java.nio.file.Files;
                .toString()
        );


        prefs.put(
            "download.prompt_for_download",
            false
        );


        prefs.put(
            "download.directory_upgrade",
            true
        );


        ChromeOptions options =
            new ChromeOptions();


        options.setExperimentalOption(
            "prefs",
            prefs
        );


        driver =
            new ChromeDriver(options);
    }


    @Test
    public void downloadTest()
            throws Exception {


        driver.get(
            "https://example.com/download"
        );


        Path downloadedFile =
            downloadDirectory.resolve(
                "test.pdf"
            );


        Files.deleteIfExists(
            downloadedFile
        );


        driver.findElement(
            By.id("download")
        ).click();


        boolean downloaded =
            FileDownloadUtils.waitForDownload(
                downloadDirectory,
                "test.pdf",
                30
            );


        Assert.assertTrue(
            downloaded,
            "File was not downloaded"
        );


        Assert.assertTrue(
            Files.size(downloadedFile) > 0,
            "Downloaded file is empty"
        );
    }


    @AfterMethod
    public void tearDown() {


        if (driver != null) {
            driver.quit();
        }
    }
}
46. Upload and Download in One Test

Example flow:

Open Application
      ↓
Upload File
      ↓
Submit
      ↓
Application Processes File
      ↓
Click Download
      ↓
Wait for Download
      ↓
Verify Downloaded File

Example:

Path uploadFile =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "input.pdf"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    uploadFile.toAbsolutePath().toString()
);


driver.findElement(
    By.id("submit")
).click();


driver.findElement(
    By.id("download")
).click();
47. Upload Validation

After selecting a file, the application may display the filename.

Example:

WebElement fileName =
    driver.findElement(
        By.id("fileName")
    );


Assert.assertEquals(
    fileName.getText(),
    "test.pdf"
);

The exact validation depends on the application.

48. Download Validation

A strong download test can validate:

1. File exists
2. File is not empty
3. File name is correct
4. File extension is correct
5. MIME type is correct
6. File content is correct
49. Validate PDF File

You can verify basic properties:

Path file =
    downloadDirectory.resolve(
        "test.pdf"
    );


Assert.assertTrue(
    Files.exists(file)
);


Assert.assertTrue(
    Files.size(file) > 0
);


Assert.assertTrue(
    file.getFileName()
        .toString()
        .endsWith(".pdf")
);

For deeper PDF validation, use a dedicated PDF library rather than Selenium.

50. Validate Excel File

For an Excel download:

Download
   ↓
Verify file exists
   ↓
Verify extension
   ↓
Read workbook
   ↓
Validate sheet/data

Selenium itself does not parse Excel files.

A Java library such as Apache POI can be used for content validation.

51. Validate CSV File

After downloading a CSV:

Path csv =
    downloadDirectory.resolve(
        "users.csv"
    );


Assert.assertTrue(
    Files.exists(csv)
);

You can then read it:

List<String> lines =
    Files.readAllLines(csv);


for (String line : lines) {
    System.out.println(line);
}
52. File Upload vs File Download
Feature	Upload	Download
Selenium direct support	Yes for file inputs	Browser configuration usually required
Common method	sendKeys()	Click download link/button
Browser dialog	Usually not required	Usually not required
File path	Input file	Download directory
Validation	File selected/uploaded	File exists/completed
Java NIO	Useful	Very useful
53. Upload Using sendKeys() vs Robot
sendKeys()

Preferred when:

<input type="file">

Example:

element.sendKeys(filePath);

Advantages:

Simple
Reliable
Cross-platform
Easy to automate
Works well in CI/CD
Robot

Robot can interact with OS-level dialogs.

It is generally less desirable because:

It depends on OS behavior
Focus can be lost
It is harder to run in headless CI
It is less reliable

Use sendKeys() whenever the file input is accessible.

54. When sendKeys() Does Not Work

Some applications may use:

Custom upload widgets
Drag-and-drop components
Hidden file inputs
Native OS file dialogs
Third-party upload controls

First inspect the DOM.

Look for:

<input type="file">

If it exists, try interacting with the file input directly.

If the application uses a custom component, additional techniques may be required.

55. Hidden File Input

Example:

<input
    type="file"
    id="fileUpload"
    style="display:none"
>

Depending on the browser and WebDriver behavior, a hidden input may not accept normal interaction.

If the application's UI triggers it through a button, inspect the DOM and determine whether the underlying file input can be interacted with safely.

Avoid modifying the DOM with JavaScript just to bypass application behavior unless there is a specific testing reason.

56. Drag-and-Drop Upload

Some applications provide drag-and-drop upload.

A common implementation may still use an underlying:

<input type="file">

If the input is available, prefer using it.

If not, specialized browser automation or application-specific support may be needed.

57. File Upload With Remote Selenium

When using Selenium Grid or a remote WebDriver, file uploads require special consideration.

Example:

Local Machine
      |
      | Upload file
      v
Remote Selenium Node
      |
      v
Browser

Selenium provides:

LocalFileDetector

for remote file uploads.

58. LocalFileDetector

Import:

import org.openqa.selenium.remote.LocalFileDetector;

Example:

RemoteWebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );


driver.setFileDetector(
    new LocalFileDetector()
);

Then:

driver.findElement(
    By.id("fileUpload")
).sendKeys(
    filePath
);

This helps Selenium transfer a local file to the remote machine.

59. Remote File Upload Flow
Local Test Machine
       |
       v
sendKeys(local file path)
       |
       v
LocalFileDetector
       |
       v
Remote Selenium Node
       |
       v
Browser
       |
       v
Application

This becomes especially important when running tests in Selenium Grid.

60. Remote Download

File downloads are different from uploads.

With a remote browser:

Test Machine
      |
      v
Selenium Grid Node
      |
      v
Browser downloads file
      |
      v
Remote download directory

The downloaded file may exist on the remote Selenium node rather than the local test machine.

For remote download validation, configure the Grid/browser environment appropriately or use a download mechanism supported by your Selenium Grid setup.

61. File Size

Get file size:

long size =
    Files.size(file);


System.out.println(
    "File size: " + size
);

Convert to KB:

double kb =
    size / 1024.0;


System.out.println(
    "File size KB: " + kb
);
62. File Name
String fileName =
    file.getFileName()
        .toString();


System.out.println(
    fileName
);
63. File Extension
String fileName =
    file.getFileName()
        .toString();


String extension =
    fileName.substring(
        fileName.lastIndexOf(".") + 1
    );


System.out.println(
    extension
);
64. Check Readable File
Assert.assertTrue(
    Files.isReadable(file),
    "File is not readable"
);
65. Check Regular File
Assert.assertTrue(
    Files.isRegularFile(file),
    "Path is not a regular file"
);
66. Delete Downloaded File
Files.deleteIfExists(file);

This is useful for test cleanup.

67. File Utility Class

A more complete utility:

import java.nio.file.Files;
import java.nio.file.Path;


public class FileUtils {


    public static boolean exists(
            Path file) {


        return Files.exists(file);
    }


    public static long size(
            Path file)
            throws Exception {


        return Files.size(file);
    }


    public static void delete(
            Path file)
            throws Exception {


        Files.deleteIfExists(file);
    }


    public static boolean isReadable(
            Path file) {


        return Files.isReadable(file);
    }


    public static boolean isRegularFile(
            Path file) {


        return Files.isRegularFile(file);
    }
}
68. TestNG Upload Example
@Test
public void uploadTest() {


    Path file =
        Path.of(
            System.getProperty("user.dir"),
            "test-data",
            "test.pdf"
        );


    Assert.assertTrue(
        Files.exists(file),
        "Upload file does not exist"
    );


    driver.findElement(
        By.id("fileUpload")
    ).sendKeys(
        file.toAbsolutePath().toString()
    );
}
69. TestNG Download Example
@Test
public void downloadTest()
        throws Exception {


    Path downloadDirectory =
        Path.of(
            System.getProperty("user.dir"),
            "downloads"
        );


    Path file =
        downloadDirectory.resolve(
            "test.pdf"
        );


    Files.deleteIfExists(file);


    driver.findElement(
        By.id("download")
    ).click();


    boolean downloaded =
        FileDownloadUtils.waitForDownload(
            downloadDirectory,
            "test.pdf",
            30
        );


    Assert.assertTrue(
        downloaded,
        "File was not downloaded"
    );


    Assert.assertTrue(
        Files.size(file) > 0,
        "Downloaded file is empty"
    );
}
70. Common Exceptions
70.1 InvalidArgumentException

Can occur if the file path supplied to sendKeys() is invalid.

Check:

Files.exists(filePath)
70.2 ElementNotInteractableException

Can occur when the file input cannot be interacted with normally.

Check whether:

The element is available
The element is enabled
The application uses a custom upload control
The underlying input is accessible
70.3 NoSuchElementException

The upload element or download button cannot be found.

Check:

By.id(...)
By.name(...)
By.cssSelector(...)
By.xpath(...)
70.4 TimeoutException

Can occur when waiting for:

Upload completion
Download completion
Downloaded file
Success message

Use appropriate explicit waits.

70.5 FileSystemException

Can occur when:

File is locked
Permission is denied
Directory does not exist
Another process is using the file
71. Common Download Problems
Problem 1: Download prompt appears

Configure:

prefs.put(
    "download.prompt_for_download",
    false
);
Problem 2: File downloaded to unexpected location

Explicitly configure:

prefs.put(
    "download.default_directory",
    downloadDirectory
);
Problem 3: Test finds an old file

Delete the expected file before clicking Download:

Files.deleteIfExists(file);
Problem 4: Test checks too early

Wait for the download to complete.

Problem 5: Temporary download file exists

Check for:

.crdownload
.part

depending on the browser.

72. Best Practices
File Upload
Prefer sendKeys() for <input type="file">.
Avoid OS-level automation when unnecessary.
Use project-relative test data.
Verify the file exists before uploading.
Do not hardcode developer-specific paths.
Use Path/Files.
Use LocalFileDetector for remote uploads when appropriate.
File Download
Configure a dedicated download directory.
Create the directory before the test.
Delete old files before starting.
Wait for download completion.
Check file existence.
Check file size.
Validate file name and extension.
Validate file content when required.
Clean up downloaded files.
Handle remote browser downloads carefully.
73. Recommended Project Structure
SeleniumStudy
│
├── FileUploadDownload
│   └── Selenium-File-Upload-Download.md
│
├── test-data
│   ├── test.pdf
│   ├── test.jpg
│   ├── users.csv
│   └── employees.xlsx
│
├── downloads
│
├── screenshots
│
└── src
    ├── main
    │   └── java
    │       └── utilities
    │
    └── test
        └── java
74. Interview Questions
Q1. How do you upload a file in Selenium?
driver.findElement(
    By.id("fileUpload")
).sendKeys(
    filePath
);
Q2. Do you need Robot for file upload?

Not when the application exposes a normal:

<input type="file">

Use:

sendKeys()

instead.

Q3. Why do we use an absolute file path?

The browser/WebDriver needs the actual file location.

However, instead of hardcoding a machine-specific absolute path, construct it dynamically:

Path.of(
    System.getProperty("user.dir"),
    "test-data",
    "test.pdf"
)
Q4. How do you upload a file using a relative project path?
Path file =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    file.toAbsolutePath().toString()
);
Q5. How do you verify that the upload file exists?
Assert.assertTrue(
    Files.exists(file)
);
Q6. How do you configure Chrome download location?

Using ChromeOptions:

Map<String, Object> prefs =
    new HashMap<>();


prefs.put(
    "download.default_directory",
    downloadDirectory
);


ChromeOptions options =
    new ChromeOptions();


options.setExperimentalOption(
    "prefs",
    prefs
);
Q7. How do you verify that a file was downloaded?
Assert.assertTrue(
    Files.exists(downloadedFile)
);

Also verify:

Files.size(downloadedFile) > 0
Q8. Why should you delete the expected file before downloading?

To prevent an old file from causing a false positive.

Q9. How do you wait for a download?

Poll the download directory until:

Final file exists
AND
file size > 0
AND
temporary download file is gone
Q10. What is .crdownload?

Chrome may use a .crdownload file while a download is in progress.

After completion, the final file normally replaces the temporary download state.

Q11. How do you upload a file to a remote Selenium Grid?

Use:

driver.setFileDetector(
    new LocalFileDetector()
);

with RemoteWebDriver.

Q12. Can Selenium validate the contents of a PDF?

Selenium itself is primarily for browser automation.

For deep PDF validation, use a dedicated Java PDF library.

Q13. Can Selenium read Excel files?

Selenium does not parse Excel files.

Use a library such as Apache POI for Excel content validation.

Q14. How do you verify file size?
long size =
    Files.size(file);

Then:

Assert.assertTrue(
    size > 0
);
Q15. What is the difference between upload and download automation?
Upload

Selenium sends a local file path to the file input:

element.sendKeys(filePath);
Download

The browser downloads the file and the test validates the resulting file using Java file APIs.

Q16. Why is Robot not preferred for uploads?

Robot interacts with the operating system's native UI.

It can be:

OS-dependent
Focus-sensitive
Less reliable
Difficult in CI/headless environments

sendKeys() is preferable when a file input is available.

Q17. How do you make upload paths portable?

Use:

Path.of(
    System.getProperty("user.dir"),
    "test-data",
    "test.pdf"
);

instead of:

"C:\\Users\\Selva\\Documents\\test.pdf"
Q18. What happens when Selenium runs on Grid and downloads a file?

The browser downloads the file on the remote Selenium node unless the environment provides a mechanism to retrieve the downloaded file.

The test framework must account for where the file physically exists.

75. Quick Revision
Upload
Path file =
    Path.of(
        System.getProperty("user.dir"),
        "test-data",
        "test.pdf"
    );


driver.findElement(
    By.id("fileUpload")
).sendKeys(
    file.toAbsolutePath().toString()
);
Verify Upload File
Assert.assertTrue(
    Files.exists(file)
);
Configure Download
Map<String, Object> prefs =
    new HashMap<>();


prefs.put(
    "download.default_directory",
    downloadDirectory
        .toAbsolutePath()
        .toString()
);


prefs.put(
    "download.prompt_for_download",
    false
);


ChromeOptions options =
    new ChromeOptions();


options.setExperimentalOption(
    "prefs",
    prefs
);
Click Download
driver.findElement(
    By.id("download")
).click();
Verify Download
Path file =
    downloadDirectory.resolve(
        "test.pdf"
    );


Assert.assertTrue(
    Files.exists(file)
);


Assert.assertTrue(
    Files.size(file) > 0
);
Remote Upload
driver.setFileDetector(
    new LocalFileDetector()
);
76. Key Takeaways
Use sendKeys() for normal Selenium file uploads.

A file upload input is normally:

<input type="file">
Use Path and Files instead of hardcoded machine-specific paths.
Use System.getProperty("user.dir") to build portable project paths.
Verify upload files exist before sending them to Selenium.
Configure browser download preferences for predictable downloads.
Always use a dedicated download directory.
Delete old files before starting a download test.
Wait for the download to finish.
Check both file existence and file size.
Check temporary files such as .crdownload where appropriate.
Use dedicated libraries for deep PDF/Excel/CSV content validation.
Use LocalFileDetector for remote file uploads with Selenium Grid.
Be careful with remote downloads because the file may be stored on the remote node.
Integrate upload/download utilities into the Selenium framework.
Use TestNG assertions for validation.
Keep test files under a reusable test-data directory.
Keep downloaded files separate from source test data.


Ad
