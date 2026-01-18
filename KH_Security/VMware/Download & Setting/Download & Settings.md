# VMware 다운로드 및 Setting

### 개요

```text
1. VMnet1, VMnet8 Network Setting
2. 윈도우 환경에서 Network Setting
3. VMware 가상머신 설치 후 세부 세팅
```

---

## VMnet1 Setting

```text
설정하고 난 뒤 Apply 버튼을 반드시 눌러주어야 합니다.
```

![01](/KH_Security/VMware/img/01.png)

---

## VMnet8 Setting

```text
설정하고 난 뒤 Apply 버튼을 반드시 눌러주어야 합니다.
```

![02](/KH_Security/VMware/img/02.png)

- `VMnet8 NAT Settings`

![03](/KH_Security/VMware/img/03.png)

- `VMnet8 DHCP Settings`

![04](/KH_Security/VMware/img/04.png)

---

## Window 환경에서 Network Setting

### 이더넷 Network Setting

![05](/KH_Security/VMware/img/05.png)

### VMnet8 Network Setting

![06](/KH_Security/VMware/img/06.png)

### VMnet1 Network Setting

- `VMnet1은 사용안함으로 설정해줘도 됩니다.`

![07](/KH_Security/VMware/img/07.png)

---

## VMware Workstation 설정

- Typical(recommended) 선택 후 Next 해줍니다.
![08](/KH_Security/VMware/img/08.png)

- Linux는 Rocky Linux환경을 선택해줍니다.
![09](/KH_Security/VMware/img/09.png)


- Disk 용량을 40GB로 설정 후 Next 합니다.
![10](/KH_Security/VMware/img/10.png)

- 다음과 같은 화면에서 Edit virtual machine settings으로 들어갑니다.
![11](/KH_Security/VMware/img/11.png)

- USB Controller과 Sound Card를 remove 합니다.
- Memory를 2GB -> 4GB로 바꿉니다.
- CD/DVD에서 Rocky Linux 이미지파일을 넣어줍니다.
![12](/KH_Security/VMware/img/12.png)

---

## VMware Workstation - 가상머신 세부 설정

#### 설정 순서
```text
1. 시간과 날짜 (아시아/서울)로 변경
2. 소프트웨어 선택
3. 설치 목적지 (디스크 설정)
4. kdump 비활성화
5. 네트워크와 호스트 이름
6. 보안 정책
7. root 계정 및 사용자 계정 설정
8. 라이센스 계약 동의
```

`설치 시 세부 설정을 위한 화면이 나옵니다.`

![13](/KH_Security/VMware/img/13.png)

---

### 소프트웨어 선택

- `서버 - GUI 사용`을 선택 후 완료 버튼을 누릅니다.

![14](/KH_Security/VMware/img/14.png)

---

### 설치 목적지 (디스크 설정)

- 사용자 정의 선택 후 완료 버튼을 누릅니다.

![15](/KH_Security/VMware/img/15.png)

```text
/boot : 1G
swap : 4G
/ : 나머지
```
- 파티션을 설정합니다.

![16](/KH_Security/VMware/img/16.png)

---

### kdump 비활성화

- 충돌 시 알려주는 알림으로 굳이 없어도 되는 설정이기에 비활성화 해줍니다.
![17](/KH_Security/VMware/img/17.png)

---

### 네트워크와 호스트 이름

`설정에서 ipv4를 설정해주는데 수동으로 바꿔줍니다.`
```text
ipv4 : 192.168.10.###
netmask : 255.255.255.0
gateway : 192.168.10.1
```
![18](/KH_Security/VMware/img/18.png)

- 이후 보안정책을 동의하지 않고 root계정과 사용자계정을 설정한 뒤  
설치 시작을 눌러줍니다.

### 라이센스 계약 동의

![19](/KH_Security/VMware/img/19.png)
![20](/KH_Security/VMware/img/20.png)

- 그럼 세부 설정을 끝이 납니다.

---
