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

## Interface Metric 조정

### 확인

```
show interface s0/0/0
```

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

---

## EIGRP 설정 예시

```
router eigrp 100
 no auto-summary
 passive-interface g0/0
 network 1.1.1.1 0.0.0.0
 network 12.1.1.1 0.0.0.0
 network 31.1.1.1 0.0.0.0
```

---

## Default Routing (EIGRP)

### Static Default Route 생성

```
ip route 0.0.0.0 0.0.0.0 loopback0
```

### EIGRP로 재분배

```
router eigrp 100
 redistribute static
```

➡️ 외부 경로로 표시됨 (D*EX)

---
