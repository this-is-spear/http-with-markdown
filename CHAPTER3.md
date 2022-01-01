# HTTP (**H**yper**T**xt **T**ransfer **P**rotocol)

## 모든 것이 HTTP

HTTP 메시지에 모든 것을 전송

- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML(API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
- **지금은 HTTP 시대**

HTTP 역사

- HTTP/0.9 1991년 : GET 메서드만 지원, HTTP 헤더 X
- HTTP/1.0 1996년 : 메서드, 헤더 추가
- **HTTP/1.1 1997년 : 가장 많이 사용, 우리에게 가장 중요한 버전**
    - RFC2068(1997) -> RFC2616(1999) -> RFC7230~7235(2014)
    - 7230 버전을 이용하자
- HTTP/2 2015년 : 성능 개선
- HTTP/3 진행중 : TCP 대신 UDP 사용, 성능 개선

> HTTP 1.1 내용에 대해 공부를 하자

기반 프로토콜

- TCP : HTTP/1.1, HTTP2
- UDP : HTTP3
- 현재는 HTTP/1.1을 주로 사용한다.

> HTTP/2, HTTP3도 점점 증가하는 추세, 구글은 2와 3을 사용하고, NAVER는 1.1과 2를 주로 사용

HTTP 특징

- 클라이언트 서버 구조
- 무상태 프로토콜(Stateless), 비연결성
- HTTP 메시지
- 단순함, 확장 가능

## 클라이언트 서버 구조

- Request Response 구조
- 클라이언트는 서버에 요청(Request)을 보내고, 응답(Response)을 대기
- 서버가 요청에 대한 결과를 만들어서 응답

## Stateful, Stateless

### 무상태 프로토콜(Stateless)

- 서버가 클라이언트의 상태를 보존하지 않는다.
- 장점 : 서버 확장성이 높다.(스케일 아웃)
- 단점 : 클라이언트가 추가 데이터 전송

### 상태 유지(Stateful) 와 무상태 프로토콜(Stateless)의 차이

상태 유지

- 중간에 응답 서버가 바뀌면 안된다.

> 항상 같은 서버가 유지되어야 한다.

무상태

- 중간에 응답 서버가 바뀌어도 된다.
- 갑자기 클라이언트 요청이 증가해도 응답 서버를 대거 투입할 수 있다.

> 무상태는 응답 서버를 쉽게 바꿀 수 있다. -> 무한한 서버 증설 가능


### 무상태 프로토콜(Stateless) 한계

- 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
- 무상태가 필요한 서비스
    - 로그인이 필요 없는 단순한 서비스 소개 화면
- 상태유지가 필요한 서비스
    - 로그인
- 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용

## 비 연결성(connectionless)

연결을 유지하는 모델

- 서버와 연결을 유지한다.

연결을 유지하지 않는 모델

- 서버는 연결 유지 하지 않는다.
- 최소한의 자원으로 서버를 유지할 수 있다.

### 비 연결성

- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 한 시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처이하는 요청은 수십개 이하로 매우 작다.
- 서버 자원을 매우 효율적으로 사용할 수 있다.

### 비 연결성의 한계와 극복

- TCP/IP 연결을 새로 맺어야 한다. - 3 way handshake 시간 추가
- 웹 브러우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, CSS, 추가 이미지 등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속 연결(Persisten Connections)로 문제 해결
- HTTP/2, HTTP/3에서 더 많은 최적화가 이루어짐

### HTTP 지속 연결 (Persistent Connection)
요청하는 요소들을 다 받을 때 까지 HTTP 연결을 유지한다.

> **스테이스리스를 기억하자**
> 서버개발자들이 어려워하는 업무
>> 같은 시간에 딱 맞춰 발생하는 대용량 트래픽(선착순 이벤트)

## HTTP 메시지

### 시작 라인

### 요청 메시지

- start-line = request-line / status-line
- request-line = method SP request-target SP HTTP-version CRLF

1. HTTP 메서드 (GET : 조회)
2. request-target - 요청 대상(/search?q=hello&hi=ko)
3. HTTP Version

#### HTTP 메서드

- 종류 : GET, POST, PUT, DELETE ...
- 서버가 수행해야 할 동작을 지정
    - GET : 리소스 조회
    - POST : 요청 내역 처리

#### request-target - 요청 대상(/search?q=hello&hi=ko)

- absolute-path\[?query\] (절대 경로\[?쿼리\])
- 절대경로 = "/"로 시작하는 경로

#### HTTP Version

- HTTP/1.1, HTTP/2, HTTP/3

### 응답 메시지


- start-line = request-line / status-line
- status-line = HTTP-version SP status-code SP reason-phrase CRLF

- HTTP 버전
- HTTP 상태 코드 : 요청 성공, 실패를 나타냄
    - 200 : 성공
    - 400 : 클라이언트 요청 오류
    - 500 : 서버 내부 오류
- 이유 문구 : 사람이 이해할 수 있는 짧은 상태 코드 설명 글 (EX : "OK")

### HTTP 헤더

- header-field = field-name":"OWS field-value OWS

> OWS: 띄어쓰기 허용

- field-name은 대소문자 구문 없음 but value는 대소문자 구분함

#### 용도

- HTTP 전송에 필요한 모든 부가 정보
- 예 : 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등등..
- 표준 헤더가 너무 많다.
- 필요시 임의의 헤더도 추가 가능하다.

#### HTTP 메시지 바디

#### 용도

- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등 BYTE로 표현할 수 있는 모든 데이터 전송 가능

> **HTTP는 단순해서 확장이 가능하다.**

## HTTP 정리

- HTTP 메시지를 이용해 모든 데이터를 전송할 수 있다.
- HTTP/1.1을 기준으로 HTTP를 학습하자.
- 클라이언트 서버 구조 (HTTP를 이용해 데이터를 주고 받는 모습)
- 무상태 프로토콜(스테이스리스)
- HTTP 메시지
- 단순함, 확장 가능성
