<html><center><h1>Overview of Mobile Automation</h1></center></html>


There are 3 types of application available in android.<br>
***1. Native Application***<br/>
***2. Web Application***<br/>
***3. Hybrid Application***<br/>
Here we have explained how to setup ***Mobile Automation*** using ***Appium*** for different applications in different types of mobile devices.<br>

## ❯ Table of Contents
- [Initial System Setup](#initial-system-setup)
- [Ways to get App package and App activity in Android Device](#ways-to-get-app-package-and-app-activity-in-android-device)
- [Verify the App package and app name](#verify-the-app-package-and-app-name)
- [Dependency for Appium](#dependency-for-appium)
- [Native Application Automation Simple Examples](#native-application-automation-simple-examples)
- [Mobile Web Application Automation Simple Examples](#mobile-web-application-automation-simple-examples)
- [Hybrid Application Automation Simple Examples](#hybrid-application-automation-simple-examples)
- [Parallel Execution on different Mobile Platforms](#parallel-execution-on-different-mobile-platforms)
- [Multiple Apps Automation](#multiple-apps-automation)
- [PageObjectModel For Android Pages](#pageobjectmodel-for-android-pages)
- [Debug Method To Detect Application Context And WebElement](#debug-method-to-detect-application-context-and-webelement)
- [Other Common Methods](#other-common-methods)
- [Get the BundleID of IOS inbuilt Apps](./MobileAutomationSolution.md#get-the-bundleid-of-ios-inbuilt-apps)
- []()
- []()
- []()
- []()
- []()
- []()
- []()
- []()

## Initial System Setup

* Developer option has to be enabled in the device
* USB Debugging Should be on
* ***Appium*** has to be installed. ***Desktop version*** as well as the ***appium node package***.<br/>
  Always install the appium server by relevant version of [Chrome driver](http://appium.io/docs/en/writing-running- appium/web/chromedriver/#automatic-discovery-of-compatible-chromedriver)<br/>
```cmd
    npm install appium --chromedriver_version="2.23"
    appium  // To start the server
```
* Install Android SDK. And set the following path.
 ```cmd
   C:\Users\test.user\AppData\Local\Android\Sdk\platform-tools;
   C:\Users\test.user\AppData\Local\Android\Sdk\tools\bin;
 ```
 * Check if ADB commands work or not

### ***STEP 1:*** Connect the Mobile Device

### ***STEP 2:*** CMD --> adb devices (You will be able to see the devices)
```cmd
   * daemon not running; starting now at tcp:5037
   * daemon started successfully
   List of devices attached
   16898cd3        device
```

### ***STEP 3:*** Install The Application
  * Method 1:<br/>
  Install it from playstore<br/>

  * Method 2:<br/>
  By Command ***adb install appname***<br/>

  * Method 3:<br/>
  By Drag and Drop the application into the Emulator<br/>
 
 
### ***STEP 4:*** Get the App Package And App Activity in case of Android<br/>
Details Given Bellow

### ***STEP 5:*** Open the Appium Desktop Client and start a session

### ***STEP 6:*** Set the Desired Capability<br/>
For Example, In case of Android
```json
   {
     "deviceName": "emulator-5554",
     "appPackage": "com.android.calculator2",
     "appActivity": "com.android.calculator2.Calculator",
     "platformName": "android",
     "noReset":true
   }
```
## Ways to get App package and App activity in Android Device

### Method 1:
For Mac/Linux:
```cmd
   adb shell dumpsys window | grep -E 'mCurrentFocus|mFocusedApp' 
```
For Windows:
```cmd
adb shell dumpsys window | find "mCurrentFocus"
```
### Method 2:
Trace the entire Mobile Logs
```cmd
   adb logcat
```
### Method 3:
If we have the apk file of the application.<br/>
Then we can extract that and see into the Manifest.xml and find the **Launcher Activity**

### Method 4:
Install any 3rd party applcation ***App Info APK**, and it will show the app package and app activity of all the Applications.

### Method 5:
```cmd
  aapt dump badging AppApkName.apk
  OR
  ./ aapt dump badging AppApkName.apk
```
## Verify the App package and app name
```cmd
adb shell am start -W -n com.package.name/com.zonier.app.ui.activities.activity -S -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -f 0x10200000
```
If the above command is successful the we have correctly found the app-package and app-activity

```json
{
  "deviceName": "16898cd3",
  "appPackage": "com.name.app",
  "appActivity": "com..app.ui.activities.login.LoginActivity",
  "platformName": "android"
}
```

## Dependency for Appium

The Java-Client Jar is used to get the ***Appium Dependencies***
```java
       <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>7.1.0</version>
        </dependency>
```
## Native Application Automation Simple Examples
```java
      public static void main(String[] args) throws MalformedURLException, InterruptedException {
              DesiredCapabilities caps = new DesiredCapabilities();
              caps.setCapability(MobileCapabilityType.PLATFORM_NAME, "Android");
              caps.setCapability(MobileCapabilityType.DEVICE_NAME, "16898cd3");
              caps.setCapability(AndroidMobileCapabilityType.APP_PACKAGE,"com.test.app");
              caps.setCapability(AndroidMobileCapabilityType.APP_ACTIVITY,"com.test.app.ui.activities.splash.SplashActivity");
              caps.setCapability(MobileCapabilityType.NO_RESET,true);

              String url = "http://0.0.0.0:4723/wd/hub";
              AppiumDriver driver=new AppiumDriver(new URL(url),caps);
              Thread.sleep(4000);
              AndroidElement userName=(AndroidElement) driver.findElement(By.id("com.test.app:id/et_email"));
              userName.clear();
              userName.sendKeys("test@gmail.com");
              Thread.sleep(2000);
              AndroidElement password=(AndroidElement) driver.findElement(By.id("com.test.app:id/et_password"));
              password.clear();
              password.sendKeys("Test@123456");
              Thread.sleep(2000);
              driver.hideKeyboard();
              AndroidElement logInBtn=(AndroidElement) driver.findElement(By.id("com.test.app:id/btn_login"));
              logInBtn.click();
              Thread.sleep(4000);
          }
```

## Mobile Web Application Automation Simple Examples
We have to use RemoteWebDriver for Mobile Web Application
```java
package com.simple.examples;

import io.appium.java_client.remote.MobileCapabilityType;
import java.net.MalformedURLException;
import java.net.URL;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;

public class BasicWebAppAutomation {

  public static void main(String[] args) throws MalformedURLException {
    DesiredCapabilities desiredCapabilities = new DesiredCapabilities();
    desiredCapabilities.setCapability(MobileCapabilityType.PLATFORM_NAME, "Android");
    desiredCapabilities.setCapability(MobileCapabilityType.DEVICE_NAME, "16898cd3");
    desiredCapabilities.setCapability(MobileCapabilityType.BROWSER_NAME, "Chrome");
    desiredCapabilities.setCapability(MobileCapabilityType.NO_RESET, true);

    URL url = new URL("http://127.0.0.1:4723/wd/hub");
    RemoteWebDriver driver;
    try {
      driver = new RemoteWebDriver(new URL("http://127.0.0.1:4723/wd/hub"), desiredCapabilities);
      driver.get("http://saucelabs.com/test/guinea-pig");
      WebElement div = driver.findElement(By.id("i_am_an_id"));
      System.out.println(div.getText()); //check the text retrieved matches expected value
      driver.findElement(By.id("comments"))
          .sendKeys("My comment"); //populate the comments field by id.

      driver.findElementByXPath("//form[@id='jumpContact']//input[@id='fbemail']").click();
      driver.getKeyboard().sendKeys("taral@gmail.com");
      Thread.sleep(10000);
      driver.quit();
    } catch (Exception e) {
      System.err.println("Error:" + e.getMessage());
    }
  }

}
```

## Hybrid Application Automation Simple Examples
Yet to be implemented


## Parallel Execution on different Mobile Platforms

In order to facilitate parallel execution across various mobile devices of same or different platforms we will be using appium with Selenium Grid. Selenium Grid is basically a proxy server that speaks the WebDriver protocol, and manages connections to a pool of WebDriver servers. It wasn't designed specifically to work with Appium, but because Appium also speaks the WebDriver protocol (and because we taught Appium how to register with Selenium Grid)
        
Architectural Diagram![image](https://user-images.githubusercontent.com/36564770/115127212-688e7280-9ff2-11eb-9bab-2a8035b1ceeb.png)


Details Steps:
*  ***Step 1:Set up Selenium Server in Hub Mode***<br/>
      Need to download Selenium-Standalone-Server and start it as hub mode in certain host or port.
      ```java
      java -jar selenium-server-standalone-2.53.0.jar -role hub -host 192.168.3.172 -port 4444
      ```
*  ***Step 2:Start Appium Servers in Node Mode***<br/>
      We can register any number of appium servers in node mode.

      * ***Appium Node 1: Android Node***
      ```java
      appium -a 192.168.3.172 -p 4723 --nodeconfig android_nodeconfig.json --chromedriver-executable "chromedriver.exe"
      ```
      Details of **android_nodeconfig.json**
```json
{
    "capabilities": [
        {
            "deviceName": "ZY224P7HZB",
            "browserName": "chrome",
            "version":"9",
            "maxInstances": 2,
            "platformName": "Android"
        }
    ],
    
"configuration": {
        "cleanUpCycle":3000,
        "timeout":30000,
        "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
        "maxSession": 2,
        "register": true,
        "registerCycle": 5000,
        "hubPort": 4444,
        "hubHost": "192.168.3.172"
    }
}

```
  * ***Appium Node 2: iOS Node***
```java
          appium -a 192.168.5.122  -p 4733 --nodeconfig iOS_nodeconfig.json
```
Details of iOS_nodeconfig.json:
```json
  {
      "capabilities": [
          {
              "deviceName": "iPad mini 3",
              "browserName": "Safari",
              "version":"12.1",
              "maxInstances": 2,
              "platformName": "i0S",
              "automationName": "XCUITest",
              "xcodeOrgId" : "",
              "xcodeSigningId" : "iPhone Developer",
          }
      ],

  "configuration": {
          "cleanUpCycle":3000,
          "timeout":30000,
          "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
          "maxSession": 2,
          "register": true,
          "registerCycle": 5000,
          "hubPort": 4444,
          "hubHost": "192.168.3.172"
      }
  }

```
*  ***Step 3:The testNgConfig File***<br/>
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
    <suite name="Suite 1" parallel="tests" thread-count="2">
        <!-- For sequential run use only platform parameter for both (iOS & Android) test cases-->
       <parameter name="hubUrl" value="http://192.168.3.117:4444/wd/hub"></parameter>
        <test name="Android_VerifiyNotifications1">
            <parameter name="deviceName" value="ZY224P7HZB" />
            <parameter name="platformVersion" value="9" />
            <parameter name="platform" value="Android" />
            <parameter name="browserName" value="chrome"/>
            <classes>
                <class name="com.basics.MobileParallelExecutionScript"/>
            </classes>
        </test>

        <test name="iOS_Execution">
            <parameter name="deviceName" value="iPad mini 3" />
            <parameter name="platformVersion" value="12.1" />
            <parameter name="platform" value="iOS" />
            <parameter name="browserName" value="Safari"/>
            <classes>
                <class name="com.basics.MobileParallelExecutionScript"/>
            </classes>
        </test>

    </suite>
```
*  ***Step 3:Java Class***<br/>
```java

        import io.appium.java_client.ios.IOSDriver;
        import io.appium.java_client.remote.MobileCapabilityType;
        import org.openqa.selenium.By;
        import org.openqa.selenium.remote.DesiredCapabilities;
        import org.openqa.selenium.remote.RemoteWebDriver;
        import org.testng.annotations.Parameters;
        import org.testng.annotations.Test;

        import java.net.MalformedURLException;
        import java.net.URL;

        public class MobileParallelExecutionScript {

            @Test
            @Parameters({"deviceName","platformVersion","platform","browserName","hubUrl"})
            public void test1(String deviceName,String platformVersion,String platformName, String browserName,String hubUrl) throws    MalformedURLException, InterruptedException {

                DesiredCapabilities caps = new DesiredCapabilities();
                caps.setCapability("platformName", platformName);
                caps.setCapability("deviceName", deviceName);
                caps.setCapability("browserName",browserName);
                caps.setCapability("platformVersion",platformVersion);
                caps.setCapability("noReset",true);
                //String url = "http://" + "192.168.3.140" + ":" + "4444" + "/wd/hub";
                String url=hubUrl;
                if(platformName.equalsIgnoreCase("Android")){
                    RemoteWebDriver driver=new RemoteWebDriver(new URL(url),caps);
                    driver.get("https://www.amazon.in/");
                    //driver.findElement(By.xpath("//span[text()='Fresh']")).click();
                    Thread.sleep(7000);
                    driver.quit();
                }else{
                    caps.setCapability(MobileCapabilityType.AUTOMATION_NAME,"XCUITEST");
                    caps.setCapability("xcodeSigningId", "iPhone Developer");
                    caps.setCapability("xcodeOrgId","3WS698JC32");
                    caps.setCapability(MobileCapabilityType.UDID,"bd21c102521b9dc2837ca16c530669006db71f1b");
                    caps.setCapability(MobileCapabilityType.BROWSER_NAME, "Safari");
                    caps.setCapability("startIWDP",true);
                    IOSDriver driver_ios=new IOSDriver(new URL(url),caps);
                    driver_ios.startRecordingScreen();
                    driver_ios.get("https://www.amazon.in/");
                    //driver_ios.findElement(By.xpath("//a[text()='Best Sellers']")).click();
                    Thread.sleep(7000);
                    driver_ios.stopRecordingScreen();
                    driver_ios.quit();
                }


            }

        }

```
*  ***Step 5:Video Browsing***<br/>


For Android, we can use Vysor Plugin of Chrome.

For iOS, we can perform driver.startRecording, and then open the address ip:9100 of the iOS node machine.

## Multiple Apps Automation

The ***testng-dualApps.xml***
```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
  <suite name="Suite 1" parallel="tests" thread-count="2">
      <!-- For sequential run use only platform parameter for both (iOS & Android) test cases-->
      <test name="Android_VerifiyNotifications1">
          <parameter name="deviceName" value="ZY224P7HZB" />
          <parameter name="platformVersion" value="9" />
          <parameter name="platform" value="Android" />
          <classes>
              <class name="com.basics.TestSuiteMutipleApps"/>
          </classes>
      </test>

  </suite>
```

The Java Class
```java
        import io.appium.java_client.android.Activity;
        import io.appium.java_client.android.AndroidDriver;
        import io.appium.java_client.remote.AndroidMobileCapabilityType;
        import org.openqa.selenium.By;
        import org.openqa.selenium.remote.DesiredCapabilities;
        import org.testng.annotations.Parameters;
        import org.testng.annotations.Test;

        import java.net.MalformedURLException;
        import java.net.URL;
        import java.util.concurrent.TimeUnit;

        public class TestSuiteMutipleApps {
            AndroidDriver driver;

            @Test
            @Parameters({"deviceName","platformVersion","platform"})
            public void testApp(String deviceName,String platformVersion,String platformName) throws MalformedURLException, InterruptedException {
                DesiredCapabilities caps = new DesiredCapabilities();
                caps.setCapability("platformName", platformName);
                caps.setCapability("deviceName", deviceName);
                //caps.setCapability("browserName",browserName);
                caps.setCapability("platformVersion",platformVersion);
                //caps.setCapability("noReset",true);

                // App1 capabilities
                String calculatorAppPackageName="com.google.android.calculator";
                String calculatorAppActivityName="com.android.calculator2.Calculator";

                // App2 capabilities
                String settingsAppPackageName="com.android.settings";
                String settingsAppActivityName="com.android.settings.Settings";
                //Calculator
                caps.setCapability(AndroidMobileCapabilityType.APP_PACKAGE, calculatorAppPackageName);
                caps.setCapability(AndroidMobileCapabilityType.APP_ACTIVITY, calculatorAppActivityName);
                driver = new AndroidDriver(new URL("http://localhost:4723/wd/hub"), caps);
                driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
                Thread.sleep(5000);
                //Perform calculation in calculator
                driver.findElement(By.id("com.google.android.calculator:id/digit_8")).click();
                driver.findElement(By.id("com.google.android.calculator:id/op_add")).click();
                driver.findElement(By.id("com.google.android.calculator:id/digit_7")).click();
                driver.findElement(By.id("com.google.android.calculator:id/eq")).click();

                //Validate results
                String result = driver.findElement(By.id("com.google.android.calculator:id/result_final")).getText();
                System.out.println("Result : " + result);

                Thread.sleep(2000);
                //launch settings App
                driver.startActivity(new Activity(settingsAppPackageName,settingsAppActivityName));
                Thread.sleep(3000);

                //Re launch calculator App
                driver.startActivity(new Activity(calculatorAppPackageName, calculatorAppActivityName));
                Thread.sleep(2000);

                //Assert.assertEquals("Incorrect Result", 15,(int) Integer.valueOf(result));
                driver.close();
                driver.quit();

            }

        }

```
## PageObjectModel For Android Pages

```java
      package com.pageobject;

      import com.utils.PropertyFileUtils;
      import io.appium.java_client.AppiumDriver;
      import io.appium.java_client.android.AndroidDriver;
      import io.appium.java_client.android.AndroidElement;
      import io.appium.java_client.remote.AndroidMobileCapabilityType;
      import io.appium.java_client.remote.MobileCapabilityType;
      import java.net.MalformedURLException;
      import java.net.URL;
      import java.util.HashMap;
      import org.openqa.selenium.remote.DesiredCapabilities;

      public class CustomMobileDriver {

        private static AppiumDriver<AndroidElement> appiumDriver;

        public AppiumDriver<AndroidElement> getAppiumDriver() {
          if (appiumDriver != null) {
            return appiumDriver;
          } else {
            return initAppiumDriver();
          }
        }

        private AppiumDriver initAppiumDriver() {
          PropertyFileUtils propertyFileUtils = new PropertyFileUtils(
              System.getProperty("user.dir") + "\\src\\test\\resources\\application.properties");
          HashMap<String, Object> propertyMap = propertyFileUtils.readPropertyFile();
          DesiredCapabilities caps = new DesiredCapabilities();

          if (propertyMap.get("platformName").toString().equalsIgnoreCase("android")) {
            caps.setCapability(MobileCapabilityType.PLATFORM_NAME, propertyMap.get("platformName"));
            caps.setCapability(MobileCapabilityType.DEVICE_NAME, propertyMap.get("deviceName"));
            caps.setCapability(AndroidMobileCapabilityType.APP_PACKAGE, propertyMap.get("appPackage"));
            caps.setCapability(AndroidMobileCapabilityType.APP_ACTIVITY, propertyMap.get("appActivity"));
            caps.setCapability(MobileCapabilityType.NO_RESET,
                Boolean.parseBoolean(propertyMap.get("noReset").toString()));
          } else {

          }

          String url = propertyMap.get("url").toString();
          try {
            appiumDriver = new AppiumDriver(new URL(url), caps);
          } catch (MalformedURLException e) {
            e.printStackTrace();
          }
          return appiumDriver;
        }


      }
```

The Page Object is as follows:
```java
      import com.pageobject.CustomMobileDriver;
      import io.appium.java_client.android.AndroidElement;
      import io.appium.java_client.pagefactory.AndroidFindBy;
      import io.appium.java_client.pagefactory.AppiumFieldDecorator;
      import org.openqa.selenium.support.PageFactory;

      public class LoginPage extends CustomMobileDriver {

        @AndroidFindBy(id = "com.test.app:id/et_email")
        private AndroidElement userName;

        @AndroidFindBy(id = "com.test.app:id/et_password")
        private AndroidElement password;

        @AndroidFindBy(id = "com.test.app:id/btn_login")
        private AndroidElement logInBtn;

        public LoginPage(){
          getAppiumDriver();
          //waitForPageToLoad();
          PageFactory.initElements(new AppiumFieldDecorator(getAppiumDriver()),this);
        }

        public void login(String userNameInp,String passwordInp) throws InterruptedException {
          userName.clear();
          userName.sendKeys(userNameInp);
          password.clear();
          password.sendKeys(passwordInp);
          getAppiumDriver().hideKeyboard();
          logInBtn.click();
          Thread.sleep(2000);
        }

      }
```
TestSuite Class
```java
import com.pageobject.pages.LoginPage;

public class TestSuite1 {

  public static void main(String[] args) throws InterruptedException {

    String username="test";
    String password="pass";
    LoginPage loginPage=new LoginPage();
    loginPage.login(username,password);
  }
}
```

## Debug Method To Detect Application Context And WebElement
```java
   /**
	 * @description : (DEBUG Method)Get all the Context Present in the APP
	 * 								And print all the Page Source included within each context
	 */
	public static void getContextAndPageSource(){

		Set<String> contextHandles=appiumDriver.getContextHandles();
		Iterator iterator=contextHandles.iterator();
		String contextName;
		while (iterator.hasNext()){
			contextName=iterator.next().toString();
			System.out.println("==================Start of :"+contextName+" context=========================");
			appiumDriver.context(contextName);
			System.out.println(appiumDriver.getPageSource());
			System.out.println("******************End of :  "+contextName+" context*************************");

		}
	}

	/**
	 * @description : (DEBUG Method) It find the status of the element
	 * 								whether it is present or not etc
	 * @param element
	 */
	public static void elementStatusFinder(AndroidElement element) {
		System.out.println("=====================================");
		System.out.println("Element:" + element.getAttribute("innerHTML"));
		System.out.println("Displayed:" + element.isDisplayed());
		System.out.println("Enabled:" + element.isEnabled());
		System.out.println("Selected:" + element.isSelected());
		System.out.println("=====================================");
	}
```

## Other Common Methods
```java
/**
	 * @description : This will scroll to specific Element if the element is visible
	 * @param element
	 */
	public static void scrollToElement(AndroidElement element){
		TouchActions action = new TouchActions(appiumDriver);
		action.scroll(element, 10, 100);
		action.perform();
	}

	/**
	 * @description : Swipe from Bottom to Top 80%Y-axis to 20%Y-Axis
	 * @throws InterruptedException
	 */
	public static void swipeFromBottomToTop() throws InterruptedException {
		Dimension size = appiumDriver.manage().window().getSize();
		System.out.println("Size:" + size);
		int starty = (int) (size.height * 0.80); //starty point which is at bottom side of screen.
		int endy = (int) (size.height * 0.20);   //endy point which is at top side of screen.
		int x = size.width / 2; //horizontal point where you wants to swipe

		TouchAction touchAction = new TouchAction(appiumDriver);
		touchAction
				.press(PointOption.point(x, starty))
				.waitAction(WaitOptions.waitOptions(Duration.ofMillis(2000)))
				.moveTo(PointOption.point(x, endy))
				.release().perform();
		Thread.sleep(2000);
	}

	/**
	 * @description : Swipe from Top to Bottom 20%Y-axis to 20%Y-Axis to 80%Y axis
	 * @throws InterruptedException
	 */
	public static void swipeFromTopToBottom(){

	}
```
Please find the complete project from the [link](https://github.com/kingshuknandy2016/CompleteMobileAutomation/tree/master/src)

# References:
* https://appiumpro.com/editions<br/>
* http://appium.io/docs/en/about-appium/intro/
