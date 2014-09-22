{
  title: "Selenium Quickstart",
  description: "An introduction to Selenium and how to get started",
  category: "Reference",
  index: 8
}
##Introducing Selenium

###Automating Web Browser Interaction

Selenium is designed to **automate web browser interaction**, so that scripts can automatically perform the same interactions that any user can perform manually.  

Selenium can perform any sort of automated interaction, but was originally intended and is primarily used for automated web application testing.  Many features developed for Selenium - e.g. Selenium Grid - are intended for use in automated web application testing.

## History of Selenium  


#### Origins

Selenium was first developed by Jason Huggins in 2004.

Jason was  developing an internal time & expense application at ThoughtWorks.  Instead of having  to perform tests manually and repetitively, upon every iteration of development, Jason wished to automate testing, for all web browsers in use at ThoughtWorks.  

Jason developed a JavaScript library for automating web browser interactions, with support for multiple browsers.  With help from others at ThoughtWorks as well as outside associates, this library was developed to support remote interaction, various programming languages, and other notable early features of Selenium.  

Eventually, Selenium became an open source project.  Jason's library became Selenium Core in Selenium 1.0, and support for remote interaction became Selenium RC.  

In 2006, Simon Stewart, a software engineer at Google, began working on a project he called WebDriver, to implement web browser interaction in an object-oriented API, interacting through web browser APIs to avoid limitations of JavaScript and web browser support for it.

Also in 2006, Shinya Kasutani created Selenium IDE, a plugin for Firefox to assist with test script development, and donated it to the Selenium open source project.

In 2007, Jason joined Google, and in 2009, the Selenium and WebDriver projects were merged, to create Selenium 2.0, based on the WebDriver API.  

Currently, work on Selenium continues forward on a w3c specification for Selenium 3.0.

#### Selenium 1.0

The first release of Selenium 1.0 included:
 * **Selenese**, a scripting language for writing automated test programs
 * **Selenium client API**, an alternative to Selenese
 * **Selenium RC** (Remote Control), for communicating with servers
 
#### Selenium 2.0

Selenium 2.0 incorporates the WebDriver API and provides a full client-server architecture, which replaces Selenium 1.0 features including Selenium RC.  Selenium 2.0 includes: 

* the **WebDriver** API, with bindings for several leading programming languages (including Java, C#, Python, Ruby, and JavaScript)
* `Remote WebDriver`, a WebDriver API component supporting a client 
* **Selenium Grid**, a platform for remote automated testing

###Selenium as a Product Suite

Selenium comprises a product suite with features to support all aspects of automated web application testing.  In Selenium 2, this suite includes the following products and features:

* the Selenium IDE development environment, to assist with test script development
* **Selenium Client** and **Selenium Server**, implementing Selenium's client-server architecture
* the **Selenium WebDriver** API, implemented in both Selenium Client and Selenium Server
* the **Selenium Grid** test platform, as a feature implemented by Selenium Server

This document covers **Selenium WebDriver** and **Selenium Grid**.  

Selenium IDE is a Firefox plugin that provides a visual, page-based development environment to assist in developing Selenium test scripts.  It is covered by this [manual](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp).  Note that Selenium IDE is intended as an assistant, and not as a complete  development environment for test scripts.


###Selenium Architecture

Selenium has a **client-server architecture**, and includes both client and server components; these components are Selenium Client and Selenium Server.   
 
####Client and Server

Selenium Client includes:

* the **WebDriver** API, using which test scripts and page objects are be developed
* the `RemoteWebDriver` class, to communicate with a remote server


Selenium Server includes:

* a server component, to receive requests from Selenium Client *via* its `RemoteWebDriver` class
* the **WebDriver** API, to run against web browsers on a server machine
* **Selenium Grid**, implemented by Selenium Server in command-line options for grid features including a central hub and nodes for various environments and desired browser capabilities



####Installation Options and Run Modes

Selenium's client-server architecture supports several installation options and run modes, including:

* local (client-only) mode, with Selenium Client running directly against a web browser on the client machine
* remote (client-server) mode, with Selenium Client sending requests *via* its `RemoteWebDriver` class to Selenium Server 
* stand-alone (client+server), with both Selenium Client and Selenium server running on the same machine

These installation options and run modes are described more fully in "Installing Selenium".

## Getting Started with Selenium

### The Basic Steps

There are **seven basic steps** for using Selenium, which apply to any test case and any application under test (AUT).  

You can get started with Selenium by developing a test script with simple implementations of these steps, as given in the code examples below.

The seven basic steps are:

1. Create a WebDriver Instance

1. Navigate to a Web Page

1. Locate an HTML Element on a Web Page

1. Perform an Action on an HTML Element

1. Anticipate Browser Response

1. Run Tests and Record Test Results Using a Test  Framework

1. Conclude a Test

### The Example Use Case and Web Application

####The Login Use Case

The code examples below involve a login use case, which occurs as follows:

* a user **logs in** to a web application by entering a user name and a password, and submitting a form
* the web application **displays a login response message** indicating success or failure  

####The Foo Web Application

The Foo web application implements the login use case.  

The home page for the Foo web application at `www.foo.com` allows a user to log in.  The web page has a **login form** with HTML text inputs for entering a user name and a password, and with submit and reset buttons.  A test script for the login use case of the Foo web application must enter a user name and a password, and submit this login form.

After a user enters a user name and a password and submits the login form, the Foo web application displays a **login response message** on a web page.  A test script for the login use case of the Foo web application must test this login response message for success or failure. 

####HTML for the Login Web Page

Below is HTML source code for the web page with a login form at `www.foo.com`:    

```html
<html>
  <body>
  	...
    <form action="loginAction" id="loginForm">
	  <label>User name:&nbsp;</label>
	  <input type="text" name="username"><br>
	  <label>Password:&nbsp;</label>
	  <input type="text" name="password"><br>
	  <button type="submit" id="loginButton">Log In</button>
	  <button type="reset" id="reset">Clear</button>
    </form>
    ...
  </body>
</html>
```	

####HTML for the Login Response Message

Below is HTML source code for the login response message displayed after submitting the login form:    

```html
<html>
  <body>
  	...
    <p class="message" id="loginResponse">Welcome to foo. You logged in successfully.</p>
  	...
  </body>
</html>
```

Note that, in the HTML given above, the login response message indicates success, but a similar login response message may indicate failure.

####Identifying HTML Elements

Following good HTML coding practices, the Foo application uses values of `name` or `id` attributes to identify HTML elements on a web page uniquely. 

On the login web page, the username and password text input elements are  identified uniquely by the values of their `name` attributes - `username` and `password`, respectively.  

The login form element is identified uniquely by the value of its `id` attribute, `login`.


 

The login response message paragraph has a generic `message` class, but is identified uniquely by the value of its `id` attribute, `loginResponse`.


### Creating an Instance of the `WebDriver` Interface

####The `WebDriver` Interface

The `WebDriver` interface is the starting point for all uses of the Selenium WebDriver API. 

You create an instance of the `WebDriver` interface using a constructor for a specific web browser.  The names of these constructors vary over web browsers, and invocations of constructors vary over programming languages.  

Once you have created an instance of the `WebDriver` interface, you use this instance to invoke methods  and to access other interfaces used in basic steps.  You do so by assigning the instance to a variable when you create it, and by using that variable to invoke methods.

####Example

The following example creates an instance of the `WebDriver` interface for Firefox, and assigns a variable named `driver` to this instance:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

WebDriver driver = new FirefoxDriver();
```

### Navigating to a Web Page

####The `get` Method

You navigate to a web page by invoking the `get` method on the unique instance of the `WebDriver` interface, e.g. on the `driver` variable.

The `get` method takes an argument value for the URL of the web page.  This value can be a string value, or an instance of a special type representing a URL or URI.

####Example

The following example invokes the `get` method on the `driver` variable to navigate to the web page at `www.foo.com`, passing a string argument value for its URL:


```java
driver.get("http://www.foo.com");
```	

### Locating an HTML Element on a Web Page

In order to interact with a web page, you first locate HTML elements on the web page, then perform actions on those elements, such as entering text (for text input elements) or clicking (for button elements).  

####Locator Expressions

You use a **locator expression** to locate a unique HTML element or a specific collection of HTML elements.  

A locator expression is a pair containing a **locator type** and a **locator value** for that type.  

The locator type indicates which aspects of any HTML element on a web page are evaluated and compared to the locator value in order to locate an HTML element.  An aspect of an HTML element indicated by a locator type can include:

* a specific **attribute** such as `name` or `id`
* the **tag name** of the element, such as `form` or `button`
* for hyperlink elements or anchor tags, the visible **linked text**, such as `Foo` in `<a href="http://www.foo.com">Foo</a>`
* any aspects given by a **CSS** selector, such as `...` 
* any aspects given by an **XPath** expression, such as `//form[@id="loginForm"]` or `//button[@type='submit']`. 

####Locator Methods on the `By` Class

The WebDriver API provides several **locator methods** to form locator expressions.  Each locator method corresponds to a locator type, and forms a locator expression containing that type and a locator value passed as an argument when invoking the method.  

In the WebDriver API for Java, locator methods are defined as `static` or class methods on the `By` class (whose name connotes that an HTML element is located *by* comparing an evaluated locator type  to a locator value).  For example, `By.name("password")` forms a locator expression whose locator type indicates the `name` attribute and whose locator value is the string `"password"`.

####Finder Methods and the `WebElement` Interface

To use locator expressions formed by locator methods, the WebDriver API provides two **finder methods**, `findElement` (singular) and `findElements` (plural), both of which take a locator expression as an argument value.  

Typically, a locator method is invoked on the `By` class in the argument position of a finder method to form a locator expression as argument value in a single line of code - e.g. `findElement(By.name("password"))`.  


In the example below, the `findElement` and `findElements` finder methods are invoked on the unique instance of the `WebDriver` interface - e.g. on the variable `driver`, as in `driver.findElement(By.name("password"))`.  

The finder methods search the DOM (Document Object Model) tree for the web page, evaluating locator types for HTML elements, and comparing their values to the locator value.  

The return value of the `findElement` (singular) method is an instance of the `WebElement` interface, which represents an element of any HTML type and which is used to perform actions on the element.  The `findElement` (singular) method returns a `WebElement` for the first HTML element in the DOM tree for which the evaluated locator type matches the locator value.  The `findElements` (plural) method returns a list of elements - in the WebDriver API for Java, a `List<WebElement>` - for all HTML elements on the web page for which the evaluated locator type matches.

####Example

The following example invokes the `findElement` method on the `driver` variable, using the `name` attribute to locate the `username` and `password` text input elements, and (optionally) the `id` attribute to locate the `form` element:


#####Locate HTML text input elements for username and password
```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;

WebElement usernameElement 	= driver.findElement(By.name("username"));
WebElement passwordElement 	= driver.findElement(By.name("password"));
```
#####Optionally locate the HTML form element 
```java
WebElement formElement		= driver.findElement(By.id("loginForm"));
```

### Performing an Action on an HTML Element

####Basic Interaction Methods on the `WebElement` Interface

You perform an action on an HTML element by invoking an interaction method on an instance of the `WebElement` interface.

The `WebElement` interface declares basic interaction methods including:

* the `sendKeys` method, to enter text
* the `clear` method, to clear entered text
* the `submit` method, to submit a form 

####Example

The following example first invokes the `sendKeys` method to enter text in the `username` and `password` elements, and then invokes the `submit` method to submit the `login` form.  

Note that the `submit` method can be invoked either on any text input element on a form, or on the form element itself; code for both these options is given in the following example.


#####Enter a user name and a password
```java
usernameElement.sendKeys("Alan Smithee");
passwordElement.sendKeys("twilightZone");
```	
#####Submit the form
```java
passwordElement.submit();	// submit by text input element
```

or

```java
formElement.submit();		// submit by form element
```

### Anticipating Browser Response

You anticipate web browser response in a test script in order to test effectively despite possible variation in web browser response times.

Anticipating web browser response generally means waiting for a definite or an indefinite elapsed time for the web browser to respond and the web page to load.

The WebDriver API supports two basic techniques for anticipating browser response by waiting: **implicit waits** and **explicit waits**.  

####Implicit Waits

Implicit waits set a definite, fixed elapsed time that applies to all `WebDriver` interactions. Using implicit waits is not a best practice because web browser response times are not definitely predictable and fixed elapsed times are not applicable to all interactions.  Using explicit waits requires more technically sophistication, but is a best practice.

####Explicit Waits

Explicit waits wait until an **expected condition** occurs on the web page, or until a maximum wait time elapses.  To use an explicit wait, you create an instance of the `WebDriverWait` class with a maximum wait time, and you invoke its `until` method with an expected condition.  

The WebDriver API provides an `ExpectedConditions` class with methods for various standard types of expected condition.  These methods return an instance of an `expected condition` class.  You can pass an invocation of these standard expected-condition methods as argument values to `until` method. You can also pass - in ways that your programming language and its WebDriver API support - any function, code block or closure that returns a boolean value or an object reference to a found web element as an argument value to the `until` method.  How this is done varies over programming languages, and is covered in depth in the Developing section of this documentation.  The `until` method checks repeatedly, until the maximum wait time elapses, for a `true` boolean return value or a non-`null` object reference, as an indication that the expected condition has occurred.

####Example

The following example gives code for setting both an explicit wait and an implicit wait to anticipate web browser response after submitting the login form:

##### Explicit Wait

```java
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

WebDriverWait wait = new WebDriverWait(driver, 10);
WebElement messageElement = wait.until(
		   ExpectedConditions.presenceOfElementLocated(By.id("loginResponse"))
 	       );
```

##### Implicit Wait

```java
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
```
*Recall that using an implicit wait is* not *a best practice*.

### Running Tests and Recording Test Results

Running tests and recording test results is the ultimate purpose of your test script: you run tests in an automated test script in order to **evaluate function and performance** in the AUT, without requiring human interaction.

####Test Frameworks

To run test and to record test results, you use methods of a **test framework** for your programming language.  There are many available test frameworks, including the frameworks in the so-called **XUnit** family, which includes:
* **JUnit** for Java
* **NUnit** for C#
* **unittest** or **pyunit** for Python
* **RUnit** for Ruby

For some programming languages, test frameworks other than those in the XUnit family are common - for example, the **RSpec** framework for Ruby.

####Assertions

Most test frameworks implement the basic concept of an **assertion**, a method representing whether or not a logical condition holds after interaction with an AUT.  Test frameworks generally declare methods whose names begin with the term `assert` and end with a term for a logical condition, e.g. `assertEquals` in JUnit.  Generally, when the logical condition represented by an `assert` method does not hold, an exception for the condition is thrown.  There are various approaches to using exceptions in most test frameworks.

####Recording Test Results

Recording of test results can be done in various ways, supported by the test framework or by a logging framework for the programming language, or by both together.  Selenium also supports taking screenshots of web browser windows as a helpful additional type of recording.  Because of the wide variations in recording technique, this beginning section omits recording, instead emphasizing a simple approach to applying a test using an `assert` method.

####Example

The following example runs a test by asserting that the login response message is equal to an expected success message:

```java
import junit.framework.Assert;
import junit.framework.TestCase;

WebElement messageElement 	= driver.findElement(By.id("loginResponse"));
String message 				= messageElement.getText();
String successMsg	 		= "Welcome to foo. You logged in successfully."
assertEquals (message, successMsg);
```

### Concluding a Test

####The `quit` Method

You conclude a test by invoking the `quit` method on an instance of the `WebDriver` interface, e.g. on the `driver` variable.  

The `quit` method concludes a test by disposing of resources, which allows later tests to run without resources and application state affected by earlier tests.  The `quit` method:

* quits the web browser application, closing all web pages
* quits the WebDriver server, which interacts with the web browser
* releases `driver`, the variable referencing the unique instance of the `WebDriver` interface.   

####Example

The following example invokes the `quit` method on the `driver` variable:

```java
driver.quit();
```

###Example with All Steps 

The following example includes code for all steps.  The example also defines a Java test class `Example`, and its `main` method, so that the code can be run.

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;

import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import junit.framework.Assert;
import junit.framework.TestCase;


public class Example  {
  public static void main(String[] args) {

	// Create an instance of the driver
	WebDriver driver = new FirefoxDriver();

	// Navigate to a web page
	driver.get("http://www.foo.com");

	// Perform actions on HTML elements, entering text and submitting the form
	WebElement usernameElement 	= driver.findElement(By.name("username"));
	WebElement passwordElement 	= driver.findElement(By.name("password"));
	WebElement formElement		= driver.findElement(By.id("loginForm"));

	usernameElement.sendKeys("Alan Smithee");
	passwordElement.sendKeys("twilightZone");

	//passwordElement.submit();	// submit by text input element
	formElement.submit();		// submit by form element


	// Anticipate web browser response, with an explicit wait
	WebDriverWait wait = new WebDriverWait(driver, 10);
	WebElement messageElement = wait.until(
		   ExpectedConditions.presenceOfElementLocated(By.id("loginResponse"))
 	       );

	// Run a test
	WebElement messageElement 	= driver.findElement(By.id("loginResponse"));
	String message 				= messageElement.getText();
	String successMsg	 		= "Welcome to foo. You logged in successfully."
	assertEquals (message, successMsg);

	// Conclude a test
	driver.quit();
	
  }
}	    
```

## Installing Selenium for Java

### Introduction

####Download Webpage

You can download installable files for Selenium from the SeleniumHQ downloads page, [here](http://docs.seleniumhq.org/download/).  

The SeleniumHQ downloads page has helpful information for downloading items in the Selenium product suite, including:
* Selenium IDE
* Selenium Server
* Selenium Client and WebDriver Language Bindings, for various programming languages

The SeleniumHQ downloads page shows the most recent releases for these items.  Files for recent releases are also [here](http://selenium-release.storage.googleapis.com/index.html), on the API release webpage. 

####Selenium Installables

#####Physical Files vs. Logical Components

The installables of any application (including Selenium) can be described as:

* **physical files**, that are downloaded, unarchived, moved between directories, and referenced in class paths or build paths
* **logical components** of an application, each of which is implemented in some physical file

Note that installable physical files may include only one archived downloadable file, but an archived downloadable file may contain many  directories and files to be unarchived, moved and referenced.

#####Installable Logical Components in Selenium

In Selenium, installable logical components include the following, by client and server:

|Side|Logical Component|Role|
|----|----|----|
|**Client**|Selenium Client|WebDriver client running test scripts and<br> `WebDriver` or `RemoteWebDriver`|
||WebDriver Language Bindings|Supports coding in programming languages<br>including Java, C#, Python, Ruby and JavaScript|
||`RemoteWebDriver` Class|Allows a WebDriver client to communicate<br>with a remote server|
|**Server**|Selenium Server|WebDriver server running WebDriver and web browsers|
||Selenium Server<br>Stand-Alone|Allows both client and server to run<br>on the same machine|
||Selenium Grid|Supports grid features including a central hub and nodes for various environments and desired browser capabilities|
 

#####Installable Physical Files for Selenium


The logical components for client and server in Selenium each come in a single archived physical file - so you never have to download more than  two files to perform  a complete installation of Selenium. 

Selenium Client is implemented with in a file with its binding for a programming language, and there are different installable client files for different languages (e.g. Java, C#, Ruby, Python and JavaScript).  You choose a file for your programming language on the downloads webpage, [here](http://docs.seleniumhq.org/download/). The `RemoteWebDriver` class is always included with Selenium Client, regardless of whether or not this class or a local class is used. 

Selenium Server is implemented in Java, and its archived file includes one `.jar` file for the server only, and another `.jar` file for its "stand-alone" version, so that Selenium Client and Selenium Server can run on the same machine.  Selenium Grid is a feature of Selenium Server, invoked by special command-line options for a central hub and for nodes at server startup.  

Each installable archived file for client or server contains unarchived files and directories including, for Java:
* a "sources" file with source code, e.g. `selenium-java-x.y.z-srcs.jar` with `.java` files 
* a file with compiled code, e.g. `selenium-java-x.y.z.jar` with `.class` files 
* a `libs` directory with more `.jar` files 

The two archived installable files for client and server have the same name - i.e. `selenium-x.y.z`, where `x`, `y` and `z` number the release - so it is important for you to take care in downloading and moving these files during installation.  

####Summary of Installables

The following table summarizes installable physical files and logical components as described above:

|Archived `.zip` File|Unarchived `.jar` Files & `libs` Directory|Logical Component|
|---|---|---|
|`selenium-x.y.z`|`selenium-java-x.y.z-srcs.jar`,<br>`selenium-java-x.y.z.jar`,<br>`libs` directory|Selenium Client|
||(same as above)|WebDriver Language Bindings|
||(same as above)|`RemoteWebDriver` class|
|`selenium-x.y.z`|`selenium-server-x.y.z-srcs.jar`,<br>`selenium-server-x.y.z.jar`,<br>`libs` directory|Selenium Server<br>(Java only)|
||`selenium-server-standalone-x.y.z.jar`|Selenium "stand-alone"<br>(Java only)|



####Installation Options & Run Modes

Generally, there are three installation options and associated run modes for Selenium:

* **local** (client-only) installation, using only Selenium Client, with web browsers also running locally, on the client machine
* **remote** (client-server) installation, using both Selenium Client and Selenium Server, with web browsers running remotely, on the server machine
* **"stand-alone"** (client+server) installation, with both Selenium Client and Selenium Server installed on the same machine, with web browsers also running on the same machine


#####Local (Client-Only)

With the local (client-only) installation and run mode, the `WebDriver` interface is used (instead of the `RemoteWebDriver` class), and the WebDriver API interacts directly with a specific web browser API, through a subclass used to instantiate the `WebDriver` interface for that specific web browser.  This is now possible for most web browsers, though originally it required either Firefox or the HTMLUnit web browser emulator, which runs only in a "headless" or undisplayed mode.

For the local (client-only) installation option and run mode, only Selenium Client is installed.  

#####Remote (Client-Server)

With the remote (client-server) installation and run mode, Selenium runs in true client-server configuration. Selenium Client communicates with a remote server *via* the `RemoteWebDriver` class, and can set desired capabilities for the remotely-running web browser and platform.  Selenium Server runs remotely, responding to client requests issued *via* `RemoteWebDriver`, and using the `WebDriver` interface to interact with web browsers running on the server machine.  You can also run Selenium Server using Selenium Grid in remote (client-server) run mode, by first starting a Selenium Server as a central hub and then starting other Selenium Servers as nodes to match desired capabilities for web browser and platform.  This run mode has advantages as a sophisticated test environment due to server features, but involves latencies between the client and the server.

For the remote (client-server) installation option and run mode, both Selenium Client and Selenium Server are installed, on separate machines.

#####Stand-Alone (Client+Server)

In stand-alone (client+server) installation and run mode, Selenium Client and Selenium Server both run, on the same machine.  An advantage of this mode is that latencies of remote run mode are eliminated, but sophistication of the test environment due to server features is retained.

For the stand-alone (client+server) installation option and run mode, the stand-alone version of installable physical files is used, which installs both Selenium Client and Selenium Server.

The following table summarizes installation options and installable physical files required:

|Installation Option|Selenium Client|Selenium Server|
|----|----|----|
|Local<br>(client-only)|installed (on client)|not installed|
|Remote<br>(client-server)|installed (on client)|installed|
|"Standalone"<br>(client+server)|installed (on server,<br>with `server-standalone` file)|installed<br>(with `server-standalone` file)|




### Basic Steps

####Install Selenium Client

1. **Download** the installable file for Selenium Client to a client machine.  

  For Java, this is a `.zip` file named `selenium-java-x.y.z.zip`, where `x`, `y` and`z` number the release.


2. **Unzip** the installable file for Selenium Client, and **move** the unzipped top-level folder to an appropriate installation folder for Selenium on the client machine..

  For Java, this top-level folder is a folder named `selenium-x.y.z`.  This top-level folder contains two top-level `.jar` files, `selenium-java-x.y.z-srcs.jar` and `selenium-java-x.y.z.jar`, and a `libs` directory with more `.jar` files.
  
3. **Add** the `.jar` files to the class path or build path for your Selenium project.  

  This can be done in your IDE, or by setting environment variables.  

####Install Selenium Server (Optional)

1. **Download** the installable file for Selenium Server to a server machine.

  This is a `.zip` file named `selenium-server-x.y.z.zip`, where `x`, `y` and `z` number the release.
  
2. **Unzip** the installable file for Selenium Server, and **move** the unzipped top-level folder to an appropriate installation folder for Selenium on the server machine.  

  This top-level folder is a folder named `selenium-x.y.z` (the same name as the top-level folder for Selenium Client).  This top-level folder contains three top-level `.jar` files, `selenium-server-x.y.z.srcs.jar`, `selenium-server-x.y.z.jar` and `selenium-server-standalone-x.y.z.jar`, and a `libs` directory with more `.jar` files. 
  
  The "standalone" version allows you to run both Selenium Client and Selenium Server on the same machine.  
  
3. If you are also using Selenium Client on the server machine, then **add** the `.jar` files to the class path or build path for your Selenium project.

  Again, this can be done in your IDE, or by setting environment variables.
  

  

###Starting the  Selenium Server

#### Simple Start Command

You can start Selenium Server simply, without command-line options, by following these steps on the server machine:

1. Open a terminal window

1. Change directory to the installation folder for Selenium on the server machine - e.g. `cd <seleniumDir>` or (if you've defined the environment variable) `cd $WEBDRIVER_HOME`

1. Enter an appropriate `java -jar ...` command, as follows. 

  To run Selenium Server with a remote client, enter:
  
  
  ```java
  java -jar selenium-server-x.y.z.jar
  ```  
  To run Selenium Server as a "standalone", enter 
  
  ```java
  java -jar selenium-server-standalone-x.y.z.jar
  ```
  
#### Start Command with Options

You can also start Selenium Server using the following command-line options:

|Option|Description|
|----|----|
|browserTimeout|Sets how long the browser is allowed to wait (value in seconds)|
|timeout| Sets how long the client is is allowed to be gone before the session is reclaimed by the server (value in seconds)|

You can start Selenium Server using these command-line options as follows:

```java
java -jar selenium-server-x.y.z.jar -timeout=20 -browserOption=60
```
## Locating HTML Elements on a Web Page

### Introduction

In order to interact with a web page, you first locate HTML elements on the web page, then perform actions on those elements, such as entering text (for text input elements) or clicking (for button elements).  

####WebDriver Support for Locating HTML Elements

The WebDriver API provides **locator methods** defined on the `By` class and **finder methods** declared on the `WebDriver` and `WebElement` interfaces to support locating HTML elements on a web page.

You locate an HTML element by:

* forming a **locator expression** using a **locator method** of the `By` class
* passing a locator expression as an argument value to a **finder method** of the `WebDriver` or `WebElement` interface


####Locator Expressions

You use a **locator expression** to unique identify an HTML element or a specific collection of HTML elements.  

A locator expression is a pair containing a **locator type** and a **locator value** for that type.  

The locator type indicates which aspects of any HTML element on a web page are evaluated and compared to the locator value in order to locate an HTML element.  An aspect of an HTML element indicated by a locator type can include:

* a specific **attribute** such as `name` or `id`
* the **tag name** of the element, such as `form` or `button`
* for hyperlink elements or anchor tags, the visible **linked text**, such as `Foo` in `<a href="http://www.foo.com">Foo</a>`
* any aspects given by a **CSS** selector, such as `form#loginForm` or `button,input[type='submit']`
* any aspects given by an **XPath** expression, such as `//form[@id='loginForm']` or `//button[@type='submit']`. 

####Locator Methods on the `By` Class

The WebDriver API provides several **locator methods** to form locator expressions.  Each locator method corresponds to a locator type, and forms a locator expression containing that type and a locator value passed as an argument when invoking the method.  

In the WebDriver API for Java, locator methods are defined as `static` or class methods on the `By` class (whose name connotes that an HTML element is located *by* comparing an evaluated locator type  to a locator value).  For example, `By.name("password")` forms a locator expression whose locator type indicates the `name` attribute and whose locator value is the string `"password"`.

The following table describes the locator methods of the `By` class:

|Locator<br>Type| Locator Method| Behavior	|  
 :------------	| :-----------	| :-----------	| 
 **Class** | `By.className("foo")` | Forms a locator expression comparing `"foo"` to the value of the `class` attribute|
 **CSS Selector** | `By.css("form#foo")`|Forms a locator expression for comparing the evaluated CSS expression - e.g. the `id` attribute value of a form element - to the value "foo" |
 **Identifier**  |`By.id("foo")`, `By.identifier("foo")` |Select the first element with `id` attribute value `"foo"`. If there is no such element, then select the element whose name attribute value is `value`. 
 **Link Text**  | `By.linkText("Foo")`| Selects the link (anchor tag `<a href="...">Text to Match</a>`) with text _content_ matching `value` 
 **Partial<br> Link&nbsp;Text**  |`By.partialLinkText("Foo")` |
 **Name** | `By.name("foo")` |Select the first element with `name` attribute value `value`
**Tag&nbsp;Name** | `By.tagName("form")` |Select the first element with tag name  `form` - i.e. the first `<form ...>` element
 **XPath** |`By.xpath("//form[@id='foo']")` |Forms a locator expression for comparing the evaluated XPath expression - e.g. the `id` attribute of a form element - to the value "foo"
 
#####Declaration

Locator methods all have similar declarations as `static` or class methods of the `By` class.  An typical locator method with a name given by the schema variable `<locatorMethod>` is declared as follows: 

```java
static By <locatorMethod> (String locatorValue);
```

#####Arguments & Return Type

A locator method takes an argument value of type `String` for the locator value .  There are no  argument subtypes for special kinds of argument, such as CSS selector or XPath expression, as type-checking is done on the locator expression and not on the locator value.

A locator method returns an instance of the `By` class that encapsulates a locator expression.  This instance is further typed as an instance of a nested class that represents the locator type - e.g. `By.ByClassName`, `By.ByCssSelector`, `By.ById`, `By.ByLinkText`, `By.ByName`, `By.ByPartialLinkText`, `By.ByTagName`, `By.ByXPath`.  This approach allows type-checking to be done on the encapsulated locator expression.  

#####Behavior

A locator method takes an argument value of type `String` for the locator value, and returns an instance of the `By` class that encapsulates a locator expression, an instance of a nested class representing the locator type.

There is no further special behavior of a locator method, although finder methods do implement special behavior.

####Finder Methods and the `WebElement` Interface

To use locator expressions formed by locator methods, the WebDriver API provides two **finder methods** - singular and plural:

* `findElement` (singular)
* `findElements` (plural)

#####Declaration

The `findElement` and `findElements` methods are defined on both the `WebDriver` and `WebElement` interfaces, as follows:

```java
WebElement 			findElement (By by);
List<WebElement> 	findElements (By by);
```   

As described below, the behavior of a finder method depends upon which interface - `WebDriver` or `WebElement` - is used to invoke the method.  Both `WebDriver` and `WebElement` implement the `SearchContext` method, so that the type of instance used to invoke gives the search context for the finder method. Actually, invocation is further delegated to finder methods of the `By` class, with take a `SearchContext` interface - `WebDriver` or `WebElement` - as an argument value. 

#####Arguments & Return Types

Both finder methods take an argument value that is an instance of the `By` class encapsulating a locator expression.  

The `findElement` (singular) method returns an instance of the `WebElement` interface, a generic interface type that can represent an HTML element of any HTML type.  This `WebElement` instance can be used to:

* interact with the element
* inspect the element
* continue finding HTML elements within the context of the element (e.g. `option` elements within an HTML `select` element)

The `findElements` (plural) method returns a specific collection of HTML elements, in Java, represented as a generic `List` with `WebElement` as its type parameter - i.e. `List<WebElement>`, a list of instances of the `WebElement` interface.

#####Behavior

The behavior of a finder method depends upon which interface - `WebDriver` or `WebElement` - is used to invoke the method.  The type of instance used to invoke gives the search context for the finder method. 

When the instance used to invoke a finder method is the unique instance of the `WebDriver` interface, the search context is the web page in the current or active browser window.

When the instance used to invoke a finder method is the `WebElement` interface, the search context is the context or contents of that web element.  This style of invocation is used when there are nested elements - e.g. `option` elements of a `select` element.   

The finder methods search the DOM (Document Object Model) tree for the web page, evaluating locator types for HTML elements, and comparing their values to the locator value.  

The `findElement` (singular) method returns the first `WebElement` whose locator type or HTML aspects evaluate to match the locator value.

The `findElements` (plural) method returns a list of all `WebElements` in the search context whose locator type or HTML aspects evaluate to match the locator value.  

#####Exceptions

The finder methods throw a `NoSuchElement` exception just when an element is not found whose locator type or HTML aspects evaluate to match the locator value.

#####Invocation


Typically, a finder method is invoked in a single line of code, with a locator method invoked on the `By` class in its argument position - e.g. `findElement(By.name("password"))`. 

To begin finding, a finder method is invoked on the unique instance of the `WebDriver` interface - e.g. on the variable `driver`, as in `driver.findElement(By.name("password"))`.

###Location Strategies

Using certain locator types and methods together represents a **location strategy** for automating interaction with the AUT.

Generally, using the value of the `id` attribute of an element as a unique identifier is an HTML coding best practice and the intended use of this attribute.  Using the `name` attribute of an element - especially a text input element - as a unique identifier is a good or at least an acceptable consistent practice.  

However, in practice, AUTs can vary in approach, consistency and effectiveness in using an attribute to identify an HTML element.

In order to respond to such variation, you can use locator methods that take an expression as an argument value, and which allow a combination of conditions on HTML aspects of elements.  These locator methods include the `css` method for a CSS selector and the `xpath` method for an XPath expression.  

There has been debate about performance and other considerations in choosing between CSS and XPath as locator strategy options, but ultimately, this choice has to do with the expressive power offered by CSS or XPath syntax when identifying an HTML element.

So a good location strategy would involve a combination of the following locator methods, applied as appropriate for an element to be located:

1. by `name` or `id` attribute value
2. by CSS selector or by XPath expression


###The Example 

####HTML for the Login Web Page

Below is HTML source code for the web page with a login form at `www.foo.com`:    

```html
<html>
  <body>
  	...
    <form action="loginAction" id="loginForm">
	  <label>User name:&nbsp;</label>
	  <input type="text" name="username"><br>
	  <label>Password:&nbsp;</label>
	  <input type="submit" name="password"><br>
	  <button type="submit" id="loginButton">Log In</button>
	  <button type="reset" id="reset">Clear</button>
    </form>
    ...
  </body>
</html>
```	
####HTML for the Login Response Message

Below is HTML source code for the login response message displayed after submitting the login form:    

```html
<html>
  <body>
  	...
    <p class="message" id="loginResponse">Welcome to foo. You logged in successfully.</p>
  	...
  </body>
</html>
```

### Locating an HTML Element by Id

This location strategy locates an HTML element on a web page by matching the value of the `id` attribute of an element.

Consider following excerpt from the HTML page source:

```html
    <form action="loginAction" id="loginForm">
		...
    </form>
```	

The login form element has an `id` attribute value of `"loginForm"`.  

Using this `id` attribute value to create a locator expression, the following finder and locator method invocations return an instance of the `WebElement` interface representing the login form element:

```java
WebElement formElement = driver.findElement(By.id("loginForm"));
```

###Locating an HTML Element by Name

This location strategy locates an HTML element on a web page by matching the  value of the `name` attribute of an element.

Consider following excerpt from the HTML page source:

```html
	  <label>User name:&nbsp;</label>
	  <input type="text" name="username"><br>
	  <label>Password:&nbsp;</label>
	  <input type="password" name="password"><br>
```	

The user name and password text input elements have `name` attribute values of `"username"` and `"password"`, respectively.  

Using these `name` attribute values to create locator expressions, the following finder and locator method invocations return instances of the `WebElement` interface representing the user name and password text input elements:

```java
WebElement usernameElement = driver.findElement(By.name("username"));
WebElement passwordElement = driver.findElement(By.name("password"));
```

###Locating an HTML Element by Tag Name

This location strategy locates an HTML element on a web page by matching the  element name or **"tag" name** of an element.  This is the name that appears at the beginning of an opening tag and the ending of any closing tag.

Consider following excerpt from the HTML page source:

```html
    <form action="loginAction" id="loginForm">
		...
    </form>
```	

The login form element is the only form element on the page, and so can be uniquely identified by its element name or "tag" name.

Using the tag name `"form"` as in the opening tag `<form...` to create a locator expression, the following finder and locator method invocations return an instance of the `WebElement` interface representing the login form element:

```java
WebElement formElement = driver.findElement(By.tagName("form"));
```

###Locating an HTML Element by Class

This location strategy locates an HTML element on a web page by matching the  **class name** - i.e. the value of the `class` attribute - of an element.  

Consider following excerpt from the HTML page source for the login response page:

```html
    <p class="message" id="loginResponse">Welcome to foo. You logged in successfully.</p>
```

The response paragraph element is the only element on this page of class `message`, and so can be uniquely identified by its class.

Using the class name `"message"` to create a locator expression, the following finder and locator method invocations return an instance of the `WebElement` interface representing the response paragraph element:

```java
WebElement responseElement = driver.findElement(By.class("message"));
```


###Locating a Hyperlink Element by Link Text

This location strategy locates a hyperlink element on a web page - i.e. an element created by opening and closing **"anchor tags"** `<a ...>` and `</a>`, as in:
```html
<a href="www.some_url.com">Some Visible Text</a>`.  
```

The strategy of locating an HTML element by link text locates an HTML element by matching anchor tags and the value of the **visible text** inside the opening and closing anchor tags.  The visible text is what is properly considered *text*, and not the URL that is the value of the `href` attribute.

In the example anchor tags above, such a match would be on the visible text `"Some Visible Text"`, but not on the value of the `href` attribute, `"www.some_url.com"`.  Consequently, a locator expression with the value `"Visible"` would match, but a locator expression with the value `"url.com"` would *not* match.

Further, there are locator methods to match **all** or only **part** of the visible text, i.e. to match the "full" text or some partial text.  Both such methods are shown in code examples below.

Consider following HTML - with a hyperlink added - as the page source for the login response web page:

```html
<html>
  <body>
    <p class="message" id="loginResponse">
      Welcome to Foo. You logged in successfully.
    </p><br>
    <a href="www.foo.com">Now return to Foo.</a>
  </body>
</html>
```

The "return to foo" hyperlink element has visible text `"Now return to Foo."`, and can be located by matching all or part of this text.

Using the partial text `"return"` or the full text `"Now return to Foo."` to create a locator expression, the following finder and locator method invocations each return an instance of the `WebElement` interface representing the "return to foo" hyperlink:


```java
WebElement responseElement = 
	driver.findElement(By.partialLinkText("return"));
```
```java	
WebElement responseElement = 
	driver.findElement(By.linkText("Now return to Foo."));
```


### Locating an HTML Element by CSS Selector 

This location strategy locates an HTML element on a web page by any valid CSS selector.

Using a CSS selector is helpful in situations in which none of the attribute, tag or content strategies above is sufficient alone to identify an element, but a logical combination of strategies may be.

For example, suppose that we do *not* know about web pages in the AUT:

* whether the HTML5 `<button type='submit'>` or the older HTML `<input type='submit'>` is used for the submit button on a form
* whether the paragraph tag `p` or the `div` tag is used for the login response message and other messages
* whether the form is identifiable by an `id` attribute value such as `"loginForm"` or by a form `action` attribute value such as `"loginAction"`

You can use the following CSS selectors:

* `foo` selects any element with tag name `foo` - i.e. `<foo ...>`
* `foo,bar` selects any element with tag name `foo` or tag name `bar`
* `foo#'bar'` selects an element with tag name `foo` and `id` attribute value `'bar'`
* `[foo='bar']` selects any element with `foo` attribute value `bar`
* `[foo='123'][bar='456']` selects any element with `foo` attribute value `'123'` *and* `bar` attribute value `'456'`

Using CSS expressions, the following finder and locator method invocations return the submit button, login response message and login form, respectively.

```java
WebElement submitElement = driver.findElement(By.cssSelector("button,input[type='submit']"));
```
```java
WebElement responseElement = driver.findElement(By.cssSelector("div,p[class='message']"));
```
```java
WebElement formElement = driver.findElement(By.cssSelector("form#'loginForm'"));
```
### Locating an HTML Element by XPath Expression 

This location strategy locates an HTML element on a web page by any valid XPath expression.  

Using an XPath expression is helpful in situations in which none of the attribute, tag or content strategies above is sufficient alone to identify an element, but a logical combination of strategies may be.

For example, suppose that we do *not* know about web pages in the AUT:

* whether the HTML5 `<button type='submit'>` or the older HTML `<input type='submit'>` is used for the submit button on a form
* whether the paragraph tag `p` or the `div` tag is used for the login response message and other messages
* whether the form is identifiable by an `id` attribute value such as `"loginForm"` or by a form `action` attribute value such as `"loginAction"`

You can use the following XPath expressions: 

* `//foo` selects elements with tag name `foo`
* `//foo#bar` selects an element with tag name `foo` and `id` attribute value `bar`
* `[@type='bar']` selects elements with a `type` attribute whose value is `bar`
* `|` is an "or" operator that combines XPath conditions on tags, ids and attributes

Using XPath expressions with an "or" operator `|`, the following finder and locator method invocations return the submit button, login response message and login form, respectively.

```java
WebElement submitButton = driver.findElement(By.xpath("//input[@type='submit']|//button[@type='submit']"));
```
```java
WebElement responseElement = driver.findElement(By.xpath("//p[@id='loginResponse']|//div[@id='loginResponse']"))
```
```java
WebElement formElement = driver.findElement(By.xpath("//form[@id=loginForm]|//form[@action='loginAction']"));
```



## Navigating in a Web Browser

###Introduction


###WebDriver Support for Navigating in a Browser

In Java, the WebDriver API provides two nested interfaces of the `WebDriver` interface to support navigating in a web browser:

* the `WebDriver.TargetLocator` interface, to support navigating to windows, frames, modal dialogs or alerts, and HTML elements
* the `WebDriver.Navigation` interface, to support navigating to a web page and through web browser history

####Accessing the `TargetLocator` and `Navigation` Interfaces

Further, the `WebDriver` interface  declares a special method to get an instance of each of these interfaces:
* the `switchTo` method,  to get an instance of the `TargetLocator` interface
* the `navigate` method, to get an instance of the `Navigation` interface  

### Navigating Using the `TargetLocator` Interface

The `TargetLocator` interface supports navigating to:

* windows
* frames
* modal dialogs or alerts
* HTML elements 

The `TargetLocator` interface declares special methods for navigating to each of these types of UI object.  

####The `switchTo` Method

You use the `switchTo` method to get an instance of the `TargetLocator` interface and to invoke its navigation methods.

#####Declaration

The `switchTo` method is declared on the `WebDriver` interface as follows:


```java
WebDriver.TargetLocator switchTo();
``` 
#####Arguments & Return Value

The `switchTo` method:

* takes no arguments
*  returns an instance of the `TargetLocator` interface for the unique instance of the `WebDriver` interface. 

#####Behavior

The `switchTo` method returns an instance of the `TargetLocator` interface for the unique instance of the `WebDriver` interface.  There is no special behavior of the `switchTo` method other than returning this instance.

You use this instance to invoke navigation methods of the `TargetLocator` interface.


#####Exceptions

Because the `switchTo` method is invoked on the unique instance of the `WebDriver` interface to return an instance of its nested `TargetLocator` interface, the `switchTo` method does not throw special exceptions.

####Using the `switchTo` Method 

You use the `switchTo` method of the `WebDriver` interface to get an instance of the `TargetLocator` interface.

You invoke the `switchTo` method on an instance of the `WebDriver` interface, e.g. on the `driver` variable.

You use this instance of the `TargetLocator` interface to invoke its navigation methods.

Typically, you invoke the `switchTo` method first, then a navigation method, in the same line of code.

This is done in the first of the following examples, which give a preview of navigation methods by invoking the `window` method, given a handle to an open window.  The second of the following examples has a first line using the `switchTo` method to get an instance of the `TargetLocator` interface, followed by a second line using this instance to invoke the `window` navigation method.


```java
driver.switchTo().window(windowHandle);
```
```java
WebDriver.TargetLocator targetLocator = driver.switchTo();
targetLocator.window(windowHandle);
```

Note that this use is similar to using the `navigate` method of the `WebDriver` interface to access the `Navigate` interface and its navigation methods, which support navigating to a web page and through web browser history.


####Navigation Methods on the `TargetLocator` Interface

The `TargetLocator` interface declares methods supporting navigation to various types of UI component or HTML element, including:

* the `activeElement` method, to switch focus to an HTML element
* the `alert` method, to navigate to a modal dialog or alert
* the `defaultContent` method, to navigate to the default content of the web page|
* the `frame` method, to navigate to a frame or iframe
* the `parentFrame` method, to navigate to a frame or iframe that is the parent of the selected frame
* the `window` method, navigate to a window

|Method|Navigation|
|----|----|
|`activeElement`|switch focus to an HTML element|
|`alert`|navigate to a modal dialog or alert|
|`defaultContent`|navigate to the default content of the web page|
|`frame`|navigate to a frame or iframe|
|`parentFrame`|navigate to a frame or iframe that is the parent of the selected frame|
|`window`|navigate to a window|


These methods are covered in sections below for various types of navigation, including:

* Navigating to a Window
* Navigating to a Dialog
* Navigating to a Frame

There is also a final section covering:

* Navigating through Web Browser History Using the `Navigation` Interface


### Navigating to a Window

The `TargetLocator` interface declares the `window` method for navigating to a window.

For use with the `window` method, the `WebDriver` interface also declares the following methods to get window handles:
 
* a **window handle** method (singular), to get a handle to the current or active window
* a **window handles** method (plural), to get handles to all open windows


####The `window` Method

You use the `window` method to navigate to a window, by name or handle.


#####Declaration
The `window` method is declared on the `TargetLocator` interface as follows:


```java
WebDriver window(java.lang.String windowHandle);
```
#####Arguments

|Type|Name<br> (Example)|Description
|---|---|---|
|`String`|`windowHandle`|Name or handle for the window to which to navigate.|

The `window` method takes a single argument value that is the name of the window or a handle for the window as an object.

#####Behavior

Because the `window` method takes a single argument value that is the name or handle for the window, a test script must track open windows by:
* getting and saving the name or handle for the current or active window before navigating to another window, or 
* getting a collection of names or handles for all open windows.  

When there is a window with this name or handle, the `window` method returns an instance of the `WebDriver` interface.  This allows window-related methods declared on the `WebDriver` interface - such as the `getTitle` method - to be used on the return value of the `window` method.

#####Exceptions

When there is no window with this name or handle, the `window` method throws a `NoSuchWindowException`.  

|Exception|Thrown|
|---|---|
|`NoSuchWindowException`|Thrown when there is no window with the name or handle |

#####Invocation

You invoke the `window` method using an instance of the `TargetLocator` interface returned by the `switchTo` method of the `WebDriver` interface.  

This is commonly done in a single line of code, as follows:


```java
driver.switchTo().window(windowNameOrHandle);
```

####The Window Handle Methods

You use methods of the `WebDriver` interface to open and to navigate to windows, as follows:

* the `get` method, to open a new window and to navigate to a web page
* the `window handle` method (singular), to get a handle for the current or active window
* the `window handles` method (plural), to get a set of handles for all open windows

You use the `window handle` method (singular) of the `WebDriver` interface to get a handle for the current or active window, after using the `get` method to open a new browser window and to navigate to a web page.

You use the `window handles` method (plural) of the `WebDriver` interface to get a set of handles for all open windows.

The `window handle` and `window handles` methods are declared on the `WebDriver` interface as follows:


```java
java.lang.String getWindowHandle();
```
```java
java.util.Set<java.lang.String> getWindowHandles();
```

####Using the `window` Method with a Window Handle

The following example:
* opens one window, navigates to one web page, gets the window handle and saves it in a first variable
* opens another window, navigates to another web page, gets the window handle and saves it in a second variable
* switches to the first window using its window handle in the first variable
* switches to the second window using its window handle in the second variable

The example uses:
* the `get` method to open windows and to navigate to a web page
* the `window` method to navigate to a window




```java
// Create an instance of WebDriver
WebDriver driver = new FirefoxDriver();

// Open window one and get its handle
driver.get("www.foo.com/pageOne.html");
String windowOne = driver.getWindowHandle();

// Open window two and get its handle
driver.get("www.foo.com/pageTwo.html");
String windowTwo = driver.getWindowHandle();

// Switch back to window one
driver.switchTo().window(windowOne);
```
 
####Using the `window` Method with a Set of Window Handles


The following example:
* gets handles for all open windows using the `window handles` method
* prints the number of open windows
* loops over window handles
* switches to a window in an iteration of the loop using the `window` method
* gets and prints the window title in an iteration of the loop
* closes any window with the title "Bar" in an iteration of the loop 

The example assumes use of the `get` method to open windows and to navigate to a web page.



```java
// Get handles for all open windows
Set<String> handles = driver.getWindowHandles();

// Get and print the number of open windows
System.out.println("There are " + handles.size() + " open.");

// Loop over window handles, navigate to a window and close if "Bar"
String title = "";
String closeTitle = "Bar";
for (String handle : handles) {
	driver.switchTo().window(handle);
	title = driver.getTitle().trim();
	System.out.println("The window title is: " + title);
	if (title.equals(closeTitle)) {
		driver.close();
	}
}
```

####Using the `window` Method with a Window Name

Instead of using a window handle, you can sometimes navigate to a window by the **window name**.  

A name is given to a window by the AUT when the window is opened by such as:

```html
<a href="bar.html" target="barWindow">Open the Bar window.</a>
```
Here, the hyperlink or anchor tag sets the window name as the value of its `target` attribute.  

The following example:
* locates the hyperlink or anchor tag element by partial link text
* gets the value of the `target` attribute using the `getAttribute` inspection method (covered later)
* clicks on the hyperlink to open the window
* waits for the window to open and the web page to load (commented here, covered later)
* uses the window name given as the value of the `target` attribute of the hyperlink to navigate to the window

The example uses the `window` method to navigate, but with a window name, instead of a window handle:

```java
// Locate the hyperlink
WebElement barLinkElement = driver.findElement(By.partialLinkText("Bar"));

// Get the window name as the value of the target attribute
String windowName = barLinkElement.getAttribute("target");

// Open the window by clicking
barLinkElement.click();
...
// wait here
...
// Navigate to the window by name
driver.switchTo().window(windowName);
```

### Navigating to a Modal Dialog

A modal dialog is called an **alert** in WebDriver and other windowing systems or APIs.

The `TargetLocator` interface declares the `alert` method to navigate to a modal dialog or alert.

For use *after* using the `alert` method, the `WebDriver` interface also provides the `Alert` interface, which declares methods to interact with an alert.

###The `alert` Method

You use the `alert` method to navigate to a modal dialog or alert. 

#####Declaration

The `alert` method is declared on the `TargetLocator` interface as follows:


```java
Alert alert();
```

#####Arguments

Because there is at most one modal dialog or alert open in the application, the `alert` method takes no arguments, and navigates to the unique current or active alert.


#####Behavior

The `alert` method navigates to the unique current or active alert.

When there is an alert, the `alert` method returns an instance of the `Alert` interface.   This instance is then used to interact with the alert.


####Exceptions

|Exception|Thrown
|---|---|
|`NoAlertPresentException`|Thrown when there is no modal dialog or alert.|

When there is no alert, the `alert` method throws a `NoAlertPresentException`.

#####Invocation

To invoke the `alert` method, you use an instance of the `TargetLocator` interface returned by the `switchTo` method of the `WebDriver` interface.  This is typically done in a single line of code, as follows:


```java
Alert alert = driver.switchTo().alert();
```

#### Interaction Methods of the `Alert` Interface

You use methods of the `Alert` interface to interact with an alert.  These methods include:


* the `accept` method, similar to clicking the `OK` button
* the `authenticateUsing` method, to authenticate using name and password *via* an authentication alert
* the `dismiss` method, similar to clicking the `Cancel` button
* the `getText` method, to get the text displayed by the alert
* the `sendKeys` method, to enter text into a text input on the alert

### Navigating to a Frame

The `TargetLocator` interface declares the following methods for navigating to a frame:

* the `frame` method
* the `defaultContent` method
* the `parentFrame` method

####The `frame`,  `defaultContent` and  `parentFrame` Methods

You use the `frame`, `defaultContent` and  `parentFrame` methods of the `TargetLocator` interface to navigate to a frame in a window.

#####Declaration

The `frame`,  `defaultContent` and  and  `parentFrame` methods are declared on the `TargetLocator` interface as follows. 

In Java, the `frame` method is overloaded, and declared explicitly with argument types for:
* an integer index
* a string name or id
* an HTML element, i.e. a previously-located instance of the `WebElement` interface

```java
WebDriver frame(int index);
```
```java
WebDriver frame(String nameOrId);
```
```java
WebDriver frame(WebElement webElement);
```
```java
WebDriver defaultContent();
```

```java
WebDriver parentFrame();
```

#####Arguments


The `frame` method takes an argument that is the index, the name or id, or a previously-located `WebElement` for the frame.

|Type|Name<br> (Example)|Description|
|---|---|---|
|`int`|`index`|the index of the frame (zero-based), as a child of a parent in the DOM|
|`String`|`nameOrId`|the name or id of the frame |
|`WebElement`|`frameElement`|the `WebElement` instance for a previously-located frame |

The `defaultContent` method takes no arguments.

The `parentFrame` method takes no arguments.


#####Behavior

The `frame` method navigates (sets focus) to the frame indicated by the index, name or id, or `WebElement` instance.

The `frame` method returns the unique instance of the `WebDriver` interface, with the frame selected (in focus) - e.g. so that `driver.parentFrame()` gets the frame's parent.

The `defaultContent` method navigates either to the first frame of the web page or to the main document for web pages using iFrames.

The `parentFrame` method navigates (sets focus) to the parent context of the current context, e.g. the current frame or element.  When the current context is the top-level context, focus remains on the current context.


#####Exceptions

When there is no frame with the index, or name or id - or when the  `WebElement` instance references an element that is not a frame - the `frame` method throws a `NoSuchFrameException`.

When the `WebElement` reference is no longer valid - i.e. when it references an element in a window that is no longer open - the  `frame` method throws a `StaleElementReferenceException`.

|Exception|Thrown
|---|---|
|`NoSuchFrameException`|Thrown when there is no frame for the index, name or id, or when the `WebElement` instance is not a frame.|
|`StaleElementReferenceException`|Thrown when the `WebElement` reference is no longer valid - i.e. when it references an element in a window that is no longer open.|

#####Invocation

You invoke the `frame` method using an instance of the `TargetLocator` interface returned by the `switchTo` method of the `WebDriver` interface.  This is typically done in a single line of code, as follows:


```java
driver.switchTo().frame(0);
```
####Using the `frame` and `defaultContent` Methods Together

You use the `frame` and `defaultContent` methods together for many common cases of navigation between multiple frames on a web page.

Navigating using the `frame` and `defaultContent` methods involves understanding frame indexes for web pages, default content for web pages with iFrames, and navigation using the `frame` method.

#####Navigating by Index

The DOM for a web page assigns successive indexes in a node list to frames that are child elements of the same parent element; these indexes begin with zero.  

Navigating by index to a frame is relative to a current element: the `frame` method navigates by index to frames that are child elements of the current element as parent.  

Once you navigate to a frame, however, that frame *becomes* the current element, and navigation by index is then to its subframes, i.e. to any frames that are child elements of the current frame element as parent.  Consequently, in order to navigate by index from a frame to its sibling frame - i.e. to another frame that is a child element of the same parent - you must first navigate to the parent element or to an element that is an ancestor of the parent.  

The `defaultContent` method supports such navigation, by navigating to the "default content" for the web page - which is generally the node for the web page itself or the top-level frame.  

So, given a web page `threeFrames.html` with three unnamed child frames each with a text box with a display name for the frame, you can navigate to those frames by index as follows:


```java
// Navigate to the web page with three frames
driver.get(www.foo.com/threeFrames.html);

// Navigate to frame 0
driver.switchTo().frame(0);
WebElement nameElement = driver.findElement(By.className("name"));
String name = nameElement.getText().trim();
System.out.println("There name of the frame is: " + name);

driver.switchTo().defaultContent();

// Navigate to frame 1
driver.switchTo().frame(1);
WebElement nameElement = driver.findElement(By.className("name"));
String name = nameElement.getText().trim();
System.out.println("There name of the frame is: " + name);

driver.switchTo().defaultContent();

// Navigate to frame 2
driver.switchTo().frame(2);
WebElement nameElement = driver.findElement(By.className("name"));
String name = nameElement.getText().trim();
System.out.println("There name of the frame is: " + name);

``` 


#####Navigating by Name or Id

Navigating to a frame by name or id is similar to navigating by index in that navigation is relative to a current element.  However, navigating to a frame by name or id path differs from navigating by index in that a **path expression** can also be used as the name, so that you can navigate downward through many levels of frame with a single invocation of the `frame` method.

To navigate by name or id, the `frame` method compares its string argument value to the `name` and `id` attribute values of HTML frames or iFrames that are children of the current element. When its string argument value is a path expression with dot (`.`) delimiters, the `frame` method navigates by path, relative to the current element, comparing the string after the last dot (`.`) to the `name` and `id` attribute values of HTML frames or iFrames that are children of the last element on the path.  Path expressions can mix string names or ids and integer indexes, using either to reference any frame in a path.

Suppose a web page has three levels of frame: 
* a top-level frame, with `name="frameTop"`
* three vertically-stacked intermediate frames for header, body and footer, without names but with assigned indexes `0,1,2`
* left, right and center frames for the body frame, with `name="frameLeft"`, `name="frameRight"`, `name="frameCenter"`

```
|-frameset
  |- frameTop
    |- frameset
  	  |- header frame (index O)
  	  |- body frame (index 1)
  	    |- frameset
  	      |-frameLeft
  	      |-frameCenter
  	      |-frameRight
  	  |- footer frame (index 2)
```

To navigate to the frame with `name="frameLeft"` in a tree of frame elements as above, you write the following:


```java
driver.navigate().frame("frameTop.1.frameLeft");
```

####Using the `parentFrame` Method

After having navigated to a child frame, you use the `parentFrame` method to navigate to its parent frame or context.

In the example above, after having navigated to `frameLeft`, you use the `parentFrame` method to navigate to `frameTop` as shown below. Invoking the `parentFrame` method successively in a single line of code works because the `parentFrame` method returns an instance of the `WebDriver` interface with the parent frame (or context) selected (in focus):


```java
driver.navigate().parentframe().parentFrame();
```

###Navigating through Web Browser History Using the `Navigation` Interface

The `Navigation` interface supports navigating to a web page and through web browser history, as you would manually using web browser controls to:
* enter a URL in the address bar
* go forward, to the next web page 
* go back, to the previous web page
* refresh the current web page

####The `navigate` Method

You use the `navigate` method to get an instance of the `Navigation` interface and its navigation methods.

#####Declaration

The `navigate` method is declared on the `WebDriver` interface as follows:

```java
WebDriver.Navigation navigate();
``` 

#####Arguments

The `navigate` method takes no arguments, because it returns an instance of the `Navigation` interface for the unique instance of the `WebDriver` interface.

#####Behavior

The `navigate` method returns an instance of the `Navigation` interface for the unique instance of the `WebDriver` interface.  This instance is used to invoke navigation methods of the `Navigation` interface.


#####Exceptions

Because the `navigate` method is invoked on an instance of the `WebDriver` interface, the method does not throw exceptions.


####Using the `navigate` Method 

You use the `navigate` method of the `WebDriver` interface to get an instance of the `Navigation` interface.

You invoke the `navigate` method on an instance of the `WebDriver` interface, e.g. on the `driver` variable.

You use this instance of the `Navigation` interface to invoke its navigation methods.

This is typically done in a single line of code, as in the first of the following examples, which give a preview of navigation methods by invoking the `to` method (which is the same as the `get` method of the `WebDriver` interface), given a URL to which to navigate. The second of the following examples has a line using the `navigate` method to get an instance of the `Navigation` interface, followed by a line using this instance to invoke the `to` navigation method.

```java
driver.navigate().to("www.foo.com");
```
```java
WebDriver.Navigation navigation = driver.navigate();
navigation.to("www.foo.com");
```

Note that this use is similar to using the `switchTo` method of the `WebDriver` interface to access the `TargetLocator` interface.


####Navigation Methods on the `Navigation` Interface

The `Navigation` interface declares methods to support navigation through web browser history, including:

* the `to`  method, the same as the `get` method on the `WebDriver` interface
* the `forward`  method, to navigate forward in history
* the `back` method, to navigate backward in history
* the `refresh` method, to refresh the current web page

|Method|Navigation|
|----|----|
|`to`|same as the `get` method on the `WebDriver` interface|
|`forward`|navigate forward in history|
|`back`|navigate backward in history|
|`refresh`|refresh the current web page|


#####Declaration of Navigation Methods on the `Navigation` Interface

Navigation methods are declared on the `Navigation` interface as follows:

```java
void to(java.lang.String url);
```
```java
void to(java.net.URL url);
```
```java
void forward();
```
```java
void back();
```
```java
void refresh();
```

#####Arguments

######The `to` Method

The `to` method is overloaded with argument types for both a URL given as a string, and a URL given as an instance of a special URL type.

|Type|Name<br> (Example)|Description|
|---|---|---|
|`String`|`url`|a string with the URL for the web page |
|`URL`|`url`|an instance of the `URL` class for the URL |

######The `forward`, `back` and `get` Methods

The `forward`, `back` and `refresh` methods take no arguments, and navigate through web browser history to preceding, succeeding and current web pages, respectively.

#####Behavior

######The `to` Method

The `to` method navigates to a web page indicate by its `url` argument value.  It uses the HTML GET operation, and blocks until the web page is loading of the web page is completed by the web browser.  The `to` method follows redirects, including both redirects issued by the web server, and redirects coded in the web page using a `meta` element - e.g. `<meta http-equiv="refresh" ... >` with the attribute setting `content="0; url=http://www.bar.com/">` (*the attribute setting is given separately here, to avoid an actual redirect in some Markdown viewers and web browsers!*). 

######The `forward`, `back` and `get` Methods

The `forward`, `back` and `refresh` methods take no arguments, and navigate through web browser history to preceding, succeeding and current web pages, respectively.  These methods have effects similar to clicking on forward, back and refresh controls of the web browser.

#####Exceptions

*No information is currently available on exceptions thrown for these navigation methods.*


####Using the `to` or the `get` Method

You use the `to` method of the `Navigation` interface or the `get` method of the `WebDriver` interface to navigate to a web page.

Given an argument value of string type, the `to` method of the `Navigation` interface  is an effective synonym for the `get` method of the `WebDriver` interface - and so you can use either method to navigate to a URL given as a string.

However, the `to` method is overloaded to take an argument value of a special URL or URI (uniform resource identifier) type - and so given an argument value of such a special type, you use the `to` method, and not the `get` method.


You invoke the `get` method on an instance of the `WebDriver` interface - e.g. on the variable `driver`.

You invoke the `to` method on an instance of the `Navigation` interface - e.g. on `driver.navigate`.

#####Example

```java
driver.get("www.foo.com");
```
```java
driver.navigate().to("www.foo.com");
```
```java
java.net.URL url = new URL ("www.foo.com");
driver.navigate.to(url);
```

####Using the `forward`, `back` and `refresh` Methods

You use the `forward`, `back` and `refresh` methods with invocations similar to the invocation you use for the `to` method, on an instance of the `Navigation` interface, as follows:

#####Example

```java
driver.navigate().forward();
```
```java
driver.navigate().back();
```
```java
driver.navigate().refresh();
```

## Interacting with HTML Elements

### Introduction

####WebDriver Support for Interacting with a Web Browser

The WebDriver API provides support for both **basic** and **advanced** interactions with a web browser.

Basic interactions include:

* entering text in an HTML text input element
* submitting a form
* selecting options of an HTML select element

Advanced interactions include:
* using modifier keys, whitespace keys or navigation keys
* dragging and dropping, and other mouse actions
* building custom action sequences

####Support for Basic Interactions

The WebDriver API provides support for basic interactions with:  

* interaction methods declared on the `WebElement` interface

* key constants of the `Keys` class, used with the `sendKeys` method of the `WebElement` interface

* the `Select` class, to represent an HTML select element 

#####Interaction Methods

Interaction methods declared on the `WebElement` interface include:

* the `sendKeys` method of the `WebElement` interface, to send or to enter text
* the `clear` method of the `WebElement` interface, to clear entered text
* the `submit` method of the `WebElement` interface, to submit a form without entering characters

#####Key Constants

Key constants of the `Keys` class - such as `RETURN`  and `SHIFT`  - are used for characters that cannot be entered as text.  These key constants are used with the `sendKeys` method of the `WebElement` interface.

#####The `Select` Class

The `Select` class represents an HTML select element, defines methods for working with properties and options of an HTML select method.  The `Select` class is covered below, in a separate section of documentation.

####Support for Advanced Interactions

Support for advanced interactions was added to the WebDriver API in the **Advanced User Interactions API**.

The Advanced User Interactions API provides:
* interfaces for standard interaction mechanisms
* classes and interfaces to represent actions

#####Interfaces for Standard Interaction Mechanisms

The interfaces for standard interaction mechanisms in the Advanced User Interactions API include:

* the `Keyboard` interface, with methods supporting keyboard interactions
* the `Mouse` interface, with methods supporting mouse interactions
* the `TouchScreen` interface, with methods supporting touch interactions such as on phones, pads and other types of device

#####Classes and Interfaces to Represent Actions

The classes and interfaces to represent actions in the Advanced User Interactions API include:

* the `Action` class, to represent and to perform a single action

* the `Actions` class, which:
  * provides access to methods declared on the `Keyboard` and `Mouse` interfaces
  * implements methods to create actions for many common types of interaction 
  * supports building and performing action sequences or action chains
* the `TouchAction` and `TouchActions` subclasses, to represent interactions with a touch screen using the `TouchScreen` interface


### Entering Text

####WebDriver Support for Basic Keyboard Interactions

The `WebElement` interface declares methods supporting basic keyboard interactions, including methods for working with text, such as:

* the `sendKeys` method, to enter text in an HTML text input element or to "send" entered keys to the web browser
* the `clear` method, to clear entered text

The `WebElement` interface also includes the `submit` and `click` interaction methods, which can be involved in interactions also involving text.  However, the `submit` method is covered in a separate section of documentation below, and the `click` is covered with mouse interactions, also in a separate section of documentation below.

The WebDriver API also provides the `Keys` class, with constants for characters that cannot be entered as text, e.g. `RETURN` or `SHIFT`.  


This section covers basic keyboard interactions - i.e. interactions that can be performed using only the `WebElement` interface and the `Keys`  class.  These interactions notably do not include interactions requiring *modifier keys* to be pressed while other keys are entered.   These and other types of advanced keyboard interaction are covered in a separate section of documentation below.

####The `sendKeys` Method

You use the `sendKeys` method to enter text in an HTML text element (or to "send" text to the web browser for the HTML text element).  

#####Declaration

The `sendKeys` method is declared on the the `WebElement` interface as follows:


```java
void sendKeys (java.lang.CharSequence... keysToSend);
```


#####Behavior

The `sendKeys` method takes a character sequence or string argument value, with text to enter.  

Constants of the `Keys` class can be used to enter characters that cannot be entered as text, e.g. `Keys.RETURN`.  

Successive invocations of the `sendKeys` method append to previously-entered (and not cleared) text.  Entering the key constant `Keys.BACK_SPACE` does clear the preceding character of entered text, but to clear all text, you use the `clear` method.

#####Invocation

You invoke the `sendKeys` method on an instance of the `WebElement` interface - e.g. on the `usernameElement` or the `passwordElement` variable.

You get an instance of the `WebElement` interface using the `findElement` method, and assign this instance to a variable.  For example you assign an instance for a found `username` HTML text input to the `usernameElement` or for a found `password` HTML text input element to the `passwordElement` variable.


Although the `sendKeys` method as declared on the `WebElement` interface can be invoked on any HTML element, you invoke it only on an HTML text element, such as a text input or a text area, in which text can be entered.

You invoke the `sendKeys` method on an instance of the `WebElement` interface for an HTML text element as follows:
  


```java
usernameElement.sendKeys("Alan B. Smithee");
passwordElement.sendKeys("twilightZone");
```


#### The `clear` Method

You use the `clear` method to clear entered text in an HTML text element - e.g. on the `usernameElement` or the `passwordElement` variable.

#####Declaration

The `clear` method is declared on the `WebElement` interface as follows:


```java
void clear();
```

#####Behavior

The `clear` method takes no arguments, and clears whatever text is in the HTML text element represented by the instance of the `WebElement` interface on which it is invoked.

#####Invocation

You invoke the `clear` method on an instance of the `WebElement` interface.  

You get an instance of the `WebElement` interface using the `findElement` method, and assign this instance to a variable - e.g. to the `usernameElement` or the `passwordElement` variable.


Although the `clear` method as declared on the `WebElement` interface can be invoked on any HTML element, you invoke it only on an HTML text element, such as a text input or a text area, which can have entered text.

In the following example:
* the `sendKeys` method is invoked first, to enter a user name and password
* the `clear` method is invoked, to clear this first-entered user name and password
* the `sendKeys` method is invoked again, second, to re-enter another user name and password

```java
usernameElement.sendKeys("Alan B. Smithee");
passwordElement.sendKeys("twilightZone");
usernameElement.clear();
passwordElement.clear();
usernameElement.sendKeys("Fred Smith");
passwordElement.sendKeys("citySlang");
```

####Key Constants of the `Keys` class

To enter characters that are not text and so cannot be typed in string literals, you use **key constants** defined on the `Keys` class.  These include constants for: 

* the **return key**, `RETURN`
* **whitespace keys**, such as `SPACE`, `TAB` and `BACK_SPACE`
* **navigation keys** including **arrow keys**, e.g. `ARROW_UP`, `PAGE_DOWN`, `RIGHT` 
* "shift" or **modifier keys**, e.g. `SHIFT`, `COMMAND`, `CONTROL`, `ALT`
* **function keys**, e.g. `F1`, `F2`
* **text operator keys**, e.g. `INSERT`,`DELETE`, `CLEAR`, `CANCEL`, `ENTER`
* **number pad keys**, e.g. `NUMBPAD0`, `NUMBPAD1`
* **math operator keys** (also for number pads), e.g. `ADD`, `SUBTRACT`, `MULTIPLY`, `DIVIDE`
* the **escape key**, `ESC` 

All the constants defined on the `Keys` class are listed in a table below.

####Using the `sendKeys` Method with Key Constants of the `Keys` class

You use the key constants of the `Keys` class with the `sendKeys` method.  

You can either append key constants to another string, or enter constants such as `Keys.RETURN` in a succeeding (sometimes final) invocation of the `sendKeys` method.

In the following example, instead of invoking the `submit` method to submit a form, the form is submitted by entering `Keys.RETURN` after entering the password:

```java
usernameElement.sendKeys("Alan B. Smithee");
passwordElement.sendKeys("twilightZone");
passwordElement.sendKeys(Keys.RETURN);
```

### Submitting a Form

####WebDriver Support for Submitting a Form

The WebDriver API provides the `WebElement` interface, with methods supporting basic keyboard interactions, including the `submit` method for submitting a form.  

Besides the `submit` method, the WebDriver API supports submitting a form through other interaction methods - i.e. the `sendKeys` method or the `click` method - which can be used to submit a form using a specific action of the keyboard or the mouse - e.g. by entering a return character (i.e. `Keys.RETURN`) on a text input HTML element  or by clicking on a submit input HTML element (i.e. a submit button).

####The `submit` Method 

You use the `submit` method to submit a form, after having used the the `sendKeys` method to enter text data in text input elements of the form.  

#####Declaration

The `submit` method is declared on the the `WebElement` interface as follows:


```java
void submit ();
```


#####Behavior

The `submit` method submits a form directly, using methods of the web browser's API.  For most web browser APIs, this means submitting the form without performing the interactions that would normally be performed to submit a form manually, such as clicking the submit button or entering a return character in a text input.

#####Invocation

You invoke the `submit` method on an instance of the `WebElement` interface.  

You can invoke the `submit` method on:


* an instance of the `WebElement` interface for the **form**
* an instance of the `WebElement` interface for any **text input** of the form

These options for invoking the `submit` method support 

The following example demonstrates both of these options for invoking the `submit` method, on the HTML form element and the HTML text input element named `password`: 


```java
formElement.submit ();
```
or
```java
passwordElement.submit ();
```


####Using the `click` and `sendKeys` Methods to Submit a Form

Other methods than the `submit` method can be used to submit a form, by performing interactions similar to those performed to submit a form manually.  

These are methods declared on the `WebElement` interface, including:

* the `click` method
* the `sendKeys` method

You use the `click` method to submit a form by clicking on the submit input element (i.e. the submit button).  

You use the `sendKeys` method to submit a form by entering a return character (i.e. `Keys.RETURN`) on an HTML text input element - e.g. on the `password` HTML text input element.

#####Using the `click` Method to Submit a Form 

The following example uses the `click` method to submit a form.  This example is the same as the example (below) using the `sendKeys` method, except for the single line submitting the form.

The code in the example:

* locates a form element
* locates the username and password elements within the form element
* locates the submit input element within the form element
* enters a user name and a password using the `sendKeys` method
* clicks the submit button using the `click` method, to submit the form

Note that the child elements of the form - the username, password and submit button elements - are located by invoking the `findElement` method on the form, and not on `driver`. This limits the scope of the search, and also limits the range over which HTML identifying an element may vary.  Here, an XPath expression, `//*[@*='submit']`, is used to locate the submit button.  XPath is used because there is no special locator method for the `type` attribute of a form `input`, and because a button can be defined in an older style such as `<input type='submit' ...>` or a newer style such as `<button id='submit'>`.  The expression `//*[@*='submit']` is used, to search for any element under the current element which is the form (`//*`) that has any attribute (`@*`) with a value 'submit' (`[@*='submit']`).  



```java
// Locate the form element
WebElement formElement = driver.findElement(By.id, "loginForm");

// Locate the text input elements for the form
WebElement usernameElement = formElement.findElement(By.name("username"));
WebElement passwordElement = formElement.findElement(By.name("password")); 

// Locate the submit button
WebElement submitElement = formElement.findElement(By.xpath, "//*[@*='submit']"); 

// Enter a user name and password
usernameElement.sendKeys("Alan B. Smithee");
passwordElement.sendKeys("twilightZone");

// Click the submit button to submit the form
submitElement.click();
```

#####Using the `sendKeys` Method to Submit a Form 

The following example uses the `sendKeys` method to submit a form.  This example is the same as the example (above) using the `click` method, except for the single line submitting the form.

The code in the example:

* locates a form element
* locates the username and password elements within the form element
* locates the submit input element within the form element
* enters a user name and a password using the `sendKeys` method
* enters a return character (i.e. `Keys.RETURN`) using the `sendKeys` method, to submit the form 

```java
// Locate the form element
WebElement formElement = driver.findElement(By.id, "loginForm");

// Locate the text input elements for the form
WebElement usernameElement = formElement.findElement(By.name("username"));
WebElement passwordElement = formElement.findElement(By.name("password")); 

// Locate the submit button
WebElement submitElement = formElement.findElement(By.xpath, "//*[@*='submit']"); 

// Enter a user name and password
usernameElement.sendKeys("Alan B. Smithee");
passwordElement.sendKeys("twilightZone");

// Enter a return character on a text input element to submit the form
passwordElement.sendKeys(Keys.RETURN);
```

### Selecting Options

####WebDriver Support for Selecting Options

The WebDriver API provides support for selecting options, with:

* the `Select` class and its methods to get, to inspect and to select or to deselect options  

* the `findElement` and `findElements` methods of the `WebDriver` interface, to locate an HTML select element on a web page

* the `findElement` and `findElements` methods of the `WebElement` interface, to locate options of a select element, for unusual cases in which methods of the `Select` class aren't sufficient
* the `isSelected` and `click` methods of the `WebElement` interface, to check and to select or to deselect an option, as an alternative to methods of `Select` class

#####The `Select` Class

You create an instance of the `Select` class by passing an instance of the `WebElement` interface for an HTML select element as an argument to the `Select` class constructor. 

The `Select` class has methods to perform the following actions involving the option elements of the HTML select element:

|Action|Objects or Conditions|    
|----|----|
|**select** or **deselect**|**one** option|
||**many** options, by a locator expression|
||**all** options|
|**get**| **all** options|
||**all selected** options|
||the **first selected** option
|**inspect**|whether only **one** option or **many** options can be selected|
||whether an option **is selected**|

#####The `WebDriver` and `WebElement` Interfaces

The `findElement` and `findElements` methods of the `WebDriver` and `WebElement` interfaces also support selecting and deselecting options.

The `findElement` method of the `WebDriver` interface is used to locate an HTML select element on a web page.  Its return value is used to create an instance of the `Select` class.

For unusual cases in which methods of the `Select` class are not sufficient to select or to deselect options, the `findElement` or `findElements` method can be used to locate one or many options.  For these cases, the `findElement` or `findElements` method is invoked on an instance of the `WebElement` interface for the select.

The `click` method of the `WebElement` interface is then used to select or to deselect an option.  The `isSelected` method of the `WebElement` interface is used to inspect an option.

The following table summarizes the uses of methods of the `WebDriver` and `WebElement` interfaces with select and options elements:

|Type|Method(s)|Description|    
|----|----|----|
|`WebDriver` |`findElement` and `findElements`|locate an HTML select element on a web page|
|`WebElement`|`findElement` and `findElements`|locate options of an HTML select element|
|`WebElement`|`isSelected`|inspect an option|
|`WebElement`|`click`|select or deselect an option|

####Example of an HTML Select Element

Suppose the Foo website is enhanced to support logging in by role - e.g. in an administrator, an operator, a customer or a guest role.

The following HTML select element is added to the web page at `www.foo.com`, to allow a user to select an option for the role in which to log in:  


```html 
<select id="role">
  <option value="admi">Administrator</option>
  <option value="oper">Operator</option>
  <option value="cust">Customer</option>
  <option value="gues">Guest</option>
</select> 
```

####Locating an HTML Select Element 


As with other types of HTML element, you locate an HTML select element using the `findElement` method, invoked on the unique instance of the `WebDriver` interface, e.g. on the `driver` variable.  

You can locate an HTML select element by any of several locator types - e.g. by tag name, name, class or id.  However, using an XPath expression supports a combination of aspects to locate an element in a way useful in locating an HTML select element - e.g. by both its tag name and the value of its `id` attribute.

Three code examples of locator expression are given below: 

1. an example using **tag name** only - e.g. with a locator value `"select"`
2. an example using **id attribute** only - e.g. with a locator value `"role"`
3. an example using an **XPath expression** combining both tag name and `id` attribute value - e.g. `"//select[@id='role']"`

Note that the XPath expression `"//select[@id='role']"`, when invoked on the `driver` variable, locates the first element in the DOM for the web page with the combination of:
* a `<select>` tag, as specified by `//select` in the XPath expression, and 
* an `id` attribute value `'role'`, as specified by `[@id='role']` in the XPath expression

This use of an XPath expression to locate an HTML select element is summarized in the following table:

|Type|HTML|Value|XPath|
|----|----|----|----|
|`tag name` |`<select...>`|`select` as tag name|`//select...`|
|`attribute` |`...id='role'...`|`'role'`|`...[@id='role']`|

The example code for selecting by tag name, `id` attribute value or both, in an XPath expression, is as follows:

```java
WebElement selectElement = driver.findElement(By.tagName("select"));
```

or 

```java
WebElement selectElement = driver.findElement(By.id("role"));
```

or 

```java
WebElement selectElement = driver.findElement(By.xpath, "//select[@id='role']");
```


####Creating an  Instance of the `Select`  Class

You create an instance of the `Select` class by passing a located `WebElement` for an HTML select element as an argument value when invoking the constructor for the `Select` class.


#####Behavior

When the `WebElement` instance passed as an argument value is indeed an HTML select element, the constructor returns an instance of the `Select` class.


#####Exceptions

When the `WebElement` instance passed as an argument value is *not* an HTML select element, the constructor throws an exception of type `UnexpectedTagNameException`.

|Exception|Thrown|
|---|---|
|`UnexpectedTagNameException`|Thrown when the instance of the `WebElement` interface passed to the constructor of the `Select` class is not an HTML select element |

####Invocation

The constructor for the `Select` class is invoked as follows, with a preceding invocation of the `findElement` method on the `driver` variable:


```java
import org.openqa.selenium.support.ui.Select;

// Locate the HTML select element
WebElement selectElement = driver.findElement(By.xpath, "//select[@id='role']");

// Create an instance of the Select class
Select select = new Select (selectElement);
```

####Alternatives for Selecting and Deselecting Options

You can select or deselect one option or many options by either of the following techniques:

* using one of the `selectBy...` or `deselectBy...`  method of the `Select` class
* first locating options using the `findElement` or `findElements` method, then inspecting and selecting options using the `isSelected` and `click` methods 

####Using the `Select` Class To Select and Deselect Options 

The `Select` class has `selectBy...` and `deselectBy...` methods to select or to deselect options, by:
* **index**, following the order in which option elements are coded within the select element in HTML source code
* **value** - i.e. the value of the `value` attribute of the option element
* **visible text** - i.e. text in the content of the option element

#####Declaration

The `selectBy...` and `deselectBy...`  methods are declared on the `Select` class as follows:


```java
void selectByIndex(int index)
void selectByValue(String value)
void selectByVisibleText(String text)
```

```java
void deselectAll()
void deselectByIndex(int index)
void deselectByValue(String value)
void deselectByVisibleText(String text)
```

####Arguments 

The `selectBy...` and `deselectBy...` methods do not return a value.  Notably, these methods do not return selected or deselected options.  

Instead, the `selectBy...` and `deselectBy...` methods have a `void` return type, which is common for the declaration of methods that perform an action, such as selection, rather than access a value.

#####Behavior

In Java, both the `selectBy...` and `deselectBy...` methods take an argument value that is a locator value, which completes a locator expression of the locator type represented by the method - i.e. index, value or visible text.

The `selectBy...` and `deselectBy...` methods select or deselect all option elements of the select element matching the locator expression. 

#####Exceptions

When there is no option element matching the index, value or visible text locator value, the `selectBy...` and `deselectBy...` methods throw an exception of type `NoSuchElementException`.

When the select element does *not* allow multiple selects - i.e. when the select element is *not* a multi-select - the common WebDriver API behavior is to throw an exception or to raise an error, e.g. an error of type `NotImplementedError`. 

|Exception|Thrown|
|---|---|
|`NoSuchElementException`|Thrown when there is no option element matching the index, value or visible text |
|`NotImplementedError`|Thrown when more than one option element is selected, but the select element does not allow multiple select.|

#####Invocation

*See the code examples below for* Using the `Select` Class to Get Options, *for combined uses of a* `selectBy...` *method and* `get...Options(s)` *methods*.

####Using the `Select` Class To Get Options

Although the `selectBy...` and `deselectBy...` methods do not return selected or deselected options, the `get...Option(s)` methods of the `Select` class do.

You use the `get...Option(s)` methods of the `Select` class to get options for further inspection or interaction.  

The `get...Option(s)` methods include the following three methods:

* the `getAllOptions` method
* the `getFirstSelectedOption` method
* the `getAllSelectedOptions` method

The `get...Option(s)` methods are often used with the `isMultiple` inspection method of the `Select` class.  For example, you can inspect the select element using the `isMultiple` method before attempting to access multiple selected options.

#####Declaration

The `get...Option(s)` methods and the `isSelected` method are declared on the `Select` class as follows:


```java
List<WebElement> 	getOptions() 
```
```java
WebElement 			getFirstSelectedOption() 
```
```java
List<WebElement> 	getAllSelectedOptions() 
```
```java
boolean 			isMultiple() 
```


#####Behavior 

The `get...Option(s)` methods take no arguments.


In Java, the `getAllSelected` and `getOptions` methods return a generic `List` of instances of the `WebElement` interface - i.e. a `List<WebElement>`. The `getFirstSelectedOption` method returns an instance of the `WebElement` interface.

The `isMultiple` method returns a `boolean` value indicating whether or not the option is selected.

#####Invocation

Two code examples using `get...Option(s)` methods are given below.

Both code examples use the `selectByVisibleText` method, and test that the value of the `value` attribute is the expected value for options selected by visible text.  

Generally, the value of the `value` option should be unique in a select element.  So the first code example assumes uniqueness, selects using full visible text, uses `getFirstSelectedOption`, and tests only that value of the first selection option is the expected value.

However, besides the possibility of the value of the `value` option is not the expected value, there is the possibility that values of the `value` option are not unique, whether or not the value is expected.  So the second code example does not assume uniqueness, selects using full visible text, uses `getAllSelectedOptions`, loops over selected options to test all selected options, and tests both the value and whether many options are selected. 



* locates the HTML select element by XPath expression
* creates a new instance of the `Select` class for the select element
* sets a value by which to select (by visible text) and an value expected value (of the `value` attribute)
* selects option elements by visible text
* gets a list of selected options 
* loops over selected options in the list
* tests whether the value of the `value` attribute is the value expected for the visible text used to select
* tests whether there is one or more option selected by the visible text


######Example: Test One Selected Option
```java
// Locate the HTML select element
WebElement selectElement = driver.findElement(By.xpath, "//select[@id='role']");

// Create an instance of the Select class
Select select = new Select (selectElement);

// Set select value (visible text) and expected value (value attribute)
String selectValue		= "Administrator"
String expectedValue 	= "admin";

// Select option(s) by visible text
select.selectByVisibleText(selectValue);

// Get the first selected option 
// NB: assumes only one option selected by visible text
WebElement selectedOption = select.getFirstSelected();

// Test value for visible text
String attrNameValue 	= "value";
String value 			= selectedOption.getAttribute(attrNameValue);
assertEquals (value, expectedValue);
```

######Example: Test Many Selected Options

```java
// Locate the HTML select element
WebElement selectElement = driver.findElement(By.xpath, "//select[@id='role']");

// Create an instance of the Select class
Select select = new Select (selectElement);

// Set select value (visible text) and expected value (value attribute)
String selectValue		= "Administrator"
String expectedValue 	= "admin";

// Select option(s) by visible text
select.selectByVisibleText(selectValue);

// Get a list of selected options
List<WebElement> selectedOptions = select.getSelected();

// Loop over selected options in list
// Test value for visible text
boolean expectedSelected 	= false;
boolean moreSelected		= false;	
boolean moreExpected		= false;		
String attrNameValue 		= "value";
for optionElement in selectedOptions {
	String value = optionElement.getAttribute (attrNameValue);
	if ( value = expectedValue ) {
		if ( expectedSelected ) {
			moreSelected = true;
		}
		expectedSelected = true;
		assertEquals ( value, expectedValue );
		assertEquals ( moreSelected, moreExpected );
	}
}
```



####Using the `findElement` and `click`  Methods To Select and Deselect Options 

Selecting and deselecting options by index, value or visible text is sufficient for most tests.  This is because options are properly identified by the value of their `value` attribute, which is the option value sent to the web server when the use selects an option.  Getting selected or deselected options and testing whether the value of the `value` attribute of an option is the correct value for its visible text identifies duplicate or otherwise-incorrect values.  

However, selecting and deselecting options by index, value or visible text using the `selectBy...` and `deselectBy...` methods of the `Select` class may not be sufficient for tests in cases in which option attributes other than `value` represent additional information about an option.  Such information can be represented using HTML5 custom attributes, i.e. attributes with the prefix `data-`, such as in `<option value="admi" data-foo="barAccess">`, and can help to minimize processing and storage on a web application server: additional information about an option can be retrieved when the option is first accessed in a database to generate a dynamic web page, so that further lookup or storage in an option object on the web application server is unnecessary. 
	
So for some tests, specific options may be identified by unique attribute values, but not by the value of the `value` attribute, and not by index or visible text.  Instead, a specific option may be identified by: 
* a unique value of an attribute, but an attribute that varies over options
* a combination of option attribute values
* an HTML5 custom attribute for options, i.e. an attribute whose name has the prefix `data-`, such as in `<option value="admin" data-foo="barAccess">`.

In order to use other locator types such as above to select or to deselect options, you  first invoke the `findElement` or `findElements` method on a `WebElement`  for the select, and then invoke the `isSelected` and  `click` methods on an instance of the `WebElement` interface for the option, as in the following steps:

1. on an instance of the `WebElement` interface for the HTML select element, locate either: 
 * one option by invoking the `findElement` (singular) method, or 
 * many options using the `findElements` (plural) method
 
2. for many options when using the `findElements` (plural) method, iterate over each option element (don't iterate when using `findElement` (singular))
3. for each option element, check whether the element is selected using the `isSelected` method on the `WebElement` for the option
4. as appropriate, invoke the `click` method on an instance of the `WebElement` interface for the option element to select an unselected option or to deselect a selected option

You invoke the `findElement` or `findElements` method on an instance of the `WebElement` interface for the HTML select element.  Note that this instance is *not*  an instance of the `Select` class for the select element (which does not implement the `WebElement` interface).

You invoke the `click` and `isSelected` methods on an instance of the `WebElement` interface for an HTML option element.  

#####Example: Select Qualifying Options

In the following example, an HTML5 custom attribute is used to identify qualifying options (e.g. for a prize, a discount, etc.) in an HTML select element.  All of the qualifying options must be selected in order for the user to qualify.  To locate option elements by this custom attribute - `data-qualifying` - you invoke the `findElements` method on an instance of `WebElement` for the HTML select element, with an XPath expression for the locator. 

```java
import org.openqa.selenium.support.ui.Select;

// Locate the selectelement
WebElement selectElement = driver.findElement(By.xpath, "//select[@id='multipleChoice']");

// Locate the qualifying options of the select element
List<WebElement> qualifyingOptions = selectElement.findElements("//option[@data-qualifying='true']");

// Loop to check whether all qualifying options are selected
boolean qualifies = true;
for (WebElement correctOption : qualifyingOptions ) {
 // Inspect whether the qualifying option is selected
 if ( !qualifyingOption.isSelected() ) {
 	qualifies = false 
 }
} 

// Test the result
assertEquals ( qualifies, true );
```

#####Example: Select All Options

In the following example, you use the `findElements` method with a tag name locator, in order to select all options using the `click` method.  This example can be used when all options must be selected for a test, so that `selectBy...` methods won't help.

```java
WebElement select = driver.findElement(By.tagName("select"));
List<WebElement> allOptions = select.findElements(By.tagName("option"));
for (WebElement option : allOptions) {
    System.out.println(String.format("Value is: %s", option.getAttribute("value")));
    option.click();
}
```


## Inspecting an HTML Element and a Web Page

###Inspecting an HTML Element

####WebDriver Support for Inspecting an HTML Element

The WebDriver API provides methods of the `WebElement` interface to support inspecting an HTML element.

These inspection methods include:

* methods to get **HTML attribute** and **CSS property** values by name
* property methods for the `WebElement` , to get:
  * the **location** of the element on the web page and its **size**
  * the **tag name** and **visible text** of the element
  * boolean values indicating whether the element is:
    * **displayed**
    * **enabled**
    * **selected**

####Declaration of `WebElement` Inspection Methods

The `WebElement` interface declares the following inspection methods:

|Return Type|Method and Description|
|----|----|
|`String`|`getAttribute (String attributeName)`|
||Gets the value of the named attribute|
|`String`|`getCssValue (String propertyName)`|
||Gets the value of the named CSS property|
|`Point`|`getLocation ()`|
||Gets the location of the element on the web page as a `Point`|
|`Dimension`|`getSize ()`|
||Gets the size of the element on the web page as a  `Dimension`|
|`String`|`getTagName ()`|
||Gets the tag name of the element|
|`String`|`getText ()`|
||Gets the text of the element|
|`boolean`|`isDisplayed ()`|
||Indicates whether the element is displayed|
|`boolean`|`isEnabled ()`|
||Indicates whether the element is enabled|
|`boolean`|`isSelected ()`|
||Indicates whether a selectable element is selected|

###Inspecting a Web Page

####WebDriver Support for Inspecting a Web Page

The WebDriver API provides methods of the `WebDriver` interface to support inspecting a web page.

These inspection methods include methods to get:

* the **URL** of the web page in the current window
* the **page source** of the web page in the current window
* the **title** of the web page in the current window
* a **handle** to the current window
* **handles** to all open window

####Declaration of `WebElement` Inspection Methods

The `WebElement` interface declares the following inspection methods:

|Type|Method and Description|
|----|----|
|`String`|`getCurrentURL ()`|
||Gets the URL of the web page in the current window as a `String`|
|`String`|`getPageSource ()`|
||Gets the source of the web page in the current window|
|`String`|`getTitle ()`|
||Gets the title of the web page in the current window|
|`String`|`getWindowHandle ()`|
||Gets a handle for the current window|
|`Set<String>`|`getWindowHandles ()`|
||Gets a set of handles for all open windows|
