## Management > Service Monitoring > 콘솔 사용 가이드

## 서비스 관리

Service Monitoring에서 제공하는 모니터링 및 장애 관리의 기준이 되는 서비스를 관리합니다.

### 서비스 추가
- Service Monitoring은 서비스 기준으로 모니터링이 등록되며, 서비스 별 장애 그룹을 대상으로 장애를 전파하며, 전파된 장애를 관리합니다. 따라서 서비스를 논리적으로 잘 구분하여 관리할 수 있도록 서비스 트리를 제공합니다. 
- 서비스 트리 상단의 **+추가** 버튼을 클릭하여 서비스를 추가할 수 있습니다.

### 전파 설정
- Service Monitoring에서 장애를 전파 받을 담당자를 설정합니다. 
- 프로젝트 멤버로 등록된 사용자만 등록 가능합니다.
- 전파 설정은 먼저 그룹을 등록하고, 그룹 별 담당자, 전파 받을 채널을 선택하여 설정할 수 있습니다.

### 전파 그룹
1. 여러 전파 그룹을 등록한 경우, 최초 장애를 감지한 경우
   - 첫 번째 그룹에만 장애가 전파됩니다. 첫 번째 그룹의 사용자가 **전파 현황** 페이지에서 다음 그룹으로 전파(추가적인 장애 전파) 또는 전파 중지(장애 아님)를 선택할 수 있습니다.
   - 3시간동안 발생한 장애를 처리하지 않았을 경우 자동으로 다음 그룹으로 장애 내용이 전파됩니다.
2. 하나의 그룹만 등록한 경우
   - 장애가 하나의 그룹에만 전파되고 종료됩니다.
3. 그룹을 등록하지 않은 경우
   - 장애가 전파되지 않고, 전파 현황에도 등록되지 않습니다.
  

### 전파 채널
1. **Email**
   - 회원정보에 등록한 아이디를 기준으로 장애를 Email을 통해 전파합니다.
2. **SMS**
   - 회원정보에 휴대폰 번호를 등록한 경우 장애를 SMS를 통해 전파합니다.
3. **KakaoTalk Alimtalk**
   - 회원정보에 휴대폰 번호를 등록하고, KakaoTalk을 사용하는 경우 장애를 KakaoTalk Alimtalk을 통해 전송합니다.
4. **웹 훅** 
   - 사용자의 요구에 맞도록 장애 발생 시 동작할 HTTP API를 등록하면, 장애 발생 시 해당 API를 호출하는 웹 훅 기능을 제공합니다. (사용 예시, GitHub 이슈 등록 및 Slack 연동 등) 장애 감지 내용, 서비스 이름, 시나리오 이름 등 감지된 장애와 관련된 정보는 미리 정의된 변수를 통해 치환되어 URL, 헤더, 요청 데이터에서 사용할 수 있으며, 장애 전파 시 치환되어 전송됩니다.
   - URL, 웹훅 헤더, 요청 데이터 입력 에디터에서 자동 완성(ctrl + space) 기능을 사용하여 미리 정의된 변수를 확인 및 사용할 수 있습니다.


### 전파 현황
- 감지 및 전파된 장애 이력을 확인할 수 있습니다.
- 전파 그룹에서 설명한 것처럼, 다음 그룹으로 장애를 전파하거나 중지하는 기능을 사용할 수 있습니다.

## 웹 모니터링
HTTP, HTTPS를 통해 서비스를 제공하는 모든 웹 서비스를 모니터링할 수 있습니다.

### 시나리오 타입
1. **API 타입** 
    - Rest API를 모니터링하기 위한 타입입니다.
    - 최소 60초 주기로 시나리오를 등록할 수 있습니다.
2. **가상 브라우저 타입** 
    - 가상 브라우저를 활용하여 웹 페이지를 모니터링하기 위한 타입입니다. 
    - 사용자 정의 자바스크립트를 등록하여 페이지의 동작을 모니터링할 수 있습니다.
    - 모듈 타입 시나리오를 포함할 수 있으며, 모듈 타입 시나리오를 순차적으로 먼저 실행 후 시나리오를 실행합니다. 모듈 및 가상 브라우저 시나리오 사이의 세션 및 쿠키는 공유되며, 이를 활용하여 로그인 후 페이지의 동작 여부 등을 테스트할 수 있습니다.
    - 최소 120초 주기로 시나리오를 등록할 수 있습니다.
3. **모듈 타입** 
    - 여러 시나리오에서 공통으로 필요한 기능(로그인 등)을 제공하기 위해 제공하는 타입입니다. 
    - 모듈 타입 단독으로 동작할 수 없으며, 가상 브라우저 타입에 포함되어 동작합니다.

### 시나리오 검증

시나리오 검증방식은 다음과 같습니다.

| 검증 방식 | 설명 | 기본 설정 | 비고 |
| -- | -- | -- | -- |
| 응답 코드 | HTTP 응답 코드를 검증 | 200 | |
| 타임아웃 | 요청 후 응답시간까지 검증 | 5000 (ms) |
| 텍스트 검증 | 응답 데이터(화면)에 특정 텍스트의 존재 여부 검증 | 없음 | JsonPath, XPath 지원 |
| 이미지 검증 | 응답 화면에 이미지가 존재하고, 다운로드 가능 여부 검증 | 없음 | 모듈, 가상 브라우저 타입만 지원 |
| 전파 제외 검증 | 응답 데이터(화면)에 특정 텍스트가 존재할 경우 장애 전파를 하지 않음 | 없음 | 점검 시 사용 |


## TCP 모니터링

TCP, UDP, ICMP 프로토콜을 활용하여 모니터링할 수 있습니다.

### 시나리오 타입
1. **ICMP 타입**
    - 서버의 상태를 모니터링하기 위한 Ping 테스트를 진행할 수 있습니다.

2. **TCP 타입**, **UDP 타입**
    - IP:Port로 접속 후, 데이터를 전송하고, 데이터를 응답받는 과정을 테스트하고, 응답받은 데이터의 검증을 수행합니다.
    - IP:Port의 상태를 체크하는 용도로도 사용할 수 있습니다.

## 배치 모니터링

웹 모니터링, TCP 모니터링과는 달리 Service Monitoring에서 직접 모니터링을 시도하는 것이 아니라, Service Monitoring에서 제공하는 API를 사용자가 호출하고 API의 데이터를 Service Monitoring에서 검증하여 장애 여부를 판단하는 모니터링입니다.

### 검증 방식
- 내용 검증
  - 사용자가 사전에 등록해놓은 시나리오를 바탕으로 실제 사용자가 전송한 데이터와 비교하여 장애 여부를 판단한다.
  - JSONPath(https://goessner.net/articles/JsonPath/) 방식을 사용하여 시나리오 데이터와 실제 데이터를 비교한다.
- 횟수 검증
  - 특정 시간동안 사용자의 요청 횟수를 확인하여 사용자가 설정해놓은 횟수와 비교하여 장애 여부를 판단한다.

### 사용 예
    - 빌드 서버의 빌드 결과를 배치 모니터링으로 전송하여, 빌드 결과를 모니터링할 수 있습니다.
    - 일간 배치의 실행 후 배치 모니터링 API를 호출하도록 하여, 일간 배치의 성공 여부 및 동작 여부를 모니터링할 수 있습니다.


## 전파 관리
- `일시적 전파 중지`란 배포와 같은 담당자가 인지하는 일시적 장애에 대해서 장애가 발생하여도 장애로 판단하지 않도록 하는 기능입니다.
- `일시적 전파 중지`는 각 모니터링 별로 설정할 수 있습니다.
- 시작 시간과 종료 시간을 설정할 수 있으며 설정한 시간이 지난후에는 장애 발생시 설정되어 있는 전파 그룹으로 장애가 전파됩니다.