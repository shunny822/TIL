# Ubuntu에 MySQL server 설치

## 목차


<br>

## MySQL server 설치

1. 패키지 업데이트
    ```
    $ sudo apt update
    ```

2. MySQL server 설치
    ```
    $ sudo apt install mysql-server
    ```

3. 설치 확인
    ```
    $ mysql --version
    ```

4. MySQL server start
    ```
    $ sudo systemctl start mysql
    ```

5. server active 확인
    ```
    $ sudo systemctl status mysql
    ```

6. 부팅 시 자동 시작 설정
    ```
    $ sudo systemctl enable mysql
    ```

7. ROOT 비밀번호 설정
    ```
    $ sudo mysql
    ```
    ```sql
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by '내가_쓸_비밀번호';
    ```
    비밀번호 설정 후 exit

8. 보안 설정 및 초기 구성
    ```
    $ sudo mysql_secure_installation
    ```
    - 현재 루트 암호 설정 : 이미 설정했으니 n
    - anonymous user 제거 : y
    - root login을 원격에서 하지 못하도록 설정 : y **(⭐root 사용자의 외부 접속은 허용하지 않는다!)**
    - 테스트 데이터베이스 제거 : y
    - 설정 완료 후 권한 테이블 새로고침 : y

9. root 사용자의 인증 방법을 기본값인 auth_socket으로 다시 변경
    ```
    $ mysql -u root -p
    ```
    `-p` : 암호 입력 프롬프트 옵션, 명령 실행 시 암호를 입력해야 함
    ```sql
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH quth_socket;
    ```
    이후 `sudo mysql -u root -p` 명령어로 mysql에 한번 접속하면 `sudo mysql` 로 접속 가능


<br>

## MySQL server 설정

### 일반 사용자 외부접속 허용

1. root 전환 & 위치 이동
    ```
    $ sudo -i
    ```
    ```
    # cd /etc/mysql
    ```

2. 'bind-address=127.0.0.1' 주석처리
    ```
    # grep -r 'bind'
    ```
    grep으로 'bind'문자열 찾기
    ```
    # cd /etc/mysql/mysql.conf.d
    ```
    해당 위치로 이동
    ```
    # sed -i 's/bind/# bind/' mysqld.cnf
    ```
    sed로 bind -> # bind 편집 = 주석 처리

3. 확인
    ```
    # cat mysqld.cnf | grep bind
    ```

4. vi로 'bind-address=0.0.0.0' 입력
    ```
    # vi /etc/mysql/mysql.conf.d/mysqld.cnf
    ```
    vi로 파일을 열어서 'bind-address=0.0.0.0' 입력하기, 위의 주석 처리도 vi로 한 번에 가능하다!!!!

5. mysql restart
    ```
    # sudo systemctl restart mysql
    ```
    이후 exit


### 방화벽 설정

1. MySQL port 열기
    ```
    $ sudo ufw allow mysql
    ```

2. 방화벽 활성화
    ```
    $ sudo ufw enable
    ```

3. 확인
    ```
    $ sudo ufw status
    ```

### VM 포트포워딩 추가

- 프로토콜:TCP

- 호스트IP: 127.0.0.1 / 호스트Port: 13306

- 게스트IP: 10.0.x.x / 게스트Port: 3306


### 일반 사용자 생성 및 권한 설정

1. DB 생성
    ```sql
    mysql> CREATE DATABASE mydb;
    ```

2. user 생성(아이디, 비밀번호 설정)
    ```sql
    mysql> CREATE USER 'myuserid'@'%' IDENTIFIED BY 'mypassword';
    ```
    '%' : 모든 ip 허용

3. 권한 부여
    ```sql
    mysql> GRANT ALL ON mydb.* TO 'myuserid'@'%';
    ```

4. 변경된 권한 적용
    ```sql
    mysql> FLUSH PRIVILEGES;
    ```

<br>

## 워크벤치

워크벤치를 통해서 외부에서 접속

- 호스트명/IP: 127.0.0.1

- 사용자: id

- 암호: password

- 포트: 13306




<br>

## 참고 자료

https://www.bddungsblog.com/2022/11/ubuntu-mysql-resolving-mysql-password.html

https://blog.uncletom.co.kr/25

https://wrallee.tistory.com/12

https://www.npmjs.com/package/mysql2