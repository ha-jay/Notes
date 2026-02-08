
# CLI 데이터 전송 도구


## curl - (Client URL) 이란?
> `curl`은 개발자들에게는 "터미널용 웹 브라우저"와 같은 필수 도구입니다.

curl은 터미널(CLI) 환경에서 **서버와 데이터를 주고받기 위해 사용하는 강력한 전송 도구**입니다.  
웹 브라우저가 그래픽 화면으로 웹사이트를 보여준다면, `curl`은 명령줄에서 HTTP, FTP, SMTP 등 수많은 프로토콜을 이용해 데이터를 요청하고 받아오는 역할을 합니다.

즉, GUI(그래픽 인터페이스) 없이 **터미널(CLI)**에서 URL 문법을 사용하여 서버와 데이터를 주고받는 프로그램입니다.
- **유사 도구:** `wget` (주로 다운로드 특화), `Postman` (GUI 기반 API 테스트 도구)

|**구분**|**웹 브라우저 (Chrome 등)**|**curl**|
|---|---|---|
|**인터페이스**|그래픽 (GUI)|명령줄 (CLI)|
|**주요 목적**|콘텐츠 렌더링 (보여주기)|데이터 전송 및 수집|
|**결과물**|이미지, 폰트가 적용된 화면|Raw 데이터 (HTML, JSON 텍스트)|
### curl의 역활
 ① 네트워크 유틸리티 (Network Utility)

네트워크 상태를 점검하거나 서버의 응답을 확인하는 용도입니다. 브라우저를 켜지 않고도 "이 사이트가 지금 응답을 하나?" 또는 "서버 설정이 어떻게 되어 있나?"를 확인할 수 있어 네트워크 관리 도구로 분류됩니다.

 ② API 테스트 및 개발 도구 (API Client)

현대 개발자들에게 가장 흔한 분류입니다. REST API 등을 호출하여 JSON 데이터를 보내거나 받을 때 사용합니다.

- 웹 브라우저는 주로 `GET` 요청(페이지 보기)만 수행하지만, `curl`은 `POST`, `PUT`, `DELETE` 등 모든 HTTP 메서드를 자유자재로 다룰 수 있습니다.
    

③ 자동화 스크립트 도구 (Automation Tool)

사람이 직접 입력하기보다 쉘 스크립트(`bash`, `zsh`) 안에서 특정 파일을 자동으로 다운로드하거나, 서비스 상태를 주기적으로 체크하여 보고하는 '자동화 로봇'의 부품으로 분류됩니다.



### 주요 특징

- **다양한 프로토콜 지원:** HTTP, HTTPS, FTP, SFTP, SCP, SMTP 등 거의 모든 통신 규약을 지원합니다.
- **교차 플랫폼:** 리눅스뿐만 아니라 macOS, Windows(최신 버전은 기본 포함) 등 대부분의 운영체제에서 동일하게 사용할 수 있습니다.
- **비대화형 도구:** 사용자의 개입 없이 스크립트 내에서 자동으로 데이터를 다운로드하거나 API를 호출하는 데 최적화되어 있습니다.

### 문법
`curl [OPTIONS] [URL]`

options

|**단축 옵션 (short)**|**긴 옵션 (long)**|**설명**|
|---|---|---|
|**-k**|`--insecure`|https URL 접속 시 SSL 인증서 검사 없이 연결|
|**-i**|`--head`|HTTP 응답 헤더를 표시|
|**-d**|`--data`|POST 요청이나 JSON 방식과 같이 request body에 데이터를 담을 때 사용|
|**-o**|`--output`|`-o [파일명]`을 사용하여 출력 결과를 파일로 저장|
|**-O**|`--remote-name`|파일 저장 시 서버의(remote) 파일 이름 그대로 저장|
|**-s**|`--silent`|진행 내역이나 메시지 등을 출력하지 않음|
|**-X**|`--request`|Request에 사용할 메서드(GET, POST, PUT 등)를 지정|
|**-v**|`--verbose`|동작하는 과정을 상세히 출력|
|**-A**|`--user-agent`|특정 브라우저인 것처럼 동작하기 위한 설정|
|**-H**|`--header`|요청할 헤더 설정|
|**-L**|`--location`|서버에서 리다이렉트(301, 302) 응답이 오면 해당 URL로 따라감|
|**-D**|`--dump-header <file>`|응답 헤더를 별도 파일에 기록|
|**-u**|`--user`|사용자 아이디 / 비밀번호 입력|
|**-f**|`--fail`|오류 발생 시 출력 없이 실패|
|**-T**|`--upload-file`|로컬 파일을 서버로 전송|
|**-C**|`--continue-at`|중지된 다운로드를 재시작|
|**-J**|`--remote-header-name`|응답 헤더에 있는 파일 이름으로 저장 (curl 7.20 이상)|
|**-I**|`--head`|응답 헤더만 출력|

### 자주 사용하는 명령어 예시
|**기능**|**명령어**|**설명**|
|---|---|---|
|**웹 페이지 소스 보기**|`curl https://www.google.com`|해당 URL의 HTML 소스를 터미널에 바로 출력합니다.|
|**파일로 저장하기**|`curl -o filename.html https://example.com`|결과를 지정한 파일명(`-o`)으로 저장합니다.|
|**원래 이름으로 저장**|`curl -O https://example.com/file.zip`|서버에 저장된 파일 이름 그대로 다운로드합니다.|
|**헤더 정보만 확인**|`curl -I https://example.com`|서버의 응답 헤더(상태 코드 등)만 보여줍니다.|
|**데이터 전송 (POST)**|`curl -d "name=user" https://api.com`|API 등에 데이터를 실어서 보낼 때 사용합니다.|
추가
- **API 테스트할 때 (JSON 데이터 전송):** `curl -X POST -H "Content-Type: application/json" -d '{"name":"goose"}' http://api.com` 
- **다운로드 이어받기:** `curl -L -C - -O http://example.com/bigfile.zip`

### 사용예시
`vim-plug` 설치 때 `curl` 사용
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/...
```
- **`-f` (fail):** 서버 오류 시 메시지를 노출하지 않고 조용히 실패합니다.
- **`-L` (location):** 주소가 변경(리다이렉트)되었다면 자동으로 새 주소를 따라갑니다.
- **`-o` (output):** 받아온 데이터를 특정 경로(`~/.vim/...`)에 파일로 저장합니다.
- **`--create-dirs`:** 파일을 저장할 디렉토리가 없다면 자동으로 생성합니다.


### API 사용예시
- 요청에 대한 응답 값 출력 (raw file 출력) : `curl https://sasca37.tistory.com` - U
- 응답내용 저장 
```
curl -o dummy.txt google.com 
curl -O google.com/dummy2.txt
```
- - POST 방식으로 JSON 데이터 요청 및 타임아웃 설정
```
curl --connect-timeout 15 \
 -i \
 -H 'Content-Type: application/json' \
 -d '{ "info" : { "country" : "korea", "name" : "sasca", "secretIdentity" : "nknown"}, "reqData" : { "id" : "sasca37","password" : "sasca37"}}' \
 -X POST http://localhost:8080/getToken
```
- - HTTP POST에서 파일을 전송할 경우 파일명 앞에 @를 붙여준다.
```
curl --data-binary -d @data.txt http://localhost:8080
```
- - 사용자 정보 입력 및 프록시가 필요한 경우 사용
```
curl -u [사용자]:[비밀번호] -x [프록시_이름]:[포트] [URL...]
```

- - Bearer token을 사용할 경우 -H 옵션 뒤에 토큰을 추가하여 전송
```
curl -v -L  -X POST -H 'Accept: application/json' -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9' 'http://localhost:8080'
```
- - 쿠키 정보를 cookie.t 파일에 저장
```
curl -I -c cookie.t http://localhost:8080
```
- - 쿠키 값을 지정하여 서버에 전송
```
curl -b "JSESSIONID=3CB361E0BE1A9A7DE7DB926DF0772BAE" http://localhost:8080
```
### 예시 - FTP
- FTP 방식으로 통신 
	- -# : 진행률 `#`으로 한줄 표시, 
	- --silent : 진행 과정 비활성화, 
	- [1-20] 숫자 시퀀스라면 여러 파일 전송, 
	- --limit-rate 전송 속도 제한)
```
curl -# -O ftp://example.com/download.zip
curl --silent ftp://example.com/download.zip
curl ftp://example.com/download[1-20].jpeg
curl --limit-rate 1000K -O ftp://example.com/download.zip
```

### 예시 SMTP
- SMTP 방식으로 메일 전송 방식
```
curl –url [SMTP URL] \
 –mail-from [sender_mail] \
 –mail-rcpt [receiver_mail] \
 -n \
 –ssl-reqd \
 -u {email}:{password} \
 -T [메일 텍스트 파일]
```



## wget
**'Web Get'**의 약자로, 웹 서버로부터 콘텐츠를 가져오는 **커맨드 라인용 파일 다운로드 도구**입니다. `curl`과 비슷해 보이지만, **'파일 다운로드와 웹 크롤링'**에 훨씬 더 특화되어 있습니다.

### 특징
- **백그라운드 실행:** 용량이 매우 큰 파일을 받을 때, 터미널을 로그아웃해도 백그라운드에서 다운로드를 계속 진행할 수 있습니다.
    
- **재시도 기능:** 네트워크 연결이 불안정하면 자동으로 다시 시도합니다.
    
- **재귀적 다운로드 (Recursive):** 웹사이트의 링크를 타고 들어가 페이지 전체나 특정 디렉토리의 모든 파일을 통째로 긁어올 수 있습니다.
    
- **간결함:** `curl`보다 사용법이 직관적이며, 복잡한 설정 없이 URL만 입력해도 파일로 바로 저장됩니다.

### 자주쓰는 명령어
|**기능**|**명령어**|**설명**|
|---|---|---|
|**단일 파일 다운로드**|`wget [URL]`|URL의 파일을 현재 폴더에 저장합니다.|
|**이름 바꿔서 저장**|`wget -O filename.zip [URL]`|다운로드하면서 이름을 변경합니다.|
|**이어받기 (Resume)**|`wget -c [URL]`|끊겼던 다운로드를 이어서 받습니다.|
|**백그라운드 다운로드**|`wget -b [URL]`|백그라운드에서 실행하며 로그는 `wget-log`에 남깁니다.|
|**웹사이트 통째로 복제**|`wget -m [URL]`|미러링 모드로 사이트 전체를 내 하드디스크에 복사합니다.|
### 예시
WSL 환경에서 대용량 데이터셋이나 설치 파일을 받을 때 `wget -c`를 사용하면 인터넷이 중간에 끊겨도 안심할 수 있습니다.
```
# 예시: 우분투 이미지 이어받기
wget -c https://releases.ubuntu.com/22.04/ubuntu-22.04.3-desktop-amd64.iso
```

## curl vs wget
|**구분**|**curl**|**wget**|
|---|---|---|
|**주 목적**|데이터 전송 및 API 테스트|**파일 다운로드 및 미러링**|
|**동작 방식**|결과를 기본적으로 화면(Stdout)에 출력|결과를 기본적으로 **파일**로 저장|
|**특장점**|다양한 프로토콜, 복잡한 HTTP 요청 가능|재귀적 다운로드(사이트 복제) 지원|
|**설치 비중**|대부분의 현대 OS에 기본 포함|별도 설치가 필요한 경우도 있음|

# 프로세스 및 자원관리
시시스템이 느려지거나 프로그램을 강제로 종료해야 할 때 사용합니다.
- **`top` / `htop`**: CPU, 메모리 사용량을 실시간 확인합니다. 
	- (WSL이라면 `htop` 설치를 추천합니다.)
- **`ps -ef`**: 현재 실행 중인 모든 프로세스 목록을 보여줍니다.
- **`kill -9 [PID]`**: 응답 없는 프로그램을 강제로 종료합니다.
- **`df -h`**: 하드디스크 남은 용량을 사람이 보기 편한 단위(GB, MB)로 보여줍니다.

### 예제 - 좀비 프로세스나 과부하 범인 잡기
- 메모리를 가장 많이 잡아먹는 프로세스 상위 5개 확인
```
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -n 6
```
- 포트 8080을 점유하고 있는 프로세스 찾아 강제 종료하기
```
kill -9 $(lsof -t -i:8080)
```

# 네트워크 (통신 및 진단)
- **`ip addr`**: 내 컴퓨터의 IP 주소를 확인합니다.
- **`ss -tuln`**: 현재 어떤 포트가 열려 있는지 확인합니다.
- **`curl -I`**: 웹 서버가 정상 작동하는지 상태 코드만 빠르게 확인합니다.
- **`ping`**: 상대방 서버와 연결이 되는지 확인합니다.

### 예제 - 서버 간 통신 확인 및 포트 체크
새로운 서비스를 배포했는데 연결이 안 될 때 사용합니다.
- 외부 서버의 특정 포트(예: 3306 DB 포트)가 열려 있는지 확인
```
nc -zv 192.168.0.10 3306
```
- 내 서버에서 현재 외부와 연결된(ESTABLISHED) 세션만 확인
```
ss -atn | grep ESTAB
```

# 파일관리
내용은 - terminal usage에서 확인

가장 기본이 되는 명령어들입니다.

- **`ls -al`**: 현재 위치의 파일 목록을 상세히(권한, 크기, 수정일 포함) 보여줍니다.
    
- **`cd`**: 디렉토리 이동. (`cd -`를 치면 직전 위치로 바로 돌아갑니다.)
    
- **`mkdir -p`**: 하위 디렉토리까지 한 번에 생성합니다. (예: `mkdir -p a/b/c`)
    
- **`rm -rf`**: 디렉토리를 강제로 삭제합니다. (주의해서 사용해야 합니다!)
    
- **`cp -r`**: 디렉토리를 통째로 복사합니다.

### 예시 - 실무에서는 단순히 파일을 복사하는 것보다, 특정 조건에 맞는 파일을 찾아 지우거나 옮기는 일이 많습니다.
- 30일 이상 지난 로그 파일들만 찾아 모두 삭제하기
```
find /var/log/myapp -name "*.log" -mtime +30 -delete
```
- 특정 단어가 포함된 파일들만 다른 디렉토리로 복사하기
```
grep -l "IMPORTANT" *.txt | xargs -i cp {} ./backup/
```

# 텍스트처리 
내용은 terminal usage에서 확인

리눅스는 "모든 것이 파일"이기 때문에 파일 내부를 들여다보는 기능이 매우 중요합니다.

- **`grep`**: 파일 내에서 특정 단어를 찾습니다.
    
    - `grep "error" log.txt`: 로그에서 에러만 추출.
        
- **`find`**: 파일 자체를 찾습니다.
    
    - `find . -name "*.conf"`: 현재 폴더에서 설정 파일만 검색.
        
- **`tail -f`**: 로그 파일의 뒷부분을 실시간으로 계속 보여줍니다. (서버 모니터링 필수)
    
- **`cat` / `less`**: 파일 내용을 화면에 출력합니다. (`less`는 페이지 단위로 끊어 볼 때 유용합니다.)

### 예시 - 실시간 로그 모니터링 및 필터링 - 서버 에러가 발생했을 때 가장 먼저 사용하는 조합입니다.

- 실시간으로 올라오는 로그에서 "ERROR"가 포함된 줄만 빨간색으로 보기
```
tail -f access.log | grep --color=auto "ERROR"
```

- 특정 로그 파일에서 방문자 IP 주소만 뽑아서 중복 제거하고 카운트하기

```
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```
(해당 서버에 누가 가장 많이 접속했는지 순위별로 보여줍니다.)

# 권한관리
내용은 terminal usage에서 확인

리눅스는 권한 관리가 매우 엄격합니다.

- **`sudo`**: 일반 사용자 권한으로 실행 안 되는 명령을 관리자(root) 권한으로 실행합니다.
    
- **`chmod`**: 파일의 읽기/쓰기/실행 권한을 변경합니다. (예: `chmod 755 script.sh`)
    
- **`chown`**: 파일의 소유자를 변경합니다.

### 예시 - 웹 서비스용 디렉토리 권한 설정

웹 서버(Nginx, Apache)가 파일을 읽지 못하는 권한 오류(`403 Forbidden`)를 해결할 때 씁니다.

- 현재 디렉토리의 모든 폴더는 755, 모든 파일은 644 권한으로 일괄 변경
```
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;
```
- 특정 폴더의 소유자를 웹 서버 계정(`www-data`)으로 변경
```
sudo chown -R www-data:www-data /var/www/html
```


# 패키지 관리

운영체제마다 다르지만, 현재 사용 중인 WSL(Ubuntu 기준)에서 가장 많이 씁니다.

- **`sudo apt update`**: 설치 가능한 프로그램 목록을 최신화합니다.
    
- **`sudo apt install [이름]`**: 새 프로그램을 설치합니다. (예: `vim`, `git`, `htop`)

### 예시 - 패키지/시스템: 용량 부족 문제 해결
디스크가 꽉 차서 서비스가 멈추기 직전에 사용하는 응급 처치입니다.

- 현재 디렉토리에서 용량을 가장 많이 차지하는 폴더 순서대로 보기
```
du -sh * | sort -hr | head -n 10
```

- APT 패키지 캐시 삭제로 용량 확보
```
sudo apt clean
```
