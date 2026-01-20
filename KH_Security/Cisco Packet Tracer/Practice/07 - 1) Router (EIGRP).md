# EIGRP (Enhanced Interior Gateway Protocol) 정리

## EIGRP 개요

* Cisco에서 개발한 **Hybrid Routing Protocol** (Distance Vector + Link State 개념)
* **DUAL (Diffusing Update Algorithm)** 사용 → 빠른 수렴(Rapid Convergence)
* 멀티캐스트 주소 **224.0.0.10** 사용
* 변경 사항만 업데이트 (초기 Neighbor 형성 시 전체 교환)

### 패킷 종류

* Hello
* Update
* Query
* Reply
* ACK

> ACK 패킷이 없으면 해당 패킷을 수신하지 못했다고 판단하고 재전송함

---

## Neighbor

* **동일 AS 번호** + **동일 K-상수**를 사용하는 라우터 간에 형성
* 정상 상태에서는 Hello 패킷만 교환

---

## EIGRP Metric

### Metric Factor

EIGRP는 총 5가지 Metric 요소를 가짐

* Bandwidth
* Delay
* Reliability
* Load
* MTU

> **기본 설정에서는 Bandwidth, Delay만 사용**

### K-상수

공식 Metric 수식:

```
(K1*BW + (K2*BW)/(256-load) + K3*Delay) * 256 * (K5/(Reliability + K4))
```

**Default 값**

* K1 = 1
* K3 = 1
* K2 = K4 = K5 = 0

- 기본 Metric 계산법 :

```
EIGRP Metric = (Bandwidth + Delay) * 256
```

---

## Metirc IMG

![01]()

---

## Bandwidth Metric

* 목적지까지 경로 중 **가장 낮은 대역폭** 기준
* 단위: Kbps

### 계산식

```
BW = (10^7 / Bandwidth) * 256
```

### 예시

```
A-B-C-D
BW = (10^7 / 56000) * 256 -> 45,714.28571428571

A-X-Y-Z-D
BW = (10^7 / 256000) * 256 -> 10,000
```

➡️ 가장 성능이 낮은 링크가 전체 경로 Bandwidth를 결정

---

## Delay Metric

* 목적지까지 **모든 Delay의 합**
* 단위: usec (μs, 마이크로초 = 1/1,000,000초)

### 계산식

```
Delay = (Σ Delay / 10) * 256
```

### 예시
```
A-B-C-D
Delay = ( 30000 / 10 ) * 256 -> 768,000
A-X-Y-Z-D
Delay = ( 40000 / 10 ) * 256 -> 1,024,000
```
---

## Total Metric

```
A-B-C-D -> 45,714 + 768,000 = 813,714
A-X-Y-Z-D -> 10,000 + 1,024,000 = 1,034,000
```
---
