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
