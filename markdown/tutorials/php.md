{
  title: "PHP",
  description: "How to run Selenium tests on Sauce Labs using PHP",
  category: 'Tutorials',
  index: 2,
  image: '/images/tutorials/php.png'
}

## Getting started with the Sausage library

We recommend using the [Sausage](http://github.com/jlipps/sausage) library to
write Selenium tests. Although there are a number of Selenium libraries for
PHP, Sausage is designed to make it easy to work with Sauce and automatically
provides some convenient functionality like test pass/fail reporting.

Sausage is built on top of [PHPUnit](http://phpunit.de), which is one of the
leading test frameworks for PHP. While neither PHPUnit nor Sausage are required
to use Sauce, this tutorial will assume that this as our framework.

Although this tutorial is not a comprehensive guide for getting PHP set up on
your system, here are some guidelines:

## PHP Setup for Mac

Mac OS X comes with PHP installed, so no further steps are required to get PHP
set up. Simply make sure that you have a recent version by opening `Terminal.app`
and running the following command:

```bash
php -v
```

As long as you have a PHP version greater than 5.3 you should be good to go.

* Continue to [Sausage setup for Mac](#sausage-setup-for-mac-and-linux)

PHP Setup for Linux
---
If you're using Ubuntu or another system with `apt-get` installed, you can
easily install PHP and several important libraries with these two commands:

```bash
sudo apt-get update
```

```bash
sudo apt-get install php-pear php5-curl php5-xdebug
```

* Continue to [Sausage setup for Linux](#sausage-setup-for-mac-and-linux)

PHP Setup for Windows
---
**Note:** Windows 7 has an extra level of security, so to complete the following
steps you'll need to do it from an Administrator console. To get this, click Start, and in the run box type `cmd`. Instead of pressing Enter,
press Ctrl+Shift+Enter and the User Account Control dialog will appear.
Log in and the command prompt opens in Administrator mode.

1.  To get PHP, visit [PHP Windows Downloads](http://windows.php.net/download/)
    and get the latest thread-safe zip archive of the PHP binaries.
2.  Unzip the files into `C:\PHP`
3.  From the console, navigate to `C:\PHP` and prepare the php.ini file:

```bat
cd C:\PHP
```

```bat
copy php.ini-development php.ini
```

4.  In order to enable the PHP [curl](http://curl.haxx.se/) library, edit
    the `C:\PHP\php.ini` file and uncomment each of these lines by removing
    the semi-colon (`;`) which is in front of them:

```ini
extension_dir = "ext"
extension=php_curl.dll
extension=php_openssl.dll
```

5.  Now we need to add `php.exe` to our Windows `PATH` so we can run it from
    anywhere in the console by typing `php`. If you're not familiar with PHP,
    visit [PHP's
    FAQ](http://php.net/manual/en/faq.installation.php#faq.installation.addtopath)
    for instructions on how to do this.

6.  Unfortunately, PHP for Windows does not ship with very good SSL support. To
set up curl/OpenSSL support for PHP, do the following steps:
    * Right-click <a href="https://raw.github.com/bagder/curl/master/lib/mk-ca-bundle.vbs">mk-ca-bundle.vbs</a>
    * Select **Save Link As...** Save the `mk-ca-bundle.vbs` file to your `C:\>` directory.
    * To create the `ca-bundle.crt` file, execute the `mk-ca-bundle.vbs` file from the Windows command prompt: `C:\>mk-ca-bundle.vbs`
    * In the `[PHP]` section of your `PHP.ini` file, add the following line: `curl.cainfo = c:\ca-bundle.crt`
    * Save and close your `PHP.ini` file.


* Continue to [Sausage setup for Windows](#sausage-setup-for-windows)

## Sausage Setup for Mac and Linux

First, let's create a project directory that we'll use for this tutorial:

```bash
mkdir ~/sauce-tutorial && cd ~/sauce-tutorial
```

Now we can use this curl one-liner to download and install Sausage using your username and your Sauce access key. You can
find your Sauce access key on your [Sauce account page](https://saucelabs.com/account):

```bash
curl -s https://raw.githubusercontent.com/jlipps/sausage-bun/master/givememysausage.php |
SAUCE_USERNAME=sauceUsername SAUCE_ACCESS_KEY=sauceAccessKey php
```

This will start the download script and install Sausage in the `~/sauce-tutorial`

directory. Sausage checks for a number of requirements. If any are
not met, notification messages display in the terminal. Fix any issues then run the curl command again.

Sausage Setup for Windows
---

1.  Let's create a new directory for this tutorial project, say `C:\sauce-tutorial`:

```bat
mkdir C:\sauce-tutorial
```

2.  To download the Sausage setup file, right-click
    [givememysausage.php](https://raw.github.com/jlipps/sausage-bun/master/givememysausage.php),
    select `Save Link As...` from the popup menu, and save the
    `givememysausage.php` file into the `C:\sauce-tutorial` directory.

3.  Navigate to the `C:\sauce-tutorial` directory in the command prompt, then
    run the script with PHP, setting your user credentials along the way:

```bat
cd C:\sauce-tutorial
```

```bat
php givememysausage.php -t sauceUsername sauceAccessKey
```

Sausage checks for a number of requirements (this may take a while). If any
are not met, notifiaction messages display in the terminal. Fix any issues
then run the php command again. **Note:** See the next step for manually
configuring Sauce with your username and Sauce access key.

You're all set up!

## Running your first test


Now that you've got PHP and Sausage set up, let's try running a simple test
to make sure that everything works.

**Mac/Linux:**

If you're using Mac or Linux, run this command from your `sauce-tutorial` directory:

```bash
vendor/bin/phpunit WebDriverDemo.php
```

**Windows:**

If you're using Windows, run this command from your `sauce-tutorial` directory:

```bat
vendor\bin\phpunit.bat WebDriverDemo.php
```

**Mac/Linux/Windows:**

This starts the PHPUnit test runner and gives it the name of an example test
suite that Sausage downloaded. After a few moments you should see that PHPUnit
has started. You might not see any output instantaneously, but eventually you
will see a series of dots inching across the screen.

Each of these dots represents a test that successfully passed. If a test
had an error or if it failed, PHPUnit prints an `E` or an `F` respectively.

While the tests are running, navigate to your [Sauce Labs tests page](https://saucelabs.com/tests).
From there you'll be able to see each test as it queues, runs, and finishes.
You'll notice that each test has a name -- that information is sent
automatically by Sausage at the beginning of each test. Sausage also
automatically notifies Sauce of the status of your tests when they complete.

Right now each test runs one at a time because PHPUnit currently doesn't
support running multiple tests in parallel, however we've developed
a way to do that which we'll describe in one of the later tutorials. For now,
take advantage of the serial nature of the tests and click on a running test
on your tests page. You'll jump to a detail view where, if you caught the
test while it was running, you'll be able to watch Selenium controlling the
browser. When the test finishes, the page updates with a video of the test
and a list of the various Selenium commands that were sent.

If you don't catch a test while it's running, you can click the test's link on
the [Sauce Labs tests page](https://saucelabs.com/tests) to see the test's
details and video.

Now that you know that your setup worked and you were able to run your first
test suite on Sauce, let's look at what actually happened under the hood. If
you open the `WebDriverDemo.php` file in your text editor, this is what
you'll see:

```php
<?php

require_once 'vendor/autoload.php';

class WebDriverDemo extends Sauce\Sausage\WebDriverTestCase
{

    protected $start_url = 'http://saucelabs.com/test/guinea-pig';

    public static $browsers = array(
        // run FF15 on Windows 8 on Sauce
        array(
            'browserName' => 'firefox',
            'desiredCapabilities' => array(
                'version' => '15',
                'platform' => 'Windows 2012',
            )
        ),
        // run Chrome on Linux on Sauce
        array(
            'browserName' => 'chrome',
            'desiredCapabilities' => array(
                'platform' => 'Linux'
          )
        ),
        // run Mobile Safari on iOS
        //array(
            //'browserName' => '',
            //'desiredCapabilities' => array(
                //'app' => 'safari',
                //'device' => 'iPhone Simulator',
                //'version' => '6.1',
                //'platform' => 'Mac 10.8',
            //)
        //)//,
        // run Chrome locally
        //array(
            //'browserName' => 'chrome',
            //'local' => true,
            //'sessionStrategy' => 'shared'
        //)
    );

    public function testTitle()
    {
        $this->assertContains("I am a page title", $this->title());
    }

    public function testLink()
    {
        $link = $this->byId('i am a link');
        $link->click();
        $this->assertContains("I am another page title", $this->title());
    }

    public function testTextbox()
    {
        $test_text = "This is some text";
        $textbox = $this->byId('i_am_a_textbox');
        $textbox->click();
        $textbox->clear();
        $this->keys($test_text);
        $this->assertEquals($textbox->value(), $test_text);
    }

    public function testSubmitComments()
    {
        $comment = "This is a very insightful comment.";
        $this->byId('comments')->value($comment);
        $this->byId('submit')->submit();
        $driver = $this;

        $comment_test = function() use ($comment, $driver) {
            $text = $driver->byId('your_comments')->text();
            return $text == "Your comments: $comment";
        };

        $this->spinAssert("Comment never showed up!", $comment_test);

    }

}
php?>
```

Let's break this test suite down, chunk by chunk:

```php
require_once 'vendor/autoload.php';
```

Since Sausage is a [Composer](http://getcomposer.org) package, we use the
Composer convention and have just one autoload.

```php
class WebDriverDemo extends Sauce\Sausage\WebDriverTestCase
```

Our `WebDriverDemo` class makes use of Sausage's `WebDriverTestCase`
functionality.

```php
<?php
    public static $browsers = array(
        // run FF15 on Windows 8 on Sauce
        array(
            'browserName' => 'firefox',
            'desiredCapabilities' => array(
                'version' => '15',
                'platform' => 'Windows 2012',
            )
        ),
        // run Chrome on Linux on Sauce
        array(
            'browserName' => 'chrome',
            'desiredCapabilities' => array(
                'platform' => 'Linux'
          )
        ),
        // run Mobile Safari on iOS
        //array(
            //'browserName' => '',
            //'desiredCapabilities' => array(
                //'app' => 'safari',
                //'device' => 'iPhone Simulator',
                //'version' => '6.1',
                //'platform' => 'Mac 10.8',
            //)
        //)//,
        // run Chrome locally
        //array(
            //'browserName' => 'chrome',
            //'local' => true,
            //'sessionStrategy' => 'shared'
        //)
    );
```

This is where we define which browsers we want to use. One browser is not
commented and is currently active. We define each browser and pass a special array to Sauce so we can specify the version and operating
system. (Check out [the full browser list](https://saucelabs.com/platforms/)
to see all of the different options.)

If you have your own Selenium server set up and running, you can use
it by setting in the `local` flag to true.

```php
<?php
public function setUpPage()
{
    $this->url('http://saucelabs.com/test/guinea-pig');
}
```

The `setUpPage()` function is run before every test in the suite. Basically, we
declare that we want to navigate to a specific URL (in this case, a sandbox
page that we set up at Sauce). So Selenium will point the browser to that URL
before each test.

```php
<?php
public function testTitle()
{
    $this->assertContains("I am a page title", $this->title());
}
```

Our first test! (Notice that all test functions begin with `test`). This test
uses a [PHPUnit assertion](http://www.phpunit.de/manual/3.4/en/appendixes.assertions.html)
to check whether the page title contains the string "I am a page title". In the
world of [PHPUnit and Selenium](http://www.phpunit.de/manual/3.4/en/selenium.html),
calling `$this->title()` tells the Selenium session to return the title of the
currently-loaded page.

```php
<?php
public function testLink()
{
    $link = $this->byId('i am a link');
    $link->click();
    $this->assertContains("I am another page title", $this->title());
}
```

This second test simply clicks a link and makes sure that the title of the
resulting page is what we expect. `$this->byId()` returns an object
representing a link element on the page with a specific ID. We can then perform
various actions with this object, including `click()`, which tells Selenium
to click on that link. In this case, clicking on the link takes us to a
new page with a different title, which we verify.

```php
<?php
public function testTextbox()
{
    $test_text = "This is some text";
    $textbox = $this->byId('i_am_a_textbox');
    $textbox->click();
    $this->keys($test_text);
    $this->assertEquals($textbox->value(), $test_text);
}
```

This test gets a textbox element on the page with the ID `i_am_a_textbox` and
clicks in it, bringing it in focus. Then it directs Selenium to send the
browser a sequence of keystrokes. Finally, we check that the text we sent
matches the value of the textbox.

```php
<?php
public function testSubmitComments()
{
    $comment = "This is a very insightful comment.";
    $this->byId('comments')->click();
    $this->keys($comment);
    $this->byId('submit')->submit();
    $driver = $this;

    $comment_test =
        function() use ($comment, $driver)
        {
            return ($driver->byId('your_comments')->text() == "Your comments: $comment");
        }
    ;

    $this->spinAssert("Comment never showed up!", $comment_test);

}
```

In this last test, we follow a similar procedure as before to enter
some text into a comment box. Then we submit the form the comment box is a
member of. This (hopefully) causes the page to reload and show us the comment
we submitted. In order to check whether this is the case, we use a _spinAssert_.

SpinAsserts are covered in detail in a later tutorial, so
for now take our word for it that the test checks periodically for the comment text we submitted,
and fails if it does not show up after a certain amount of time.

That's it! You've seen how to run several important Selenium commands
in the context of a custom PHPUnit test suite.

## Running Tests Against Web Applications

Testing a static sandbox is one thing. Testing a real application's functionality
is another. In this tutorial we'll run tests against a real live app sitting
on the web. Specifically, we'll look at Selenium tests for signup and login
behaviors. Once we're done you'll have a good idea how to write Selenium
tests for login and signup in your own app, and you'll have a general understanding
of how to test other site functionality as well.

## The Test App

We have a demo app set up at <a href="http://tutorialapp.saucelabs.com"
target="_blank">http://tutorialapp.saucelabs.com</a> that we can run Selenium
scripts against. It's a web-based "idea competition" demo app called <a
href="https://github.com/Pylons/shootout" target="_blank">Shootout</a>).
Shootout is a voting platform for ideas designed for the Pyramid Python web
framework. We won't test voting functionality in this demo, but feel free to
play around with it.

The Test Suite
---

If you downloaded Sausage you'll the have the `WebDriverDemoShootout.php` file
in your `sauce-tutorial` directory. It's reproduced below. However, you can run
these 8 tests before we examine them, and you can view them in your [Sauce Labs
tests page](https://saucelabs.com/tests) just like we did during the first
test:

**Mac/Linux:**
```bash
vendor/bin/phpunit WebDriverDemoShootout.php
```

**Windows:**
```bat
vendor\bin\phpunit.bat WebDriverDemoShootout.php
```
And here's the test suite:

```php
<?php

require_once 'vendor/autoload.php';

class WebDriverDemoShootout extends Sauce\Sausage\WebDriverTestCase
{

    protected $start_url = 'http://tutorialapp.saucelabs.com';
    protected $base_url = 'http://tutorialapp.saucelabs.com';

    public static $browsers = array(
        // run FF15 on Windows 8 on Sauce
        array(
            'browserName' => 'firefox',
            'desiredCapabilities' => array(
                'version' => '15',
                'platform' => 'Windows 2012'
            )
    );

    protected function randomUser()
    {
        $id = uniqid();
        return array(
            'username' => "fakeuser_$id",
            'password' => 'testpass',
            'name' => "Fake $id",
            'email' => "$id@fake.com"
        );
    }

    protected function doLogin($username, $password)
    {
        $this->url('/');
        $this->byName('login')->value($username);
        $this->byName('password')->value($password);
        $this->byCss('input.login')->click();

        $this->assertTextPresent("Logged in successfully", $this->byId('message'));
    }

    protected function doLogout()
    {
        $this->url('/logout');
        $this->assertTextPresent("Logged out successfully", $this->byId('message'));
    }

    protected function doRegister($user, $logout = false)
    {
        $user['confirm_password'] = isset($user['confirm_password']) ?
            $user['confirm_password'] : $user['password'];
        $this->url('/register');
        $this->byId('username')->value($user['username']);
        $this->byId('password')->value($user['password']);
        $this->byId('confirm_password')->value($user['confirm_password']);
        $this->byId('name')->value($user['name']);
        $this->byId('email')->value($user['email']);
        $this->byId('form.submitted')->click();

        if ($logout)
            $this->doLogout();
    }

    public function testLoginFailsWithBadCredentials()
    {
        $fake_username = uniqid();
        $fake_password = uniqid();

        $this->byName('login')->value($fake_username);
        $this->byName('password')->value($fake_password);
        $this->byCss('input.login')->click();

        $this->assertTextPresent("Failed to login.", $this->byId('message'));
    }

    public function testLogout()
    {
        $this->doRegister($this->randomUser(), true);
    }

    public function testLogin()
    {
        $user = $this->randomUser();
        $this->doRegister($user, true);
        $this->doLogin($user['username'], $user['password']);
    }

    public function testRegister()
    {
        $user = $this->randomUser();
        $this->doRegister($user);
        $logged_in_text = "You are logged in as {$user['username']}";
        $this->assertTextPresent($logged_in_text);
    }

    public function testRegisterFailsWithoutUsername()
    {
        $user = $this->randomUser();
        $user['username'] = '';
        $this->doRegister($user);
        $this->assertTextPresent("Please enter a value");
    }

    public function testRegisterFailsWithoutName()
    {
        $user = $this->randomUser();
        $user['name'] = '';
        $this->doRegister($user);
        $this->assertTextPresent("Please enter a value");
    }

    public function testRegisterFailsWithMismatchedPasswords()
    {
        $user = $this->randomUser();
        $user['confirm_password'] = uniqid();
        $this->doRegister($user);
        $this->assertTextPresent("Fields do not match");
    }

    public function testRegisterFailsWithBadEmail()
    {
        $user = $this->randomUser();
        $user['email'] = 'test';
        $this->doRegister($user);
        $this->assertTextPresent("An email address must contain a single @");
        $this->byId('email')->clear();
        $this->byId('email')->value('@bob.com');
        $this->byId('form.submitted')->click();
        $this->assertTextPresent("The username portion of the email address is invalid");
        $this->byId('email')->clear();
        $this->byId('email')->value('test@bob');
        $this->byId('form.submitted')->click();
        $this->assertTextPresent("The domain portion of the email address is invalid");
    }

}
```

You're already familiar with a lot of what's going on in this test suite. 
Let's just take a quick look at some new commands and concepts.

Setting `$base_url` allows us to relativize all further uses of `$this->url()`
to a particular domain or URL.

```php
<?php
protected $base_url = 'http://tutorialapp.saucelabs.com';
```

Likewise, `$start_url` allows us to tell the testsuite which url should be
loaded at the beginning of every test:

```php
<?php
protected $start_url = 'http://tutorialapp.saucelabs.com';
```

The `randomUser()` function generates unique random user details for the registration
and login tests. The randomness is important because it allows our tests to
run in parallel as many times as we want without fear of collisions.

```php
<?php
protected function randomUser()
{
    $id = uniqid();
    return array(
        'username' => "fakeuser_$id",
        'password' => 'testpass',
        'name' => "Fake $id",
        'email' => "$id@fake.com"
    );
}
```

The next three functions are helper functions for our tests. If we were testing
a local app, it would be more efficient to perform these actions on the
server-side than to have Selenium generate states of being logged in, logged out,
etc. However, since we're testing an app on the Internet whose backend is inaccessible
to us, we're using these Selenium commands to put the user into these states.

```php
<?php
protected function doLogin($username, $password)
{
    $this->url('/');
    $this->byName('login')->value($username);
    $this->byName('password')->value($password);
    $this->byCss('input.login')->click();

    $this->assertTextPresent("Logged in successfully", $this->byId('message'));
}

protected function doLogout()
{
    $this->url('/logout');
    $this->assertTextPresent("Logged out successfully", $this->byId('message'));
}

protected function doRegister($user, $logout = false)
{
    $user['confirm_password'] = isset($user['confirm_password']) ?
        $user['confirm_password'] : $user['password'];
    $this->url('/register');
    $this->byId('username')->value($user['username']);
    $this->byId('password')->value($user['password']);
    $this->byId('confirm_password')->value($user['confirm_password']);
    $this->byId('name')->value($user['name']);
    $this->byId('email')->value($user['email']);
    $this->byId('form.submitted')->click();

    if ($logout)
        $this->doLogout();
}
```

In our first test we make sure that logging in doesn't work with bad username/password
values by asserting that we get a login failure message when we put in random
text.

```php
<?php
public function testLoginFailsWithBadCredentials()
{
    $fake_username = uniqid();
    $fake_password = uniqid();

    $this->byName('login')->value($fake_username);
    $this->byName('password')->value($fake_password);
    $this->byCss('input.login')->click();

    $this->assertTextPresent("Failed to login.", $this->byId('message'));
}
```

Next we test login and logout functionality. The first test creates a new user
and uses the `doLogout()` helper function to assert that the logout message
appears. The second test creates a random user, logs it out, and then uses
the `doLogin()` helper function to assert that the successful login message
appears.

```php
<?php
public function testLogout()
{
    $this->doRegister($this->randomUser(), true);
}

public function testLogin()
{
    $user = $this->randomUser();
    $this->doRegister($user, true);
    $this->doLogin($user['username'], $user['password']);
}
```

Then we test Shootout's signup functionality by using the
registration helper function to create a new user, then we assert that the
user is logged in (which happens after a successful registration).

```php
<?php
public function testRegister()
{
    $user = $this->randomUser();
    $this->doRegister($user);
    $logged_in_text = "You are logged in as {$user['username']}";
    $this->assertTextPresent($logged_in_text);
}
```

And finally we have a set of tests for validation logic in the signup form. First we test that each of the required fields results in an error on signup 
if the field is empty. Next we test if a mismatched password and password
confirmation generate the desired error, and then we test to make sure that the app doesn't allow a successful 
registration if various incorrect email formats are used.

```php
<?php
public function testRegisterFailsWithoutUsername()
{
    $user = $this->randomUser();
    $user['username'] = '';
    $this->doRegister($user);
    $this->assertTextPresent("Please enter a value");
}

public function testRegisterFailsWithoutName()
{
    $user = $this->randomUser();
    $user['name'] = '';
    $this->doRegister($user);
    $this->assertTextPresent("Please enter a value");
}

public function testRegisterFailsWithMismatchedPasswords()
{
    $user = $this->randomUser();
    $user['confirm_password'] = uniqid();
    $this->doRegister($user);
    $this->assertTextPresent("Fields do not match");
}

public function testRegisterFailsWithBadEmail()
{
    $user = $this->randomUser();
    $user['email'] = 'test';
    $this->doRegister($user);
    $this->assertTextPresent("An email address must contain a single @");
    $this->byId('email')->clear();
    $this->byId('email')->value('@bob.com');
    $this->byId('form.submitted')->click();
    $this->assertTextPresent("The username portion of the email address is invalid");
    $this->byId('email')->clear();
    $this->byId('email')->value('test@bob');
    $this->byId('form.submitted')->click();
    $this->assertTextPresent("The domain portion of the email address is invalid");
}
```

Now you have a basic
conceptual framework that you can use to start writing tests for your apps. 

All we're doing is 
using Selenium to input values and make sure that the app's response is
exactly what we want. Simple as they are, these login/signup tests are extremely valuable. 
Running them before every deployment will help ensure that you can 
welcome new users into your community and get them where they need to go.

## Testing local apps with Sauce Connect


Developing apps on `localhost` is extremely quick and efficient. However, `localhost` is not a publicly-accessible
address on the Internet, so by default the browsers in the Sauce Labs cloud cannot
load and test an app that you are running locally.

To get around this limitation, we created [Sauce Connect](https://saucelabs.com/docs/connect).
Sauce Connect uses a secure tunnel protocol that gives specific Sauce machines
access to your local network. Sauce Connect sessions are sandboxed
from outside data flows and are a convenient way to securely test apps that
aren't ready to be deployed on the Internet.

To download Sauce Connect for the tutorial project, edit the `composer.json` file in the
`sauce-tutorial` directory by putting a comma after the `sauce/sausage` line and adding the `sauce/connect` line so
that it looks like this:

```json
{
  "require": {
    "sauce/sausage": ">=0.16",
    "sauce/connect": ">=4.0"
  }
}
```

Now run this command to download Sauce Connect:

```bash
php composer.phar update
```

Sauce Connect is a fairly large binary file, so it may take a little while to
download. After it finishes downloading run this command:

**Mac/Linux:**

```bash
vendor/bin/sauce_connect
```


**Windows:**

```bat
vendor\bin\sauce_connect.bat
```


Since we already configured the Sauce credentials in an earlier tutorial,
Sauce Connect starts up without further ado. It takes a while to load because
it's provisioning a new clean virtual machine to handle the
secure connection. When it says "Connected! You may start your tests..." you
are good to go.

When Sauce Connect is running, all tests that you run using your Sauce Labs
account use the network on the machine where Sauce Connect is located.

For more information about Sauce Connect, or to download and configure the binary on your own, see the [Sauce Connect documentation](https://saucelabs.com/docs/connect).

## Running tests in parallel


As you may recall from earlier tutorials, Selenium tests can take a long time!
They may take even longer on Sauce because we start each test on a new virtual
machine that has never been used before (don't worry, we don't charge you for
the spin-up time).

The solution to this is to run more than one test at a time. Since we have
thousands of clean virtual machines on standby, we encourage you to run as many
tests as you can at once. For an overview of how many tests you can run in
parallel, see the parallelization section of our [plan
page](http://saucelabs.com/pricing).

Because PHPUnit doesn't have built-in parallel test execution, we work around
this by using [Paratest](http://github.com/brianium/paratest), a wrapper around
PHPUnit that is bundled with Sausage.  Let's try running some tests in parallel
using Paratest.

Navigate to the project directory and run the following command. This command
specifies the path to the test file we want to run and tells Paratest that we
want to simultaneously run two instances of PHPUnit.

**Mac/Linux:**

```bash
vendor/bin/paratest -p 2 -f --phpunit=vendor/bin/phpunit WebDriverDemo.php
```

**Windows:**

```bat
vendor\bin\paratest.bat -p 2 -f --phpunit=vendor\bin\phpunit.bat WebDriverDemo.php
```


Your tests should run approximately twice as fast as before. You can see the
test running simultaneously on your [Sauce tests
page](https://saucelabs.com/tests/). Depending on your Sauce account level,
consider increasing the number of processes to speed things up even more.

By default tests running in parallel may
execute in any order. In the next section we'll talk about
this and discuss some tips for avoiding potential snags when speeding up
test suites using parallelism.

## Tips for better Selenium test performance


In this section we'll share some tips about how to improve the performance of Selenium tests.

Avoid test dependencies
---

Simply put, dependencies between tests prevent your tests from being able to run in parallel. And running your tests in parallel is by far
the best way to speed up the execution of your entire test suite. It's much
easier to add a virtual machine than to try to figure out how to squeeze out another second
of performance from a single test.

What are dependencies? Imagine if we had a test suite with these two tests:

```php
<?php
function testLogin()
{
    // do some stuff to trigger a login
    $this->assertEquals("My Logged In Page", $this->title());
}

function testUserOnlyFunctionality()
{
    $this->byId('userOnlyButton')->click();
    $this->assertTextPresent("Result of clicking userOnlyButton");
}
```

In the first function's pseudocode the `testLogin()` function triggers the
browser to log in 
and asserts that the login was successful. The second test clicks a button on the
logged-in page and asserts that a certain result occurred.

This test suite works fine as long as the tests run in order. But the
assumption the second test makes (that we are already logged in) creates a 
dependency on the first test. If these tests run at the same time, or if the
second one runs before the first one, the browser's cookies will
not yet allow Selenium to access the logged-in page and the second test fails.

The right way to get rid of these dependencies is to make sure each test can 
run completely independently of the other. Let's fix the example above so
there are no dependencies:


```php
<?php
function doLogin()
{
    // do some stuff to trigger a login
    $this->assertEquals("My Logged In Page", $this->title());
}

function testLogin()
{
    $this->doLogin();
}

function testUserOnlyFunctionality()
{
    $this->doLogin();
    $this->byId('userOnlyButton')->click();
    $this->assertTextPresent("Result of clicking userOnlyButton");
}
```

The main point is that it is dangerous to assume any state whatsoever when
developing tests for your app. Instead, find ways to quickly generate
desired states for individual tests the way we did above with the `doLogin()` function 
-- it generates a logged-in state instead of assuming it. You might
even want to develop an API for the development and test versions of your app
that provides URL shortcuts that generate common states. For example, 
a URL that's only available in test that creates a random user account and 
logs it in automatically.

Don't use brittle locators
---
WebDriver provides a number of [locator strategies](http://code.google.com/p/selenium/wiki/JsonWireProtocol#/session/:sessionId/element) for accessing elements on a webpage. 
It's tempting to use complex XPath expressions like `//body/div/div/*[@class="someClass"]`
or CSS selectors like `#content .wrapper .main`. While these might work when
you are developing your tests, they will almost certainly break when you make
unrelated refactoring changes to your HTML output.

Instead, use sensible semantics for CSS IDs and form element names, and try to
restrict yourself to using `$this->byId()` or `$this->byName()`. This makes it 
much less likely that you'll inadvertently break your page by shuffling some
lines of code around.

Use spinAsserts
----
One issue with Selenium is that it doesn't know when it's supposed to wait
before executing the next command. When people click a submit button,
they know not to check for text or try another action until the next page
loads. Selenium, however, immediately executes the next command. If
the next command is part of an assertion in your code the assertion may
fail, even though if Selenium had waited a little bit longer it would have succeeded.

The solution is to use a special kind of assertion called a spinAssert that was introduced earlier in this tutorial. 
This kind of assertion only fails after retrying periodically over a specified amount of time.

The definition of the spinAssert function in Sausage looks like this:

```php
<?php
public function spinAssert($msg, $test, $args=array(), $timeout=10)
```

* `$msg` is the message that's passed to the Exception if the spinAssert fails.
* `$test` is the function we are going to use for the assertion. It can be:
  * An [anonymous function](http://php.net/manual/en/functions.anonymous.php)
  * A string representing a function name
  * An array of the form ```array($class_name, $method_name)```
* `$args` is an array of arguments that's passed to the test function.
* `$timeout` is an integer representing the number of seconds to spin.

The spinAssert function calls the `$test` function once every second until
the `$timeout` seconds limit is reached. If the `$test` function returns a true 
value the assertion succeeds If the `$test` function returns a false value the test is 
retried every second until it hits `$timeout` seconds. If it hits `$timeout` seconds the
assertion fails.

If spinAssert example earlier in the tutorial didn't have a spinAssert it would have looked like this:

```php
<?php
public function testSubmitComments()
{
    $comment = "This is a very insightful comment.";
    $this->byId('comments')->click();
    $this->keys($comment);
    $this->byId('submit')->submit();
    $this->assertEquals($this->byId('your_comments')->text(), "Your comments: $comment");
}
```

The trouble with this is that after Selenium hits the submit button it 
immediately gets the text from the comment results box (the element 
id `#your_comments`). If the page didn't have time to get updated from the
server the assertion fails even though it would have succeeded if Selenium had waited a 
bit longer. 

Let's look at the same function using spinAssert:

```php
<?php
public function testSubmitComments()
{
    $comment = "This is a very insightful comment.";
    $this->byId('comments')->click();
    $this->keys($comment);
    $this->byId('submit')->submit();
    $driver = $this;

    $comment_test =
        function() use ($comment, $driver)
        {
            return ($driver->byId('your_comments')->text() == "Your comments: $comment");
        }
    ;

    $this->spinAssert("Comment never showed up!", $comment_test);

}
```

In this function, we create an anonymous closure called `$comment_test` that
returns true if the comments field has the right text in it and false
if it doesn't. Then we call `$this->spinAssert` and pass it the test function.
We're using the default timeout of 10 seconds, so our script keeps retrying 
the Selenium `text()` command until its value is what we expect or we hit 10 seconds.

Using spinAssert functions is a great way to make tests less brittle and more
accepting of differences in network speeds, surges in traffic, and other challenges in the test environment.

## Integrating Sauce into existing PHPUnit WebDriver Tests


Do you already have a test suite running WebDriver tests? While we recommend
using Sausage, it's also possible to integrate with Sauce in whichever test framework you
happen to be using -- you'll just miss out on a lot of features you get for
free in Sausage (like automatic pass/fail reporting).  Let's take a look at an
example `PHPUnit_Extensions_Selenium2TestCase` test suite, so we can see what
needs to be modified to have it running on Sauce.

Before Sauce Integration
----
Below is a very simple example test suite consisting of two tests. Right now,
this test suite directly subclasses `PHPUnit_Extensions_Selenium2TestCase`,
and thereby uses a locally-running Firefox browser:

```php
<?php

require_once 'vendor/autoload.php';

class WebDriverExample extends \PHPUnit_Extensions_Selenium2TestCase
{

    protected $start_url = 'http://saucelabs.com/test/guinea-pig';

    public static $browsers = array(
        array(
            'browserName' => 'firefox',
            )
        )
    );

    public function testTitle()
    {
        $this->assertContains("I am a page title", $this->title());
    }

    public function testLink()
    {
        $link = $this->byId('i am a link');
        $link->click();
        $this->assertContains("I am another page title", $this->title());
    }
}
```

After Sauce Integration
----
Integrating is simple: we simply add a set of desired capabilities, and point
to a different Selenium server, one in our cloud of browsers. To ensure only
you have access to the tests you run, we'll need to augment the server address
with your Sauce username and access key (found on your [account page](http://saucelabs.com/account)).

Note that in this example, we assume these values are stored in PHP constants
(`SAUCE_USERNAME` and `SAUCE_ACCESS_KEY` respectively), defined wherever you like.

```php
<?php

require_once 'vendor/autoload.php';

define('SAUCE_HOST', SAUCE_USERNAME.':'.SAUCE_ACCESS_KEY.'@ondemand.saucelabs.com');

class WebDriverExample extends \PHPUnit_Extensions_Selenium2TestCase
{
    protected $start_url = 'http://saucelabs.com/test/guinea-pig';

    public static $browsers = array(
        array(
            'browserName' => 'firefox',
            'host' => SAUCE_HOST,
            'port' => 80,
            'desiredCapabilities' => array(
                'version' => '15',
                'platform' => 'Windows 2012'
            )
        )
    );

    public function testTitle()
    {
        $this->assertContains("I am a page title", $this->title());
    }

    public function testLink()
    {
        $link = $this->byId('i am a link');
        $link->click();
        $this->assertContains("I am another page title", $this->title());
    }
}
```

So you can see, getting going on Sauce with your existing tests is really quite
simple. However, by migrating to Sausage you get several useful features in
addition to a more convenient interface. Pass/fail reporting, spinAsserts,
and more streamlined browser configuration, etc...

