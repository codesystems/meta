Spoon offers a solution for automated browser testing by running [Selenium](/selenium) tests on a variety of browser containers all on your local machine with minimal setup. This lets you avoid the pain and expense of setting up and maintaining a local Selenium Grid.

Running your Selenium tests on Spoon is almost exactly like running them on a local Selenium Grid. What does this mean for you?

1. Porting your tests to run on Spoon requires very few changes.
2. You can use native Selenium APIs - no extra dependencies or libraries to import.

The key difference is that Spoon takes care of all the Selenium infrastructure and networking for you! Point your tests to the Spoon hub at **http://localhost:4444/wd/hub** and Spoon will automatically provision, stream, and start the test on the required browser.

If you've used a different cloud-based testing service in the past, check out the **Adapting Tests from Other Services** section.

If you're a Selenium veteran, but you've never used Selenium Grid before, you'll need to change a couple lines of code in your tests.

If you're already using Grid to run your tests, configuring your tests for Spoon is a one-line change. Substitute the URL of your current Selenium Hub with **http://localhost:4444/wd/hub** and you're ready to go!

#### Supported Browsers

- Chrome 27+
- Firefox 3+
- Internet Explorer 6+

*Don't see your preferred browser? [Let us know](/contact) and we'll do our best to get it added.*

#### Starting the Spoon Hub

In your web browser, click the **Start Grid** button in the top-left corner of the page. A buffering dialog will appear on your desktop. When the buffering dialog completes, check the **Hub** window on the page. When the Spoon hub is ready, this output will appear in the window:

```
Jun 26, 2014 3:21:23 PM org.openqa.grid.selenium.GridLauncher main
INFO: Launching a selenium grid server
2014-06-26 15:21:24.064:INFO:osjs.Server:jetty-7.x.y-SNAPSHOT
2014-06-26 15:21:24.088:INFO:osjsh.ContextHandler:started o.s.j.s.ServletContextHandler{/,null}
2014-06-26 15:21:24.094:INFO:osjs.AbstractConnector:Started SocketConnector@0.0.0.0:4444
```

#### Adapting Your Test

To adapt an existing test for use on Spoon, you'll need to change all of your WebDriver instances to use the **RemoteWebDriver** class, instead of a native driver for Firefox, Chrome, or Internet Explorer.

If you are already using Selenium Grid with RemoteWebDriver, you only have to change the **hub url** of the driver to **http://localhost:4444/wd/hub**.

Below, we've included approximate comparisons of the driver setup for a test using **FirefoxDriver** versus that same test run on Spoon.

**Java**

Using FirefoxDriver:

```java
/*
 * Using FirefoxDriver
 */
import org.openqa.selenium.firefox.FirefoxDriver;

WebDriver driver = new FirefoxDriver();

/*
 * Adapted for Spoon
 */
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver

DesiredCapabilities caps = DesiredCapabilities.firefox();
WebDriver driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), caps);
```

**C#**

```csharp
/*
 * Using FirefoxDriver
 */
using OpenQA.Selenium.Firefox;

IWebDriver driver = new FirefoxDriver();

/*
 * Adapted for Spoon
 */
using OpenQA.Selenium.Remote;

ICapabilities caps = DesiredCapabilities.Firefox();
IWebDriver driver = new RemoteWebDriver(new Uri("http://localhost:4444/wd/hub"), caps);
```

**Python**

```python
"""
Using webdriver.Firefox
"""
from selenium import webdriver

driver = webdriver.Firefox()

"""
Adapted for Spoon
"""
from selenium import webdriver
from selenium import webdriver.common

caps = desired_capabilities.FIREFOX
driver = webdriver.Remote(command_executor="http://localhost:4444/wd/hub", desired_capabilities=caps)
```

#### Choosing a Browser to Test

The Spoon Hub determines which browser you would like to test against using the **DesiredCapabilities** of the WebDriver that the test is using.

All of the major language bindings have a **DesiredCapabilities** (or **desired_capabilities** in the case of Python) class that have attributes for each browser. Change this attribute to change the browser the test will run against.

```java
// Java
DesiredCapabilities.firefox(); 				// Mozilla Firefox
DesiredCapabilities.chrome();				// Google Chrome
DesiredCapabilities.ie();					// Internet Explorer
```

```csharp
// C#
DesiredCapabilities.Firefox();
DesiredCapabilities.Chrome();
DesiredCapabilities.InternetExplorer();
```

```python
# Python
desired_capabilities.FIREFOX
desired_capabilities.CHROME
desired_capabilities.INTERNETEXPLORER
```

To specify a version to test against, add a **version** capability into your existing test capabilities using a property setter or a `setCapability`/`SetCapability` instance method.

```java
// Java
capabilities.setCapability("version", "30");		// Test against version 30
```

```csharp
// C#
capabilities.SetCapability("version", "30");
```

In Python, you can modify the `desired_capabilities` just like a dictionary:

```python
capabilities['version'] = '30'
```

#### Adapting Tests from Other Services

If you've used another cloud-based Selenium service in the past, switching over to Spoon is as easy as switching a couple of lines of code.

**BrowserStack**

When switching from BrowserStack, you'll need to make 2 changes to your existing code:

1. Delete the **browserstack.user** and **browserstack.key** capabilities from your test's capabilities, along with any other BrowserStack-specific commands or libraries.
2. Change the RemoteWebDriver hub URL to **http://localhost:4444/wd/hub**.

If you were passing your BrowserStack credentials through the hub url, the only required change is to change the RemoteWebDriver hub url from **[browser.user]:[browserstack.key]@hub.browserstack.com:80/wd/hub** to **http://localhost:4444/wd/hub**.

**Sauce Labs**

Sauce Labs provides 2 means of authentication when sending tests to their cloud server. If you are passing in your username and access key as DesiredCapabilities, you'll need to delete those capabilities and change the hub URL to **http://localhost:4444/wd/hub**.

If you are using the user-specific hub URL (**http://USERNAME:ACCESSKEY@ondemand.saucelabs.com:80/wd/hub**), change this to **http://localhost:4444/wd/hub** and you're ready to go!

**Testing Bot**

Testing Bot uses Selenium's **DesiredCapabilities** object as a means of authentication. When switching to Spoon, delete the **api_key** and **api_secret** capabilities from all of your tests - they are not necessary in Spoon.

If you were previously using any of the extension methods provided by the **TestingBotDriver**, those should also be removed from your code, as they may lead to unexpected results.

In addition to deleting Testing Bot-specific capabilities, change the hub URL in your tests from **http://hub.testingbot.com:4444/wd/hub** to **http://localhost:4444/wd/hub**.

### Debugging Tests

Unlike other cloud-based testing services, Spoon makes live debugging of your tests as easy as setting a breakpoint in your IDE.

When your test contains a breakpoint, the test will execute normally until it hits your breakpoint. At this point, all execution will freeze and the browser window will idle, waiting for the breakpoint to be passed. At this point, you can interact with the browser window as if it were natively installed. When you are done debugging, press **Continue** in your IDE, or continue to **Step Through** your code. Your test will then resume, with the browser continuing to respond to WebDriver commands from your test.

**Note**: Spoon is currently configured to automatically recycle any session that continues for 90 seconds without a successive command. This may cause your debugging session to end abruptly if it lasts longer than 90 seconds. We are currently working towards an option to turn this feature off for cases where a longer debugging time is required.

### Integrating Spoon with an Existing Grid

If you or your development team already have an in-house Selenium Grid, you can use Spoon to "fill in the gaps" in your Grid to support browsers that you may not have the capacity or hardware to host internally.

For example, if your in-house Grid is only large enough to host and support the latest versions of each browser, you can use Spoon to add legacy browsers to your Grid.

After initial configuration and setup, the Spoon hub will automatically provision and run tests on any browser that is not in your internal Grid.

#### Setup Guide

The Spoon hub has the same external API as the standard Selenium hub. You can connect external nodes to the Spoon hub just like you would a normal Selenium hub.

1. Start the Spoon hub
	1. *If using Spoon as part of your CI process*:
		1. Log on, or RDP in, to your build/test server (must be a Windows machine).
		2. Open a web browser and navigate to [http://spoon.net/selenium](/selenium). Log in with your Spoon.net username and password.
		3. Click **Start Grid** to start the Spoon hub. If the Spoon.net plugin is not already installed, you will have to install it before Spoon will start.
	2. *If using Spoon from a development machine*: Log in to [http://spoon.net/selenium](/selenium) with your Spoon.net username and password. Click **Start Grid** and the Spoon hub will start.
2. Connect your internal nodes to the Spoon hub.
	1. The hub will launch on port 4444 of the machine Spoon was accessed from. You can connect to the hub from a remote node by executing the following command (from the remote node): `java -jar selenium-server-standalone-2.xx.y.jar -role node -hub http://hub-machine:4444/grid/register`. For more information on configuring nodes with additional command line parameters, see [the official Selenium Grid Documentation](https://code.google.com/p/selenium/wiki/Grid2).

Once your Grid is configured, you can send your tests to it just as you normally would. If the hub cannot find a matching browser on your internal nodes, it will automatically provision a fresh browser from Spoon.net and run the test on it.

### Testing Internal Sites

Testing internal websites with Spoon is just as easy as testing public websites.

All browsers and test scripts run on your local machine, so there is no need for any special proxy configuration or modifications to the URL when testing an internal site.

The security benefit of this is that no browser data or network traffic passes through Spoon servers. The only data stored on the server is the test results, which can be viewed in your online account. You can turn off test result storage by unchecking the **Save test reports** check box in the top-right corner of the [http://spoon.net/selenium](/selenium).

Below is some example code demonstrating how you would run your test against an internal site running at **http://my-internal-server:8080**.

```java
// Java
driver.navigate().to("http://my-internal-server:8080");
```

```csharp
// C#
driver.Navigate().GoToUrl("http://my-internal-server:8080");
```

```python
# Python
driver.get("http://my-internal-server:8080")
```

### Testing Internet Explorer

When testing parallel instances of Internet Explorer on Spoon, we recommend setting the following options in your test.

1. Force Internet Explorer to use the Create Process API
2. Launch Internet Explorer with the -private flag (for **Internet Explorer 8+** only)

Using these settings helps prevent cookies and other session-specific items from being shared between different instances of Internet Explorer.

If you are not testing multiple instances of Internet Explorer in parallel, we recommend setting the **Ensure Clean Session** capability to **True**.

#### Configuring Internet Explorer Options

See below for language-specific instructions for how to properly configure your Internet Explorer tests for Spoon.

**Java**

```java
// Import the ie package
import org.openqa.selenium.ie;

// Create DesiredCapabilities for ie
DesiredCapabilities capabilities = DesiredCapabilities.ie();
// Force Windows to launch IE through Create Process API and in "private" browsing mode
capabilities.setCapability(InternetExplorerDriver.FORCE_CREATE_PROCESS, true);
capabilities.setCapability(InternetExplorerdriver.IE_SWITCHES, "-private");

// If testing serial instances of IE, add IE_ENSURE_CLEAN_SESSION
capabilities.setCapability(InternetExplorerDriver.IE_ENSURE_CLEAN_SESSION, true);

WebDriver driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), capabilities);
```

**C#**

```csharp
// Add using directive for IE namespace
using OpenQA.Selenium.IE;

// Use this class in leiu of DesiredCapabilities
InternetExplorerOptions ieOptions = new InternetExplorerOptions();

// Force Windows to launch IE through Create Process API and in "private" browsing mode
ieOptions.ForceCreateProcessApi = true
ieOptions.BrowserCommandLineArguments = "-private";
ieOptions.AddAdditionalCapability("version", "10");

// Convert ieOptions to an ICapabilities object and instantiate driver
IWebDriver driver = new RemoteWebDriver(new Uri("http://localhost:4444/wd/hub"), ieOptions.ToCapabilities());
```

**Python**

```python
# Create desired_capabilities
capabilities = webdriver.DesiredCapabilities.INTERNETEXPLORER

# Force Windows to launch IE through Create Process API and in "private" browsing mode
capabilities['ie.forceCreateProcessApi'] = True
capabilities['ie.browserCommandLineArguments'] = '-private'

# Instantiate the driver
driver = webdriver.Remote(command_executor="http://localhost:4444/wd/hub", desired_capabilities=capabilities)
```

#### Internet Explorer Container Configuration

Spoon's Internet Explorer containers are packaged and pre-configured to work with Selenium's Remote WebDriver without any end-user configuration. Each container is pre-configured with the following settings:

- Packaged with the latest IEDriverServer installed in the virtual environment's PATH.
- Protected Mode Enabled in all zones.

For Internet Explorer 10 and 11, Enhanced Protected Mode is Disabled.

For Internet Explorer 11, the registry key **HKLM\Software\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE** was added to the container with DWORD iexplore.exe and value 0.

For more information on these changes, as they relate to the InternetExplorerDriver, see [the Selenium documentation](https://code.google.com/p/selenium/wiki/InternetExplorerDriver).

#### Internet Explorer "Gotchas"

The InternetExplorerDriver in Selenium has some unique features that may cause unexpected test results. Below, we've compiled a list of some of the most commonly encountered "gotchas" we've seen testing Internet Explorer.

- When testing Internet Explorer, the browser zoom level should always be left at 100%. The IEDriverServer relies on this zoom level for native mouse events - configuring the zoom to be greater or less than 100% may cause inadvertent failures in your tests.
- If you are using a hover command in your tests, do not place your mouse over the Internet Explorer window. Doing so causes the hover to fail.

### Reporting

Though all tests execute locally in containers, test reports and screenshots are logged and saved on your Spoon.net account for easy sharing and access.

If you would not like us to save your test reports for you, this feature can be toggled by checking or unchecking the **Save test reports** check box in the top-right corner of [/selenium](/selenium).

When unchecked, command and screenshot logs will still appear for review after your tests complete, but they will not be saved after your session is ended.

Reports can also be disabled for the organization as a whole by modifying the privacy settings for the organization. This can be done by the organization owner and overrides the settings for the users within the organization.

#### Sharing Tests

The visibility of your test results can be controlled through two methods:

1. *In the live Grid View*: After a test completes, a **Make Public** link will be displayed in that test's header. Clicking this link will make the report page for that test publicly visible. You can then share the link for that test with any of your teammates or coworkers for review. You can similarly revert the test back to private by clicking the **Make Private** link that appears when the test is publicly-visible.
2. *In the Test Reports table*: Each test has a corresponding **Visibility** property that shows the **Public/Private** state of the test's report. Clicking the lock icon in the **Actions** column will toggle the **Visibility** of that test.

To share a test, make sure the test's visibility is set to Public. Once this is set, the corresponding report is visible to anyone with the share link.

#### Deleting Tests

If you're running low on storage space, or you'd just like to clear up your account history, you can delete old tests.

To delete a test, go to the Test Reports table and click the X icon corresponding to the test you would like to delete.

#### Test Reports

Each test report contains a step-by-step breakdown of all the commands run for a given test.

For each command, the following information is recorded:

- The request made to the remote server. This details the specific command, and any associated parameters, that was run.
- The corresponding response from the remote server. This details the result(s) of the command.
A screenshot of the webpage during the command.

In the report, commands are grouped by their associated screenshot. When you see a sequence of commands associated with a single screenshot, this means that the page did not visually change throughout this sequence of commands.

### Common Issues and Troubleshooting

#### Cannot connect to the Spoon Hub

Before proceeding, make sure the Spoon hub is running on your local computer. You can check this by opening **Windows Task Manager** and checking the **Processes** tab for *SpooniumComponent.exe*.

**Solution 1: Check your Firewall**: This issue may occur if your computer has a restrictive firewall that blocks incoming and outgoing connections to/from your computer.

If possible, check your firewall and make sure port 4444 is not blocked. This is the port the Spoon hub listens for commands on. If this port is blocked, it must be unblocked before using Spoon.

#### Cannot Launch the Spoon Hub

Ensure that the Spoon Plugin is installed and running. The Spoon Plugin can be downloaded from [http://start.spoon.net/install](http://start.spoon.net/install).

To run or restart the Spoon Plugin once installed, go to the **Start Menu** > **All Programs** > **Startup** and select **Spoon.net Sandbox Manager 3.33**.

#### After clicking **Start Grid**, "Pending" appears but the Grid never launches

This issue occurs when the Spoon Plugin is not activate or installed. If you have not installed the Spoon Plugin, it can be downloaded from [http://start.spoon.net/install](http://start.spoon.net/install).

If the Spoon Plugin is installed and you continue to see this issue, verify that your browser is not blocking the Spoon Plugin from running on Spoon.

**Mozilla Firefox**

1. Navigate to [http://spoon.net/selenium](/selenium).
2. To the left of the browser's address bar, a "building block" icon should appear (it looks like a small LEGO).
3. Click this icon and a small box will appear beneath it with the dialog "Allow *Spoon.net* to run Spoon?"
4. Select **Allow and Remember**
5. Refresh the page and click **Start Grid**.

**Google Chrome**

1. In the address bar, type **chrome://plugins**.
2. Locate **Spoon Plugin** and check the **Always allowed** box.
3. Restart Google Chrome to apply this new setting.

#### Selenium Errors

**Internet Explorer does not launch with Error "IELaunchURL() returned HRESULT 80070012"**

This error occurs due to a bug in Selenium's IEDriverServer. For more information, see this issue report: [https://code.google.com/p/selenium/issues/detail?id=7045](https://code.google.com/p/selenium/issues/detail?id=7045).

To avoid this error, enable **ForceCreateProcessApi** in your test capabilities. For language-specific instructions, see **Testing Internet Explorer**.