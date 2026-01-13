# NIC 추가 및 설정
- 가상머신에 랜카드 추가 후 XWindow와 nmtui 명령어를 통해 IP 주소를 설정하는 과정

---

## NIC 설정 서비스 확인
- 설정 전에 NetworkManager를 통해 서비스를 확인합니다.
- `systemctl is-active NetworkManager.service`
- `systemctl is-enabled NetworkManager.service`의 명령어를 통해 서비스를 확인합니다.
-  현재 활동 - 프로그램 표시 - 설정 - 네트워크 메뉴에서 설정
-  변경한 내용은 즉시 적용됩니다.

---

## 설정 적용 방식

### 시스템 재시작에 의한 적용

- 설정 파일은 시스템 재시작 시 새로 적용됩니다.

### 명령어에 의한 적용

- `systemctl restart NetworkManager.service`
  - IP가 추가됨
  - 기존 IP를 보존하여 기존 접속 유지

- `nmcli conn up [NIC]`
- `nmcli dev disconnect [NIC] && nmcli dev connect [NIC]`
  - IP가 변경됨
  - 원격 접속 중일 경우 터미널 연결이 끊길 수 있음

---

## NIC 추가 (Xwindow에서의 네트워크 설정)

- VMware 화면에서 설정 버튼 누르고 Network Adapter(NAT방식)을 추가합니다.
- 설정 완료 후 VMware 화면에서 Network Adapter 2가 추가된 것을 확인할 수 있습니다.

![01]()
