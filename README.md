# Linux
## Linux 기초

### 리눅스 설치해보기
```
1. 윈도우 안에 설치
1.1 가상머신을 이용해서 설치(virtual Box)
 : 현재 컴퓨터의 cpu, ram, hdd, network 자원을 활용
 : id : mbc320 / pw : Mbc320!!
 : IP주소 확인 : 10.0.2.15
 : 리눅스 터미널 -> ping 8.8.8.8 -> 인터넷확인완료
 : 최신화 업데이트 sudo apt update
 : 터미널에서 ip주소 확인 ifconfig 
   -> 없으면 sudo apt install net-tools 설치
 : mysql(mariadb)을 설치 해보자.
   -> sudo apt install mariadb-server 
   -> sudo systemctl status mariadb (서비스 상태보기)
   -> sudo systemctl disable mariadb (서비스 중지)
   -> sudo systemctl enable mariadb (서비스 시작)
   -> sudo systemctl restart mariadb (서비스 시작)
   -> sudo mysql -u root (마리아db 콘솔로 접속) -> 성공
   -> 문제점 : root@localhost 암호가 없다. -> 암호생성
   -> create user 'root'@'%' identified by 'admin';
   ->                  id                    pw (계정/암호생성)
   -> grant all privileges on *.* to 'root'@'%' 
      identified by 'admin'; (권한부여)
```
---

   ### 일반계정 생성 및 db 생성연결
   ```
   -> CREATE USER 'flask'@'%' IDENTIFIED BY 'pythonDB';
   ->               일반계정id                   pw
   -> CREATE DATABASE flaskDB;
   -> grant all privileges on flaskDB.* to 'flask'@'%';

   -> FLUSH PRIVILEGES; (즉시적용)
   -> exit
   ### 우분투 마리아db 네트워크 허용 설정
   -> /etc/mysql/mariadb.conf.d/50-server.cnf
   -> 40행 부분에 bind-address = 127.0.0.1을 bind-address = 0.0.0.0으로 변경

 : windows에서 워크벤치 접속 시도 -> 실패!!! -> 리눅스 방화벽
   -> sudo ufw status verbose -> 방화벽 설치 확인(상태확인) -> 비활성
   -> sudo ufw enable -> 방화벽켜기 -> 시스템시작시 사용됨 -> reboot
   -> sudo ufw status -> 상태 : 활성
   -> sudo ufw allow 3306/tcp -> mariadb포트 열기 (규칙추가)
   -> sudo ufw reload -> 방화벽 규칙 새로 고침
   -> sudo ufw status verbose -> 상태확인 3306열린 리스트 확인

   
 : windows에서 CMD -> ping 10.0.2.15
```

---
### -------------- 기본 설치 완료 -----------------
```
설치 완료 후에는 vm 스냅샷을 찍어 놓으면 좋다.
: 리눅스를 이용해서 각종 설정을 진행 하다 초기화 하고 싶을때
: 가상머신이라는 동작에서 사용됨 (wsl은 안됨)
: 버추얼박스나 vmware에서 활용도 높다.
: aws, gcp 등 클라우드 플렛폼에서는 비용이 추가 됨
```

### --------------- 기본 명령어 사용 ---------------
```
터미널 : 대부분 리눅스는 명령어로 기능을 사용함
ls : list로 현재 위치에 내용 보기
ls -l : list 자세히 보기
  퍼미션 링크수 소유자 소유그룹 사이즈 날짜 이름
  퍼미션 내용 확인 하기
  d : 디렉토리(폴더) 방
  - : 파일

  rwx     rwx  rwx 설정 (-안됨, 영문은 됨)
  소유자  그룹  그외
  r  : 읽기권한
  w : 쓰기권한
  x  : 실행권한

  rwx------ : 소유자는 모든권한 나머지는 제외
  r--r--r-- : 소유자,그룹,그외사용자는 읽기만
  rw-rw-rw- : 모든사용자가 읽기와 쓰기가 가능

권한 부여방법 : chmod 777 파일명
연습용 파일 생성 : touch test.txt -> ls -l
-rw-rw-r-- test.txt
연습용 폴더 생성 : mkdir test     -> ls -l
drwxrwxr-x test

umask 값을 이용해서 폴더나 파일을 생성한다.
         - 7777 
umask 엔터 0002 (8진법)
           7775
            775 -> 8진법을 rwx로 변환
            7
           rwx
             7
            rwx
              5
             r-x

조원들과 본인만 모든 권한으로 변경
chmod 770 test.txt  -> ls -l -> -rwxrwx---

나만 모든 권한으로 변경
chmod 700 test.txt  -> ls -l -> -rwx------

퍼미션변경시 알파벳으로 변경
chmod o+r test.txt  -> ls -l -> -rwx---r--
chmod g+r test.txt  -> ls -l -> -rwxr--r--
chmod u-x text.txt  -> ls -l -> -rw-r--r--
chmod a+r text.txt  -> ls -l -> -rwxr--r--

-----------------------------------------
웹개발시 표준 퍼미션 755로 많이 함

-----------------------------------------
pwd : 프린팅워킹디렉토리
cd test  -> pwd -> /home/id/test
cd ..    -> pwd -> /home/id
cd test  -> cd ~ -> pwd -> /home/id
cd /     -> pwd  (최상위 디렉토리) -> ls
cd home  -> pwd  -> ls 계정확인 -> cd 계정명

윈도우에서는 c:/users/계정명
리눅스에서는 /home/계정명

du // 디렉토리별 사용량 확인용
df // 디스크별 사용량 확인용
tree  // sudo apt install tree 설치
   (현재위치에 파일과 디렉토리 구조 보기)

-------------------------------------------
사용자 및 그룹관리용 명령어

sudo useradd -m mbc1  -> 사용자 계정만들기
sudo adduser mbc2 ->  홈디렉토리 자동생성
sudo passwd mbc1   -> 사용자의 암호 변경

주의사항 : 계정을 만들면 개인그룹이 만들어짐
su mbc1   -> 다른계정으로 로그인 
비밀번호 1234

sudo groupadd lms  -> 그룹생성

sudo gpasswd -a mbc1 lms 
  -> mbc1계정을 lms그룹에 추가
sudo gpasswd -a mbc2 lms 
sudo gpasswd -a mbc3 lms 
sudo gpasswd -a 이름 lms 

touch board.txt -> ls -l
sudo chgrp lms board.txt -> ls -l (그룹변경)
sudo chown mbc1 board.txt -> ls -l (소유자변경)
-rw-rw-r-- mbc1 lms 0 날짜 board.txt
```
### -------------------기본명령어 종료--------------

# 다시 정리
📁 1. 경로 & 기본 명령어
```
pwd        # 현재 위치
ls         # 목록 보기
ls -al     # 숨김파일 포함 상세
cd 폴더    # 이동
cd ..      # 상위
cd ~       # 홈

👉 / = 루트
👉 ~ = 홈 (/home/사용자)
```
---
📂 2. 파일 & 폴더 관리
```
touch a.txt        # 파일 생성
mkdir dir          # 폴더 생성
rm a.txt           # 파일 삭제
rm -rf dir         # 폴더 삭제 (강제 ⚠️)
cp a b             # 복사
mv a b             # 이동 / 이름변경
🔐 3. 권한 & 사용자
sudo 명령어        # 관리자 권한 실행
chmod 755 파일     # 권한 변경

👉 권한:

r (읽기) w (쓰기) x (실행)

👉 숫자:

7 = rwx
5 = r-x
4 = r--
📦 4. 패키지 관리 (apt)
sudo apt update        # 목록 갱신
sudo apt upgrade -y    # 전체 업데이트
sudo apt install git   # 설치
apt search 이름        # 검색
🔍 5. 검색 & 파일 확인
grep -R "키워드" .   # 전체 검색
cat 파일             # 내용 출력
less 파일            # 페이지 보기
▶️ 6. 실행 & 제어
python run.py     # 실행
Ctrl + C          # 강제 종료 (중요🔥)
🐧 7. WSL 핵심
/home/...         # 리눅스 영역 (추천)
 /mnt/c/...       # 윈도우 접근

👉 윈도우에서:
\\wsl$\Ubuntu

⚙️ 8. 환경 개념
venv = 가상환경 (프로젝트 독립)
.env = 환경변수 저장
PATH = 실행 경로
⌨️ 9. 필수 단축키
Tab → 자동완성
↑ → 이전 명령
Ctrl + L → 화면 정리
Ctrl + C → 종료
```




