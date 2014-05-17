# Ansible health check module.

This module checks that an HTTP server is reachable and responding
as expected.

## Description

Sends multiple HTTP requests to a URL until the expected response is
received. The number of retries, the delay between retries as well as
the expected response are configurable.  

## Installation

Install this module by placing the file `health_check` in a directory
named `libaries` under your playbook or role directory.

## Parameters

| parameter             | required | default | comments |
| --------------------- | -------- | ------- | -------- |
| url                   | yes      |         | URLs to perform health checks on. |
| headers               | no       |         | Dictionary of HTTP headers to send in the request. |
| initial_delay         | no       | 0       | Number of seconds to wait before sending the first request. |
| delay_between_retries | no       | 5       | Number of seconds to wait between tries. |
| max_retries           | no       | 10      | Number of times to try before giving up. |
| timeout               | no       | 10      | Number of seconds to wait for a response for each request. If a response is not received within this number of second, the attempt is considered to be a failure. |
| expected_status       | no       | 200     | Expected HTTP status code. If the server responds with a different status code, then the attempt is considered to be a failure. |
| expected_regexp       | no       |         | An optional regular expectation that can be used to validate the response from the server. If the response body does not match the regular expression the attempt is considered as failed. Note that the regular expression tries to match from the beginning of the response. If you want to search anywhere within the response body use an expression like `".*OK"` |

## Examples

```yaml
# Performs a health check for a remote server from the machine running the
# play.

- name: Wait for server to pass health-checks
  health_check:
  url: "http://{{ inventory_name }}"
  delegate_to: 127.0.0.1

# Runs a health check for an HTTP server running on the current host.
# passes an Host header to reach the virtual host we want to test.

- name: Wait for API to pass health-check
  health_check:
    url: "http://127.0.0.1/api/v1/ok"
    delay_between_tries: 5
    max_retries: 20
    headers:
      Host: api.example.com
    expected_regexp: "ok"
```
