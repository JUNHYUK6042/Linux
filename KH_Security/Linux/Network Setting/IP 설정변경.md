# IP 설정 변경 (IP, Gateway 변경)

---

## IP 주소 변경

### IP 변경 명령어 개요

- IP 주소 변경 명령어
  - `ip addr change [IP/MASK] dev [NIC]`
- `change` 명령어는 내부적으로 `add`와 동일하게 동작합니다.
- 따라서 기존 IP는 직접 `del` 명령으로 삭제해야 합니다.

---

### 현재 IP 주소 확인

- 다음 명령어로 인터페이스의 현재 IP 주소를 확인합니다.
```text
ip a show enp224 | grep enp224
```
![10]()

- 현재 IP 주소는 `192.168.0.126/24` 입니다.

---

### 새로운 IP 주소 추가

- 다음 명령어로 새로운 IP 주소를 추가합니다.
```text
ip a change 192.168.11.137/24 dev enp0s1
```
- 다시 IP 주소를 확인합니다.
```text
ip a show enp0s1 | grep enp0s1
```
![11]()

- 기존 IP `192.168.0.126/24` 와  
  새로 추가한 IP `192.168.11.136/24` 두 개가 동시에 존재하는 것을 확인할 수 있습니다.

---

### 기존 IP 주소 삭제

- 기존 IP 주소를 삭제합니다.
```text
ip a del 192.168.0.126/24 dev enp224
```
- 다시 확인합니다.
```text
ip a show ens224 | grep ens224
```
![12]()

- 기존 IP는 제거되고 새 IP만 남은 것을 확인할 수 있습니다.

---

### 임시 설정의 한계

- 이 상태에서는 네트워크 통신이 정상적으로 가능합니다.
- 하지만 다음과 같은 경우 설정은 유지되지 않습니다.
  - 인터페이스 down / up
  - 시스템 재부팅

---

### NIC down / up 테스트
```text
ip link set ens224 down & ip link set ens224 up
```
![13]()

- 인터페이스를 다시 올리면  
  기존에 설정된 원래 IP로 되돌아간 것을 확인할 수 있습니다.

---

## Default Gateway 변경 (임시 설정)

### 현재 Default Route 삭제 및 재설정
```text
ip r del default  
ip r add default via 192.168.10.254
```
![14]()

- `ip r show default`로 확인 시 변경된 것을 확인할 수 있습니다.
- 하지만 이 역시 임시 설정입니다.

---

### NIC 재시작 후 상태 확인
```text
ip link set ens down & ip link set enp0s1 up
```
![15]()

- Default Gateway가 원래 값으로 복구된 것을 확인할 수 있습니다.

---

## DNS 설정 변경 (임시 설정)

### 현재 DNS 확인
```text
cat /etc/resolv.conf | grep nameserver
```
![16]()

- 현재 nameserver는 `8.8.8.8` 입니다.

---

### DNS 동작 확인
```text
nslookup www.naver.com | grep Server
```
- DNS 서버가 `8.8.8.8`로 사용되고 있음을 확인합니다.

![17]()

---

### DNS 서버 변경
```text
cat <<EOF > /etc/resolv.conf
nameserver 192.168.0.1
EOF
```
- EOF가 나오기 전까지 입력된 내용을 파일에 기록합니다.

---

### 변경 확인
```text
cat /etc/resolv.conf | grep nameserver
```
![18]()

- nameserver가 `192.168.0.1`로 변경된 것을 확인합니다.

---

### DNS 테스트
```text
nslookup www.naver.com | grep Server
```
- DNS 서버가 변경된 주소로 동작함을 확인합니다.

---

### 임시 설정 요약

- `ip addr`, `ip route`, `/etc/resolv.conf` 직접 수정은
  - 인터페이스 down / up
  - NetworkManager 재시작
  - 시스템 재부팅
  시 원래 상태로 복구됩니다.

---

## 네트워크 설정 영구 저장 방법

### 설정 파일 경로

- 다음 파일을 직접 수정하면 영구 설정이 됩니다.

- `/etc/sysconfig/network-scripts/ifcfg-NIC`
- `/etc/NetworkManager/system-connections/NIC.nmconnection`

- IP, Gateway, DNS 값을 파일에 직접 정의합니다.

---

### 설정 적용 방법
```text
nmcli connection reload && nmcli connection up ens224
```
- 수정된 설정을 즉시 적용합니다.
```text
systemctl restart NetworkManager.service
```
- 전체 네트워크를 재시작합니다.
- 기존 IP와 다를 경우 `ip a del` 명령으로 기존 IP를 직접 삭제해야 할 수 있습니다.

---

## Hostname 변경
```text
hostnamectl set-hostname [호스트명]
```
- `/etc/hostname` 파일을 수정하는 것과 동일합니다.
- 쉘 재시작 필요
```text
exec bash  
또는  
exec $SHELL
```
---

## 주의 사항

- `.nmconnection` 파일을 직접 수정하는 방식은
  - NetworkManager 캐시를 수정하는 것이 아닙니다.
- 따라서 다음 명령어로는 반영되지 않습니다.
```text
nmcli conn down/up  
nmcli dev disconnect/connect  
```
---
