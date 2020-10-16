## Management > Service Monitoring > Release Notes

### 2020. 10. 27.

#### 버그 수정
* openAPI를 활용하여 시나리오를 생성할 때 requestBody의 url 필드에 '-'문자가 포함될 경우 시나리오가 생성되지 않던 문제 수정
* 배치 모니터링 시나리오를 생성할 때 검증 정보를 입력하지 않아도 생성 가능했던 문제 수정
* 웹훅 정보를 수정할 때 요청 데이터의 편집 항목의 내용이 정상적으로 보이지 않던 문제 수정

### March 24, 2020

#### Feature Updates 
* Provides [JsonPath Method](/ko/Management/Service%20Monitoring/ko/console-guide/#_9) to validate web monitoring data
* _Organization Name_ and _Project Name_ are added to an email error message
* Supports [Validate Multiple Batch Monotoring API](/ko/Management/Service%20Monitoring/ko/api-guide/) 
* Emphasizes failures from batch monitoring validation results 

### January 21, 2020

#### Feature Updates
* Supports the US Region for Web/TCP monitoring 
* Added the option of monitoring region for the search of monitoring history

### December 24, 2019

#### Feature Updates
* Updated detail history messages when it fails to transmit failure 
* Upgraded the web page performance

#### Bug Fixes
* Fixed the transmission status page which did not properly show transmission history to users 

### November 26, 2019

#### Feature Updates
* Separated transmission status information from the transmission status page. 
* Fixed the issue of broken UIs when the user language is English 
* Updated scenario editing for web monitoring 
  * Updated to show different placeholder for each text verification operator 
  * Text verification operator may be limited in exposure, depending on the [Scenario Verification Type].
  * Updated to enable **Ignore Script Errors**, **Exclude Images from Downloading**, **Activate CSS**, even for when the [Scenario Verification Type] is module. 

#### Bug Fixes  
* Fixed the issue, in which mail for [Guide for Canceling Faulty Transmission] is sent by the recipient's name, not by the user who canceled.


### August 27, 2019

#### Feature Updates
* Japanese font changed into TOAST Common Font.
* Separated input labels for IP from PORT, on the editing page of TCP monitoring scenario for better readability.

#### Bug Fixes
* Fixed the display error of time on the [View Transmission Details] page.
* Updated, for the registration of monitoring error, to register transmission title in the language set when a scenario is saved.
* Fixed the wrong notification of expiration, even if it is not expired, when you access error notification page via error transmission message
* Fixed error in which a wrong message title is added, for Japanese error transmission messages

### July 23, 2019

#### New Service Releases
Service Monitoring is a service failure management platform to allow stable service operations
	* Transmission administrator assigned for each service, to manage transmission channels
	* Supports a variety of monitoring methods - Web Monitoring, TCP Monitoring, or Batch Monitoring
