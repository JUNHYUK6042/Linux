# FTP & vsftpd 서버 구성 정리

## 1. FTP 서비스 개요

### FTP(File Transfer Protocol) 특징
- 대용량 파일 전송에 적합한 서비스
- 최근에는 웹 서비스의 일부로 통합 운영되는 추세
- `xinetd`보다 **standalone 방식**으로 운영
- 시스템 리소스를 비교적 많이 사용하는 서비스
- **Out of Band 방식** 사용

### 포트 구성
- **21/TCP** : Control Connection
- **20/TCP** : Data Connection (Active Mode)
- **Passive Mode** : 1024번 이후 임의 포트 사용

---
