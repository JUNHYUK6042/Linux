# Router(Static, RIP) 실습

- 라우터는 서로 다른 네트워크를 연결하는 장치입니다.
- 물리적인 환경과는 무관하게 네트워크 ID가 다른 네트워크를 연결합니다.
- 라우팅을 제공하는 장치는 라우터 이외에도 거의 모든 네트워크가 지원되는 OS에서 가능합니다.

---

## Router Protocol

- 여기는 Static과 RIP 프로토콜에대해 정리할 것입니다.

1. Static
2. RIP
3. OSPF
4. EIGRP
5. BGP

---

## Routing Protocol

- 라우팅 프로토콜은 라우터가 목적지까지 가는 최적의 경로를 찾기 위해 사용하는 규칙이다.
- 경로 계산 방식과 서브넷 마스크 전달 여부로 분류할 수 있다.


- `Distance Vector (거리 벡터 방식)`
  - Hop count base (거쳐 가는 라우터 개수)
  - `RIP (대표 프로토콜)`
- `Link-State (링크 상태 방식)`
  - Cost base
  - `OSPF, IS-IS (대표 프로토콜)`
- `Advanced Distance Vector`
  - `EIGRP (대표 프로토콜)`
- `Classful - 라우팅 업데이트 시 서브넷 마스크를 함께 전달하지 않는 방식`
  - Subnet mask를 교환하지 않는다.
  - `RIP, IGRP (대표 프로토콜)`
- `Classless - 라우팅 업데이트 시 IP 주소 + 서브넷 마스크를 함께 전달하는 방식`
  - Subnet mask를 교환한다.
  - `RIPv2, OSPF, EIGRP, IS-IS, BG (대표 프로토콜)`
- `Bast Path - 하나의 목적지 네트워크로 가는 여러 경로 중 라우터가 최종적으로 선택한 경로를 의미`
  - Administrative Distance : 라우팅 프로토콜 간 신뢰도를 나타내는 값이며, 숫자가 낮을수록 우선된다

---

## Static Routing

- 모든 경로에 대한 정보를 직접 관리자가 입력해주는 방식입니다.
- 라우터 장치 이외에 리눅스나 유닉스 윈도우즈등의 OS에서도 기본적인 기능으로 제공합니다.
  - 일반 PC나 서버도 로컬 라우터로 이용할 수 있습니다.

---

## Static Routing 실습

### Static Routing 구성

![51](/KH_Security/Cisco%20Packet%20Tracer/img/51.png)
- 라우터 간 시리얼 라인으로 연결해줍니다.

- Router 장치 구성

![52](/KH_Security/Cisco%20Packet%20Tracer/img/52.png)

- PC1(1.1.1.3), PC2(2.2.2.3) 구성

![53](/KH_Security/Cisco%20Packet%20Tracer/img/53.png)
![54](/KH_Security/Cisco%20Packet%20Tracer/img/54.png)

---

### Static Routing - Router 설정

- 각각 R1과 R2의 라우터에 IP를 설정 해줍니다.

- R1의 기본 설정 & IP 할당

![55](/KH_Security/Cisco%20Packet%20Tracer/img/55.png)
![56](/KH_Security/Cisco%20Packet%20Tracer/img/56.png)


- R2의 IP 할당

![57](/KH_Security/Cisco%20Packet%20Tracer/img/57.png)

- R1, R2 시리얼 포트에 IP 주소를 할당한 뒤 상태 확인 해줍니다.

![58](/KH_Security/Cisco%20Packet%20Tracer/img/58.png)
![59](/KH_Security/Cisco%20Packet%20Tracer/img/59.png)

---

### Static Routing - Routing 정보 입력 & 상태 확인

- R1 -> R2에 대한 라우팅 정보를 정적으로 입력해줍니다.

![60](/KH_Security/Cisco%20Packet%20Tracer/img/60.png)

- R2 -> R1에 대한 라우팅 정보를 정적으로 입력해줍니다.

![61](/KH_Security/Cisco%20Packet%20Tracer/img/61.png)

- S 라인에 라우팅 정보가 입력되어 있습니다.

---

### Static Routing - PC간 통신 테스트

#### PC1(1.1.1.3) -> PC2(2.2.2.3) 통신 테스트

![62](/KH_Security/Cisco%20Packet%20Tracer/img/62.png)

#### PC1(2.2.2.3) -> PC2(1.1.1.3) 통신 테스트

![63](/KH_Security/Cisco%20Packet%20Tracer/img/63.png)

---

### Default Routing 실습

![64](/KH_Security/Cisco%20Packet%20Tracer/img/64.png)

---

## 동적 라우팅 (Router - RIP)

- 범위
  -  IGP(Interior Gateway Protocol) : RIP(v1, v2), IGRP, OSPF
  -  EGP(Exterior Gateway Protocol) : EGP, BGP
 
- 라우팅 정보
  - Distance Vector : RIP(v1, v2), IGRP
  - Link State : OSPF
 
- VLSM
  - Classful : RIP v1, IGRP
  - Classless : RIP v2, OSPF
 
---

### 명령어

| 명령어 | 설명 |
| --- | --- |
| router rip | RIP 프로토콜 설정 모드 진입 |
| router ospf | OSPF 프로토콜 설정 모드 진입 |
| router eigrp | EIGRP 프로토콜 설정 모드 진입 |
| router bgp | BGP 프로토콜 설정 모드 진입 |
| network IP | 광고할 네트워크 주소 지정 |
|  | 라우터에 연결된 모든 네트워크 주소를 입력 |
|  | rip은 넷마스크를 입력 X |

---

### Router RIP 설정 및 정보 확인 

#### R1 RIP 설정 및 확인

![65](/KH_Security/Cisco%20Packet%20Tracer/img/65.png)
![66](/KH_Security/Cisco%20Packet%20Tracer/img/67.png)

- R1 RIP 정보확인

![67](/KH_Security/Cisco%20Packet%20Tracer/img/66.png)

#### R2 RIP 설정 및 확인

![68](/KH_Security/Cisco%20Packet%20Tracer/img/68.png)
![69](/KH_Security/Cisco%20Packet%20Tracer/img/69.png)

- R2 RIP 정보확인
  - R 라인을 보면 라우팅 정보를 확인할 수 있습니다.

![70](/KH_Security/Cisco%20Packet%20Tracer/img/70.png)

#### R3 RIP 설정 및 확인

![71](/KH_Security/Cisco%20Packet%20Tracer/img/71.png)
![72](/KH_Security/Cisco%20Packet%20Tracer/img/72.png)

- R3 RIP 정보확인
  - R 라인을 보면 라우팅 정보를 확인할 수 있습니다.

![73](/KH_Security/Cisco%20Packet%20Tracer/img/73.png)

---

## 불필요한 업데이트

- 라우터들의 GigbitEthernet 영역은 라우팅 정보를 수신할 라우터가 없는 곳이므로 해당 인터페이스로 업데이트 할 필요가 없습니다.
- 그러므로 `passive-interface [Interface 명] 명령어를 사용해줍니다.
  - 해당 인터페이스로는 라우팅 정보를 업데이트 하지 않습니다.

---

## Router RIP V2

### Router RIP V2 설정 및 정보 확인 

#### R1 RIP V2 설정 및 확인

![74](/KH_Security/Cisco%20Packet%20Tracer/img/74.png)
![75](/KH_Security/Cisco%20Packet%20Tracer/img/75.png)

#### R2 RIP V2 설정 및 확인

![76](/KH_Security/Cisco%20Packet%20Tracer/img/76.png)
![77](/KH_Security/Cisco%20Packet%20Tracer/img/77.png)

#### R3 RIP V2 설정 및 확인

![78](/KH_Security/Cisco%20Packet%20Tracer/img/78.png)
![79](/KH_Security/Cisco%20Packet%20Tracer/img/79.png)

---

## Router RIP - Default Route

- Default Route 설저 시 RIP 환경에서는 default route 정보를 RIP를 통해 전달이 가능하다.

### Default Router 설정

- loopback 인터페이스 활성화합니다.
- 가상 인터페이스는 no shutdown 명령이 불필요합니다.

![80](/KH_Security/Cisco%20Packet%20Tracer/img/80.png)

### default route 인터페이스를 지정한 뒤에 광고하도록 설정

![81](/KH_Security/Cisco%20Packet%20Tracer/img/81.png)

### `show ip route` 명령어를 통해 상태 확인

![82](/KH_Security/Cisco%20Packet%20Tracer/img/82.png)

- R2 라우팅 정보 갱신 확인
  - R 라인을 보면 라우팅 정보가 갱신되었는지 확인할 수 있습니다.
  - 
![83](/KH_Security/Cisco%20Packet%20Tracer/img/83.png)

- R3 라우팅 정보 갱신 확인
  - R 라인을 보면 라우팅 정보가 갱신되었는지 확인할 수 있습니다.

![84](/KH_Security/Cisco%20Packet%20Tracer/img/84.png)

### 10.10.10.1 통신 테스트

- PC에서 10.10.10.1로 ping 명령어를 통해 통신 테스트를 했습니다.

![85](/KH_Security/Cisco%20Packet%20Tracer/img/85.png)

---
