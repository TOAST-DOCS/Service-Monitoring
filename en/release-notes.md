<!-- pre-align:aligned sig=1a93f00c2c5a -->

<a id="monitoring-service-monitoring-release-notes"></a>
## Monitoring > Service Monitoring > Release Notes { #monitoring-service-monitoring-release-notes }

<a id="may-28-2024"></a>
### May 28, 2024 { #may-28-2024 }
* Changed the category for Service Monitoring from Management to Monitoring.

<a id="november-15-2022"></a>
### November 15, 2022 { #november-15-2022 }
* Changed the domain of a notification page URL to nh.nu that is provided via transmission channels in case of failure.

<a id="monitoring-service-monitoring-release-notes-november-15-2022"></a>
### November 15, 2022 { #monitoring-service-monitoring-release-notes-november-15-2022 }
* Changed the domain of a notification page URL to nh.nu that is provided via transmission channels in case of failure.

<a id="december-28-2021"></a>
### December 28, 2021 { #december-28-2021 }

<a id="december-28-2021-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where, if the name of a scenario exceeds 31 characters, the history Excel file for two or more scenarios including the scenario could not be downloaded

<a id="july-27-2021"></a>
### July 27, 2021 { #july-27-2021 }

<a id="july-27-2021-feature-updates"></a>
#### Feature Updates
* New progress bar applied
* Security improvements applied

<a id="april-27-2021"></a>
### April 27, 2021 { #april-27-2021 }

<a id="april-27-2021-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where diffusion target could not be viewed in a project with specific user

<a id="march-23-2021"></a>
### March 23, 2021 { #march-23-2021 }

<a id="march-23-2021-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue of being unable to view depending on the search conditions on the failure notification page
* Fixed an issue of displaying an expiration prompt upon selecting a list on the failure notification page
* Fixed an issue where the previous HTTP header would not go away when editing webhooks
* Fixed an issue of failing to include the start time when searching history


<a id="december-29-2020"></a>
### December 29, 2020 { #december-29-2020 }

<a id="december-29-2020-bug-fixes"></a>
#### Bug fixes

* Fixed the issue where API call's resultCode and resultMessage are displayed differently for each method (POST, GET, PUT, DELETE) of scenario management open API
* Fixed the issue where CloudTrail event is always applied as USER_CONSOLE
* Fixed the issue where CloudTrail PM change event is registered as PM register event


<a id="november-24-2020"></a>
###  November 24, 2020 { #november-24-2020 }

<a id="november-24-2020-functions-added"></a>
####  Functions added
*  The open API function [Modification](/Monitoring/Service%20Monitoring/ko/api-guide/#_8) added for scenario management
*  Webhook request parameters now supported
*  "LINE" message send webhook templates added


<a id="october-27-2020"></a>
### October 27, 2020 { #october-27-2020 }

<a id="october-27-2020-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the scenario failed to be created if the '-' character was included in the url field of requestBody when creating a scenario with openAPI
* Fixed an issue where a batch monitoring scenario could be created when no validation information is entered
* Fixed an issue where the content of data edit request was not displayed correctly when editing the webhook information


<a id="september-22-2020"></a>
### September 22, 2020 { #september-22-2020 }

<a id="september-22-2020-more-features"></a>
#### More Features
* Added open API functions for scenario management: [Create](/Monitoring/Service%20Monitoring/ko/api-guide/#_8), [Query](/Monitoring/Service%20Monitoring/ko/api-guide/#_19), [Delete](/Monitoring/Service%20Monitoring/ko/api-guide/#_25)

<a id="august-25-2020"></a>
### August 25, 2020 { #august-25-2020 }

<a id="august-25-2020-bug-fixes"></a>
#### Bug Fixes
* Fixed failed application of a call result of API even when it is called immediately after batch monitoring scenario is registered. 

<a id="july-28-2020"></a>
### July 28, 2020 { #july-28-2020 }

<a id="july-28-2020-feature-updates"></a>
#### Feature Updates 
* Changed the mimimum unit for web monitoring operations
  * API Monitoring: 60 seconds ---> 30 seconds 
  * Virtual browser, Module monitoring: 120 seconds ---> 60 seconds

<a id="july-28-2020-bug-fixes"></a>
#### Bug Fixes
* Fixed cases, in which case sensitivity was not included as part of batch monitoring verification  

<a id="may-26-2020"></a>
### May 26, 2020 { #may-26-2020 }

<a id="may-26-2020-feature-updates"></a>
#### Feature Updates 
* Fixed failed cases of web monitoring test when it took more than 30 seconds 
* Added operators to be applied for each type of scenario or response content, to verify web monitoring texts
  * API
    * Only contain and !contain are available, when the response is _HTML_ or _XML_  
    * (==, !=, >, >=, <, <=) are available, when the response is _JSON_, by using _JsonPath_ 
  * Browser, Module
    * (==, !=, >, >=, <, <=) are available, when the response is _HTML_, _XML_, by using contain, !contain, or _xPath_
    * (==, !=, >, >=, <, <=) are available, when the response is _JSON_, by using _JsonPath_
* Service Integration with TOAST CloudTrail 
  * User events that occur from Service Monitoring console can be found at TOAST CloudTrail 


<a id="march-24-2020"></a>
### March 24, 2020 { #march-24-2020 }

<a id="march-24-2020-feature-updates"></a>
#### Feature Updates 
* Provides [JsonPath Method](/ko/Monitoring/Service%20Monitoring/ko/console-guide/#_9) to validate web monitoring data
* _Organization Name_ and _Project Name_ are added to an email error message
* Supports [Validate Multiple Batch Monitoring API](/ko/Monitoring/Service%20Monitoring/ko/api-guide/) 
* Emphasizes failures from batch monitoring validation results 

<a id="january-21-2020"></a>
### January 21, 2020 { #january-21-2020 }

<a id="january-21-2020-feature-updates"></a>
#### Feature Updates
* Supports the US Region for Web/TCP monitoring 
* Added the option of monitoring region for the search of monitoring history

<a id="december-24-2019"></a>
### December 24, 2019 { #december-24-2019 }

<a id="december-24-2019-feature-updates"></a>
#### Feature Updates
* Updated detail history messages when it fails to transmit failure 
* Upgraded the web page performance

<a id="december-24-2019-bug-fixes"></a>
#### Bug Fixes
* Fixed the transmission status page which did not properly show transmission history to users 

<a id="november-26-2019"></a>
### November 26, 2019 { #november-26-2019 }

<a id="november-26-2019-feature-updates"></a>
#### Feature Updates
* Separated transmission status information from the transmission status page. 
* Fixed the issue of broken UIs when the user language is English 
* Updated scenario editing for web monitoring 
  * Updated to show different placeholder for each text verification operator 
  * Text verification operator may be limited in exposure, depending on the [Scenario Verification Type].
  * Updated to enable **Ignore Script Errors**, **Exclude Images from Downloading**, **Activate CSS**, even for when the [Scenario Verification Type] is module. 

<a id="november-26-2019-bug-fixes"></a>
#### Bug Fixes  
* Fixed the issue, in which mail for [Guide for Canceling Faulty Transmission] is sent by the recipient's name, not by the user who canceled.


<a id="august-27-2019"></a>
### August 27, 2019 { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### Feature Updates
* Japanese font changed into TOAST Common Font.
* Separated input labels for IP from PORT, on the editing page of TCP monitoring scenario for better readability.

<a id="august-27-2019-bug-fixes"></a>
#### Bug Fixes
* Fixed the display error of time on the [View Transmission Details] page.
* Updated, for the registration of monitoring error, to register transmission title in the language set when a scenario is saved.
* Fixed the wrong notification of expiration, even if it is not expired, when you access error notification page via error transmission message
* Fixed error in which a wrong message title is added, for Japanese error transmission messages

<a id="july-23-2019"></a>
### July 23, 2019 { #july-23-2019 }

<a id="july-23-2019-new-service-release"></a>
#### New Service Release
Service Monitoring is a service failure management platform to allow stable service operations
	* Transmission administrator assigned for each service, to manage transmission channels
	* Supports a variety of monitoring methods - Web Monitoring, TCP Monitoring, or Batch Monitoring
