
# TCP 3 - way Handshake

## TCP 3 way handshake 란?

TCP는 장치들 사이에 논리적인 접속을 성립하기 위해  3 way handshake를 사용한다고 한다.

## 역할

- 양쪽 모드 데이터를 전송할 준비가 되었다는 것을 보장하고, 데이터 전달이 시작하기 전, 한 호스트가 다른 호스트가 준비되었다는 것을 알 수 있도록 한다.
- 양쪽 모두 상대편에 대한 초기 순차 시퀀스 번호를 얻을 수 있도록 한다.
- 순차 시퀀스 번호 초기 값을 0으로 하지 않는 이유는 동일한 연결의 두 개의 구현체가 너무 빠르게 움직여 어떤 구현체 값이 먼저 도착하는지 모르는 네트워크 통신에서 동일한 시퀀스 번호를 재사용하는 경우를 방지하기 위함이다.

## 동작 원리

정상적인 TCP 연결을 설정하는 데는 세 가지 단계가 필요하다:

1. 첫 번째 호스트는 두 번째 호스트에게 자체 시퀀스 번호 x와 함께 "동기화"(SYN) 메시지를 보내고, 이 메시지를 두 번째 호스트인 서버가 수신합니다.
2. 서버는 클라이언트가 수신하는 자체 시퀀스 번호 y 및 확인 번호 x+1로 동기화-승인(SYN-ACK) 메시지로 응답합니다.
3. 클라이언트는 인증 번호  y+1로 답장하고 서버는 답장할 필요가 없다.


<p><a href="https://commons.wikimedia.org/wiki/File:Three-way-handshake-example.gif#/media/File:Three-way-handshake-example.gif"><img src="https://upload.wikimedia.org/wikipedia/commons/f/f0/Three-way-handshake-example.gif" alt="Three-way-handshake-example.gif">

</a><br> By <a href="//commons.wikimedia.org/w/index.php?title=User:Guojunzheng&action=edit&redlink=1" class="new" title="User:Guojunzheng (page does not exist)">Guojunzheng</a> - Create some pictures and made it gif, <a href="https://creativecommons.org/licenses/by-sa/3.0" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=24426758">Link</a></p>

이 설정에서 동기화 메시지는 한 서버에서 다른 서버로 서비스 요청으로 작동하는 반면, 승인 메시지는 메시지가 수신되었음을 알리기 위해 요청 서버로 돌아간다.


>💡 클라이언트와 서버가 연결을 설정하기 위해 0과 같은 기본 시퀀스 번호를 사용하지 않는 이유는 동일한 연결의 두 가지 구현체가 너무 빨리 동일한 시퀀스 번호를 재사용하는 것을 방지하기 위함이다.