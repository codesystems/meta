The Turbo Selenium Sandbox offers a solution for automated browser testing by running [Selenium](/selenium) tests on a variety of browser containers all on your local machine with minimal setup. This lets you avoid the pain and expense of setting up and maintaining a local Selenium Grid.

Running your Selenium tests in the Selenium Sandbox is almost exactly like running them on a local Selenium Grid. What does this mean for you?

1. Porting your tests to run in the Selenium Sandbox requires very few changes.
2. You can use native Selenium APIs - no extra dependencies or libraries to import.

The key difference is that Turbo.net takes care of all the Selenium infrastructure and networking for you! Point your tests to the Selenium hub at **http://localhost:4444/wd/hub** and the Selenium Sandbox will automatically provision, stream, and start the test on the required browser.

If you've used a different cloud-based testing service in the past, check out the **Adapting Tests from Other Services** section.

If you're a Selenium veteran, but you've never used Selenium Grid before, you'll need to change a couple lines of code in your tests.

If you're already using Grid to run your tests, configuring your tests for Turbo.net is a one-line change. Substitute the URL of your current Selenium Hub with **http://localhost:4444/wd/hub** and you're ready to go!

#### Supported Browsers

- Chrome 27+
- Firefox 3+
- Internet Explorer 6+

*Don't see your preferred browser? [Let us know](/contact) and we'll do our best to get it added.*

#### Starting the Selenium Sandbox

In your web browser, click the **Run** button for the Selenium Grid. A buffering dialog will appear on your desktop. When the buffering dialog completes, check the **Hub** window on the page. When the hub is ready, this output will appear in the window:

    Jun 26, 2014 3:21:23 PM org.openqa.grid.selenium.GridLauncher main
    INFO: Launching a selenium grid server
    2014-06-26 15:21:24.064:INFO:osjs.Server:jetty-7.x.y-SNAPSHOT
    2014-06-26 15:21:24.088:INFO:osjsh.ContextHandler:started o.s.j.s.ServletContextHandler{/,null}
    2014-06-26 15:21:24.094:INFO:osjs.AbstractConnector:Started SocketConnector@0.0.0.0:4444

#### Adapting Your Test

To adapt an existing test for use in the Selenium Sandbox, you'll need to change all of your WebDriver instances to use the **RemoteWebDriver** class, instead of a native driver for Firefox, Chrome, or Internet Explorer.

If you are already using Selenium Grid with RemoteWebDriver, you only have to change the **hub url** of the driver to **http://localhost:4444/wd/hub**.

Below, we've included approximate comparisons of the driver setup for a test using **FirefoxDriver** versus that same test run in the Selenium Sandbox.

**Java**

Using FirefoxDriver:

    java
    /*
     * Using FirefoxDriver
     */
    import org.openqa.selenium.firefox.FirefoxDriver;
    
    WebDriver driver = new FirefoxDriver();
    
    /*
     * Adapted for Turbo.net Selenium Sandbox
     */
    import org.openqa.selenium.remote.DesiredCapabilities;
    import org.openqa.selenium.remote.RemoteWebDriver
    
    DesiredCapabilities caps = DesiredCapabilities.firefox();
    WebDriver driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), caps);

**C#**

    csharp
    /*
     * Using FirefoxDriver
     */
    using OpenQA.Selenium.Firefox;
    
    IWebDriver driver = new FirefoxDriver();
    
    /*
     * Adapted for Turbo.net Selenium Sandbox
     */
    using OpenQA.Selenium.Remote;
    
    ICapabilities caps = DesiredCapabilities.Firefox();
    IWebDriver driver = new RemoteWebDriver(new Uri("http://localhost:4444/wd/hub"), caps);

**Python**

    python
    """
    Using webdriver.Firefox
    """
    from selenium import webdriver
    
    driver = webdriver.Firefox()
    
    """
    Adapted for Turbo.net Selenium Sandbox
    """
    from selenium import webdriver
    from selenium import webdriver.common
    
    caps = desired_capabilities.FIREFOX
    driver = webdriver.Remote(command_executor="http://localhost:4444/wd/hub", desired_capabilities=caps)

#### Choosing a Browser to Test

The Turbo Selenium Hub determines which browser you would like to test against using the **DesiredCapabilities** of the WebDriver that the test is using.

All of the major language bindings have a **DesiredCapabilities** (or **desired_capabilities** in the case of Python) class that have attributes for each browser. Change this attribute to change the browser the test will run against.

    java
    // Java
    DesiredCapabilities.firefox(); 				// Mozilla Firefox
    DesiredCapabilities.chrome();				// Google Chrome
    DesiredCapabilities.ie();					// Internet Explorer

    csharp
    // C#
    DesiredCapabilities.Firefox();
    DesiredCapabilities.Chrome();
    DesiredCapabilities.InternetExplorer();

    python
    # Python
    desired_capabilities.FIREFOX
    desired_capabilities.CHROME
    desired_capabilities.INTERNETEXPLORER

To specify a version to test against, add a **version** capability into your existing test capabilities using a property setter or a `setCapability`/`SetCapability` instance method.

    java
    // Java
    capabilities.setCapability("version", "30");		// Test against version 30

    csharp
    // C#
    capabilities.SetCapability("version", "30");

In Python, you can modify the `desired_capabilities` just like a dictionary:

    python
    capabilities['version'] = '30'

#### Adapting Tests from Other Services

If you've used another cloud-based Selenium service in the past, switching over to Turbo.net is as easy as switching a couple of lines of code.

**BrowserStack**

When switching from BrowserStack, you'll need to make 2 changes to your existing code:

1. Delete the **browserstack.user** and **browserstack.key** capabilities from your test's capabilities, along with any other BrowserStack-specific commands or libraries.
2. Change the RemoteWebDriver hub URL to **http://localhost:4444/wd/hub**.

If you were passing your BrowserStack credentials through the hub url, the only required change is to change the RemoteWebDriver hub url from **[browser.user]:[browserstack.key]@hub.browserstack.com:80/wd/hub** to **http://localhost:4444/wd/hub**.

**Sauce Labs**

Sauce Labs provides 2 means of authentication when sending tests to their cloud server. If you are passing in your username and access key as DesiredCapabilities, you'll need to delete those capabilities and change the hub URL to **http://localhost:4444/wd/hub**.

If you are using the user-specific hub URL (**http://USERNAME:ACCESSKEY@ondemand.saucelabs.com:80/wd/hub**), change this to **http://localhost:4444/wd/hub** and you're ready to go!

**Testing Bot**

Testing Bot uses Selenium's **DesiredCapabilities** object as a means of authentication. When switching to Turbo.net, delete the **api_key** and **api_secret** capabilities from all of your tests - they are not necessary in Turbo.net.

If you were previously using any of the extension methods provided by the **TestingBotDriver**, those should also be removed from your code, as they may lead to unexpected results.

In addition to deleting Testing Bot-specific capabilities, change the hub URL in your tests from **http://hub.testingbot.com:4444/wd/hub** to **http://localhost:4444/wd/hub**.

### Debugging Tests

Unlike other cloud-based testing services, Turbo.net makes live debugging of your tests as easy as setting a breakpoint in your IDE.

When your test contains a breakpoint, the test will execute normally until it hits your breakpoint. At this point, all execution will freeze and the browser window will idle, waiting for the breakpoint to be passed. At this point, you can interact with the browser window as if it were natively installed. When you are done debugging, press **Continue** in your IDE, or continue to **Step Through** your code. Your test will then resume, with the browser continuing to respond to WebDriver commands from your test.

**Note**: The Selenium Sandbox is currently configured to automatically recycle any session that continues for 90 seconds without a successive command. This may cause your debugging session to end abruptly if it lasts longer than 90 seconds. We are currently working towards an option to turn this feature off for cases where a longer debugging time is required.

### Integrating the Selenium Sandbox with an Existing Grid

If you or your development team already have an in-house Selenium Grid, you can use the Selenium Sandbox to "fill in the gaps" in your Grid to support browsers that you may not have the capacity or hardware to host internally.

For example, if your in-house Grid is only large enough to host and support the latest versions of each browser, you can use the Selenium Sandbox to add legacy browsers to your Grid.

After initial configuration and setup, the Turbo Selenium hub will automatically provision and run tests on any browser that is not in your internal Grid.

#### Setup Guide

The Turbo Selenium hub has the same external API as the standard Selenium hub. You can connect external nodes to the Turbo Selenium hub just like you would a normal Selenium hub.

1. Start the Turbo Selenium hub
	1. *If using Turbo.net as part of your CI process*:
		1. Log on, or RDP in, to your build/test server (must be a Windows machine).
		2. Open a web browser and navigate to [http://turbo.net/selenium](/selenium). Log in with your Turbo.net username and password.
		3. Click **Run** next to the Selenium Grid to start the Turbo Selenium hub. If the Turbo.net plugin is not already installed, you will have to install it before the hub will start.
	2. *If using Turbo.net from a development machine*: Log in to [http://turbo.net/selenium](/selenium) with your Turbo.net username and password. Click **Run** next to the Selenium Grid to start the Turbo Selenium hub.
2. Connect your internal nodes to the Turbo Selenium hub.
	1. The hub will launch on port 4444 of the machine Turbo.net was accessed from. You can connect to the hub from a remote node by executing the following command (from the remote node): `java -jar selenium-server-standalone-2.xx.y.jar -role node -hub http://hub-machine:4444/grid/register`. For more information on configuring nodes with additional command line parameters, see [the official Selenium Grid Documentation](https://code.google.com/p/selenium/wiki/Grid2).

Once your Grid is configured, you can send your tests to it just as you normally would. If the hub cannot find a matching browser on your internal nodes, it will automatically provision a fresh browser from the Turbo Selenium Sandbox and run the test on it.

### Testing Internal Sites

Testing internal websites with the Selenium Sandbox is just as easy as testing public websites.

All browsers and test scripts run on your local machine, so there is no need for any special proxy configuration or modifications to the URL when testing an internal site.

The security benefit of this is that no browser data or network traffic passes through Turbo.net servers. 

Below is some example code demonstrating how you would run your test against an internal site running at **http://my-internal-server:8080**.

    java
    // Java
    driver.navigate().to("http://my-internal-server:8080");

    csharp
    // C#
    driver.Navigate().GoToUrl("http://my-internal-server:8080");

    python
    # Python
    driver.get("http://my-internal-server:8080")

### Testing Internet Explorer

When testing parallel instances of Internet Explorer with the Selenium Sandbox, we recommend setting the following options in your test.

1. Force Internet Explorer to use the Create Process API
2. Launch Internet Explorer with the -private flag (for **Internet Explorer 8+** only)

Using these settings helps prevent cookies and other session-specific items from being shared between different instances of Internet Explorer.

If you are not testing multiple instances of Internet Explorer in parallel, we recommend setting the **Ensure Clean Session** capability to **True**.

#### Configuring Internet Explorer Options

See below for language-specific instructions for how to properly configure your Internet Explorer tests for the Selenium Sandbox.

**Java**

    java
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

**C#**

    csharp
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

**Python**

    python
    # Create desired_capabilities
    capabilities = webdriver.DesiredCapabilities.INTERNETEXPLORER
    
    # Force Windows to launch IE through Create Process API and in "private" browsing mode
    capabilities['ie.forceCreateProcessApi'] = True
    capabilities['ie.browserCommandLineArguments'] = '-private'
    
    # Instantiate the driver
    driver = webdriver.Remote(command_executor="http://localhost:4444/wd/hub", desired_capabilities=capabilities)

#### Internet Explorer Container Configuration

Turbo.net's Internet Explorer containers are packaged and pre-configured to work with Selenium's Remote WebDriver without any end-user configuration. Each container is pre-configured with the following settings:

- Packaged with the latest IEDriverServer installed in the virtual environment's PATH.
- Protected Mode Enabled in all zones.

For Internet Explorer 10 and 11, Enhanced Protected Mode is Disabled.

For Internet Explorer 11, the registry key **HKLM\Software\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE** was added to the container with DWORD iexplore.exe and value 0.

For more information on these changes, as they relate to the InternetExplorerDriver, see [the Selenium documentation](https://code.google.com/p/selenium/wiki/InternetExplorerDriver).

#### Internet Explorer "Gotchas"

The InternetExplorerDriver in Selenium has some unique features that may cause unexpected test results. Below, we've compiled a list of some of the most commonly encountered "gotchas" we've seen testing Internet Explorer.

- When testing Internet Explorer, the browser zoom level should always be left at 100%. The IEDriverServer relies on this zoom level for native mouse events - configuring the zoom to be greater or less than 100% may cause inadvertent failures in your tests.
- If you are using a hover command in your tests, do not place your mouse over the Internet Explorer window. Doing so causes the hover to fail.

### Common Issues and Troubleshooting

#### Cannot connect to the Turbo Selenium Hub

Before proceeding, make sure the Turbo Selenium hub is running on your local computer. You can check this by opening a browser and going to [http://localhost:4444/grid/console](http://localhost:4444/grid/console).

**Solution 1: Check your Firewall**: This issue may occur if your computer has a restrictive firewall that blocks incoming and outgoing connections to/from your computer.

If possible, check your firewall and make sure port 4444 is not blocked. This is the port the Turbo Selenium hub listens for commands on. If this port is blocked, it must be unblocked before using the Selenium Sandbox.

#### Cannot Launch the Turbo Selenium Hub

Ensure that the TurboLauncher is installed and running. The Turbo Launcher can be downloaded from [http://start.turbo.net/install](http://start.turbo.net/install).

To run or restart the TurboLauncher once installed, go to the **Start Menu** > **All Programs** > **Startup** and select **TurboLauncher**.

#### After clicking **Run**, the Grid never launches

This issue occurs when the TurboLauncher is not activate or installed. If you have not installed the TurboLauncher, it can be downloaded from [http://start.turbo.net/install](http://start.turbo.net/install).

If the TurboLauncher is installed and you continue to see this issue, verify that your browser is not blocking the TurboLauncher from running on Turbo.net.

**Mozilla Firefox**

1. Navigate to [http://turbo.net/selenium](/selenium).
2. To the left of the browser's address bar, a "building block" icon should appear (it looks like a small LEGO).
3. Click this icon and a small box will appear beneath it with the dialog "Allow *Turbo.net* to run TurboLauncher?"
4. Select **Allow and Remember**
5. Refresh the page and click **Start Grid**.

**Google Chrome**

1. In the address bar, type **chrome://plugins**.
2. Locate **TurboLauncher** and check the **Always allowed** box.
3. Restart Google Chrome to apply this new setting.

#### Selenium Errors

**Internet Explorer does not launch with Error "IELaunchURL() returned HRESULT 80070012"**

This error occurs due to a bug in Selenium's IEDriverServer. For more information, see this issue report: [https://code.google.com/p/selenium/issues/detail?id=7045](https://code.google.com/p/selenium/issues/detail?id=7045).

To avoid this error, enable **ForceCreateProcessApi** in your test capabilities. For language-specific instructions, see **Testing Internet Explorer**.
