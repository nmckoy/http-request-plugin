# Http Request Plugin for Jenkins

This plugin sends a HTTP/HTTPS request to a user speficied URL.

## Features

The following features are available in both Pipeline and traditional
project types:

* Programmable HTTP method: GET, POST, PUT, DELETE, or HEAD
* Programmable range of expected response codes (a response code outside the range fails the build)
* Supports Basic Authentication (see global configuration)
* Supports From Authentication (see global configuration)
* You can specify a string that must be present in the response (if the string is not present, the build fails)
* You can set a connection timeout limit (build fails if timeout is exceeded)
* You can set an "Accept" header
* You can set a "Content-type" header

### Basic plugin features

The following features are only present in the non-pipeline version of
the plugin. For the Pipeline version, these features are available
programmatically.

* You can send the build parameters as URL query strings
* You can store the response to a file, built-in to the plugin

### Pipeline features

In a Pipeline job, you have total control over how the url is
formed. Suppose you have a build parameter called "param1",
you can pass it to the HTTP request programmatically like so:

```groovy
httpRequest "http://httpbin.org/response-headers?param1=${param1}"
```

If you wish to save the response to a file, you need to grab a
workspace. You can do this with a `node` Pipeline step. For
example:

```groovy
def response = httpRequest "http://httpbin.org/response-headers?param1=${param1}"
node() {
    writeFile file: 'response.txt', text: response.content
}
```

You can access the response status code and content programmatically:

```groovy
def response = httpRequest "http://httpbin.org/response-headers?param1=${param1}"
println('Status: '+response.status)
println('Response: '+response.content)
```

For details on the Pipeline features, use the Pipeline snippet generator
in the Pipeline job configuration.

### Known limitations

* If Jenkins is restarted before the HTTP response comes back, the build will fail.
