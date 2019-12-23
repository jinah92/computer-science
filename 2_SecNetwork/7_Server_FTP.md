# (Day 7) FTP 서버 설치와 운영

## vsftpd

- 우분투에서 기본적으로 제공되는 ftp 서비스
- 리눅스, 유닉스 환경에서 보안성 및 성능이 우수하다고 평가받음
- 설치 및 운영이 쉬워 리눅스 환경의 FTP 서버 운영시 많이 이용

**실무에서 FTP 서버를 운영할 경우, 익명의 사용자(anonymous)는 다운로드만 허용함!**

## 실습 - vsftpd를 설치 및 운영

---

### Server

1. vsftpd 패키지 설치 `apt-get -y install vsftpd`
2. 일반적으로 FTP의 용도는 anonymous(익명) 사용자의 접속을 허용
    - `/etc/vsftpd.conf` 파일에 anonymous 사용자의 허용여부 설정
    - anonymous 사용자가 접속 및 파일 업로드를 허용하도록 설정
    anonymous_enable=NO
    #write_enable=YES (주석# 제거) 
    #anon_upload_enable=YES (주석# 제거)
    #anon_mkdir_write_enable=YES (주석# 제거)

3. `/srv/ftp` 아래에 pub 디렉토리를 만들고 모든 사용자에게 R/W 권한을 부여 
 (`/srv/ftp` 는 vsftpd에 익명으로 접속되는 디렉토리)

    ![Day%207%20FTP/Untitled.png](images/Day%207%20FTP/Untitled.png)