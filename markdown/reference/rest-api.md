{
  title: "REST API",
  description: "Documentation for the Sauce Labs REST API.",
  category: 'Reference',
  index: 2
}

The Sauce Labs REST API allows customers to retrieve information about Sauce Labs resources programmatically over HTTP using JSON. Customers can retrieve job information, video replays of tests, Selenium logs of tests, start and stop SauceTunnels, and so on.

## Sauce API libraries

If you use Java, Ruby, PHP or node.js, you can use one of the below API libraries to conveniently access our API:

<ul class="list-inline inline-container">
  <li><a title="Java API Library" href="https://github.com/saucelabs/saucerest-java"><img src="/images/tutorials/java.png" alt="Java API Library"></a></li>
  <li><a title="Ruby API Library" href="https://github.com/saucelabs/sauce_whisk"><img src="/images/tutorials/ruby.png" alt="Ruby API Library"></a></li>
  <li><a title="PHP API Library" href="https://github.com/jlipps/sausage"><img src="/images/tutorials/php.png" alt="PHP API Library"></a></li>
  <li><a title="Node.js API Library" href="https://github.com/holidayextras/node-saucelabs"><img src="/images/tutorials/nodejs.png" alt="Node.js API Library"></a></li>
</ul>

## Getting Started

Users can authenticate using a [HTTP Basic Authentication][5] base64 encoded _Username_ and _API Access Key_. The easiest way to authenticate is to include the Sauce username and access key in the request URL.

*Note: All requests **must** have the `Content-Type` header set to `application/json`.*

Here is an example retreiving a user's recent jobs:

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs
```

Note that all below endpoints have a base URL of `saucelabs.com/rest/v1/` and default to a `GET` request unless specified.

## Account

> Access account information and create new sub-accounts.

### users/:username

> Access basic account information.

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername
```

Result:
```json
{
  "access_key": "sdgk234lkjl223j",
  "minutes": "200",
  "manual_minutes": "infinite",
  "mac_minutes": "100",
  "mac_manual_minutes": "infinite",
  "id": "sauce_username"
}
```

### users/:username POST

> Create a sub-account.

```bash
curl -X POST https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername
-H 'Content-Type: application/json' 
-d '{"username":"subaccount-username", "password":"subaccount-password", "name":"subaccount-name", "email":"subaccount-email-address"}'
```
Result:

```json
{
  "access_key": "subaccount-api-key",
  "minutes": 200,
  "id": "new-subaccount-username"
}
```

### users/:username/concurrency

> Check account concurrency limits.

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername/concurrency
```
Result:

```json
{
  "timestamp": 1397657955659,
  "concurrency": 10
}
```

## Test Activity and Usage

### :username/activity

> Returns active job counts broken down by job status and sub-account.

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/activity
```
Result:

```json
{
  "subaccounts": {
    "subaccount_user_1": {
      "in progress": 25,
      "all": 30,
      "queued": 5
    },
    "subaccount_user_2": {
      "in progress": 5,
      "all": 5,
      "queued": 0
    }
  },
  "totals": {
    "in progress": 30,
    "all": 35,
    "queued": 5
  }
}
```

### users/:username/usage
> Access historical account usage data.

**Accepted Query Params:**
* `start` and `end` in YYYY-MM-DD format.

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername/usage
```
Result:

```json
{
  "usage": [
    [
      "2011-3-18",
      [
        2,
        38
      ]
    ],
    [
      "2011-3-31",
      [
        5,
        5533
      ]
    ],
    [
      "2011-4-1",
      [
        7,
        5076
      ]
    ]
  ],
  "username": "sauceUsername"
}
```

## Jobs

### :username/jobs

> List all job ids belonging to a given user.

Example getting last 100 job ids:
```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs
```

Optional params:

#### ?limit
> Displays the specified number of jobs, instead of truncating the list at the default 100.

Default: `100`

Example getting last 200 job ids:
```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs?limit=200
```

#### ?full
> Returns full job information, rather than just IDs.

Default: `false`

**Available Job Attributes:**

* `id`: [string] Job Id
* `owner`: [string] Job owner
* `status`: [string] Job status
* `error`: [string] Error (if any) for the job
* `name`: [string] Job name
* `browser`: [string] Browser the job is using
* `browser_version`: [string] Browser version the job is using
* `os`: [string] Operating system the job is using
* `creation_time`: [integer] The time the job was first requested
* `start_time`: [integer] The time the job began executing
* `end_time`: [integer] The time the job finished executing
* `video_url`: [string] Full URL to the video replay of the job
* `log_url`: [string] Full URL to the Selenium log of the job
* `public`: [string or boolean] [Visibility mode][6] [public, public restricted, share (true), team (false), private]
* `tags`: [array of strings] Tags assigned to a job

Example getting full information about last 100 jobs:
```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs?full=true
```

#### ?skip
> Skips the specified number of jobs.

Default: `0`

Example getting last 100 job ids skipping 20 most recent jobs:
```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs?skip=20
```

#### ?to and ?from
> Returns jobs since/until the specified time (in epoch time, calculated from UTC).

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs?from=1357747500&to=1357748700
```

#### ?format
> Returns jobs in specified format. Currently we support `json` and `csv`.

Default: `json`

Example getting last 100 job ids using the CSV format:
```bash
 curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs?format=csv
```

### :username/jobs/:job_id
> Show the full information for a job given its ID.

Example:

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID
```

### :username/jobs/:job_id PUT
> Edit an existing job

**Request fields:**
* `name`: [string] Change the job name
* `tags`: [array of strings] Change the job tags
* `public`: [string or boolean] Set [job visibility][6] to "public", "public restricted", "share" (true), "team" (false) or "private"
* `passed`: [boolean] Set whether the job passed or not on the user end
* `build`: [int] The AUT build number tested by this test
* `custom-data`: [JSON] a set of key-value pairs with any extra info that a user would like to add to the job

Example:

```bash
curl -X PUT https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID
-H "Content-Type: application/json" 
-d '{"tags":["test","example","taggable"],"public":true,"name":"changed-job-name","passed": false, "custom-data":{"error":"step 17 failed"}}'
```

### :username/jobs/:job_id DELETE

> Removes the job from the system with all the linked assets.

Example:

```bash
curl -v -X DELETE http://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID
```

### :username/jobs/:job_id/stop PUT

> Terminates a running job.

Example:

```bash
curl -X PUT https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID/stop 
-d ''
```

### :username/jobs/:job_id/assets

> Get a details about the static assets collected for a specific job.

Response fields (each of these fields will be set to "null" if the specific asset isn't captured for a job):

* `sauce-log`: [string] Name of the Sauce log recorded for a job
* `selenium-log`: [string] Name of the selenium Server log file produced by a job
* `video`: [string] Name of the video file name recorded for a job
* `screenshots`: [array of strings] List of screenshot names captured by a job

Example:
```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID/assets
```

### :username/jobs/:job_id/assets/:file_name

> Download Job Assets - You can download every asset created after your test runs on Sauce through our REST API. These include the video recording, Selenium log, and screenshots taken on crucial steps.

Available Values for `:file_name`:

* selenium-server.log
* video.flv
* XXXXscreenshot.png (where XXXX is a number between 0000 and 9999)
* final_screenshot.png
Example:

```bash
curl -O https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID/assets/final_screenshot.png
```

### :username/jobs/:job_id/assets DELETE

> Delete all the data gathered during test run from our servers. That includes the video recording, Selenium log, and all the screenshots.

Example:
```bash
curl -X DELETE https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/jobs/YOUR_JOB_ID/assets
```

## Tunnels

Tunnels are used by [Sauce Connect][7] to redirect traffic for a given domain to a server on your internal network. They are unique to users and will not affect any other users. For more information, please read our [Sauce Connect documentation][7]

### :username/tunnels

>Retrieves all running tunnels for a given user.

Attributes:
'id': [string] Tunnel ID
'owner': [string] Tunnel owner
'status': [string] Tunnel status
'host': [string] Public address of the tunnel
'creation_time': [integer]
Example:

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/tunnels
```

### :username/tunnels/:tunnel_id
> Show the full information for a tunnel given its ID.

Example:

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/tunnels/YOUR_TUNNEL_ID
```

### :username/tunnels/:tunnel_id DELETE
> Shuts down a tunnel given its ID.

Example:

```bash
curl -X DELETE https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/tunnels/YOUR_TUNNEL_ID
```


## Information

Information resources are publicly available data about Sauce Lab's service.

### info/status

> Returns the current status of Sauce Labs' services.

```bash
curl -X GET http://saucelabs.com/rest/v1/info/status
```

Result:

```json
{
  "wait_time": 0.5,
  "service_operational": true,
  "status_message": "Basic service status checks passed."
}
```


### info/browsers/:selenum_version

> Returns an array of strings corresponding to all the browsers currently supported on Sauce Labs. (Choose the termination that defines which list you need, bearing in mind that Selenium 1 [RC] and 2 [WebDriver] are compatible with different browser/OS combinations.)

Accepted Values for `:selenium_version`: `all`, `selenium-rc`, or `webdriver`
Example:

```bash
curl -X GET http://saucelabs.com/rest/v1/info/browsers/webdriver
```

### info/counter

> Returns the number of test executed so far on Sauce Labs.

Example:

```bash
curl -X GET http://saucelabs.com/rest/v1/info/counter
```

## Partners

This section is for use only by Sauce Labs partner accounts. If you use these commands with a standard account, they may cause unexpected behavior. To add and manage sub-accounts, head to [your sub-accounts page][8].

Create a new subaccount, specifying a Sauce Labs service plan.

```bash
curl -X POST https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername -H 'Content-Type: application/json' -d '{"username":"subaccount-username", "password":"subaccount-password", "name":"sub-account-name", "email":"subaccount-email-address", "plan":"free"}'
```
**Available plans: ** 'free', 'small', 'team', 'com', 'complus'

Result:

```json
{
  "access_key": "subaccount-api-key",
  "minutes": 200,
  "id": "new-subaccount-username",
  "plan": "free"
}
```
Update a subaccount Sauce Labs service plan.

```bash
curl -X POST https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/users/sauceUsername/subscription -H 'Content-Type: application/json' -d '{"plan":"small"}'
```
Unsubscribe a subaccount from it's Sauce Labs service plan.

```bash
curl -X DELETE https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/sauceUsername/subscription
```

## Temporary Storage

Sauce Labs provides temporary storage inside our network for mobile apps, Selenium jars, prerun executables, and other assets required by your tests. Storing assets in our network can eliminate network latency problems when sending big files to Sauce. Here's how you use our storage:

### storage/:username/:your_file_name

**Accepted Query
overwrite=true URL parameter allows files already stored in the Sauce network to be overwritten. It can be removed if you prefer to prevent overwriting.

  * Before tests start, upload the file via our REST API as described below.
  * During tests, use a special URL for the file, in the following format: `"sauce-storage:your_file_name"`
  * Sauce will find the file, download it through our fast internal network and get your tests started right away

To upload the file via our REST API:

```bash
curl -u sauceUsername:sauceAccessKey 
-X POST "http://saucelabs.com/rest/v1/storage/sauceUsername/your_file_name?overwrite=true" 
  -H "Content-Type: application/octet-stream" --data-binary @/path/to/your_file_name
```

This can be scripted in any programming language. Just make sure the HTTP method being used is POST and the Content-Type header is correct.

Please note that our temporary storage retains files for only 24 hours. We recommend users of this service upload files via our REST API every time their tests are about to run, as part of their build process.

## JS Unit Testing

If you already have JS unit tests, running them on Sauce using the REST API is simple.

### :username/js-tests POST

> Start your JavaScript unit tests on as many browsers as you like with a single request:

Required POST data:

`url`:  should point to the page that hosts your tests.
`framework` can be `"qunit"`, `"jasmine"`, `"YUI Test"`, `"mocha"`, or `"custom"`.

The `"custom"` framework allows you to display generic test information on the Sauce Labs website. Set `window.global_test_results` on the javascript on your unit test page to an object that looks like the following and Sauce will report any failing tests: `

Example:

```bash
    curl -X POST https://saucelabs.com/rest/v1/sauceUsername/js-tests 
        -u sauceUsername:sauceAccessKey 
        -H 'Content-Type: application/json' 
        --data '{
            "platforms": [["Windows 7", "firefox", "27"],
                          ["Linux", "googlechrome", ""]],
            "url": "https://saucelabs.com/test_helpers/front_tests/index.html",
            "framework": "jasmine"}'
```


Response:

```json
{
      "passed": 1,
      "failed": 3,
      "total": 4,
      "duration": 4321,
      "tests": [
    {
          "name": "foo test",
          "result": false,
          "message": "sumthin bad",
          "duration": 4000
        },
    {
          "name": "bar test",
          "result": false,
          "message": "failure",
          "duration": 300
        },
    {
          "name": "baz test",
          "result": true,
          "message": "passed",
          "duration": 20
        },
    {
          "name": "qux test",
          "result": false,
          "message": "test bad",
          "duration": 1
        }
      ]
    }
```

Hosting your tests on your LAN or your laptop? You'll need to run [Sauce Connect][9] to bridge Sauce Labs to your local network. Optional parameters related to Sauce Connect include:

`tunnel_identifier` specifies the ID of a specific tunnel when using multiple Sauce Connect tunnels.
`parent_tunnel` specifies the username of a parent account whose shared Sauce Connect tunnel your tests should use.

Any other parameters get passed on as [Optional Desired Capabilities][10] for the selenium server. This means you can set things like: `max-duration`

The default `max-duration` for all JS Unit Tests is 180 seconds.

The response will look something like this:

```json
{
  "js tests": [
    "064df78366ea4b25b32f88878c9d7aa4",
    "1e5ed949711545bd952456ac37479ada"
  ]
}
```


### :username/js-tests/status

> Get the status of your JS Unit Tests

```bash
curl -X POST https://saucelabs.com/rest/v1/sauceUsername/js-tests/status 
        -u sauceUsername:sauceAccessKey 
        -H 'Content-Type: application/json' 
        --data '{"js tests": ["064df78366ea4b25b32f88878c9d7aa4", "1e5ed949711545bd952456ac37479ada"]}'
```

Do that a few times as the tests run, waiting until the response contains `"completed": true`.

Once the tests are completed, the result will look something like this:

```json
{
  "completed": true,
  "js tests": [
    {
      "id": "064df78366ea4b25b32f88878c9d7aa4",
      "job_id": "1a7aec8ef0c64165bcde1230e213ad44",
      "platform": [
        "Windows 8",
        "internet explorer",
        "10"
      ],
      "url": "http://saucelabs.com/jobs/ff737d47e03e47bfb45100a45e4b5ca5",
      "result": {
        "durationSec": 0.005,
        "passed": true,
        "suites": [
          {
            "description": "Player",
            "durationSec": 0.005,
            "passed": true,
            "specs": [
              {
                "description": "should be able to play a Song",
                "durationSec": 0.002,
                "failedCount": 0,
                "passed": true,
                "passedCount": 2,
                "skipped": false,
                "totalCount": 2
              }
            ]
          }
        ]
      }
    }
  ]
}
```
You can control the job attached to the JS unit test via the `job_id`.

## Bugs

Interacting with Jobs bug tracking system

### bugs/types

> Get list of available bug types

```bash
curl https://saucelabs.com/rest/v1/bugs/types
```
Result:

```json
[
  {
    "id": "bug-type-example-id",
    "name": "Bug-example-name",
    "description": "Bug example description"
  }
]
```

### bugs/types/:bug_id

> Get description of each field for a particular bug type

```bash
curl https://saucelabs.com/rest/v1/bugs/types/bug-type-example-id-1234
```
Result:

```json
[
  {
    "type": "string",
    "name": "Field-name",
    "id": "Field-id"
  },
  {
    "type": "int",
    "name": "Field-name",
    "id": "Field-id"
  }
]
```

### bugs/details/:bug_id

> Get detailed info for a particular bug

```bash
curl https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/bugs/detail/YOUR_BUG_ID
```
Result:

```json
{
  "Description": "Description of really nasty bug",
  "Title": "What a nasty bug",
  "ScreenshotEmbedURL": "https://saucelabs.com/jobs//YOUR_JOB_ID0001screenshot.png?auth=secret-auth-token",
  "CreationTime": 1234567890,
  "VideoEmbedURL": "https://saucelabs.com/bugs/?YOUR_JOB_IDshow_video=1&auth=secret-auth-token",
  "Job": "https://saucelabs.com/jobs/?YOUR_JOB_IDauth=secret-auth-token",
  "BugEmbedURL": "https://saucelabs.com/bugs/?YOUR_JOB_IDauth=secret-auth-token",
  "OS": "Linux",
  "Browser": "android 4."
}
```

### bugs/query/ids=[:id_1,:id_2]

> Get detailed info for a specified list of bugs

```bash
curl -G https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/bugs/query/ --data-urlencode 'ids=[""YOUR_BUG_ID, "0123401234-example-id-12345"]'
```
Result:

    List of JSON objects containing detailed info on each queried bug id

### bugs/update/:bug_id

> Update bug id `:bug_id` with specified key-value pairs

```bash
curl -G https://sauceUsername:sauceAccessKey@saucelabs.com/rest/v1/bugs/update/YOUR_BUG_ID --data-urlencode 'update={"Property-name-1": "Property-Value-1", "Property-name-2": "Property-Value-2"}
```

**Valid keys: **Only following bug properties can be modified with the API: 'Title', 'Description'.

Result(After successful command execution):

```json
{
  "status": "success"
}
```

   [1]: http://en.wikipedia.org/wiki/Representational_State_Transfer
   [2]: http://en.wikipedia.org/wiki/HTTP
   [3]: http://en.wikipedia.org/wiki/JSON
   [4]: http://en.wikipedia.org/wiki/Content-Type
   [5]: http://en.wikipedia.org/wiki/Basic_access_authentication
   [6]: https://saucelabs.com/docs/additional-config#public-jobs
   [7]: /reference/sauce-connect/
   [8]: https://saucelabs.com/sub-accounts
   [9]: /reference/sauce-connect/
   [10]: /reference/test-configuration/
