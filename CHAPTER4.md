# HTTP 메서드

## HTTP API를 만들어보자

요구사항
회원 정보 관리 API를 만들어라.

- 회원 목록 조회
- 회원 조회
- 회원 등록
- 회원 수정
- 회원 삭제

API URI 설계를 한다면...

- 회원 목록 조회
    /read-member-list
- 회원 조회
    /read-member-by-id
- 회원 등록
    /create-member
- 회원 수정
    /update-member
- 회원 삭제
    /delete-member

> 가장 주요한 것은 리소스 식별

- 리소스의 의미는 뭘까?
- 회원을 등록하고 수정하고 조회하는게 리소스가 아니다.
- **회원이라는 개념 자체가 바로 리소스다.**
- 리소스를 어떻게 식별하는게 좋을까? 
- 회원을 등록하고 수정하고 조회하는 것을 모두 배제
- **회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑**


그렇다면...

- 회원 목록 조회
    /members
- 회원 조회
    /members/{id}
- 회원 등록
    /members/{id}
- 회원 수정
    /members/{id}
- 회원 삭제
    /members/{id}

> 계층 구조상 상위를 컬렉션으로 보고 복수 단어 사용 권장 (member -> members)

그럼 각 URI 끼리는 어떻게 구분하지?

## 그럼 리소스와 행위를 분리해야 한다.

**가장 중요한 것은 리소스를 식별하는 것**

- **URI는 리소스만 식별!**
- 리소스와 해당 리소스를 대상으로 하는 행위를 분리
    - 리소스 : 회원
    - 행위 : 조회, 등록, 삭제, 변경
- 리소스는 명사, 행위는 동사
- 행위는 HTTP 메서드를 통해 구분하자

## HTTP 메서드 종류

주요 메서드

- GET : 리소스 조회
- POST : 요청 데이터 처리, 주로 등록에 사용
- PUT : 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH : 리소스 부분 변경
- DELETE : 리소스 삭제

기타 메서드

- HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

### GET

- 리소스 조회
- 서버에 전달하고 싶은 데이터는 쿼리 스트링을 통해 전달
- 메시지 바디를 사용해 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아 권장하지 않음

### POST

- 요청 데이터 처리
- **메시지 바디를 통해 서버로 요청 데이터 전달**
- 서버는 요청 데이터를 처리
  - 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행
- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

### POST 정리

리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 한다. -> 정해진 것이 없다면

1. 새 리소스 생성
   - 서버가 아직 식별하지 않은 새 리소스 생성
2. 요청 데이터 처리
   - 단순히 데이터를 생성하거나, 변경하는 것을 넘어 어떤 프로세스를 처리해야 하는 경우
   - 예 : 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
   - POST의 결과로 새로운 리소스가 생성되지 않을 수 있음(리소스 만으로는 해결이 안될 때는 컨트롤 URI를 이용하자)
   - 예 : POST /orders/{orderId}/start-delivery (컨트롤URI)
3. 다른 메서드로 처리하기 애매한 경우
   - 예 : JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우 (예 : 메시지 바디에 넣고 싶을 때, GET 메시지 바디에 넣으면 메시지 바디를 아예 안읽을 수 있다.)
   - 애매하면 POST

조회 데이터는 최대한 GET, 새 리소스를 생성하거나 프로세스를 처리 아니면 어쩔 수 없는 경우는 POST를 사용 (POST를 이용해 캐싱하기가 정말 힘들기 때문에 조회는 GET을 이용)

### PUT

- 리소스를 대체
  - 리소스가 있으면 대체(완벽하게 대체)
  - 리소스가 없으면 생성
- **클라이언트가 리소스를 식별** (EX : members/{memberId} ) 
  - 클라이언트가 리소스 위치를 알고 URI 지정
  - POST와의 큰 차이점

### PATCH

- 리소스 부분 변경

### DELETE

- 리소스 제거

## HTTP 메서드의 속성

- 안전 (Safe Methods)
- 멱등 (Indempotent Methods)
- 캐시가능 (Cacheable Methods)

<table class="wikitable sortable jquery-tablesorter" style="text-align: center; font-size: 85%; width: auto; table-layout: fixed;">
<thead><tr>
<th class="headerSort headerSortDown" tabindex="0" role="columnheader button" title="첫글자로 정렬">HTTP 메소드
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">RFC
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">요청에 Body가 있음
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">응답에 Body가 있음
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">안전
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">멱등(Idempotent)
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="오름차순 정렬">캐시 가능
</th></tr></thead><tbody>
<tr>
<td>TRACE
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td></tr><tr>
<td>PUT
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td></tr><tr>
<td>POST
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td></tr><tr>
<td>PATCH
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc5789">RFC 5789</a>
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td></tr><tr>
<td>OPTIONS
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background: #ddffdd; color: black; vertical-align: middle; text-align: center;">선택 사항
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td></tr><tr>
<td>HEAD
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td></tr><tr>
<td>GET
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td></tr><tr>
<td>DELETE
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td></tr><tr>
<td>CONNECT
</td>
<td><a class="external mw-magiclink-rfc" rel="nofollow" href="https://tools.ietf.org/html/rfc7231">RFC 7231</a>
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;" class="table-yes">예
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td>
<td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;" class="table-no">아니요
</td></tr></tbody><tfoot></tfoot></table>

출처 : https://ko.wikipedia.org/wiki/HTTP


### 안전

- 호출해도 리소스를 변경하지 않는다.
- 계속 호출해서 로그가 쌓여 장애가 발생하는 문제도 고려하지 않는다.
- **안전은 해당 리소스만 고려한다.**

### 멱등

- f(f(x)) = f(x)
- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 같다.
- 멱등 메서드
  - GET : 한 번 조회해도 두 번 조회해도 같은 결과가 조회된다.
  - PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
  - DELETE : 결과를 삭제한다. 같은 요청을 여러 번 해도 삭제된 결과는 똑같다.
  - **POST : 멱등이 아니다. 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.**

멱등의 활용

- 자동 복구 메커니즘
- **서버가 TIMEOUT 등으로 정상 응답을 못줄 때, 클라이언트가 같은 요청을 다시 해도 되는가의 판단 근거가 된다.**

만약 재요청 중간에 다른 곳에서 리소스를 변경해버리면?

- **멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지 고려하지 않는다.**

### 캐시 가능

- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- GET, HEAD, POST, PATCH 캐시 가능
- 실제로 GET, HEAD 정도만 캐시로 사용
  - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데 구현이 어려움
  
