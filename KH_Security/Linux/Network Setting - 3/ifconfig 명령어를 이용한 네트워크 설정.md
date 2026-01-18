# ifconfig 명령어를 이용한 네트워크 설정

---

## 인터페이스 및 기본 정보 확인

- 인터페이스 상태를 확인하는 명령어는 다음과 같습니다.

```text
ifconfig [NIC]
```

![01](/KH_Security/Linux/Network%20Setting%20-%203/img/01.png)

- ens160, ens224 그리고 루프백 인터페이스(lo)가 출력됩니다.
- 각 인터페이스의 IP 주소, Netmask, MAC 주소 및 활성화 상태를 확인할 수 있습니다.

---

## Default Gateway 확인

- Default Gateway를 확인하기 위해 다음 명령어를 사용합니다.

```text
route -n
```

![02](/KH_Security/Linux/Network%20Setting%20-%203/img/02.png)

### 첫 번째 기본 라우트

```text
0.0.0.0 → 192.168.10.254 via ens160 (Metric 100)
```

- 의미  
  목적지를 알 수 없는 모든 트래픽은 192.168.11.254 게이트웨이로 ens160 인터페이스를 통해 전송됩니다.  
  Metric 값이 103으로 가장 낮아 우선순위가 가장 높습니다.

---

## 직접 연결된 네트워크 (Connected Route)

### 192.168.11.0/24

```text
192.168.11.0 → 직접 연결 (ens160)
```

- 의미 : 동일 네트워크 대역이므로 게이트웨이를 거치지 않고 ARP를 통해 직접 통신합니다.

```text
192.168.10.20 → ARP → ens160 → 직접 전송
```

### 192.168.11.0/24

```text
192.168.11.0 → 직접 연결 (ens224)
```
- 의미 : ens224 인터페이스에 연결된 두 번째 내부 네트워크입니다.

---

## 인터페이스 활성화 및 비활성화

- 인터페이스는 up/down 상태로 제어할 수 있습니다.

- ens224 인터페이스를 down 시킵니다.

```text
ifconfig ens224 down
ifconfig
```

![03](/KH_Security/Linux/Network%20Setting%20-%203/img/03.png)

- 인터페이스를 비활성화한 후 ifconfig 실행 시 ens224가 출력되지 않는 것을 확인할 수 있습니다.

- 다시 인터페이스를 up 시킵니다.

```text
ifconfig ens224 up
ifconfig
```

![04](/KH_Security/Linux/Network%20Setting%20-%203/img/04.png)

- ens224 인터페이스가 다시 출력되는 것을 확인할 수 있습니다.

---

## IP 주소 변경

- ifconfig 명령어를 이용한 IP 변경은 즉시 적용되며 임시 설정됩니다. 설정 후 ifconfig 명령어를 쳐서 확인해줍니다.

```text
ifconfig ens224 192.168.11.112 netmask 255.255.255.0
ifconfig
```

![05](/KH_Security/Linux/Network%20Setting%20-%203/img/05.png)

- ifconfig 실행 결과 ens224의 IP 주소가 192.168.11.112로 변경된 것을 확인할 수 있습니다.

---

## Gateway 변경 (삭제 / 추가)

- Gateway 설정 변경은 임시 설정입니다.

```text
route -n
```

![06](/KH_Security/Linux/Network%20Setting%20-%203/img/06.png)

- 현재 시스템은 모든 외부 트래픽을 ens160 인터페이스를 통해 192.168.11.254 게이트웨이로 전송합니다.
- 192.168.11.0/24 대역은 동일 네트워크이므로 직접 통신합니다.

### Default Gateway 삭제

```text
route del default gateway 192.168.11.254
```

![07](/KH_Security/Linux/Network%20Setting%20-%203/img/07.png)

- Default Gateway가 삭제된 것을 확인할 수 있습니다.

### Default Gateway 추가

```text
route add default gateway 192.168.11.1
```

![08](/KH_Security/Linux/Network%20Setting%20-%203/img/08.png)

- 변경된 Default Gateway가 적용된 것을 확인할 수 있습니다.

---

## ARP 테이블 확인

- ARP 테이블 확인 명령어는 다음과 같습니다.

```text
arp -a (전체 ARP 테이블 확인)
```

![09](/KH_Security/Linux/Network%20Setting%20-%203/img/09.png)

- IP 주소와 해당 IP에 매핑된 MAC 주소, 인터페이스 정보가 출력됩니다.

---

## ARP 엔트리 수동 추가

- ARP 정보를 수동으로 등록하면 ARP 스푸핑 공격에 대한 하나의 대응 방법이 될 수 있습니다.

```text
arp -s 192.168.11.1 00:50:56:e2:c6:1a
arp -a
```

![10](/KH_Security/Linux/Network%20Setting%20-%203/img/10.png)

- arp -a 실행 시 수동으로 추가한 ARP 엔트리가 등록된 것을 확인할 수 있습니다.

---

## ARP 엔트리 삭제

```text
arp -d 192.168.11.1
arr -a
```

![11](/KH_Security/Linux/Network%20Setting%20-%203/img/11.png)

- 해당 ARP 엔트리가 삭제된 것을 확인할 수 있습니다.

---

## netstat 명령어

- netstat은 시스템의 연결 상태, 포트, 인터페이스 및 통계 정보를 확인하는 명령어입니다.

#### 옵션

```text
-i : 인터페이스 상태 확인  
-s : 프로토콜별 통계 정보  
-nr : 라우팅 테이블 확인  
```

```text
MTU : Maximum Transmission Unit  
RX : 수신 패킷 수  
TX : 송신 패킷 수  
DRP : 드롭된 패킷 수  
OVR : 오버플로우 패킷 수
ERR : 에러 수
```
![12](/KH_Security/Linux/Network%20Setting%20-%203/img/12.png)

- ens160, ens224, lo 인터페이스 모두 오류 없이 정상 동작 중입니다.

---

## netstat -nr를 이용한 라우팅 테이블 정보 확인

![13](/KH_Security/Linux/Network%20Setting%20-%203/img/13.png)

```text
Destination : 대상 주소
  - 0.0.0.0 : 모든 호스트
Gateway : 대상과 연결해주는 관문 역할을 담당하는 장치의 IP
Genmask : 대상의 netmask
  - 255.255.255.255 : 호스트
  - 0.0.0.0 : default route
Flage
  U(라우트), G(게이트웨이), R(동적 라우트 재생성), D(리다이렉트에 의해 재생성)
  M(리다이렉트나 라우트 데몬에의해 변경), !(거부)
• MSS : Maximun Segment Size
• irtt : 패킷 윈도우 사이즈
```

- route -n과 동일한 라우팅 테이블 정보가 출력됩니다.
- 최신 Linux 환경에서는 ip route 명령어 사용이 권장됩니다.

---

## nslookup 및 host 명령어

- 도메인 이름과 IP 주소 간의 변환을 확인합니다.
- IP를 이용한 도메인 확인은 Reverse Zone 조회로 큰 의미는 없습니다.
```text
nslookup www.daum.net
```
![14](/KH_Security/Linux/Network%20Setting%20-%203/img/14.png)

```text
host www.daum.net
```
![15](/KH_Security/Linux/Network%20Setting%20-%203/img/15.png)

---

## 라우팅 정보 추가

- route 명령어로 라우팅 정보를 추가합니다.
- 해당 설정은 임시 설정입니다.

```text
route add -net 10.0.0.0/24 gw 192.168.11.1 metric 100 dev ens160
```

![16](/KH_Security/Linux/Network%20Setting%20-%203/img/16.png)

- `route -n` 실행 결과 입력한 라우팅 정보가 정상적으로 등록된 것을 확인할 수 있습니다.

---
