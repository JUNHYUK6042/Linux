# Service Managemend & Booting

이 문서는 리눅스 시스템이 프로그램을 어떻게 실행하고, 문제가 생겼을 때 어떻게 고치는지 핵심만 정리한 가이드입니다.

---

## 서비스(Service)와 데몬(Daemon)
**서비스**란 리눅스 시스템이 켜져 있는 동안 뒤에서 묵묵히 일하는 프로그램들을 말합니다.

### 실행 방식 차이  

**항상 대기형 (Stand Alone)**: 항상 메모리에 상주하며 손님이 오면 즉시 응대합니다. 빠르지만 메모리를 계속 사용합니다.  
**부르면 나타나는 형 (Super Daemon)**: 평소엔 쉬다가 필요할 때만 불려 나옵니다. 메모리는 아끼지만 반응은 조금 느릴 수 있습니다.

---

## systemctl 명령어

### 서비스 조회

| 명령어 | 설명 |
|----|----|
| `systemctl list-unit-files` | 모든 서비스의 상태 확인 | 

#### STATE 종류

| 상태 | 설명 |
|----|----|
| enabled | 활성화된 상태 (부팅 시 자동 실행) |
| disabled | 비활성화된 상태 |
| static | enable 불가, 종속 유닛 |
| generated | enable 가능하지만 종속 서비스로만 운영 |

---

### 서비스 상태 확인

| 명령어 | 설명 |
|----|----|
| `systemctl is-enabled [서비스]` | 자동 실행 등록 여부 |
| `systemctl is-active [서비스]` | 현재 실행 여부 |

---

### 서비스 등록

| 명령어 | 설명 |
| --- | ---- |
| `systemctl enable [서비스]` | 서비스 자동 실행 등록 |
| `systemctl disable [서비스]` | 서비스 자동 실행 해제 |
| `--now` 옵션 | start / stop을 동시에 수행 |

---

### 서비스 실행

| 명령어 | 설명 |
| --- | ---- |
| `systemctl start [서비스]` | 서비스 실행 |
| `systemctl stop [서비스]` | 서비스 종료 |
| `systemctl restart [서비스]` | 서비스 재실행 |

---

### systemctl 추가 옵션

| 옵션 | 설명 |
| --- | --- |
| try-restart | |실행 중일 때만 재시작 |
| reload | 설정 재구동 |
| status | 상태 확인 |
| disable --now | disable + stop 동시 수행 |

---

## 초기 서비스 목록

## 반드시 필요한 서비스 목록 (초기 서비스)

| 서비스명 | 설명 |
|--------|------|
| NetworkManager*.service | 네트워크 매니저 |
| chronyd.service | 시간 관리 데몬 |
| crond.service | 스케줄 관리 |
| dbus-broker.service | 메시지 버스 |
| display-manager.service | X Window 필수 서비스 |
| gdm.service | X Window 필수 서비스 |
| sshd.service | 원격 접속 서비스 (sshd) |
| rsyslog.service | 로그 관리 데몬 |
| vmtoolsd.service | VMware Tools Service |

---

## Systemctl 명령어 실습

### 서비스 조회

- 모든 서비스 상태를 확인한다.

```
systemctl list-unit-files
```

![01]()

- STATE 항목을 통해 각 서비스가 `static`, `generated`, `enabled`, `disabled` 중 어떤 상태로 설정되어 있는지 확인할 수 있습니다.


```
 systemctl[is-enabled | is-active] [서비스]
```

![02]()

- `is-enabled`는 서비스의 자동 실행 등록 여부를 확인하고,  
  `is-active`는 현재 실행 중인지 여부를 확인합니다.

---

### 서비스 등록

```
systemctl [enable | disable] [Service]
```

![03]()

- `enable` / `disable` : 서비스 자동 실행 등록 및 해제  
- `--now` 옵션 : 자동 실행 설정과 동시에 start 또는 stop 수행

---

### 서비스 실행

```
systemctl [start | restart | stop ] [Service]
```

![04]()

- `start`, `restart`, `stop` 명령어를 사용해 서비스를 실행, 재시작, 종료할 수 있습니다.

---

## ntsysv

![05]()

- `ntsysv`는 텍스트 기반 메뉴를 통해 부팅 시 실행되는 서비스를 설정하는 도구로,  
현재는 systemctl 사용이 권장된다.
- 키보드 방향키로 움직이며, Tab 키를 누를 시 `확인 및 취소` 버튼으로 이동합니다. 

---

## Target 개념

Target은 systemd에서 사용하는 부팅 단위로,  
기존 init의 Runlevel과 동일한 개념이다.  
Runlevel보다 더 다양한 부팅 모드를 제공한다.

---

### 주요 Target 예시

| Target | 설명 | 대응 Runlevel |
|------|------|-------------|
| multi-user.target | 콘솔 다중 사용자 모드 | init 3 |
| graphical.target | GUI 모드 | init 5 |
| rescue.target | 단일 사용자 복구 모드 | init 1 |
| emergency.target | 응급 복구 모드 | - |

---

### 제공되는 Target 확인

```
systemctl list-units --type target --all
```

![06]()  
![07]()

---



### Default Target 설정 및 확인

- Default Target 확인

```
systemctl get-default
```

![08]()

- Default Target 설정 후 확인

```
systemctl set-default
```

![09]()

- 변경 후에는 시스템을 리부팅합니다.

---

## 시스템 손상 시 Target 변경 (응급 복구 모드)

- 시스템 손상 또는 부팅 장애 발생 시 `systemctl`을 이용하여 복구 타겟으로 전환할 수 있다.

### 복구 타겟 종류

| 구분 | Target | 설명 |
|----|----|----|
| Rescue Mode | rescue.target | 기본 서비스 일부 로드, 시스템 복구용 |
| Emergency Mode | emergency.target | 최소 환경, 응급 복구 전용 |

### 사용 명령어

```
systemctl rescue
systemctl emergency
```

---

## Single Mode 부팅 순서

**Step 1** GRUB 편집 진입

```
시스템 이상시에 시스템 복구를 위해 사용합니다.
```

**Step 2** 수정 후 ^x로 재부팅

```
ro-> rw
init=/bin/bash 마지막에 추가
```

**Step 3** Single mode 부팅 후 패스워드 변경

```
재마운트 : mount -o remount,rw /
패스워드 변경 : passwd
```

> 주의 : SELINUX가 enforcing인 경우 먼저 disabled로 바꾸고 다시 진행한다.

---
