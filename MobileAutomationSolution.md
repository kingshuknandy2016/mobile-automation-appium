# Mobile Automation Troubleshooting and Solution

### TROUBLESHOOTING - IOS  ** BUNDLE ID --HOW TO GET THE BUNDLE ID OF IOS INBUILT APPS

These are from iPhone 4S iOS 5.0.1:
```java
	Camera:       com.apple.camera
	AppStore:     com.apple.AppStore
	Contacts:     com.apple.MobileAddressBook
	Mail:         com.apple.mobilemail
	GameCenter:   com.apple.gamecenter
	MobileSafari: com.apple.mobilesafari
	Preferences:  com.apple.Preferences
	iPod:         com.apple.mobileipod
	Photos:       com.apple.mobileslideshow
	Calendar:     com.apple.mobilecal
	Clock:        com.apple.mobiletimer						
```

### Datepicker without SendKeys:
```java
public static void selectYear(String Xpath, String year) {
      IOSElement element = (IOSElement) driver.findElementByXPath(Xpath);
      Integer yearInt = Integer.valueOf(year);
      String direction;
      if (Integer.valueOf(element.getAttribute("value").toString()) < yearInt) {
        direction = "UP";
      } else {
        direction = "DOWN";
      }
  
      Point point = element.getCenter();
      int endx = point.getX();
      int endy;
  
      if (direction == "DOWN") {
        endy = point.getY() - 80;
      } else {
        endy = point.getY() + 80;
      }
  
      while (!(driver.findElementByXPath(Xpath).getAttribute("value").toString().contains(year))) {
        ((IOSDriver) driver).tap(1, endx, endy, 1);
      }
  }
```
### TROUBLESHOOTING -  XPATH NOT WORKING(UNABLE TO CLICK)

* Try for the Tap option ***AndroidDriver (driver)***
```java
	driver.tap(fingers, element, duration);
	driver.tap(fingers, x, y, duration);
```
#### Common issues with Tap Option and there solution
* As trying to search element by its displayed text, there might be **white space** making mismatch. <br/>
  Please use **normalize-space()** function for xpath.<br/>
  
* The specific element's attribute **clickable** is **false** please use **tap** method from AppiumDriver

* The specific element is partially getting **overlapped** by checkbox<br/> 
We can get the location of the element and add some digit to it to get away from overlap and use **tap** method for location from AppiumDriver instance of the class.

* Try to check  the visibility of an element from the very next page.<br/> 
If its **not visible** then perform the click operation once again


### TROUBLESHOOTING -  RESET APPLICATION DATA
```java
	Driver.resetApp();
```

### Custom Template - Sendkeys not working for when application is expecting from numeric Keyboard

 Creating a method which can press the key as per key sequence. 
 This is working good, please see execution video: https://www.screencast.com/t/hNZQIFxDm5 
 ```java
 private static void sendNumericKeys(String keySequence) {
      List<IOSElement> keyBoardList = driver.findElementsByClassName("XCUIElementTypeKey");
      HashMap<String, IOSElement> keyBoardMap = new HashMap<String, IOSElement>();
        for (IOSElement ele : keyBoardList) {
          keyBoardMap.put(ele.getAttribute("label"), ele);
        }
      String[] keyArray = keySequence.split("(?!^)");
      for (String chr : keyArray) {
        keyBoardMap.get(chr).click();
      }

  }
```

### Common Methods:

```java
  public static void tabByCoordinates(AppiumDriver driver, WebElement element, int fingers,
      int duration) {
    driver.tap(fingers, element, duration);
  }

  public static void tabByElement(AppiumDriver driver, WebElement element, int fingers,
      int duration) {
    Point point = element.getLocation();
    driver.tap(fingers, point.x, point.y, duration);
  }

  public static void scrollToNativeElement1(AppiumDriver driver, WebElement element) {
    driver.scrollTo(element.getText().toString());
  }

  public static void clickWebelement(WebElement element) throws InterruptedException {
    try {
      element.click();
    } catch (Exception e) {
      Thread.sleep(2000);
      element.click();
    }
  }

  public static void clickWebelementOther(AppiumDriver driver, WebElement elementToBeClicked,
      WebElement elementNextPage) throws InterruptedException {
    elementToBeClicked.click();
    if (!elementNextPage.isDisplayed()) {
      Thread.sleep(2000);
				/*JavascriptExecutor js= JavascriptExecutor(driver);
				js.executeScript("Argument[0].setAttribute('visible','true')", elementToBeClicked);*/
      elementToBeClicked.click();
    }
  }

  public static void clickOnElement(AppiumDriver driver, WebElement elementToBeClicked) {
    if ((boolean) elementToBeClicked.getAttribute("clickable").equalsIgnoreCase("false")) {
      JavascriptExecutor js = (JavascriptExecutor) driver;
      js.executeScript("Argument[0].setAttribute('clickable','true')", elementToBeClicked);
    }
    elementToBeClicked.click();
  }

  public static void scrollToNativeElement(AppiumDriver driver, WebElement element) {
    driver.scrollTo(element.getText().toString());
    //driver.scrollToExact(arg0)	
  }

  public static void scrollToHalfScreensize(AppiumDriver driver, int startx, int starty, int endx,
      int endy, int duration) {
    driver.swipe(startx, starty, endx, endy, duration);
  }

  public static void swipeFromBottomToTop(AppiumDriver driver) throws InterruptedException {
    Dimension size = driver.manage().window().getSize();
    System.out.println("Size:" + size);

    //Find starty point which is at bottom side of screen.
    int starty = (int) (size.height * 0.80);

    //Find endy point which is at top side of screen.
    int endy = (int) (size.height * 0.20);

    //Find horizontal point where you wants to swipe. It is in middle of screen width.
    int startx = size.width / 2;

    //Swipe from Bottom to Top.
    driver.swipe(startx, starty, startx, endy, 1000);
    Thread.sleep(2000);

  }

  public static void swipeFromTopToBottom(AppiumDriver driver) throws InterruptedException {
    Dimension size = driver.manage().window().getSize();
    System.out.println("Size:" + size);

    //Find starty point which is at bottom side of screen.
    int starty = (int) (size.height * 0.80);

    //Find endy point which is at top side of screen.
    int endy = (int) (size.height * 0.20);

    //Find horizontal point where you wants to swipe. It is in middle of screen width.
    int startx = size.width / 2;

    //Swipe from Bottom to Top.
    driver.swipe(startx, endy, startx, starty, 1000);
    Thread.sleep(2000);

  }

  public static void swipeToNativeElement(AppiumDriver driver, WebElement elementStart,
      WebElement elementEnd, int duration) {
    int startx = driver.scrollTo(elementStart.getText()).getLocation().getX();
    int starty = driver.scrollTo(elementStart.getText()).getLocation().getY();
    ;

    int endx = driver.scrollTo(elementEnd.getText()).getLocation().getX();
    int endy = driver.scrollTo(elementEnd.getText()).getLocation().getY();
    driver.swipe(startx, starty, endx, endy, duration);
  }
}

public void scrollToElementAndTap(String name) throws InterruptedException {
		AndroidElement nameOfEmp;
		try {
			nameOfEmp = getAndroidElement("//*[contains(@text,'%s')]", name);
			if (nameOfEmp.isDisplayed()) {
				nameOfEmp.tap(1, 10);
			}
		} catch (Exception e) {
			swipeFromBottomToTop();
			scrollToElementAndTap(name);
		}
	}
	
	public static void swipeFromBottomToTop() throws InterruptedException {
		Dimension size = driver.manage().window().getSize();
		System.out.println("Size:" + size);

		// Find starty point which is at bottom side of screen.
		int starty = (int) (size.height * 0.20);

		// Find endy point which is at top side of screen.
		int endy = (int) (size.height * 0.80);

		// Find horizontal point where you wants to swipe. It is in middle of screen
		// width.
		int startx = size.width / 2;

		// Swipe from Bottom to Top.
		driver.swipe(startx, endy, startx, starty, 1000);
		// Thread.sleep(2000);
	}
	
	public static AndroidElement getAndroidElement(String locator, String args) {
		return (AndroidElement) driver.findElement(By.xpath(String.format(locator, args)));
	}
	
	public static void scrollToTop(String eleText, String platform) {
		WebElement qafElement = null;
		WebDriver driver1 ;

		if (platform.contains(CommonProperties.PlatformName.Android)) {
			((AppiumDriver) driver1).scrollTo(eleText);
			((AppiumDriver) driver1).swipe(100, 1275, 100, 275,
					1000);
		} else if (platform.contains(CommonProperties.PlatformName.iOS)) {
			int i = 0;
			TouchAction touchAction =
					new TouchAction((MobileDriver) driver1);
			Point point = null;
			try {
				point = driver1
						.findElement("")
						.getLocation();
			} catch (Exception e) {
			}
			int y = point.getY();
			i = 0;
			while ((i < 15) || driver1
					.findElement(String.format("", eleText))
					.getLocation().getY() < 300) {
				touchAction.press(50, 1300).moveTo(50, 1250).release();
				((MobileDriver) driver1)
						.performTouchAction(touchAction);
				i++;
			}

		}

	}
	
	public static void scrollTillDisplayed(String eleText, String platform) {
		boolean isElementDisplayed = false;
		

		if (platform.contains(CommonProperties.PlatformName.Android))
			((AndroidDriver) androidDriver).scrollTo(eleText);
		else if (platform.contains(CommonProperties.PlatformName.iOS)) {
			IOSDriver driver2 ;
			try {
				if (driver2.findElementByPartialLinkText(eleText).isDisplayed()) {
					isElementDisplayed = true;
				}
			} catch (WebDriverException e) {
				e.printStackTrace();
			}
			while (!isElementDisplayed) {
				driver2.swipe(750, 1000, 750, 850, 1000);
				if (driver2.findElementByPartialLinkText(eleText).isDisplayed()) {
					isElementDisplayed = true;
				}

			}
		}

	}
	
	public static void swipeRight(WebElement ele) {
		Point point = ele.getLocation();
		int startx = point.getX();
		int endx = startx + 500;
		int starty = point.getY() + 20;
		int endy = starty;
		
		((AppiumDriver) appiumDriver)
				.swipe(startx, starty, endx, endy, 500);
	}
	
	public static void swipHalfScreenUp() {

		TouchAction touchAction =
				new TouchAction((MobileDriver) mobileDriver);
		((IOSDriver) iosDriver).swipe(550, 1120, 550, 1290, 2);

		touchAction.press(550, 1290).moveTo(550, 1120).waitAction(2000).release();
	}
	
		public static void clickKeyBoardKey(String keyValue) {
		try {
			element.click();
		} catch (Exception e) {
			
			((IOSDriver) IosDriver).tap(1,
					element,
							keyValue, keyValue)),
					1);
		}

	}
	
	public static void selectFromSinglePicker(String text) {
		try {
			element.click();
		} catch (Exception e) {
					IOsDriver.tap(1, element, 1);
		}

	}
	
	public static void scrollToEle(String exactText) {
		try {
			AppiumDriver appiumDriver ;
			appiumDriver.scrollTo(exactText);
		} catch (SeleniumException e) {
			new getDriver().getTouchScreen().flick(0, (CommonUtilityFunctions.getIntNum("400")));
		}
	}
```
