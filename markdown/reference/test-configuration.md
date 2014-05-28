{
  title: "Test Configuration",
  description: "How to configure your jobs on Sauce Labs",
  category: "Reference",
  index: 1
}

In Sauce, we refer to an individual run of a single Selenium test as a **job**. For example, if your test suite contains 100 Selenium tests, and you run the whole suite 3 times, Sauce will keep records of 300 jobs, one for each browser session. The following are additional settings you can use to annotate your jobs and configure Sauce on a per-job basis. This is done differently depending on the Selenium version you are using: [Selenium 1][1] or [Selenium 2][2] or with our [alternative job annotation methods][3].

Here you can find configurations for:

### Selenium 1 tests: The JSON Configuration

In Selenium 1 tests, Sauce-specific settings are given inside Selenium's "browser" parameter. This is generally a string in the form "*browser" (e.g. "*iexplore", "*firefox"), but will now need to be a full [JSON object][4] like this:

```json
{
  "username": "**your username here**",
  "access-key": "**your access key here**",
  "os": "Linux",
  "browser": "firefox",
  "browser-version": "3"
}
```

Any key-value pair specified in this documentation can be set through this JSON object.

### Selenium 2 tests: Desired Capabilities

In Selenium 2 tests, Sauce specific settings are provided using the Desired Capabilities object that Remote Webdriver libraries provide. Basically any key-value pair specified in this documentation can be set through this hash-like object.

Find more about RemoteDriver and the Desired Capabilities object in [Selenium's RemoteDriver wiki][5].

### Name Your Jobs

To make it easier to find and identify individual tests, use the _name_ setting to record test names on your jobs:


`Key:`
name

`Value Type:`
str

Example:

```python
"name": "my example name"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Record the Build Number

When looking through test results on our website, you'll probably want to know which version of your app the test was running against. Use this setting to annotate test jobs with a build number or app version. Once you set the build info on your job, it will be displayed on the job results page.

`Key:`
build

`Value Type:`
str

Example:

```python
"build": "build-1234"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Tag Your Jobs

To filter and group jobs more easily, users can provide tags for them.

`Key:`
tags

`Value Type:`
list

Example:

```python
"tags": [ "tag1", "tag2", "tag3" ]
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Record Pass/Fail Status

Selenium handles sending commands to control a browser, but doesn't report to the server whether a test passed or failed. To record pass/fail status in Sauce, set the _passed_ flag on the job.

Since you can't know in advance whether a test passed or failed, this flag can't be set in the initial configuration. Instead, you'll need to use one of our [alternative job annotation methods][3].

`Key:`
passed

`Value Type:`
bool

Example:

```python
"passed": true
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Record Custom Data

To give you an extensible way to annotate and differentiate tests, Sauce provides a custom annotation you can set that will accept any valid JSON object. This field is limited to 64KB in size.

`Key:`
custom-data

`Value Type:`
object

Example:

```python
"custom-data": { "release": "1.0",
                 "commit": "0k392a9dkjr",
                 "staging": true,
                 "execution_number": 5,
                 "server": "test.customer.com" }
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Alternative Job Annotation Methods

As an alternative to the settings that you can provide in advance, Sauce has two additional methods by which tests can set a subset of the job settings described earlier.
These are generally used to update the job with information that couldn't be foreseen at the time the test was created, such as the [pass/fail status of a test][10]. The methods are:

  * [Selenium 1's setContext()][21]
  * [Our REST API][22]

Both of these methods receive a JSON object and accept the subset of settings described below:

#### Accepted Keys

In both setContext and the REST API, the JSON object to update the job's information accepts the following keys and values:
  
  * [name][23]: string
  * [passed][10]: boolean
  * [public][24]: string
  * [tags][25]: array
  * [build][26]: integer
  * [custom-data][27]: JSON object

Here's an example of the JSON object you can send with either of these methods:

```python
{
    "name": "my job name",
    "passed": true,
    "public": "public",
    "tags": ["tag1", "tag2", "tag3"],
    "build": 234,
    "custom-data": {
        "release": "1.0",
        "server": "test.customer.com"
    }
}
```


#### setContext()

_setContext_ is a **Selenium 1** command that is not meant to do anything but send information to the server showing what's going on. When the value passed to setContext starts with _sauce:_, Sauce intercepts the command and parses it for job annotations.

We allow two formats for setContext: basic and advanced. The basic format lets you set tags, name, and pass/fail status for jobs. The advanced format lets you set more fields, and you can set them all in a single command.

Basic format:

```java
this.selenium.setContext("sauce:job-tags=tag1,tag2,tag3")
this.selenium.setContext("sauce:job-name=My awesome job")
this.selenium.setContext("sauce:job-result=passed")
this.selenium.setContext("sauce:job-result=failed")
```

Advanced format:

Our advanced format involves submitting a JSON-encoded dictionary as the value of sauce:job-info. You can set as many or as few of the fields as you wish. For example, in Java, include the following code in your test to add information to a job:

```java
this.selenium.setContext("sauce:job-info={\"name\": \"my job name\"," +
                                "\"tags\": [\"tag1\", \"tag2\", \"tag3\"]," +
                                "\"passed\": true,"+
                                "\"build\": \"103\","+
                                "\"custom-data\": {\"field\": \"value\"}"+
                                }");
```

#### Update jobs via our REST API

Our REST API provides a way to set the same additional information in jobs via a JSON object sent with an HTTP PUT command.

Selenium 2 tests, which can't leverage the setContext command, can use this API and some custom code to set job info even after the test is over. Here's an example of setting job info using curl, from the command line:

```bash
curl -H "Content-Type:text/json" -s -X PUT -d '{"name": "my job name 2"}' http://<username>:<key>@saucelabs.com/rest/v1/<username>/jobs/<job-id>
```

If you were to use this from your tests, you would probably want to build a simple set of functions that do the request for you. We've created a [Java library][6] for this, and here are some examples for [Python][7] and [Ruby][8]. We would love to see users share libraries for other languages!

[Read more about our REST API.][9]

## Performance Improvements and Data Collection

### Disable Video Recording

By default, Sauce records a video of every test you run. This is generally handy for debugging failing tests, as well as having a visual confirmation that certain feature works (or still works!) However, there is an added wait time for screen recording during a test run. You can avoid this by optionally disabling video recording with this setting:

`Key:`
record-video

`Value Type:`
bool

Example:

```python
"record-video": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

As an alternative, the _video-upload-on-pass_ setting will let you discard videos for passing tests identified using the<a href="#record-pass-fail-status"><em>passed</em> setting</a>. This disables video post-processing and uploading that may otherwise consume some extra time after your test is complete.

`Key:`
video-upload-on-pass

`Value Type:`
bool

Example:

```python
"video-upload-on-pass": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Disable Step By Step Screenshots

Sauce captures step-by-step screenshots of every test you run. Most users find it very useful to get a quick overview of what happened without having to watch the complete video. However, this feature may add some extra time to your tests. You can avoid this by optionally turning off this feature.

`Key:`
record-screenshots

`Value Type:`
bool

Example:

```python
"record-screenshots": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Disable Logs Gathering

By default, Sauce creates a log of all the actions that you execute to create a report for the test run that lets you troubleshoot test failures easier.

`Key:`
record-logs

`Value Type:`
bool

Example:

```python
"record-logs": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Enable HTML Source Captures

In the same way Sauce [captures step-by-step screenshots][11], we can do the same with HTML source captures for you. Even though this feature is turned off by default, you can turn it on anytime and find the sources in your job result page:

`Key:`
capture-html

`Value Type:`
bool

Example:

```python
"capture-html": true
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Enable Selenium 2's Automatic Screenshots

Selenium 2 captures automatic screenshots in every server side failure (e.g. element not found). Differently from Selenium, Sauce prevents this by default to reduce traffic back and forth from your tests, causing a considerable improvement in most tests. If you are willing to spend some of your testing time in them, you can get these screnshots back by turning this feature back on:

Notice: this setting only affects Selenium 2 tests.

`Key:`
webdriver.remote.quietExceptions

`Value Type:`
bool

Example:

```python
"webdriver.remote.quietExceptions": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Disable Sauce Advisor

Sauce Advisor analyzes your tests and suggests ways to make them faster and more robust. It may add a small amount of extra time to your tests. To disable this feature, use the following setting:

`Key:`
sauce-advisor

`Value Type:`
bool

Example:

```python
"sauce-advisor": false
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

## Selenium Specific Modifications

### Use a Specific Selenium Version

We keep our service up to date with the current state of the Selenium project. For the cutting edge improvements from the project to make it to our service, we periodically update the default version of Selenium that we are running in our test servers.

If you find any problems with a particular version of Selenium or for any other reason you'd like to keep your tests running on a specific version without keeping up with our updates, you can do so using this key.

`Key:`
selenium-version

`Value Type:`
str

Example:

```python
"selenium-version": "2.41.0"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

The current version being used as default is: **`2.30.0`**.
The list of supported versions you can choose from:
`2.26.0` `2.27.0` `2.28.0` `2.29.0` `2.30.0` `2.31.0` `2.32.0` `2.33.0` `2.34.0` `2.35.0` `2.36.0` `2.37.0` `2.38.0` `2.39.0` `2.40.0` `2.41.0`

### Selenium RC's Single Window Mode

By default, to get the most out of videos and screenshots, our tests run in [ multi-window mode][12]. You can change that by setting the _single-window_ setting to true:

Notice: this setting only affects Selenium 1 tests.

`Key:`
single-window

`Value Type:`
bool

Example:

```python
"single-window": true
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Selenium RC's User Extensions

User extensions are available for custom Selenium RC functionality on the Sauce service. Given the URL of a file on an accessible HTTP or FTP server (public or connected with Sauce Connect), Sauce will download it and use it in your test. A list of several extensions can be provided.

Notice: this setting only affects Selenium 1 tests.

`Key:`
user-extensions-url

`Value Type:`
list

Example:

```python
"user-extensions-url": [ "http://saucelabs.com/ext/flex.js", "ftp://username:password@server.com/bleh.js" ]
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Selenium RC's Custom Firefox Profiles

Custom Firefox profiles allow you to configure the browser running in our cloud on a per-job basis. This includes both plugins and any particular setting your tests may need.

This feature is provided for Selenium 1 tests. WebDriver users, should use the official FirefoxProfile class [ as specified in the WebDriver documentation][13].

To use this feature, a zip file with the contents of the Firefox profile directory you wish to use needs to be provided. Given the URL of a file on an accessible HTTP or FTP server (public or connected with Sauce Connect), Sauce will download it and use it in your test.

For more info on Firefox profiles, you can check [Mozilla's knowledge base][14].

`Key:`
firefox-profile-url

`Value Type:`
str

Example:

```python
"firefox-profile-url": "http://saucelabs.com/example_files/notls.zip"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

**Note**: If you actually zip the directory, it will not work. The zip file needs to contain the contents of the profile, not a directory with the contents of it.

## Timeouts

### Maximum Test Duration

As a safety measure to prevent broken tests from running indefinitely, Sauce limits the duration of tests to 30 minutes by default. You can adjust this limit on a per-job basis. The value of this setting is given in seconds.

`Key:`
max-duration

`Value Type:`
int

Example:

```python
"max-duration": 300
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Command Timeout

As a safety measure to prevent Selenium crashes from making your tests run indefinitely, Sauce limits how long Selenium can take to run a command in our browsers. This is set to 300 seconds by default. You can adjust this limit on a per-job basis. The value of this setting is given in seconds.

`Key:`
command-timeout

`Value Type:`
int

Example

```python
"command-timeout": 300
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Idle Test Timeout

As another safety measure to prevent tests from running too long after something has gone wrong, Sauce limits how long a browser can wait for a test to send a new command. This is set to 90 seconds by default. You can adjust this limit on a per-job basis. The value of this setting is given in seconds.

`Key:`
idle-timeout

`Value Type:`
int

Example:

```python
"idle-timeout": 60
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

## Sauce Specific

### Pre-run Executables

Sauce allows users to provide a URL to an executable file, which we will download and run for them before tests start. If you want to pre-configure the VM before your test starts, you need to use the _prerun_ capability.

This capability takes a JSON object with 3 main keys:

  * **executable**: The url to the executable that you want to be run before your browser session starts
  * **args**: A list of the command line parameters that you want the executable to receive
  * **background**: A boolean that defines whether Sauce should wait for this executable to finish before your browser session starts. If background isn't set or is set to _false_, Sauce will wait for up to 90 seconds for the executable to finish. Just then your browser will start and your test will proceed.

`Key:`
prerun

`Value Type:`
object

Example:

```python
"prerun": { "executable": "http://url.to/your/executable.exe",
            "args": [ "--silent", "-a", "-q" ], "background": true }
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

**AutoIt**: If you want to run an AutoIt script during your test, compile it as an exe, send it using this capability and set _background_ to _true_ so it stays running throughout the full duration of your test.

**Multiple Pre-run Executables**: If you need to send multiple pre-run executables, the best way is to bundle them into a single executable file.

### Use Identified Tunnels

If an [identified tunnel][15] is started using Connect, your jobs can choose to proxy through it using this set of keys with the right identifier.

`Key:`
tunnel-identifier

`Value Type:`
str

Example:

```python
"tunnel-identifier": "MyTunnel01"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Use specific screen resolution

Sauce allows users to provide their desired resolution of the screen by using "screen-resolution" key. This feature is in beta and is available for tests running on Windows XP, Windows 7 (except Windows 7 with IE 9), Windows 8/Windows 8.1, OSX 10.6 and OSX 10.8. We do not yet offer specific resolutions for OSX Mavericks

Valid values for Windows XP, Windows 7, and OSX 10.6 are:<br/> "800x600", "1024x768", "1280x1024", "1440x900" and "1920x1200".

Valid values for OSX 10.8 are:<br/> "1024x768", "1280x1024", "1400x900" and "1920x1200".

Valid values for Windows 8/8.1 are:<br/> "1024x768" and "1280x1024"

`Key:`
screen-resolution

`Value Type:`
str

Example:

```python
"screen-resolution": "1280x1024"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Custom Time Zones

Sauce has recently added support for setting custom time zones using the "time-zone" key. This feature should work on all operating systems, however time zones on Windows machines are approximate. They will default to the time zone that the provided location falls into. A complete list of valid locations [can be found here on Wikipedia][20]. Sauce takes only location names, not their paths, like in the example below.

Example:

```python
"time-zone": "Samoa"
```

###64-Bit Internet Explorer Driver

We have recently added support for launching 64-bit IE on our 64-bit vms: Windows 7, Windows 8, and Windows 8.1. This provides a workaround for a known selenium bug where screencaptures using the 32-bit driver on a 64-bit operating system do not capture the whole web page. If you would like to use a 64-bit IE Driver, you can do so using this key.

`Key:`
iedriver-version

`Value Type:`
str

Example:

```python
"iedriver-version": "x64_2.41.0"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

The list of supported IE Drivers you can choose from:
`x64_2.29.0` `x64_2.39.0` `x64_2.40.0` `x64_2.41.0`

### Disable Popup Handler

Sauce has its own Popup killer that automatically clicks through some types of browser popup windows to let tests continue. By default, this feature is turned on for Selenium 1 and off for Selenium 2 tests. You can control the popup handler yourself with the following capability:

`Key:`
disable-popup-handler

`Value Type:`
bool

Example:

```python
"disable-popup-handler": true
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Avoid Selenium Proxy

By default, Sauce routes all traffic from browsers through the Selenium HTTP proxy server so that HTTPS connections with self-signed certificates work everywhere. Sometimes, though, the Selenium proxy server can cause problems for some users. If that's the case for you, you can configure Sauce to avoid using the proxy server and have browsers communicate directly with your servers.

**Note**: Using Selenium 1, avoid-proxy doesn't work with `*safariproxy`, `*firefoxproxy`, `*iexploreproxy` or `*opera` browsers. This flag is currently incompatible with [Sauce Connect][16].

`Key:`
avoid-proxy

`Value Type:`
bool

Example:

```python
"avoid-proxy": true
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

## Job Sharing

### Make Your Jobs Public

If you want to share your test's result page and video, you can make it public. This can be done manually from the test result page, but also programatically using the **public** setting with [desired capabilities][17] when starting a test or with our [REST API][9]. Making your test public means that it is accessible to everyone and visible on the [Sauce Now][18] page.

`Key:`
public

`Value Type:`
str

Example:

```python
"public": "public"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Restrict What's Shown in Public Jobs

If you want to share your job's result page and video, but keep the logs only for you, you can certainly do so with **public restricted** visiblity mode. This visibility mode will hide the fancy job log as well as prohibit access to the raw Selenium log, so that anonymous users with the link will be able to watch the video and screen shots but won't be able to see what's being typed and done to get there.

`Key:`
public

`Value Type:`
str

Example:

```python
"public": "public restricted"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Share with co-workers

If you want to share your jobs with other team members (that were created as a sub-accounts of one parent account), you can use **team** visiblity mode. Making your test acessible by team means that it is only accessible to people under the same root account as you.

`Key:`
public

`Value Type:`
str

Example:

```python
"public": "team"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Share only with friends

You can also decide to make your test **sharable**. Making your test sharable means that it is only accessible to people who have a valid link and it is NOT VISIBLE on the [Sauce Now][18] page.

`Key:`
public

`Value Type:`
str

Example:

```python
"public": "share"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

### Do not allow anyone to view your jobs - set them to Private

If you don't want to share your test's result page and video with anyone, you should use **private** job visibility mode. This way, only you (owner) will be able to view assets and test result page.

`Key:`
public

`Value Type:`
str

Example:

```python
"public": "private"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

**Note**: For more details about sharing jobs, check our [Job Results Integration][19] docs.

## Mobile Testing Options

### Device Orientation

By default, mobile emulators are run in portrait orientation. You can also set them to landscape orientation.

`Key:`
device-orientation

`Value Type:`
str

Example:

```python
"device-orientation": "landscape"
```

Learn more about how to configure your tests with these settings in [Selenium 1][1] or [Selenium 2][2] tests.

## Building links to jobs

### Login required links to jobs

In Selenium, when a client requests a new browser session, the server returns a session ID, which is used to identify that session throughout the test. The session ID is stored as a member variable of the instantiated Selenium object and named "sessionId" or "session_id," depending on the client library. We use that session ID as the job id for accessing test results on our website.

To directly access a specific job, you will first need to note the session ID locally, usually by writing it to a log file. You can then use it to create a URL with the following format and replace <b>&lt;jobid&gt;</b> with the session ID.


    http://saucelabs.com/jobs/<jobid>

Notice that links to jobs in this format will only work if you are logged in with the account that ran the job or if that account is a sub-account of yours. For generating public links, read the section below, [ no-login links to jobs](#public-job-links).

**Note**: Selenium 1's Java client does not give public access to the session ID attribute of the DefaultSelenium object. However, we store a `selenium.sessionId` JavaScript variable that you can access using [getEval](http://bit.ly/cI51Dv).

### No-login links to jobs

The links generated in [login required links to jobs](#login-links) can be made in a way that doesn't require anonymous viewers to login and use your credentials. This mechanism is based in authentication tokens.

Auth tokens are generated on a per-job basis and give the receiver access using an [hmac-based algorithm](http://en.wikipedia.org/wiki/HMAC). You can also find [hmac implementations for different programming languages](http://en.wikipedia.org/wiki/HMAC#External_links).

The digest algorithm to use is **MD5**. The message and key used to generate the token should be the following:

Key:&lt;username&gt;:&lt;access-key&gt;<br/>
Message:&lt;job-id&gt;

Here's an example in Python for generating the token for a job with id: 5f9fef27854ca50a3c132ce331cb6034

```python
import hmac
from hashlib import md5
hmac.new("example_user:123456-asdf-8dcf81f1fc71", "5f9fef27854ca50a3c132ce331cb6034", md5).hexdigest()
      '3fca4184e106622adf2d33d8023271c1'
```

Once the auth token has been obtained, it can be used to build a link in the following format:

`https://saucelabs.com/jobs/<job-id>?auth=<token>`

For our example job, the link would end up being:

`https://saucelabs.com/jobs/5f9fef27854ca50a3c132ce331cb6034?auth=8859d634f5a51fea1a66e74708cf822a`

**Notice**: the link won't work as this job doesn't really exist.

### Temporary links to jobs

There's a way to extend the links generated in [no-login links to jobs][1] to make them work only temporarily.

The authentication token can be generated in a way that provides 1 hour or 1 day of access to the job by using the following information for the hmac generation:


Key:&lt;username&gt;:&lt;access-key&gt;:&lt;date-range&gt;<br/>
Message:&lt;job-id&gt;

The date range can take two formats: **YYYY-MM-DD-HH** and **YYYY-MM-DD**. These **should be set in UTC time** and will only work during the date or hour chosen and the following.

## Downloading job assets

You can download every asset created after your test runs on Sauce through our REST API. These include the video recording, Selenium log, and screenshots taken on crucial steps. After a standard job execution, the following files are created:

  * selenium-server.log
  * video.flv
  * XXXXscreenshot.png (where XXXX is a number between 0000 and 9999)
  * final_screenshot.png

To pull them, make a request matching the following pattern:


      GET https://saucelabs.com/rest/v1/<username>/jobs/<job_id>/assets/<file_name>


Sauce uses HTTP Basic Authentication. Each request needs to include an authorization HTTP header with your username and access key.

Here's an example using curl to download selenium-server.log:

```bash
curl -u sauceUsername:sauceAccessKey https://saucelabs.com/rest/v1/jhuggins/jobs/2z234fd4c4006b1e73faf3fa3cdadc75/assets/selenium-server.log
```

## Embedding Results in HTML Pages

### Embedding full job pages

We offer a simple way to embed job pages in CI test results or other test reports. Using the format below, add the HTML to any page you need to embed job results on, replacing <b>&lt;job_id&gt;</b> with the ID of the job you want:

```html
<script type="text/javascript" src="https://saucelabs.com/job-embed/<job_id>.js"></script>
```

**Note**: this will only work for browsers logged in using your account, and authentication tokens can be used to make this work for anonymous viewers. Check out [no-login links to jobs][1] for directions on generating these tokens.

### Embedding the video player

In addition to full job results, we offer a simple way to embed videos as well. Using the format below, add the HTML to any page you need to embed job videos on, replacing <b>&lt;job_id&gt;</b> with the ID of the job you want:

```html
<script type="text/javascript" src="https://saucelabs.com/video-embed/<job_id>.js"></script>
```

**Note**: this will only work for browsers logged in using your account, and authentication tokens can be used to make this work for anonymous viewers. Check out [no-login links to jobs][1] for directions on generating these tokens. Here's how such a script might look:

```html
<script src="https://saucelabs.com/video-embed/7dcb077bfcfd43a0a9d50011dd3bc01c.js?auth=6a7dcf9f2d8e7039699bd0280a7f4504"></script>
```

## REST API

For more advanced usage and integrations, you should read our [REST API](/reference/rest-api/) documentation.

   [1]: #selenium-1-tests-the-json-configuration
   [2]: #selenium-2-tests-desired-capabilities
   [3]: #alternative-job-annotation-methods
   [4]: http://www.json.org
   [5]: http://code.google.com/p/selenium/wiki/RemoteWebDriver
   [6]: https://github.com/saucelabs/saucerest-java
   [7]: https://gist.github.com/1644439
   [8]: https://gist.github.com/DylanLacey/5218959
   [9]: /reference/rest-api/
   [10]: #record-pass-fail-status
   [11]: #disable-step-by-step-screenshots
   [12]: http://seleniumhq.org/docs/05_selenium_rc.html#multi-window-mode
   [13]: http://code.google.com/p/selenium/wiki/FirefoxDriver
   [14]: http://support.mozilla.com/en-US/kb/Managing-profiles
   [15]: /reference/sauce-connect/#managing-multiple-tunnels
   [16]: /reference/sauce-connect/
   [17]: #selenium-2-tests-desired-capabilities
   [18]: https://saucelabs.com/now
   [19]: https://saucelabs.com/docs/integration
   [20]: http://en.wikipedia.org/wiki/List_of_tz_database_time_zones
   [21]: #setcontext-
   [22]: #update-jobs-via-our-rest-api
   [23]: #name-your-jobs
   [24]: #make-your-jobs-public
   [25]: #tag-your-jobs
   [26]: #record-the-build-number
   [27]: #record-custom-data
