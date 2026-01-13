# VI Editor

---

## VI Editor 실행
- Vi Editor는 vi [파일명] 명령어를 통해 실행합니다.

![01](/KH_Security/Linux/Vi%20Editor/img/01.png)
- 파일이 존재하지 않는 경우, vi는 새 파일을 생성

![02](/KH_Security/Linux/Vi%20Editor/img/02.png)
- 하단에 `"a.txt" [New File]` 이 표시되며, 이는 새 파일이 생성되었음을 의미함

---

## Insert 모드
- 처음 상태는 명령모드이다.
- 명령모드에서 i를 입력시 Insert Mode로 바뀐다.
- i : 커서 위치부터 입력 가능
- o : 커서 아래 새로운 라인을 삽입하고 입력 가능

![03](/KH_Security/Linux/Vi%20Editor/img/03.png)

- Insert모드 전환 시 문자열을 입력할 수 있다.

---

## 파일 저장 및 종료

- Insert 모드에서 **명령 모드로 돌아가기 위해 `Esc` 키**를 누릅니다.
- 명령 모드에서 사용할 수 있는 기본 명령은 다음과 같습니다.

  - `:wq` → 저장 후 종료
  - `:q!` → 저장하지 않고 강제 종료

- 저장하기 위해 `:wq` 입력해 주었습니다.

![04](/KH_Security/Linux/Vi%20Editor/img/04.png)

---

## 파일 저장되어 있는 내용 출력

![05](/KH_Security/Linux/Vi%20Editor/img/05.png)

- 저장하지 않고 종료하려면 `:q!`를 입력해 주면 됩니다.

---

## 줄 번호 표시

- 명령 모드에서 `:set nu` 명령어를 입력해 주면 좌측에 번호를 표시합니다.

![06](/KH_Security/Linux/Vi%20Editor/img/06.png)
![07](/KH_Security/Linux/Vi%20Editor/img/07.png)

---

## 문자열 치환

- vi editor에서는 특정 문자열을 다른 문자열로 치환할 수 있습니다.
- 명령 모드에서 다음 형식의 명령어를 사용합니다.


`:%s/원래문자열/바꿀문자열/옵션`


- 옵션 :
  - `g` : 문서 전체에서 전역 치환
  - `i` : 대소문자 구분 없이 치환
  - `c` : 치환 여부를 하나씩 확인
 
  ![08](/KH_Security/Linux/Vi%20Editor/img/08.png)

- 문자 s를 t로 전역 치환하기 위해 :%s/s/t/g 명령어를 입력했습니다.
 
  ![09](/KH_Security/Linux/Vi%20Editor/img/09.png)

  ---

  ## 문자열 검색

- vi editor에서는 특정 문자열을 검색할 수 있습니다.
   
- 명령 모드에서 다음 형식의 명령어를 사용합니다.

`/문자열`

- 옵션 :
  - `n` : 순방향으로 재 검색, /와 동일
  - `N` : 역방향(위로)으로 검색
 
![10](/KH_Security/Linux/Vi%20Editor/img/10.png)

- 문자열 sa를 검색하기위해 /sa 명령어를 입력했습니다.

![11](/KH_Security/Linux/Vi%20Editor/img/11.png)

---
