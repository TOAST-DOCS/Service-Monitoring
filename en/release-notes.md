## Management > Service Monitoring > Release Notes

### 2020. 07. 28.

#### 기능 개선
* 웹 모니터링 동작 최소 단위 변경
  * API 모니터링: 60초 ---> 30초
  * 가상 브라우저, 모듈 모니터링: 120초 ---> 60초

### 버그 수정
* 배치 모니터링 내용 검증 시 대소문자를 구분하지 않은 문제 수정

### 2020. 05. 26.

#### 기능 개선
* 웹 모니터링 테스트가 30초 이상 걸릴 경우 실패하는 문제 수정
* 웹 모니터링 텍스트 검증 시 시나리오 타입, 응답 콘텐츠 타입에 따라 사용할 수 있는 연산자 추가
  * API
    * 응답이 _HTML_, _XML_일 경우 contain, !contain만 사용 가능
    * 응답이 _JSON_일 경우 _JsonPath_를 활용하여 (==, !=, >, >=, <, <=) 사용 가능
  * Browser, Module
    * 응답이 _HTML_, _XML_일 경우 contain, !contain, _xPath_를 활용하여 (==, !=, >, >=, <, <=) 사용 가능
    * 응답이 _JSON_일 경우 _JsonPath_를 활용하여 (==, !=, >, >=, <, <=) 사용 가능
* TOAST CloudTrail 서비스 연동
  * Service Monitoring 콘솔에서 발생한 사용자 이벤트를 TOAST CloudTrail에서 확인 가능

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
