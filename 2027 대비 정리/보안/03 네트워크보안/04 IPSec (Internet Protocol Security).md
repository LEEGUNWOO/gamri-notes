## 📘 IPSec (Internet Protocol Security)

### 📌 개념 및 목적

|구분|내용|
|---|---|
|개념|•TCP/IP 구조적 결함 극복, IP 프로토콜 수준에서 인증·암호화를 제공하는 인터넷 표준 확장 기술|
|목적|•Inbound/Outbound 패킷 단위로 암호·인증 서비스 제공하여 상위 계층 보호<br>•주소 부족·느린 속도·보안취약성 등 TCP/IP 보안성 강화|
|활용 영역|•인증, 기밀성, 키 관리 3개 영역|

### 📌 특징

|특징|내용|
|---|---|
|IETF 표준화|•RFC 2401~2412로 문서화|
|TCP/IP 보안취약성 방지|•호스트 사이 암호화된 안전한 경로 설정 통신|
|환경변화에 유연|•IP보안 프로토콜과 키관리 매커니즘 독립적 설계|
|필요한 보안 서비스 선택사용|•인증(AH)과 암호화(ESP) 별도 프로토콜로 선택적 사용 가능|
|다양한 암호·인증 알고리즘|•각 보안 프로토콜에 새로운 보안 모드 추가 용이|

### 📌 구성요소

| 구성요소                                  | 내용                                                                        |
| ------------------------------------- | ------------------------------------------------------------------------- |
| SA (Security Association)             | •두 호스트가 사용할 보안 매커니즘·보안모드·인증·암호 알고리즘·키 값 등 매개변수에 대한 논리적 설정                 |
| AH (Authentication Header)            | •IP패킷 무연결 무결성·데이터 발신지 인증·선택적 재전송 방지 제공<br>•HMAC 메시지 인증코드 사용               |
| ESP (Encapsulating Security Payload)  | •IP패킷 기밀성 보안서비스 제공<br>•선택적으로 AH 제공 서비스 포함 가능                              |
| IKE (Internet Key Exchange) **(키교환)** | •IPSec 통신을 위한 호스트 인증·키관리를 SA를 통해 협상<br>•SA 설정·변경·삭제 절차와 형식을 정의한 키 관리 프로토콜 |

### 📌 AH vs ESP 보안 서비스 비교

|보안 서비스|AH|ESP|
|---|:-:|:-:|
|접근 제어 (Access Control)|O|O|
|비연결형 무결성|O|O|
|데이터 근원지 인증|O|O|
|재전송 방지 (Anti-Replay)|O|O|
|**비밀성 (Confidentiality)**|**✕**|**O**|
|**제한적 트래픽 흐름 비밀성**|**✕**|**O**|

※ AH와 ESP 차이 핵심: **비밀성(암호화)은 ESP만 제공**
### 📌 AH vs ESP 헤더 구조
![[Pasted image 20260614133503.png]]
#### 🔥 핵심 포인트 정리

**공통 필드**

SPI(Security Parameters Index)와 Sequence Number는 AH·ESP 둘 다 갖고 있어. SPI는 수신측이 SA(Security Association)를 찾는 32비트 인덱스고, Sequence Number는 재전송 공격 방지용 카운터야.

**AH만의 특징**

- Next Header, Payload Length, Reserved 필드가 맨 앞에 붙음
- **IP 헤더의 불변 필드까지 인증 범위에 포함** → 이게 NAT에서 망하는 이유 (NAT가 IP주소 바꾸면 ICV 검증 실패)
- 암호화 없음 → 도청 가능, 기밀성 제로

**ESP만의 특징**

- Payload 전체를 암호화 (블록 암호 정렬용 Padding 필드 존재)
- Pad Length + Next Header가 **암호화된 영역 끝에** 붙음 (Trailer)
- ICV는 암호화 이후 맨 뒤에 위치
- IP 헤더는 인증 범위 제외 → NAT 통과 가능 (NAT-T: UDP 4500 포트 캡슐화)

**시험 출제 포인트**

|구분|암기 키워드|
|---|---|
|AH|51번, 인증만, IP헤더 포함, NAT 불가|
|ESP|50번, 암호화+인증, IP헤더 제외, NAT-T 가능|
|둘 다|SPI + Sequence Number + ICV|
|ESP Trailer|Padding + Pad Len + Next Header (암호화 영역 내)|

### 📌 터널 모드 vs 전송 모드

| 구분    | 전송모드 (Transport)        | 터널모드 (Tunnel)                      |
| ----- | ----------------------- | ---------------------------------- |
| 적용 구간 | •End to End (단말기 ↔ 단말기) | •G/W to G/W (라우터 ↔ 라우터 ) **(VPN)** |
![[Pasted image 20260613112924.png]]



