## Management > Service Monitoring > Console User Guide

## Service Management

You can manage the services to monitor, the targets to propagate failures, and channels.

### Add Service
-  Register monitoring and propagate / manage failure per service . To classify services logically for efficient management, a service tree is provided. 
- Click the **+Add** button at the top of the service tree to add a service.

### Set Propagation
-  Set the assignee who will receive the propagated failure. 
- Only those who are registered as project members can be registered.
- First, register a group, and then select an owner of each group and a channel to receive the propagation information.

### Propagation Group
- When several propagation groups have been registered, if the first failure is detected,
  - the failure is propagated to the first group only. User of the first group can select Propagate to the next group (additional failure propagation) or Stop propagation (not a failure) in the **Propagation Status** page.
  - If a failure is not handled within three hours, the failure is propagated to the next group.

- When only one group has been registered
  - The failure is propagated to only one group and terminated.

- When no group has been registered
  - The failure is not propagated nor registered to the Propagation Status.


### Propagation Channel
1. **Email**
   - Propagates a failure via email based on the ID registered in the member profile.
2. **SMS**
   - Propagates a failure via SMS when a mobile phone number has been registered in the member profile.
3. **Webhook** 
   - Provides a webhook function which calls the HTTP API on failure. The HTTP API should be registered according to the user's request (e.g. registration of GitHub issues and slack link). Information related to the detected failure, such as detection details, service name, and scenario name, is substituted to the pre-defined variables and used in the URL, header, and request data. The information is substituted and sent in case of failure propagation.
   - In the editor where URL, webhook header, and request data are entered, you can view or use pre-defined variables using the AutoComplete (Ctrl + Space) function.


## Propagation Status
- You can check the history of detected and propagated failures.
- You can use a function that propagates failure to the next group or stops propagation.

## Web monitoring
You can monitor all web services which serve via HTTP and HTTPS.

### Scenario Type
- **API Type** 
    - Monitors REST API.
    - You can register a scenario at every 30 seconds or higher.
- **Virtual browser type** 
    - Monitors the web page with a virtual browser. 
    - You can register the page behavior by registering a customized JavaScript.
    - Module-type scenario can be included; if a module-type scenario is included, it is executed first sequentially and then the other scenarios are executed. Sessions and cookies of the module and the virtual browser scenarios are shared. With those, you can test whether or not the page is working after logging in.
    - You can register a scenario at every 60 seconds or higher.
- **Module Type** 
    - Provides common features (such as login) for several scenarios. 
    - The module type cannot operate by itself; it only operates as part of the virtual browser type.

### Scenario Verification

Scenario is verified in the following way.

| Verification type | Description | Default settings | Remark |
| -- | -- | -- | -- |
| Response code | HTTP response code verification | 200 | |
| Timeout | Verifies until the response time after request | 5000 (ms) ||
| Text verification | Verifies whether or not a specific text exists on the response data (screen) | None | JsonPath and XPath supported |
| Image verification | Verifies whether an image exists on the response screen and it is downloadable or not. | None | Only module and virtual browser types are supported |
| Propagation Exception Verification | No failure is propagated when a specific text exists in the response data (screen). | None | Used for maintenance |

#### Function Support of Array Data for JsonPath Text Verification
| Function | Description | Output Type |
| -- | -- | -- |
| min() | Minimum Value of Array Data | double | 
| max() | Maximum Value of Array Data | double | 
| sum() | Sum of Array Data | double | 
| avg() | Average Value of Array Data | double | 
| stddev() | Standard Deviation of Array Data | double | 
| length() | Array Data Count | integer |
> 응답 바디에 포함된 배열 데이터만 함수 사용 가능

## TCP monitoring

You can use TCP, UDP, and ICMP protocols for monitoring.

### Scenario Type
- **ICMP Type**
  - You can perform the ping test to monitor the server status.

- **TCP Type**, **UDP Type**
  - Accesses with IP:Port, tests the process from sending data to receiving a response and verifies the response data.
  - It can be used for checking the IP:Port status.

## Batch Monitoring

Unlike web monitoring and TCP monitoring, this batch monitoring does not try monitoring directly from the Service Monitoring. Users call the API provided by Service Monitoring and then Service Monitoring verifies API data to determine a failure state.

### Verification Type
- Contents Verification
  - Determines whether failure occurred or not by comparing user-registered scenario in advance with data which are actually sent by users.
Compares scenario data with actual data using   - JsonPath (https://goessner.net/articles/JsonPath/).
- Count Verification
  - Determines whether failure occurred or not by comparing the count of user requests with the user-specified count during a specific period of time.

### Usage Example
    - A build result from the build server can be sent to the batch monitoring server to monitor the result.
    - Executes the daily batch and then calls the batch monitoring API to monitor whether or not the daily batch has been successfully operated.


## Propagation Management
- **Stop Propagation Temporarily** is a function that does not consider known failures (such as distribution) as failures even if this kind of failure occurs.
- **Stop Propagation Temporarily** can be set for each monitoring.
- You can set the start and end time. After the specified period, the failure is propagated to the specified propagation group.
