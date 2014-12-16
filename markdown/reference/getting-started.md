{
  title: "Getting Started",
  description: "An introduction to Sauce Labs and how to get started",
  category: "Reference",
  index: 10
}

##Sauce Connect

**Sauce Connect** is a secure tunneling application which allows you to execute tests securely when testing behind firewalls via an encrypted connection between Sauce Labs’ client cloud and your environment.

*Download the latest versions from https://docs.saucelabs.com/reference/sauce-connect/, our up-to-date site for all things Sauce Connect related. Sauce Connect will also notify you of updates when they are released on running.*

####System requirements 
Depending on the number of parallel tests you plan to run. Here are some samples based on simultaneous test volume:

|Machine Type|RAM|Processor|Parallel Tests|
|:------------:|:---:|:---------:|:--------------:|
| Local      | 4GB | 4GHz | 10 |
|  Dedicated | 8GB | 4GHz  | 100  |    
                              
For increased reliability and security, use a dedicated server. You may need to up your open file limit if your parallel test count is high (ulimit -n 8192).

####Points to Note
* It can serve as an alternative to IP whitelisting
* Sauce Connect 4.x will not work on Mac OS X versions less than 10.8
* Better performance is noted when run on a Linux or Mac OSX machine

###Setup

As of Sauce Connect 4.3.1, Proxies will be **auto-configured** based on the running system's settings.

* On **Windows**, Internet Explorer proxy settings will be checked as well as system-wide proxy settings set via Control Panel.
* On **Mac OS X**, Sauce Connect will use the proxy set in Preferences / Network. We support both the proxy and the PAC settings.
* On **Linux**, Sauce Connect looks for the following variables, in order: *http_proxy*, *HTTP_PROXY*, *all_proxy*, and *ALL_PROXY*. They can be in the form http://host.name:port or just *host.name:port*.

####Proxy Configuration

If auto-configuration fails, or the settings need to be overridden, there are a few command-line options that can be used to configure proxies manually: *-p, --proxy <host:port>, -w, --proxy-userpwd <user:pwd>, -T, --proxy-tunnel, and --pac
 
For all command-line options when starting Sauce Connect, view this page: https://docs.saucelabs.com/reference/sauce-connect/#command-line-options


##Latency

You should expect an individual test sent to Sauce to run about 3 times as slow as when run locally (per https://saucelabs.com/selenium/selenium-grid). Once you start running your tests in **parallel** however, you will see your tests run with all the quickness of a greased marble.

####Troubleshooting 

In case you observe severe latency, few helpful steps to fix it:

- Make sure the network connection is good (check here: http://sjc.saucelabs.com/) or ping -c 10 ondemand.saucelabs.com

- Use WebDriver Expected Conditions – in order to manage latency, expected conditions will keep your tests from failing when page loads are taking longer than they would on a local selenium grid.

- Set overall test time out – ***Key: max-duration*** – test timeouts by default are set to 1800 seconds. Make sure yours are tailored right for your needs. Find more info here: https://docs.saucelabs.com/reference/test-configuration/#maximum-test-duration

- Set Selenium command timeout – Key: command-timeout – these by default are set to 300 seconds. Make sure yours are tailored right for your needs. Find more info here: https://docs.saucelabs.com/reference/test-configuration/#command-timeout

- Rerun tests on failure

##Selenium 

\- Get introduced to the wonders of selenium with an extensive tutorial, just for you: https://docs.saucelabs.com/reference/selenium-quickstart/

\- Play with Selenium Builder to get introduced to Selenium scripting: http://sebuilder.github.io/se-builder/. Sauce Labs has a **plugin** for Builder that allows you to run tests in parallel on the sauce cloud.

\- Create short autonomous tests https://support.saucelabs.com/entries/23388772-Tips-for-lean-speedy-tests

\- Load a local Selenium server to test your tests — http://www.seleniumhq.org/download/

\- **Page Object Pattern** — When building your tests, you will want to make them as nimble to UI changes as possible. Setting them up using the page object design pattern will allow your tests to reference methods within the page object. When making changes to the application under test you will only make changes to your page object rather than multiple tests referencing that page object. More information here:   http://docs.seleniumhq.org/docs/06_test_design_considerations.jsp#page-object-design-pattern

\- **Identify the correct locator strategy** — Although you won't always be able to 
locate an element on all browsers with CSS, it is typically the speediest locating strategy. XPATH will get you there, but it can require your test to search the entire page to find the element you want. More info and a helpful video here: http://sauceio.com/index.php/tag/xpath/ 

\- Get good at the tricky stuff. Like all frameworks, there are many ways to skin the proverbial LOLcat, as there are limitations and tricks to overcome those limitations. A good place to start might be here: http://davehaeffner.com/works/

\- Keep up to date with Selenium releases!

##Test Management
###Set environment variables for Sauce Labs. 
Keep your Sauce Labs credentials out of your repositories and available to all your Sauce Labs tools using projects by adding them as environment variables.

- For Macs, 
 - Open ~/.bash_profile and add the following lines (an example):
 
            export SAUCE_USERNAME=JohnSmith
            export SAUCE_ACCESS_KEY=1234


 - You'll then need to re-load that profile with source ~/.bash_profile

- For Windows, 
 - Access System and Security within Control Panel
 - Select System
 - Under the Advanced Tab (or Advanced system settings depending on which version of Windows you are using) select "Environmental Variables..."
 - Select "New" and input the variable name `SAUCE_USERNAME` and your user name and `SAUCE_ACCESS_KEY` and your key

###Use a framework

A test automation framework is a system that integrates the function libraries, test data sources, object details and various reusable modules. The main advantage of using a framework is in case there is change to any test, then only the test case file needs to be updated, without having to modify the underlying components. 

Ideally, there is no need to update the scripts in case of changes to the application. Also, choosing the right framework/scripting technique helps in maintaining lower costs. 
Majority of our customers use a test framework to manage  their tests. Examples include JUnit for Java, RSPEC, Cucumber, for Ruby, MBUnit for .Net, Pyunit for Python


###Run tests as part of a build
SauceLabs has plugins for all the major CI tools:

- Jenkins: https://docs.saucelabs.com/ci-integrations/jenkins/
- Travis: https://docs.saucelabs.com/ci-integrations/travis-ci/
- Circle CI: https://docs.saucelabs.com/ci-integrations/circleci/
- Team City: https://docs.saucelabs.com/ci-integrations/teamcity/
- Bamboo: https://docs.saucelabs.com/ci-integrations/bamboo/

###Platform Configurator

Use our Platform Configurator: https://docs.saucelabs.com/reference/platforms-configurator/#/, to quickly create your desired capabilities (with the configurations of your choice) that can be plugged into selenium scripts. 

It's a good practice to store the generated code for your desired caps in a ***configuration file or database***. This enables you to:
- Initiate a test by calling the desired capabilities from this file instead of hard coding them within your test. 
- If there are any changes that need to be made to the tests such as adding new configurations or latest mobile versions, only the config file will require modification. 

### Parallel Execution
Another huge advantage of using Sauce labs is that you are able to test across multiple configurations in parallel significantly reducing execution time. Any language or test framework will allow for achieving parallel testing by utilizing multithreading or multiprocessing. 

Choose your preferred mode of parallelism within your framework. For example, Ruby has 2 popular libraries (or gems) for achieving parallelism known as *Parallel Gem* and *Peach* respectively. And both have their own merits. 


##Job Management

The only limit to running parallel tests on Sauce is the number of **concurrent VM's** that have been subscribed. Also you are able to queue several jobs beyond the subscribed concurrency. The formula for queued jobs is *2 x Concurrency + 150*. 

Use additional settings to configure Sauce on a per-job basis to collect more data, improve performance, and more: 

###Job Annotation
As part of your desired capabilities, you can send additional test information to Sauce Labs such as pass/fail metrics or build numbers. This enables you in optimizing test management as well as effectively using the reporting capabilities inherent to Sauce. Take a look: https://docs.saucelabs.com/reference/test-configuration/#job-annotation 

###Test Assets
Configure the capture of test assets (screenshots, screencasts and logs) or whether you want to disable them completely: https://docs.saucelabs.com/reference/test-configuration/#optional-features

###REST API
Use the REST API to pull test assets and other job details: https://docs.saucelabs.com/reference/rest-api/

### Selenium - Specific
We use different versions of selenium for varying browser versions especially IE and firefox. You are able to change the version of selenium by specifying it as a desired capability. https://docs.saucelabs.com/reference/test-configuration/#selenium-specific-modifications. It helps in debugging as well.

###Timeouts
Add adequate timeouts to the test sessions in order to ensure perfect execution: https://docs.saucelabs.com/reference/test-configuration/#timeouts

###Sauce - Specific

Want to set browser profiles, run a particular file/script in the background before or during test execution, do so using our ***pre-run*** capabilities:https://docs.saucelabs.com/reference/test-configuration/#pre-run-executables

Look at other sauce specific-settings that provide coverage on major test aspects: https://docs.saucelabs.com/reference/test-configuration/#sauce-specific-settings


##Account Structure


Many developers/testers are operating with a team of others, sometimes on the same project, sometimes differing projects. Sauce Lab's account structure allows you to create tiered accounts to manage team usage and partition teams into groups so their usage and permissions only pertain to what is applicable to them.  – more here https://docs.saucelabs.com/reference/team-management/#account-types

###Helpful hints:

- Create a parent account – You will want to create a parent account with Admin privileges. This is a special type of account intended to be used only by highly-trusted team members to hand off responsibility from the management staff. Admin accounts can add and delete other subaccounts of their parents. This is a powerful feature so give admin privileges only to a limited number of people.

- Create a CI account – If you're running your tests through a CI server, you will want to create an account strictly for this server to use.

- Create team accounts and nest Users
