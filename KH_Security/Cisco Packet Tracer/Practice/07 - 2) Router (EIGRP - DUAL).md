# EIGRP - DUAL 실습

## EIGRP Distance 개념

![01](02.png)

### Successor

* **최적 경로**
* 라우팅 테이블에 실제로 등록되는 경로
* Next-hop 라우터

### Feasible Distance (FD)

* 로컬 라우터 기준 목적지까지의 총 Metric

### Reported Distance (RD)

* Neighbor 라우터가 광고한 목적지까지의 Metric

### Feasible Successor

* **백업 경로**
* 조건:

```
RD < 현재 Successor의 FD
```

* 라우팅 루프 방지를 위한 핵심 조건

---

## DUAL 동작 과정

1. Neighbor로부터 경로 정보 수신
2. Successor / Feasible Successor 선정
3. Successor 장애 발생 시 즉시 Feasible Successor 사용
4. Feasible Successor가 없으면 Query 발생

---

## Load Balancing (부하 분산)

### Equal-Cost Load Balancing

* Metric(FD)가 동일한 경로는 **모두 사용**
* 최대 경로 수: 4 (default)

### 8.2 Unequal-Cost Load Balancing

* **Variance** 명령어 사용
* Feasible Successor만 대상

조건:

```
FD(successor) * variance ≥ FD(feasible successor)
```

---

## EIGRP - DUAL 실습

### EIGRP 구성도

![02](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/10.png)

---
 
## R1 Setting

`no auto-summary` : 클래스풀 자동 요약 방지 → 정확한 서브넷 경로 전달

```
enable
conf t
hostname R1

int g0/0
ip add 1.1.1.1 255.255.255.0
no sh
int s0/0/0
ip add 12.1.1.1 255.255.255.0
no sh
int s0/0/1
ip add 41.1.1.1 255.255.255.0
no sh

router eigrp 100
no auto-summary
passive-interface g0/0
network 1.1.1.1 0.0.0.0
network 12.1.1.1 0.0.0.0
network 41.1.1.1 0.0.0.0

end
copy run start
```

---

## R2 Setting

```
enable
conf t
hostname R2
no ip domain-lookup

int s0/0/0
ip add 23.1.1.2 255.255.255.0
no sh
int s0/0/1
ip add 12.1.1.2 255.255.255.0
no sh

router eigrp 100
no auto-summary
network 12.1.1.2 0.0.0.0
network 23.1.1.2 0.0.0.0

end
copy run start
```

---

## R3 Setting

```
enable
conf t
hostname R3
no ip domain-lookup

int g0/0
ip add 3.3.3.1 255.255.255.0
no sh
int s0/0/0
ip add 34.1.1.3 255.255.255.0
no sh
int s0/0/1
ip add 23.1.1.3 255.255.255.0
no sh

router eigrp 100
no auto-summary
passive-interface g0/0
network 3.3.3.1 0.0.0.0
network 23.1.1.3 0.0.0.0
network 34.1.1.3 0.0.0.0

end
copy run start
```

---

## R4 Setting

```
enable
conf t
hostname R4
no ip domain-lookup

int s0/0/0
ip add 41.1.1.4 255.255.255.0
no sh
int s0/0/1
ip add 34.1.1.4 255.255.255.0
no sh

router eigrp 100
no auto-summary
network 34.1.1.4 0.0.0.0
network 41.1.1.4 0.0.0.0

end
copy run start
```

---

## EIGRP - Topology Table 확인

### R1 Router - Topology Table  
![03](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/11.png)

### R2 Router - Topology Table  
![04](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/12.png)

### R3 Router - Topology Table  
![05](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/13.png)

### R4 Router - Topology Table  
![06](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/14.png)

---

## Interface Metric 조정

### Bandwidth 변경

```
bandwidth [Kbps]
```

### Delay 변경

```
delay [단위: 1/10,000 초]
```

> CLI 입력은 1/10,000초 단위
> 출력은 usec(μs) 단위

#### R2-R3 라인 delay : 30000으로 변경

![07](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/15.png)  

- delay 변경 후 다음과 같이 3.3.3.0/24로의 successor가 1개로 바뀌었다.  
하지만 사용 경로는 2개 경로를 모두 사용한다. 즉 FD가 2682112보다 큰 12.1.1.2 경로의 라우터가 feasible successor가 된다.
  
![08](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/16.png)

---

## Default Routing (EIGRP) 실습 

### 구성도

![09](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/17.png)

### R0 Setting

```
enable
conf t
hostname R0
no ip domain-lookup

int g1/0
ip add 10.1.1.10 255.255.255.0
no sh

int g2/0
ip add 20.1.1.20 255.255.255.0
no sh

int loopback 0
ip add 10.10.10.1 255.255.255.0

> Static Default Route 생성
ip route 0.0.0.0 0.0.0.0 loopback0

router eigrp 100
no auto-summary
network 10.1.1.10 0.0.0.0
network 20.1.1.20 0.0.0.0
redistribute static 

end
copy run start
```

- redistribute static : default routing을 포함한 static routing 정보를 EIGRP 네트워크에 재분배 한다.

### R0 Routing 정보 확인

![10](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/18.png)

---

### R1 Setting

```
enable
conf t
hostname R1
no ip domain-lookup

int g0/0
ip add 1.1.1.1 255.255.255.0
no sh

int g1/0
ip add 10.1.1.1 255.255.255.0
no sh

router eigrp 100
no auto-summary
passive-interface g0/0
network 10.1.1.1 0.0.0.0
network 1.1.1.1 0.0.0.0

end
copy run start
```

---

### R2 Setting

```
enable
conf t
hostname R2
no ip domain-lookup

int g0/0
ip add 2.2.2.1 255.255.255.0
no sh

int g1/0
ip add 20.1.1.1 255.255.255.0
no sh

router eigrp 100
no auto-summary
passive-interface g0/0
network 20.1.1.1 0.0.0.0
network 2.2.2.1 0.0.0.0

end
copy run start
```

---

## PC 통신 테스트

- PC1에서 R0와 PC2(2.2.2.11)로 통신 테스트를 했습니다.

![11](/KH_Security/Cisco%20Packet%20Tracer/img/Router%20EIGRP/19.png)

---

## EIGRP - DUAL 실습 마무리 요약

- DUAL은 Successor / Feasible Successor를 사용하여 **Loop-free 라우팅**과 **빠른 수렴**을 제공한다.
- Successor는 최적 경로이며, **RD < FD** 조건을 만족하는 경로는 **Feasible Successor(백업 경로)** 가 된다.
- EIGRP는 기본적으로 **Bandwidth + Delay**를 Metric으로 사용하며, **Variance**를 통해 비대칭 부하 분산이 가능하다.
- `no auto-summary`는 클래스풀 자동 요약을 방지하여 **정확한 서브넷 경로 광고**를 보장한다.
- `redistribute static`을 통해 **Default Route를 EIGRP 네트워크로 전파**할 수 있다.
