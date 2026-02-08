# bash scripting?
SH파일을 생성하여 BASH COMMAND를 통해 파일을 실행하면 여러 커맨드들이 실행되는 프로그램을 만들어 자동화를 하는것

# 스크립트 파일 작성 및 실행

## 파일생성 및 편집기 열기
- 파일생성 `touch filename.sh`
- 편집기 열기 
	- `nano filename.sh` : 컨트롤 O 저장 / 컨트롤 X 종료
	- `vi filename.sh` : i 입력모드, esc 명령어모드(입력종료), :wq 저장후종료 / :q! 저장하지않고 강제종료
	- `code .` vscode 편집기 open

## 실행권한 부여
파일을 만든 직후에는 실행 권한이 없습니다. `chmod` 명령어를 통해 실행 가능하게 만들어야 합니다.
`chmod +x myscript.sh`


## 스크립트 실행
`./myscript.sh`

`sh questionnaire.sh` : SH명령어를 통해 shell interpreter를 이용해 스크립트 실행
`bash asd.sh` : bash interpreter를 이용해 스크립트 실행


## 스크립트작성
```
codeally@450ce1864ec4:~/project$ touch script.sh 
codeally@450ce1864ec4:~/project$ chmod +x script.sh 
codeally@450ce1864ec4:~/project$ which bash 
/usr/bin/bash
```

nano script.sh 로 편집기 열어서 내용작성


```
#!/bin/bash 
#Program to run my other four programs

echo "hello world"
```

스크립트 실행
`./script.sh`

-> hello world 출력 확인


---
# 기본문법

### 세미콜론
콜론을 통해서 라인을 구분한다. 즉 콜론을 통해서 한줄에 여러 명령어를 작성가능하다
- `[[ 4 -ge 5 ]]; echo $?`

### `#`은 주석 
```bash
# 이것은 주석입니다
echo "helloworld"

```

### shebang
스크립트 파일을 실행할 인터프리터의 path표시하기
shebang을 추가하고나면, 파일명만 터미널에 입력하면, 해당 인터프리터를 통해서 스크립트가 실행된다.
```bash
#!/bin/bash
```


# 변수
## 변수 기초
### 변수 (Variables) 선언 및 사용
- **선언:** 대입 연산자(`=`) 앞뒤에 **공백이 없어야** 합니다.
    - 공백이 있으면 컴퓨터는 명령어로 인식을 합니다.
    - 변수는 대소문자를 구분합니다. 
    - 영문자,숫자,언더바만 사용가능합니다. 
    - 숫자로 시작하면 안됩니다. 
- 관례 : 현재세션에 잠깐 쓰는 지역변수는 소문자로 사용합니다. / 시스템에 영향을 주는 환경변수는 대문자로 작성합니다. 

- **사용:** 변수명 앞에 `$`를 붙입니다.

```BASH
name="Gemini"
echo "반갑습니다, $name님!"
```

### 값 할당 시 따옴표 활용 ''사용시 있는그대로 출력 ""사용시 변수치환
- **작은따옴표 (`' '`):** 안에 있는 내용을 **있는 그대로(문자열)** 처리합니다.
    - `GREETING='Hello $USER'` → 결과: `Hello $USER`
- **큰따옴표 (`" "`):** 안의 내용을 해석합니다. **변수를 치환**할 수 있습니다.
    - `GREETING="Hello $USER"` → 결과: `Hello goose` (실제 사용자 이름 출력)


### 변수사용시 중괄호의 활용 - 확실히 하기위해서는 변수에는 중괄호를 항상 쓰는 습관이 좋다.
변수 이름이 다른 글자와 붙어 있어 어디까지가 변수명인지 모호할 때 사용합니다.

- **예시:** `FRUIT=apple`
    - `echo "I like $FRUITs"` (컴퓨터는 `$FRUITs`라는 변수를 찾으려다가 실패합니다.)
    - `echo "I like ${FRUIT}s"` (정확히 `apple` 뒤에 `s`를 붙여 `apples`라고 출력합니다.)


### 변수 수정금지 readonly
`readonly 변수명` (한번 설정하면 값을 바꿀 수 없게 고정합니다.)
![[Pasted image 20260205123954.png]]

### 변수삭제
변수 삭제: `unset 변수명` (설정한 변수를 메모리에서 지웁니다.)
![[Pasted image 20260205124020.png]]

## 변수 생명주기 

부모와 자식의 관계

터미널을 열면 하나의 부모프로세스가 됩니다. 
터미널안에서 새로운프로그램 실행시 그 프로그램은 자식 프로세스가 되어 실행됩니다. 

지역변수 - 부모만 알고있는 비밀
- 부모는 자신의 기억을 자식에게 모두다 물려주지 않는다. 

환경변수 - 유산으로 물려주는 기억
- export Name=goose 라고 선언하는것은 이 변수를 유산목록에 등록하는것과 같다. 
- 부모는 자식에게 이러한 변수들의 복사본을 손에 쥐어줍니다. 



### 지역 변수 (Local Variables)

가장 수명이 짧은 변수입니다.

- **생성:** `NAME=goose`와 같이 일반적인 방식으로 선언할 때 생성됩니다.
- **범위:** 현재 실행 중인 **해당 터미널 창(Shell)** 안에서만 유효합니다.
- **소멸:** 
	- 터미널 창을 닫을 때.
    - `unset NAME` 명령어를 입력할 때.
    - **주의:** 현재 터미널에서 실행한 '자식 프로그램'(예: 쉘 스크립트 실행) 안으로는 이 변수가 전달되지 않습니다.
		- 자식프로세스는 부모프로세스의 지역변수에 접근이 불가합니다. 
		- 이것은 리눅스의 프로세스상속개념과 관련이 있습니다. 

### 환경 변수 (Environment Variables)

지역 변수보다 한 단계 더 넓은 생명주기를 가집니다.

- **생성:** `export NAME=goose`와 같이 **`export`** 키워드를 사용할 때 생성됩니다.
- **범위:** 현재 터미널은 물론, 그 터미널에서 실행되는 **모든 자식 프로세스**에 복사되어 전달됩니다.
- **소멸:** * 해당 변수를 처음 생성(export)한 부모 터미널 창을 닫을 때.
    - **한계:** 터미널을 새로 열면 다시 사라집니다. 즉, '현재 세션' 동안만 유지되는 것은 지역 변수와 같습니다.



---

### 3. 영구 변수 (Persistent Variables)

터미널을 껐다 켜도, 컴퓨터를 재부팅해도 살아남는 변수입니다.

- **생성:** 사용자의 홈 디렉토리에 있는 **설정 파일(`.bashrc` 또는 `.profile`)**에 `export` 구문을 직접 적어 넣어야 합니다.
- **작동 원리:** 터미널이 켜질 때마다 이 설정 파일을 읽어서 변수를 자동으로 다시 생성해 주는 방식입니다.
- **수명:** 설정 파일에서 해당 줄을 삭제하기 전까지 **영원히** 유지됩니다.


## 변수 확인
### set -  모든변수확인

쉘 세션에 정의된 **모든 변수(지역 변수 + 환경 변수)**와 쉘 함수까지 전부 보여줍니다.

- **명령어:** `set`
- **특징:** 양이 굉장히 많기 때문에 보통 `grep`과 함께 사용하여 특정 변수를 찾습니다.
    - 예: `set | grep MY_NAME`

### declare -p - 모든변수확인

```bash
declare -p
```

환경변수 포함 현재 쉘에서 생성된 변수 까지 모든 변수 보기


특정변수를 확인하려면 뒤에 변수명을 입력하세요.
```bash
declare -p J 
```


### 환경변수 확인하기 env or export or printenv

시스템 전체에 영향을 주거나 다른 프로그램으로 전달되는 **환경 변수**들만 골라서 보여줍니다.

- **명령어:** `env`
- **명령어 2:** `export` (변수 목록을 `export 변수명=값` 형식으로 보여주어 나중에 재사용하기 좋습니다.)
- **특징:** 개발 환경 설정(PATH, Editor, OS 종류 등)을 확인할 때 가장 많이 사용합니다.

-  명령어3 : `printenv` shell가 함께 제공된 환경변수확인


## 유용한 환경변수 - 스크립트작성시 유용한 환경변수를 적극 활용하세요

환경 변수 활용: 스크립트 내부에서 사용자의 홈 디렉토리가 필요하다면
`$HOME` 같은 환경 변수를 적극 활용하세요.

### RANDOM 
 RANDOM 변수는 0에서 32767 사이의 난수생성합니다.



## 변수 ${} 중괄호 사용법 - 변수의 확장기능
이미 정의된 **변수의 값**을 가져오거나, 그 값을 조작할 때 사용합니다. 그냥 `$VAR`라고 써도 되지만, `${}`를 쓰면 훨씬 강력하고 안전한 기능을 제공합니다.

### 대소문자 변경
```
STR="HELLO WORLD!"
echo ${STR,}   #=> "hELLO WORLD!" (lowercase 1st letter)
echo ${STR,,}  #=> "hello world!" (all lowercase)
```

```
STR="hello world!"
echo ${STR^}   #=> "Hello world!" (uppercase 1st letter)
echo ${STR^^}  #=> "HELLO WORLD!" (all uppercase)
```


### slicing - 문자열 자르기  `${변수:시작위치:길이}` 
변수의 내용 중 일부분만 추출할 때 사용합니다. `${변수:시작위치:길이}` 형식을 따릅니다.

- `${FILE:0:5}` : 0번째 인덱스부터 **5글자** 추출
- `${FILE:7}` : 7번째 인덱스부터 **끝까지** 추출
- `${FILE: -3}` : 뒤에서부터 **3글자** 추출 (마지막 확장자 확인 시 유용)

### replace 문자열 변경  `${STR/old/new}`
변수 안의 특정 문자를 다른 문자로 바꿉니다.

- `${STR/old/new}` : 처음 발견된 `old` 하나만 `new`로 교체
- `${STR//old/new}` : 발견된 **모든** `old`를 `new`로 교체
- `${STR/old/}` : `old`를 삭제 (공백으로 치환)

### strip 특정패턴 삭제
파일 경로명이나 확장자를 다룰 때 정말 많이 사용됩니다.

- **`#` (앞에서부터 삭제):**
    - `${PATH#*/}` : 앞에서부터 **가장 짧게** 일치하는 패턴 삭제
    - `${PATH##*/}` : 앞에서부터 **가장 길게** 일치하는 패턴 삭제 (파일명만 추출할 때 사용)
- **`%` (뒤에서부터 삭제):**
    - `${FILE%.*}` : 뒤에서부터 **가장 짧게** 일치하는 패턴 삭제 (확장자 제거 시 사용)
    - `${FILE%%.*}` : 뒤에서부터 **가장 길게** 일치하는 패턴 삭제

### 변수상태 체크 및 기본값 설정  `${VAR:-default}`
변수가 비어있을 경우를 대비해 예외 처리를 할 때 씁니다.

|                       |                                      |
| --------------------- | ------------------------------------ |
| **케이스**               | **의미**                               |
| `**${VAR:-default}**` | 변수가 비어있으면 `default`를 사용 (변수 값은 안 바뀜) |
| `**${VAR:=default}**` | 변수가 비어있으면 `default`를 **변수에 저장**하고 사용 |
| `**${VAR:?error}**`   | 변수가 비어있으면 `error` 메시지를 출력하고 스크립트 중단  |
| `**${VAR:+value}**`   | 변수가 설정되어 있을 때만 `value`를 사용           |
### 문자열 길이와 갯수  `${#VAR}`
`${#VAR}` : : 변수에 담긴 문자열의 글자 수를 반환합니다.

### 실전응용
파일 경로 `/home/user/data.tar.gz`  가 있을 때:
```
FILE="/home/user/data.tar.gz"

echo "파일명만: ${FILE##*/}"      # 결과: data.tar.gz
echo "폴더경로: ${FILE%/*}"       # 결과: /home/user
echo "첫번째 확장자 제거: ${FILE%.*}" # 결과: /home/user/data.tar
echo "모든 확장자 제거: ${FILE%%.*}"  # 결과: /home/user/data
```
이 `${ }` 문법들만 잘 활용해도 별도의 sed 나 awk 같은 외부 명령어 없이 쉘 자체 기능만으로 아주 빠른 문자열 처리가 가능합니다!
```
name="John"
echo ${name}
echo ${name/J/j}    #=> "john" (substitution)
echo ${name:0:2}    #=> "Jo" (slicing)
echo ${name::2}     #=> "Jo" (slicing)
echo ${name::-1}    #=> "Joh" (slicing)
echo ${name:(-1)}   #=> "n" (slicing from right)
echo ${name:(-2):1} #=> "h" (slicing from right)
echo ${food:-Cake}  #=> $food or "Cake"
```

## `$()` - 명령어 치환 - 명령어를 실행한 결과를 변수에 담는다.
괄호 안에 있는 **명령어를 실행한 결과(Standard Output)**를 텍스트로 가져와서 변수에 담거나 다른 명령에 전달할 때 사용합니다.
- 실행결과 담기
```
CURRENT_DIR=$(pwd)
echo "현재 위치는 ${CURRENT_DIR}입니다."
```

- **중첩 사용 가능:** 백틱(`` ` ``)보다 읽기 쉽고 중첩해서 쓰기 편합니다.
```
FILES_COUNT=$(ls $(pwd) | wc -l)
```
 


## 커맨드라인 arguments 

커맨드라인 아규먼츠란?

변수를 '미리 약속된 상자'에 담아 보내는 것이라면, 아규먼츠는 프로그램을 실행할 때 **뒤에 공백을 두고 직접 던져주는 데이터**입니다.

터미널에서 명령어를 칠 때 뒤에 붙이는 모든 단어들이 아규먼츠입니다.

- 예: `ls -al /home`
    - `ls`: 실행할 프로그램
    - `al`: 첫 번째 아규먼트 (옵션)
    - `/home`: 두 번째 아규먼트 (대상 경로)


arguments 는 프로그램이 실행하는 순간에만 반짝하고 전달되는 일회성 데이터입니다. 변수와는 다릅니다. 


###  스크립트에서 아규먼츠 읽는 법 (위치 매개변수)

쉘 스크립트 내부에서는 던져진 아규먼츠들을 **숫자**로 인식합니다. 이를 '위치 매개변수'라고 부릅니다.

|          |                                                                                                                                                                                              |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **변수명**  | **의미                                                                                                                                                                                         |
| `**$0**` | **실행된 프로그램의 이름** 그                                                                                                                                                                           |
| `**$1**` | **첫 번째**                                                                                                                                                                                     |
| `**$2**` | **두 번째**                                                                                                                                                                                     |
| `**$#**` | 전달된 아규먼트의 **                                                                                                                                                                                 |
| `$*`     | 전달된 **모든** 아규먼트 목록(목록전체를 하나의 문자열                                                                                                                                                             |
| `$@`     | 전달된 모든 아큐먼트 목록(각각을 독립된 문자열로 유지함) ->이것을 훨                                                                                                                                                     |
| `$_`     | 마지막인수 / 직전실행한 명령어의 맨 마지막 argument를 기억합니다. 명령어를 실행할때마다 실시간 업데이트 됩니다. 똑같은 긴 결로명이나 파일명을 다시 타이핑하지않고 재사용하기위해 사용합니다. / 마지막 명령어의 인자가 없는경우에는 명령어 자체가 저장됩니다.  / 스크립트내부에서는 스크립트내 직전라인의 마지막 인자를 가져옵니다.  |

보통은 파일이름을 arguments로 받아 처리할때 많이 사용됩니다. 
`./process.sh data.txt` 라고 실행하면, 스크립트 안에서 `cat $1` 을 통해 `data.txt` 의 내용을 읽어오는 식이죠.


## 특별변수 - 종료상태코드  `$?`

리눅스에서 실행되는 모든 명령어는 종료될 때 시스템에 **"내가 잘 끝났는지"** 를 숫자로 보고합니다. 이 숫자가 저장되는 특별한 변수가 바로 **`$?`** 입니다.

- **0**: **성공 (Success)** - 아무 문제 없이 명령어가 실행됨.
- **0이 아닌 숫자 (1~255)**: **실패 (Failure)** - 오류 발생. 숫자에 따라 어떤 종류의 오류인지 나타내기도 합니다.

명령어를 치고 나서 "이게 제대로 된 건가?" 의심이 들 때 바로 `echo $?` 를 쳐보세요.
### 명령어가 성공했을때만 다음 단계 진행하라
```
if [ $? -eq 0 ]; then 
	echo "작업 성공!" 
else 
	echo "작업 실패, 로그를 확인하세요." 
fi
```

`&& OR ||` 를 통해 연쇄명령을 내릴수도 있습니다.
- **`A && B` (AND)**: A가 **성공($?=0)**하면 B를 실행해라.
    - 예: `mkdir backup && cp photo.jpg backup/` (폴더가 잘 만들어져야 복사함)
- **`A || B` (OR)**: A가 **실패($?≠0)**하면 B를 실행해라.
    - 예: `ls important.txt || echo "파일이 없습니다!"`


# 표현식 단일대괄호`[]`, 이중대괄호`[[]]`, 이중소괄호`(())`, `$()` , `$(())` 
쉘 스크립트에서 **표현식(Expressions)**은 조건문(`if`, `while`)이 실행될 때 "이게 참(True)인가, 거짓(False)인가?"를 판단하는 기준이 됩니다. 

## 표현식 작성시 유의점
대괄호 `[` `]`를 쓸 때는 **양 끝에 반드시 공백**이 있어야 합니다.

- **잘못된 예:** `if [$A=$B]` (오류 발생)
- **올바른 예:** `if [ $A = $B ]` (정상 작동)

이중소괄호 공백에 관대함: `[ ]` 와 달리 괄호 안의 공백 유무가 실행에 영향을 주지 않습니다. 
- `((A+B))` 와 `(( A + B ))` 모두 잘 작동합니다.




가장 자주 쓰이는 세 가지 유형(파일, 숫자, 문자열)으로 나누어 정리해 드릴게요.
## 여러 표현식들의 차이점

### 1. `[ ]` (단일 대괄호) - 기본 조건식

가장 오래된 표준(POSIX) 방식입니다. 사실 이것은 `test`라는 명령어의 다른 이름입니다.

- **용도:** 파일 상태 확인, 문자열 비교, 숫자 비교.
- **특징:** 아주 엄격합니다. 변수에 공백이 있으면 에러가 잘 나기 때문에 항상 변수를 `"$VAR"`처럼 따옴표로 감싸야 합니다.
- **연산자:** `eq`, `ne`, `gt`, `f`, `z` 등.
- **예시:** `if [ "$NAME" = "goose" ]`

### 2. `[[ ]]` (이중 대괄호) - 확장 조건식 (권장)

`[ ]`의 단점을 보완한 Bash 전용 버전입니다. 훨씬 유연하고 강력해서 **최신 스크립트에서는 이 방식을 권장**합니다.

- **장점:**
    - 변수를 따옴표로 감싸지 않아도 공백 에러가 잘 안 납니다.
    - `&&`, `||` 같은 논리 연산자를 직접 쓸 수 있습니다.
    - *패턴 매칭(와일드카드 )**과 **정규표현식(`=~`)**을 사용할 수 있습니다.
- **예시:** `if [[ $NAME == g* ]]` (이름이 g로 시작하면 참)

### 3. `(( ))` (이중 소괄호) - 산술 연산 및 비교

오직 **'숫자(정수) 계산'**만을 위해 존재합니다. 프로그래밍 언어(C, Java 등)와 가장 유사한 문법을 가집니다.

- **용도:** 수학 계산, 숫자 크기 비교.
- **장점:**
    - `gt`, `lt` 대신 우리에게 익숙한 `>`, `<`, `>=`, `<=` 기호를 그대로 씁니다.
    - 변수 앞에 `$`를 붙이지 않아도 변수로 인식합니다.
- **예시:** `(( SUM = A + B ))` 또는 `if (( A > 10 ))`

## `help [[ expression ]]`

```
codeally@450ce1864ec4:~/project$ help [[
[[ ... ]]: [[ expression ]]
    Execute conditional command.
    
    Returns a status of 0 or 1 depending on the evaluation of the conditional
    expression EXPRESSION.  Expressions are composed of the same primaries used
    by the `test' builtin, and may be combined using the following operators:
    
      ( EXPRESSION )    Returns the value of EXPRESSION
      ! EXPRESSION              True if EXPRESSION is false; else false
      EXPR1 && EXPR2    True if both EXPR1 and EXPR2 are true; else false
      EXPR1 || EXPR2    True if either EXPR1 or EXPR2 is true; else false
    
    When the `==' and `!=' operators are used, the string to the right of
    the operator is used as a pattern and pattern matching is performed.
    When the `=~' operator is used, the string to the right of the operator
    is matched as a regular expression.
    
    The && and || operators do not evaluate EXPR2 if EXPR1 is sufficient to
    determine the expression's value.
    
    Exit Status:
    0 or 1 depending on value of EXPRESSION.
```

종료상태가 0이면 참, 1이면 false이다. 
## 비교 표현식

### 파일 이나 디렉토리 비교
파일이나 디렉토리가 존재하는지, 권한은 있는지 확인할 때 씁니다.

|   |   |
|---|---|
|**표현식**|**의미**|
|`**-e**` 파일명|파일(또는 폴더)이 **존재하면** 참 (Exists)|
|`**-f**` 파일명|일반 **파일**이면 참 (File)|
|`**-d**` 파일명|**디렉토리**이면 참 (Directory)|
|`**-r**` 파일명|**읽기** 권한이 있으면 참 (Read)|
|`**-x**` 파일명|**실행** 권한이 있으면 참 (eXecutable)|

- `[[ -r FILE ]]` → readable 읽을수있나?
    
- `[[ -w FILE ]]` → writable 쓸수있나?
    
- `[[ -x FILE ]]` → executable 실행할수있나?
    
- `[[ -e FILE ]]` → exist 파일존재하나?
    
- `[[ -a file ]]` → exist 파일존재하나?(deprecated)
    
- `[[ -s FILE ]]` → 파일 size가 0bytes 이상이냐?
    
- `[[ -h FILE ]]` → symlink
    
- `[[ -d FILE ]]` → directory (폴더냐?)
    
- `[[ -f FILE ]]` → file? 일반파일이냐(not 폴더 or device 파일)
    
- `[[ FILE1 -nt FILE2 ]]` → newer than → 1이 2보다 더 최신파일이냐?
    
- `[[ FILE1 -ot FILE2 ]]` → older than → 2가 1보다 더 최신파일이냐?
    
- `[[ FILE1 -ef FILE2 ]]` → equal files 같은 파일이냐?

### 숫자 비교
숫자를 비교할 때는 `> , <` 기호 대신 영문 약자를 사용합니다.
주의: `[ 10 > 5 ]` 라고 쓰면 리눅스는 숫자를 비교하는 게 아니라 출력을 파일로 보내는 것으로 오해할 수 있습니다. 반드시 `[ 10 -gt 5 ]` 라고 써야 합니다.

|           |        |                          |
| --------- | ------ | ------------------------ |
| **표현식**   | **의미** | **풀네임**                  |
| `**-eq**` | 같다     | **Eq**ual                |
| `**-ne**` | 같지 않다  | **N**ot **E**qual        |
| `**-gt**` | 크다     | **G**reater **T**han     |
| `**-lt**` | 작다     | **L**ess **T**han        |
| `**-ge**` | 크거나 같다 | **G**reater or **E**qual |
| `**-le**` | 작거나 같다 | **L**ess or **E**qual    |
### 문자열 비교

|   |   |
|---|---|
|**표현식**|**의미**|
|`**=**`|두 문자열이 같으면 참|
|`**!=**`|두 문자열이 다르면 참|
|`**-z**` 문자열|문자열이 **비어있으면(길이가 0)** 참 (Zero)|
|`**-n**` 문자열|문자열이 **비어있지 않으면** 참 (Not zero)|


### 정규표현식 비교 `[[ STRING =~ STRING ]]`
- 특정 문자를 포함하는가? - 공백이 있으면 ""로 묶고 아니면 그냥 써도된다. 
```
[[ hello =~ el ]]; echo $?
0


[[ "hello world" =~ "lo wor" ]]; echo $?
0
```
- 정규표현식과 일치하는가? 정규표현식은 ""로 묶으면 안된다. 
	- `^h` 는 h로 문자가 시작함을 의미한다.  
```
[[ "hello world" =~ ^h ]]; echo $?
0
```


- `^h.+d$`  
	- `^h` h로 시작 하고 
	- `.` 이후에 최소 1개의 문자가 존재하고
	- `+` 앞의 패턴(`.`)이 **1번 이상** 반복되어야 함을 뜻합니다.
	- `d$` `$` 문자열의 **끝**이 바로 앞의 문자와 일치해야 함을 뜻합니다.
```
 [[ "hello world" =~ ^h.+d$ ]]; echo $?
0
```

```
VAR="hello world" 
echo $VAR 
hello world

[[ $VAR =~ \?$ ]]; echo $? 
1 
[[ test? =~ \?$ ]]; echo $? 
0
```
- `\`의 역활은 뒤의 문자를 특수문자를 일반문자로 인식하게 만든다. 
	- 즉 위의 `\?$` 는 순수하게 문자가 `?` 로 끝나냐라고 묻는것
	- `.`은 문자1개를 의미하는데, `\.` 은 `.` 텍스트 그자체를 의미합니다. 

### 정규표현식 관련 스크립트 연습

```
#!/bin/bash

  

# Program to tell a persons fortune

  

echo -e "\n~~ Fortune Teller ~~\n"

  

RESPONSES=("Yes" "No" "Maybe" "Outlook good" "Don't count on it" "Ask again later")

N=$(( RANDOM % 6 ))

  

GET_FORTUNE() {

  echo Ask a yes or no question:

  read QUESTION

}

  

until [[ $QUESTION =~ \?$ ]]

do

  GET_FORTUNE

done

  

echo ${RESPONSES[$N]}
```

## 논리 
- `[[ ! EXPR ]]` not
- `[[ X && Y ]]` and
- `[[ X || Y ]]` or

```
[[ -x countdown.sh && 5 -le 4 ]]; echo $?
#-> 1 (2개모두 참이아니므로 false의미하는 1반환)

[[ -x countdown.sh ||  5 -le 4 ]]; echo $?
#-> 1 (2개중 1개가 참이므로 0반환)
```

### 연습 스크립트
```
#!/bin/bash

SAMPLE_FILE="./example1.sh"

if [ ! -f "$SAMPLE_FILE" ]
then
    echo "Sample file doesn't exists"
fi
```

## 산술연산
`help let` 을 하면 이중괄호와 사용가능한 연산자를 볼수있다.

Bash 쉘에서 오직 **'산술 연산(Arithmetic)'**과 **'정수 비교'**를 위해 태어난 녀석입니다.

일반적인 쉘 문법은 모든 것을 '문자열'로 보려 하기 때문에 계산이 복잡하지만, `(( ))` 안에서는 C 언어나 Java 같은 프로그래밍 언어와 거의 똑같은 방식으로 수학을 다룰 수 있습니다

### 특징 `(( ))`의 핵심 특징 (왜 편한가?)

- **`$` 표시 생략 가능:** 변수를 가져올 때 `$A`라고 쓰지 않고 그냥 `A`라고만 써도 변수로 인식합니다.
- **익숙한 연산자:** `gt`, `lt` 대신 `>`, `<`, `==`, `!=`, `>=`, `<=`를 그대로 사용합니다.
- **공백에 관대함:** `[ ]`와 달리 괄호 안의 공백 유무가 실행에 영향을 주지 않습니다. `((A+B))`와 `(( A + B ))` 모두 잘 작동합니다.
- **복합 연산:** 한 줄에 여러 계산을 넣거나 증감 연산(`++`, `-`)을 할 수 있습니다.

### 꼭 기억해야 할 한계: "정수만 가능"

`(( ))`는 오직 **정수(Integer)**만 다룹니다. `10 / 3`을 하면 `3.333`이 아니라 몫인 `3`만 나옵니다.

### 사용가능 연산자
|   |   |   |
|---|---|---|
|**종류**|**연산자**|**예시**|
|**기본 산술**|`+`, `-`, `*`, `/`, `%` (나머지), `**` (거듭제곱)|`(( 2 ** 3 ))` (결과: 8)|
|**증감**|`++`, `--`|`(( i++ ))`|
|**논리**|`&&` (AND), `||
|**비교**|`==`, `!=`, `<`, `>`, `<=`, `>=`|`(( A != B ))`|
|**비트 연산**|`<<`, `>>`, `&`, `|`,` ~`,` ^`|
### 사용법
 ① 값 계산 및 할당

변수에 계산 결과를 바로 저장할 수 있습니다.

```bash
SCORE=80
(( SCORE = SCORE + 10 ))  # SCORE는 90이 됨
(( SCORE += 5 ))          # SCORE는 95가 됨 (복합 대입 연산)
(( SCORE++ ))             # SCORE는 96이 됨 (증감 연산)
echo $SCORE
```

 ② 조건문(`if`)에서의 활용

숫자 크기를 비교할 때 가장 직관적입니다.

```bash
AGE=25

if (( AGE >= 20 && AGE < 30 )); then
    echo "당신은 20대입니다."
fi
```

③ 반복문(`for`)에서의 활용

C 언어 스타일의 `for` 문을 터미널에서 구현할 수 있습니다.

```bash
# 1부터 5까지 출력
for (( i=1; i<=5; i++ )); do
    echo "숫자: $i"
done
```


## `(( ))` vs `$(( ))` 의 차이 (중요!)

이 부분이 가장 헷갈릴 수 있습니다. **`$`가 있고 없고**에 따라 결과값이 출력되느냐 실행만 되느냐가 결정됩니다.

1. **`(( ))` (명령어 형태로 산술연산을 수행만한다.):**
    - 계산을 **실행**만 합니다.
    - 결과값을 변수에 담거나 조건문에서 성공/실패만 따질 때 씁니다.
    - `(( A = 1 + 2 ))` (O) / `echo (( 1 + 2 ))` (X - 에러 발생)

```
COUNT=0
((COUNT++))      # COUNT 값을 1 증가시킴
if ((COUNT > 0)); then
    echo "0보다 큽니다."
fi
```
    
2. **`$(( ))` (산술연산의 결과값 치환 형태):**
    - 계산된 **결과값**으로 통째로 바뀝니다.
    - `echo`로 출력하거나 다른 변수에 대입할 때 씁니다.
    - `echo $(( 1 + 2 ))` → 결과: `3` 출력

## `$()` VS `$(())` 차이
`$()`가 "명령어 실행"이었다면, 괄호가 하나 더 붙은 **`$(())`는 "산술 연산"**을 위한 전용 공간입니다.

- 명령어 수행 결과를 **텍스트**로 받고 싶으면 `$( )`   
- 산술연산 결과를 **숫자(계산값)**로 받고 싶으면 `$(( ))`

`$(())` : 산술 확장 (Arithmetic Expansion)

괄호 안의 내용을 **수학식**으로 계산하고 그 결과를 반환합니다.
- **특징:**
    - `bc` 같은 외부 명령어 없이 Bash 자체에서 정수 계산을 처리합니다. (매우 빠름)
    - 변수 앞에 `$`를 붙이지 않아도 변수값으로 인식합니다.
    - **정수 연산만 가능**합니다. (소수점은 앞서 배운 `bc`를 써야 합니다.)

```
A=10
B=20
RESULT=$((A + B))  # 변수 앞에 $가 없어도 됨
echo $(( (A + 5) * 2 )) 
# 결과: 30
```

비교
```
# 1. $( )의 경우: 명령어를 찾아 실행하려 함
echo $(1+2)
# 결과: -bash: 1+2: command not found (1+2라는 명령어가 없으므로 에러)

# 2. $(( ))의 경우: 수학 계산을 함
echo $((1+2))
# 결과: 3
```
## 소수점 계산 필요시 bc 활용
- 소수점 계산이 꼭 필요하다면 `bc` (Binary Calculator)라는 별도의 도구를 활용해야 합니다.
    - 예: `echo "scale=2; 10 / 3" | bc` → 결과: `3.33`
가장 핵심은 **`scale`**이라는 변수를 설정하여 **소수점 몇째 자리까지 표시할지** 지정하는 것입니다.
가장 흔히 쓰이는 방식입니다. `scale`을 지정하지 않으면 기본값이 `0`이라 정수 결과만 나옵니다.

-  기본적인 사용법 (`echo`와 파이프 활용)

가장 흔히 쓰이는 방식입니다. `scale`을 지정하지 않으면 기본값이 `0`이라 정수 결과만 나옵니다
```
# scale을 지정하지 않으면 정수만 출력
echo "5 / 2" | bc
# 결과: 2

# scale=2로 지정 (소수점 둘째 자리)
echo "scale=2; 5 / 2" | bc
# 결과: 2.50
```

-  복잡한 수식과 변수 활용

여러 수식을 한꺼번에 처리하거나 변수를 섞어서 사용할 수 있습니다.

```
VAR1=10
VAR2=3
RESULT=$(echo "scale=4; $VAR1 / $VAR2" | bc)

echo $RESULT
# 결과: 3.3333
```

**앞자리가 0일 때 생략됨**: `bc`는 `0.5`를 `.5`로 출력하는 특징이 있습니다. 만약 앞의 0을 꼭 붙이고 싶다면 `printf`와 함께 사용해야 합니다.
```
printf "%.2f\n" $(echo "scale=2; 1/2" | bc)
# 결과: 0.50
```

 `-l` 옵션 (수학 라이브러리 사용)

`-l` 옵션을 붙이면 소수점 자릿수가 기본 **20자리**로 설정되며, 삼각함수나 로그 같은 수학 함수를 쓸 수 있습니다.


# 조건문

## 조건문 (If Statement)

조건을 비교할 때는 `[[ ]]`를 사용하는 것이 안전하고 기능이 많습니다.
```
if [[ CONDITION ]]
then
  STATEMENTS
elif [[ CONDITION ]]
then
	STATEMENTS
else
	STATEMENTS
fi
```

```
SCORE=85

if (( SCORE >= 90 )); then
    echo "A 학점입니다."
elif (( SCORE >= 80 )); then
    echo "B 학점입니다."
else
    echo "더 노력하세요!"
fi
```

```BASH
#!/bin/bash

# Program that counts down to zero from a given argument

if [[ $1 -gt 5 ]]

then

    echo "5초과입니다"

else

    echo "5이하입니다"

fi
```

## case문

if 문이 "이것 아니면 저것" 식의 복잡한 논리를 따질 때 좋다면,
case문은 "여러 후보 중 하나를 고르는" 상황에서 훨씬 깔끔하고 가독성이 좋습니다. 마치 키오스크 메뉴에서 번호를 선택하는 것과 비슷

```
case EXPRESSION in
  PATTERN) STATEMENTS ;;
  PATTERN) STATEMENTS ;;
  PATTERN) STATEMENTS ;;
  *) STATEMENTS ;;
esac
```
```
case $변수 in
    패턴1)
        # 패턴1에 해당할 때 실행할 내용
        ;;
    패턴2)
        # 패턴2에 해당할 때 실행할 내용
        ;;
    *)
        # 위 패턴 중 아무것도 맞지 않을 때 (Default)
        ;;
esac
```
- **`;;`**: 각 선택지가 끝났음을 알리는 마침표 같은 역할입니다.
- **`)`**: "그 외 나머지"라는 뜻으로, 예외 처리를 위해 반드시 넣어주는 것이 좋습니다.

```
#!/bin/bash

echo "어떤 언어로 인사할까요? (ko, en, jp)"
read LANG

case $LANG in
    ko)
        echo "안녕하세요!"
        ;;
    en)
        echo "Hello!"
        ;;
    jp)
        echo "こんにちは!"
        ;;
    *)
        echo "지원하지 않는 언어입니다."
        ;;
esac
```

## case문의 강력기능 - 패턴활용(와일드카드, 파이프)
case 문은 단순히 글자가 일치하는 것뿐만 아니라, **와일드카드(* ,?)**와 **파이프(
|)**를 사용해 여러 조건을 묶을 수 있습니다.

### ① 여러 조건을 하나로 묶기 (`|`)

"yes", "y", "YES" 중 무엇을 입력해도 통과하게 만들 수 있습니다.
```
case $answer in
    y|yes|YES)
        echo "동의하셨습니다."
        ;;
    n|no|NO)
        echo "거절하셨습니다."
        ;;
esac
```

### ② 패턴 매칭 활용 - 와일드카드 `*`

확장자에 따라 파일을 분류할 때 유용합니다.

```
filename="photo.jpg"

case $filename in
    *.jpg|*.png|*.gif)
        echo "이미지 파일입니다."
        ;;
    *.txt|*.doc)
        echo "문서 파일입니다."
        ;;
    *)
        echo "알 수 없는 형식입니다."
        ;;
esac
```

## case vs if

|   |   |   |
|---|---|---|
|**구분**|**if 문**|**case 문**|
|**장점**|범위 비교(`>`), 복합 조건(`&&`)에 강함|고정된 값의 나열에서 훨씬 읽기 편함|
|**단점**|조건이 많아지면 `elif`가 길어져 지저분함|숫자 크기 비교(`> 100`) 등은 하기 힘듦|
|**추천 상황**|"점수가 80점 이상인가?" (범위)|"입력한 메뉴가 1번인가 2번인가?" (선택)|

# 반복문

## 반복문 (Loops)

리스트의 요소를 하나씩 꺼내어 처리합니다.

```
`for NAME [in WORDS ... ] ; do COMMA>`

`for (( exp1; exp2; exp3 )); do COMM>`
```

```BASH
for i in {1..5}; do
  echo "숫자: $i"
done
```

## 목록기반 for문 (가장많이 사용) for in
```
# 과일 목록 출력
for fruit in 사과 포도 수박; do
    echo "내가 좋아하는 과일: $fruit"
done

# 현재 폴더의 모든 .txt 파일 이름 출력
for file in *.txt; do
    echo "텍스트 파일 발견: $file"
done
```

## for in {range}

1~5까지 반복
```
for i in {1..5}; do
    echo "Welcome $i"
done
```


중괄호 확장
```
for i in {5..50..5}; do
    echo "Welcome $i"
done
```
- 특정 범위 내에서 일정한 간격으로 숫자를 생성하며 명령을 실행하는 구문입니다.
- 가장 핵심적인 부분은 `{5..50..5}`라는 **중괄호 확장(Brace Expansion)** 문법입니다.
- **5**부터 시작해서 **50**까지, **5씩** 더해가며 숫자를 생성하라는 뜻입니다.
- **숫자 앞에 0을 붙이고 싶다면? (Padding)** `{05..50..5}`라고 쓰면 `Welcome 05`, `Welcome 10` 처럼 자릿수를 맞춰줍니다.


## c언어 스타일 for문
```
for (( i=1; i<=5; i++ )); do
    echo "$i번째 반복 중..."
done
```

## while문
조건이 참인 동안에는 계속 반복해라"라는 뜻입니다.

```
count=1
while (( count <= 3 )); do
    echo "카운트: $count"
    (( count++ ))  # 값을 증가시키지 않으면 무한 루프에 빠집니다!
done
```


## until문 
- while루프와 유사합니다.
- 조건이 충족될때까지 루프를 실행합니다.
```
until [[ CONDITION ]]
do
  STATEMENTS
done
```


```
#!/bin/bash

# Program to tell a persons fortune

echo -e "\n~~ Fortune Teller ~~\n"

RESPONSES=("Yes" "No" "Maybe" "Outlook good" "Don't count on it" "Ask again later")
N=$(( RANDOM % 6 ))

GET_FORTUNE() {
  echo Ask a yes or no question:
  read QUESTION 
}

until [[ $QUESTION == test? ]]
do
  GET_FORTUNE
done

echo ${RESPONSES[$N]}
```

## while 문은 파일의 내용을 한 줄씩 읽어서 처리할 때 가장 빛을 발합니다.

```
while read line; do
    echo "읽은 내용: $line"
done < data.txt

```


## break와 continue


반복문 안에서 흐름을 더 세밀하게 조정합니다.

- **`break`**: 반복문을 즉시 탈출합니다.
- **`continue`**: 현재 순서를 건너뛰고 다음 반복으로 넘어갑니다.

# 배열 

여러 데이터를 하나의 이름으로 묶어 관리할 수 있는 **배열(Array)** 기능을 제공합니다. Bash 배열은 크기를 미리 정할 필요가 없고, 숫자와 문자열을 섞어서 저장할 수 있어 매우 유연


## 1. 배열 선언 및 할당 `(FRUITS=("Apple" "Banana" "Cherry")`

가장 일반적인 방법은 괄호 `( )`를 사용하는 것입니다.

- **동시 할당:** `FRUITS=("Apple" "Banana" "Cherry")`
- **특정 인덱스에 할당:** `FRUITS[0]="Apple"` (인덱스는 **0**부터 시작합니다.)
- **추가하기:** `FRUITS+=("Orange")` (기존 배열 끝에 새 요소 추가)

---

## 배열변수 확인 declare -p 배열이름

```bash
codeally@7f71b936eaaf:~/project$ declare -p ARR
declare -a ARR=([0]="a" [1]="b" [2]="c")
```

-a 는 array를 말합니다.

## 2. 배열 요소 접근 (가져오기)

배열을 사용할 때는 **중괄호 `${ }`**를 반드시 사용해야 정확한 값을 가져올 수 있습니다.

|**문법**|**의미**|
|---|---|
|**`${FRUITS[0]}`**|첫 번째 요소 (`Apple`)|
|**`${FRUITS[@]}`**|배열의 **모든 요소** (공백 구분)|
|**`${#FRUITS[@]}`**|배열의 **전체 개수** (길이)|
|**`${!FRUITS[@]}`**|배열의 **인덱스 목록** (0, 1, 2...)|

### 💡 요약 및 팁

- 배열 전체를 지칭할 때는 **`"${ARRAY[@]}"`** 처럼 큰따옴표를 꼭 써주세요. (요소 안에 공백이 있어도 안전하게 분리됩니다.)
- `$*`와 `$@`의 차이처럼, 배열도 `[@]`를 쓰는 것이 데이터 보존에 유리합니다.

## 3. 배열과 반복문 활용 (실전)

배열은 `for` 문과 만났을 때 가장 강력합니다.

```bash
#!/bin/bash

# 배열 정의
SERVERS=("Web01" "Web02" "DB01" "Storage")

# 1. 모든 요소 출력
echo "전체 서버 목록: ${SERVERS[@]}"

# 2. 개수 확인
echo "관리할 서버 개수: ${#SERVERS[@]}"

# 3. 반복문으로 하나씩 처리
for server in "${SERVERS[@]}"; do
    echo "현재 ${server} 서버 상태를 점검 중입니다..."
done
```

## 4. 슬라이싱과 삭제

- **일부만 가져오기 (Slicing):** `${FRUITS[@]:1:2}` (1번 인덱스부터 2개 가져오기)
- **요소 삭제:** `unset FRUITS[1]` (1번 인덱스의 'Banana' 삭제)
- **배열 전체 삭제:** `unset FRUITS`

## 예제

DEFINE ARRAY

```bash
Fruits=('Apple' 'Banana' 'Orange')

Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

WORKING WITH ARRAYS

- `*` 또는 `@` 으로 전체 배열을 인쇄할수있습니다.

```bash
echo ${Fruits[0]}           # Element #0 
echo ${Fruits[-1]}          # Last element
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
echo ${!Fruits[@]}          # Keys of all elements, space-separated
```

배열변수는 declare list에 존재해야합니다

- -a 는 array를 말합니다.

```bash
codeally@7f71b936eaaf:~/project$ ARR=("a" "b" "c")
codeally@7f71b936eaaf:~/project$ echo ${ARR[1]}
b
codeally@7f71b936eaaf:~/project$ echo ${ARR[1]}
b
codeally@7f71b936eaaf:~/project$ declare -p ARR
declare -a ARR=([0]="a" [1]="b" [2]="c")
```

### Operation

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

### iteration

```bash
for i in "${arrayName[@]}"; do
  echo $i
done
```

# 연관배열 - dictionary

연관 배열 (Associative Array) - Bash 4.0+

숫자 인덱스 대신 **이름(Key)**으로 값을 저장하는 방식입니다. 파이썬의 딕셔너리와 비슷합니다.

## 선언 저장 값 가져오기
사용 전 반드시 `declare -A`로 선언해야 합니다.

```bash
# 연관 배열 선언
declare -A USER_INFO

# 값 저장
USER_INFO["name"]="goose"
USER_INFO["city"]="Seoul"

# 값 호출
echo "사용자 이름: ${USER_INFO["name"]}"
```

## 연관배열 예제

```bash
declare -A sounds

sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

working with dict

```bash
echo ${sounds[dog]} # Dog's sound
echo ${sounds[@]}   # All values
echo ${!sounds[@]}  # All keys
echo ${#sounds[@]}  # Number of elements
unset sounds[dog]   # Delete dog

```

iteration

- **Iterate over values**

```bash
for val in "${sounds[@]}"; do
  echo $val
done
```

- **Iterate over keys**

```bash
for key in "${!sounds[@]}"; do
  echo $key
done
```


# 함수
쉘 스크립트에서 **함수(Function)**는 반복되는 코드 덩어리를 하나로 묶어 이름을 붙여놓은 것입니다. 함수를 쓰면 전체 코드가 깔끔해지고, 한 번 만들어둔 로직을 필요할 때마다 이름만 불러서 재사용할 수 있습니다.

## 함수사용법
### 함수의 정의

함수를 만드는 법은 두 가지가 있지만, 첫 번째 방식이 가장 많이 쓰입니다.

```bash
# 방식 1 (권장)
function_name() {
    # 실행할 내용
    echo "함수가 실행되었습니다."
}

# 방식 2 (function 키워드 사용)
function function_name() {
    echo "함수가 실행되었습니다."
}
```

### 함수호출

- 함수호출에는 `$`가 필요치 않습니다. **이름만** 적으면 됩니다.

```bash
GET_FORTUNE() {
  echo Ask a yes or no question:
}
GET_FORTUNE
```

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"
```

### 함수로 데이터 전달

함수는 특이하게도 정의할 때 매개변수 이름을 적지 않습니다. 대신 스크립트 실행 인자처럼 **위치 매개변수(`$1`, `$2`...)**를 사용하여 값을 받습니다.

```bash
#!/bin/bash

# 인사하는 함수 정의
greet() {
    echo "안녕하세요, $1님! 오늘 $2하기 좋은 날이네요."
}

# 함수 호출 시 옆에 값을 던져줍니다.
greet "Gemini" "코딩"
greet "사용자" "산책"

```

### returning values - 함수결과반환

다른 프로그래밍 언어의 `return`과 쉘 스크립트의 `return`은 의미가 조금 다릅니다.

① `return` (종료 상태 코드 반환)

- 함수의 **성공(0) 또는 실패(1~255)** 여부만 전달합니다.
- 실제 '값'을 돌려주는 것이 아니라, 아까 배운 **`$?`*에 숫자를 저장하는 것입니다.

② `echo` (결과값 반환)

- 함수의 실행 결과(문자열이나 숫자)를 호출한 곳으로 돌려주고 싶을 때는 `echo`를 사용하고 **명령어 치환(`$( )`)**으로 받습니다.

```bash
myfunc() {
    local myresult='some value'
    echo $myresult
}
result="$(myfunc)"
```

### 지역변수 사용 local

함수 내부에서만 사용하는 변수는 반드시 앞에 **`local`** 키워드를 붙여야 합니다. 그렇지 않으면 함수 밖의 변수 값이 의도치 않게 바뀔 수 있습니다.

```bash
var="부모 변수"

my_func() {
    local var="함수 내부 지역 변수"
    echo "함수 안: $var"
}

my_func
echo "함수 밖: $var"  # local이 없다면 이 값도 "함수 내부..."로 바뀌어버립니다.
```

### raising errors - 함수수행 실패처리 if문활용용

```bash
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

## 함수의 생명주기와 위치

- **정의가 먼저:** 함수는 호출되기 전(코드 위쪽)에 반드시 먼저 정의되어야 합니다.
- **스코프:** 함수는 해당 스크립트가 실행되는 동안 메모리에 머물며, 스크립트가 종료되면 함께 사라집니다. 만약 터미널에서 항상 쓰고 싶다면 `.bashrc`에 등록하면 됩니다.

```bash
#!/bin/bash

check_file() {
    if [[ -f "$1" ]]; then
        echo "'$1' 파일이 존재합니다."
        return 0
    else
        echo "'$1' 파일이 없습니다."
        return 1
    fi
}

# 함수 실행 후 종료 상태($?) 확인
check_file "test.txt"
if [[ $? -eq 0 ]]; then
    echo "작업을 계속합니다."
fi
```

```bash
#!/bin/bash

# Program to tell a persons fortune

echo -e "\\n~~ Fortune Teller ~~\\n"

RESPONSES=("Yes" "No" "Maybe" "Outlook good" "Don't count on it" "Ask again later")
N=$(( RANDOM % 6 ))

GET_FORTUNE() {
  echo Ask a yes or no question:
  read QUESTION 
}

GET_FORTUNE

echo ${RESPONSES[$N]}
```


# `<<EOF  .... EOF` 사용법
쉘 스크립트에서 **EOF**는 **End Of File**의 약자로, 파일이나 입력 데이터의 **'끝'**을 알리는 약속된 기호입니다.

주로 **Here Document**라고 불리는 문법과 함께 쓰이며, 터미널에서 **여러 줄의 텍스트를 한꺼번에 파일에 저장하거나 프로그램에 입력할 때 사용**합니다.


### 1. 왜 사용하는가?

보통 한 줄짜리 글은 `echo "내용" > file.txt`로 충분합니다. 하지만 내용이 10줄, 20줄이라면 `echo`를 20번 쓰기엔 너무 번거롭죠. 이때 EOF를 쓰면 **"지금부터 EOF라는 글자가 다시 나올 때까지 모든 내용을 쏟아붓겠다"**는 선언을 할 수 있습니다.

---

### 2. 기본 문법 구조

기호 `<<` 뒤에 시작을 알리는 단어(주로 EOF)를 적고, 내용 끝에 다시 그 단어를 적어 닫아줍니다.

```bash
cat << EOF > my_file.txt
안녕하세요.
여기는 EOF를 테스트하는 구간입니다.
여러 줄을 한꺼번에 적을 수 있어 편리하죠?
EOF
```

- **`cat << EOF`**: "EOF라는 단어가 나올 때까지 입력받은 내용을 `cat`으로 보여줘라."
- **`> my_file.txt`**: "그 보여준 내용을 `my_file.txt`라는 파일에 저장해라."
- **맨 마지막 `EOF`**: 반드시 **줄의 맨 앞**에 있어야 하며, 뒤에 공백이 있으면 안 됩니다.

### 3. EOF의 숨은 기능 (따옴표 활용)

EOF를 어떻게 쓰느냐에 따라 안의 내용을 해석하는 방식이 달라집니다.

- **`<< EOF` (따옴표 없음):** 내용 중의 **변수를 해석**합니다.

```bash
NAME="Gemini"
cat << EOF
Hello, $NAME
EOF
# 결과: Hello, Gemini
```

**`<< "EOF"` (따옴표 있음):** 내용을 **있는 그대로(Literal)** 출력합니다. 변수가 작동하지 않습니다.

```bash

cat << "EOF"
Hello, $NAME
EOF
# 결과: Hello, $NAME
```

### 4. 탭(Tab) 공백 제거: `<<-EOF`

스크립트를 짤 때 들여쓰기를 하면 EOF 내부에도 공백이 포함되어 버립니다. 이때 `<<-EOF`를 쓰면 줄 앞부분의 **탭(Tab) 문자**를 자동으로 제거해 코드를 깔끔하게 유지할 수 있습니다.

```bash
if true; then
    cat <<- EOF
	이 문장 앞의 탭(Tab)은 
	자동으로 무시됩니다.
	EOF
fi
```

### 💡 요약

- **의미:** 데이터 입력의 끝을 나타내는 표시자.
- **이름:** 반드시 `EOF`일 필요는 없습니다. `END`, `STOP`, `MYDATA` 등 아무 단어나 써도 되지만, 관례상 **`EOF`*를 가장 많이 씁니다.
- **핵심 용도:** 여러 줄의 텍스트를 파일에 쓰거나(설정 파일 생성 등), 스크립트 내에서 긴 메시지를 출력할 때 사용.


# 사용자 입력 받기 READ

## 1. 기본 사용법

`read` 뒤에 변수 이름을 적으면, 사용자가 입력한 내용이 그 변수에 저장됩니다.

```bash
echo "이름을 입력하세요:"
read NAME
echo "안녕하세요, $NAME님!"
```

## 주요 옵션 (실무 활용)

`read` 명령어는 단순히 입력받는 것 외에도 다양한 편리한 옵션을 제공합니다.

|**옵션**|**의미**|**설명**|
|---|---|---|
|**`-p`**|**Prompt**|안내 문구를 출력하고 바로 입력을 받습니다. (`echo` 생략 가능)|
|**`-s`**|**Silent**|입력하는 글자가 화면에 보이지 않게 합니다. (비밀번호 입력 시)|
|**`-t`**|**Timeout**|정해진 시간(초) 동안 입력이 없으면 다음으로 넘어갑니다.|
|**`-n`**|**nchars**|지정한 글자 수만큼만 입력받으면 자동으로 종료합니다.|

## 실전 예시

### ① `p` 옵션으로 깔끔하게 받기

`echo`를 따로 쓸 필요가 없어 가장 많이 쓰이는 방식입니다.

```bash
read -p "나이를 입력해 주세요: " AGE
echo "귀하의 나이는 $AGE세입니다."
```

### ② 비밀번호 입력받기 (`s`)

화면에 글자가 표시되지 않아 보안이 필요한 경우에 사용합니다.

```bash
read -sp "비밀번호를 입력하세요: " PASSWORD
echo -e "\\n비밀번호가 안전하게 저장되었습니다." # -s는 줄바꿈을 안해주므로 따로 추가
```

### ③ 시간 제한 설정하기 (`t`)

사용자가 한참 동안 응답이 없을 때 기본값을 설정하고 넘어가는 용도로 씁니다.

```bash
read -t 5 -p "5초 안에 응답하세요 (Y/N): " ANSWER
if [ -z "$ANSWER" ]; then
    echo -e "\\n시간 초과로 기본값 'N'으로 진행합니다."
fi
```

## 4. 여러 변수 한꺼번에 받기

공백을 기준으로 여러 데이터를 동시에 입력받을 수도 있습니다.

```bash
echo "성(First name)과 이름(Last name)을 입력하세요:"
read FIRST LAST
echo "성: $FIRST / 이름: $LAST"
```

## 5. 입력받은 내용 검증하기 (응용)

앞서 배운 **`case` 문**과 결합하면 사용자 선택에 따른 분기 처리를 완벽하게 할 수 있습니다.

```bash
#!/bin/bash

read -n 1 -p "진행하시겠습니까? (y/n): " choice
echo "" # 줄바꿈용

case $choice in
    y|Y)
        echo "작업을 시작합니다."
        ;;
    n|N)
        echo "작업을 중단합니다."
        exit 0
        ;;
    *)
        echo "잘못된 입력입니다."
        ;;
esac
```

### 💡 요약

- **`read 변수명`**: 입력받아 변수에 저장.
- **`read -p "문구" 변수명`**: 안내 메시지와 함께 입력받기.
- **`read -s`**: 입력 가리기 (비밀번호용).

### while loop로 WORD piping하기

- 내용을 인쇄하는대신, while루프로 한번에 하나씩 행을 살펴볼수있습니다.

```bash
cat courses.csv | while read MAJOR COURSE
do
  <STATEMENTS>
done
```

- 각각의 새로운 라인은 MAJOR 및 COURSE 변수로 읽혀질것입니다.
- echo를 사용해서 MAJOR변수를 인쇄하세요

```bash
#!/bin/bash
#Script to insert data from courses.csv and students.csv into students database
cat courses.csv | while read MAJOR COURSE
do
  echo $MAJOR
done
```

```bash
codeally@cabf2a134080:~/project$ ./insert_data.sh
major,course
Database
Web
Database
Data
Network
Database
Data
Network
Computer
Database
Game
Data
Computer
System
Game
Web
Data
Web
Game
System
Game
System
System
Computer
Computer
Network
Web
Network
```

- MAJOR변수는 첫번째 단어로만 설정이 됩니다.


# STDIN STDOUT STDERR과 리다이렉션 및 PIPING
## 표준 출력
- stdout은 a command is successful을 의미한다.
- stderr는 a command is not successful을 의미한다.

기본적으로 2가지모두 터미널에 출력된다.
- 유효한 명령어가 아닌 것을 입력하면 해당명령어는 stderr를 출력한다.

## STDOUT과 덮어쓰기 > 및 APPEND >>

### overwrite >
`command > filename` 명령문의 실행결과를 터미널에 출력하는 대신 파일에 쓸수있다. (redirect the output to the file)
- 파일이 없으면 새로 생성해준다. 
- 파일이 있고 내용이 있으면 덮어써버린다. 
```
codeally@32b0080048c9:~/project$ echo hello bash hello bash codeally@32b0080048c9:~/project$ echo hello bash > stdout.txt
```
```
#stdout.txt
hello bash
```
- echo문을 터미널에 출력하는 대신에, output을 file로 redirect 시킨다

```
codeally@32b0080048c9:~/project$ > stdout.txt
```
command를 생략하고 쓰면 파일을 텅텅 비운다. 

### append >>
명령문의 실행결과를 터미널에 출력하는 대신 파일에 추가한다. append to the file
```
codeally@32b0080048c9:~/project$ echo hello bash >> stdout.txt
```

## STDERR와 > 및 >>

### 덮어쓰기 - Stderr는 >로 안된다
```
codeally@32b0080048c9:~/project$ bad_command > stderr.txt
bash: bad_command: command not found
```
- 잘못된 명령을 입력하니, redirect되지않고 그대로 터미널에 출력된다.
- stderr.txt파일은 생성되었지만 내용은 아무것도 없다.

### 덮어쓰기 - stderr는 `2>` 로 해야함
```
codeally@32b0080048c9:~/project$ bad_command 2> stderr.txt
codeally@32b0080048c9:~/project$
```
- 결과가 터미널에 출력되지않고, file로 redirect된다.

STDOUT은 `1>` 로도 할수있다. 
```
codeally@32b0080048c9:~/project$ echo hello bash 1> stdout.txt
codeally@32b0080048c9:~/project$
```
- 1은 기본값으로 생략해도 가능



## 표준입력
stdin은 command가 사용할수 있는 3번째 항목이며, 입력을 받기 위한것입니다.
기본값은 키보드입니다.

stdin을 사용하는 명령어로는 예를들면 read 명령어가 있습니다.

```
codeally@32b0080048c9:~/project$ read NAME


```
read명령어는 stdin(입력-기본값 키보드) 을 쳐다보고 있습니다.

즉, read명령어는 stdin에서 키보드를 가리키는 입력을 받을 위치를 찾고있습니다.

이름을 입력하세요
```
codeally@32b0080048c9:~/project$ read NAME 
jay 
codeally@32b0080048c9:~/project$
```

이름을 출력해 봅시다. 
```
codeally@32b0080048c9:~/project$ echo $NAME 
jay 
codeally@32b0080048c9:~/project$
```

## 표준입력과 Redirection <
키보드로 일일이 입력하는 대신, 파일을 stdin으로 입력하는 방법
```
codeally@32b0080048c9:~/project$ read NAME < name.txt
codeally@32b0080048c9:~/project$ echo $NAME
freeCodeCamp
```

- 아래 명령어는 read명령을 사용하여 stdin을 redirect하여 name.txt의 내용을 NAME변수에 할당합니다.

## 예제 - 표준출력 표준에러러
```
#!/bin/bash
read NAME; #stdin 입력을 받아서 NAME변수에 저장
echo "Hello $NAME" #입력받은 변수를 stdout으로 출력
bad_command #stderr 출력
```

스크립트 실행 - ` ./script.sh`
```
#스크립트실행
codeally@f738086bfdad:~/project$ ./script.sh
#read command가 인풋을 기다리고있다. 
jay #jay입력
Hello jay 
./script.sh: line 4: bad_command: command not found
codeally@f738086bfdad:~/project$
```

스크립트 실행 - `echo zz | ./script.sh`
piping을 통해 인풋을 전달
```
codeally@f738086bfdad:~/project$ echo zz | ./script.sh
Hello zz
./script.sh: line 4: bad_command: command not found
```

스크립트 실행 - `echo zz | ./script.sh 2> stderr.txt`
- piping을 통해 input전달
- stderr를 stderr로 redirection
```
codeally@f738086bfdad:~/project$ echo zz | ./script.sh 2> stderr.txt
Hello zz
codeally@f738086bfdad:~/project$
```

스크립트 실행 `echo jap | ./script.sh 2> stderr.txt > stdout.txt`
- piping을 통해 input전달
- stderr를 stderr로 redirection
- stdout을 stdout.txt로 전달
```
Hello jap
```

## 예제 - 표준입력
스크립트 실행 `./script.sh < name.txt`
- 입력으로 다른 파일을 사용
```

codeally@5d79d93723c9:~/project$ ./script.sh < name.txt
Hello freeCodeCamp
./script.sh: line 5: bad_command: command not found
```

스크립트 실행 - `./script.sh < name.txt 2> stderr.txt > stdout.txt`
- 입력으로 다른 파일받아서 사용
- 표준에러와 표준출력 리다이렉션
```
codeally@5d79d93723c9:~/project$ ./script.sh < name.txt 2> stderr.txt > stdout.txt
codeally@5d79d93723c9:~/project$
```

## piping
명령어들을 진정으로 강력하게 만드는 것이 바로 파이프(|) 기호입니다. 파이프는 앞 명령어의 결과물을 뒤 명령어의 입력으로 바로 넘겨줍니다.
즉, 한명령어의 stdout을 다른명령어의 stdin으로 사용

### command의 stdout을 |를 통해 stdin으로 사용할수 없는경우
당신의 이름 을 echo하고, output을 pipe해서, read명령으로 보내어, 당신의 이름을 NAME 변수 값으로 할당해보세요.
```

codeally@32b0080048c9:~/project$ echo jay | read NAME
codeally@32b0080048c9:~/project$
```

```

codeally@32b0080048c9:~/project$ echo $NAME
freeCodeCamp

```
- 결과가 이상합니다.

 🏠 왜 변수 할당이 안 될까요? (부모와 자식 이야기)

터미널에서 `|` (파이프)를 사용하는 순간, 컴퓨터는 **새로운 자식 프로세스(Subshell)**를 하나 만듭니다. 이걸 가족 관계에 비유하면 이해가 빠릅니다.

1. **현재 터미널 (부모):** 당신이 지금 명령어를 치고 있는 메인 공간입니다.
2. **파이프 이후 공간 (자식):** `| read NAME`이 실행되는 순간, 부모는 자식에게 일을 시키기 위해 **새로운 방(Subshell)**을 만들어 보냅니다.
3. **문제의 핵심:** 자식은 그 방 안에서 `NAME="Gemini"`라고 벽에 적어두었지만, **그 방은 일이 끝나자마자 파괴됩니다.**
4. **결과:** 부모 방으로 돌아오면 자식이 적어둔 벽지가 없으니, 부모는 예전에 가지고 있던 값(혹은 빈 값)을 그대로 보여주게 됩니다.


### 해결 방법: "내 방에서 직접 처리하기"

파이프를 쓰지 않고 현재 쉘(부모 방)에서 변수를 직접 할당하는 방법들이 있습니다.

1. Here-String (`<<<`) 사용하기 (추천)

가장 깔끔한 방법입니다. 파이프를 만들지 않고 문자열을 직접 `read`로 밀어 넣습니다.

Bash

```
read NAME <<< "Gemini"
echo $NAME
# 결과: Gemini
```

 2. Command Substitution (`$()`) 사용하기

명령어의 실행 결과를 변수에 바로 담는 방식입니다.

Bash

```
NAME=$(echo "Gemini")
echo $NAME
# 결과: Gemini
```

 3. Process Substitution (`< <()`) 사용하기

조금 복잡해 보이지만, 파이프처럼 동작하면서도 현재 쉘에서 변수를 할당할 수 있게 해줍니다.

Bash

```
read NAME < <(echo "Gemini")
echo $NAME
# 결과: Gemini
```

---

요약

> `|`를 쓰면 **"임시로 만든 옆방"**에서 변수를 만들기 때문에, 내 방으로 돌아오면 그 변수는 사라지고 없습니다! 현재 내 방에서 값을 바로 받으려면 `<<<`나 `$()`를 사용하세요.

혹시 이 외에 **쉘 스크립트 작성 중에 막히는 다른 문법**이나 **환경 변수 설정**에 대해 더 궁금한 점이 있으신가요? 도와드릴 준비가 되어 있습니다!

## `>, >>, <, << , <<<, >>>, <<(), ` 차이점
### 출력 리다이렉션
- `>`**Overwrite**파일의 기존 내용을 싹 지우고 새 결과로 덮어씁니다.
- `>>`**Append**기존 내용은 유지하고, 파일 맨 뒷부분에 결과를 이어 붙입니다
### 입력 리다이렉션 (파일/값을 명령어로)
- `<`**Input**파일의 내용을 읽어서 명령어에게 전달합니다.
- `<<`**Here-Doc**특정 종료 문자가 나올 때까지 여러 줄의 텍스트를 한꺼번에 입력합니다.
- `<<<`**Here-String**아주 짧은 문자열 하나를 명령어의 입력으로 바로 던져줍니다.

### 프로세스리다이렉션
이것은 앞서 질문하신 **"파이프(`|`)의 Subshell 문제"**를 해결할 때 자주 쓰이는 고난도 기호입니다.

- **작동 방식:** `()` 안의 명령어 실행 결과를 마치 **임시 파일**처럼 만든 뒤, 그 파일의 경로를 바깥 명령어에게 넘겨줍니다.
- **왜 쓸까?** 파이프(`|`)와 달리 **현재 쉘**의 컨텍스트를 유지하면서 다른 명령어의 출력을 입력으로 받고 싶을 때 사용합니다.
- **예시:** `read NAME < <(echo "Gemini")`
    - `echo "Gemini"`의 출력을 임시 파일처럼 만들고, `<`를 통해 현재 쉘의 `read` 명령어가 읽게 합니다.


| **기호**  | **데이터 방향**  | **대상**  | **특징**              |
| ------- | ----------- | ------- | ------------------- |
| `>`     | `명령어 → 파일`  | 파일      | 덮어쓰기 (새로 만들기)       |
| `>>`    | `명령어 → 파일`  | 파일      | 이어붙이기               |
| `<`     | `파일 → 명령어`  | 파일      | 파일 내용 읽기            |
| `<<`    | `키보드 → 명령어` | 텍스트 블록  | 스크립트 내 여러 줄 입력 시 유용 |
| `<<<`   | `문자열 → 명령어` | 단순 문자열  | 변수에 값 넣을 때 가장 편함    |
| `< <()` | `명령어 → 명령어` | 프로세스 결과 | 현재 쉘의 변수 값을 바꿀 수 있음 |

## 파이프와 리다이렉션의 차이점
가장 많이 헷갈리는 부분입니다.

- **리다이렉션 (`>`, `<`)**: 명령어와 **파일** 사이의 통로를 만듭니다.
- **파이프 (`|`)**: 명령어와 **명령어** 사이의 통로를 만듭니다. (앞 명령어의 출력이 뒤 명령어의 입력이 됨)
```
ls -l | grep ".txt" > result.txt
# ls의 결과를 grep에 넘기고, 최종 결과만 파일에 저장
```


## 실무에서 자주쓰는 활용법
### ① 출력과 에러를 각각 다른 파일에 저장

Bash

`./script.sh > success.log 2> fail.log`

### ② 표준 에러를 표준 출력으로 합치기 (`2>&1`)

에러 메시지도 정상 출력처럼 취급하여 파이프로 넘기거나 파일에 저장할 때 씁니다.

`# 에러까지 포함해서 모두 log.txt에 저장` 
`./myscript.sh > log.txt 2>&1`


## 리다이렉션 정리
|   |   |   |
|---|---|---|
|**기호**|**의미**|**용도**|
|`**>**`|표준 출력 리다이렉션|파일 새로 만들기/덮어쓰기|
|`**>>**`|표준 출력 추가|파일 끝에 내용 이어 붙이기|
|`**<**`|표준 입력 리다이렉션|파일 내용을 명령어 입력으로 사용|
|`**2>**`|표준 에러 리다이렉션|에러 메시지만 따로 저장|
|`**&>**`|모든 출력 리다이렉션|정상 출력과 에러를 한곳에 저장|
# CONDITIONAL EXECUTION


```bash
git commit && git push
git commit || echo "Commit failed"
```


# STRING 입력


```bash
NAME="John"
echo "Hi $NAME"  #=> Hi John
echo 'Hi $NAME'  #=> Hi $NAME
```


# 나머지 연산자 활용
### modulus 연산자 `%`

원하는 범위로 출력

- 예로 RANDOM환경변수는 0~32767 무작위 숫자 생성
- `%` 연산자를 통해서 RANDOM변수가 0~74사이의 값을 출력하도록설정

```bash
NUMBER=$RANDOM%75
```

그러나 이렇게 하면 `0~32767난수`+ `%75` 가 그대로 출력

이렇게 되지않게 하려면 이중괄호를 활용해서 연산후에 값을 할당해야합니다.

```bash
NUMBER=$(( RANDOM % 75  ))
```


# 실무사용할법한 예제들

## 실무 스크립트 작성 시 팁

- **로그 남기기:** `echo "메시지" >> script.log`를 생활화하세요. 나중에 스크립트가 왜 실패했는지 알 수 있는 유일한 단서입니다.
- **절대 경로 사용:** 스크립트가 어디서 실행될지 모르므로 `ls` 보다는 `/bin/ls`처럼 쓰거나, 상단에 `PATH`를 지정하는 것이 안전합니다.
- **주석 작성:** `${ }`나 `awk` 명령어가 복잡할수록 주석을 달아두어야 미래의 본인이 고생하지 않습니다.

## 서버 상태 점검 및 알림 스크립트
서버의 디스크 사용량을 체크하여 특정 수치(예: 80%)가 넘으면 경고 메시지를 출력하거나 로그를 남기는 스크립트입니다.

```bash
#!/bin/bash

# 점검할 임계치 (%)
THRESHOLD=80
LOG_FILE="/var/log/disk_usage.log"

# df 명령어로 디스크 사용량 추출 (awk 활용)
# $5는 사용률(%), sed로 % 기호를 제거하여 숫자로 만듦
usage=$(df -h / | grep '/' | awk '{print $5}' | sed 's/%//')

if [ "$usage" -gt "$THRESHOLD" ]; then
    echo "$(date): [경고] 디스크 사용량이 ${usage}%에 도달했습니다!" >> "$LOG_FILE"
    # 실제 실무에서는 여기서 메일 발송(mail) 명령어를 추가하기도 합니다.
else
    echo "$(date): [정상] 현재 사용량 ${usage}%"
fi
```


## 웹서버 로그분석
Apache나 Nginx 같은 웹 서버의 로그 파일에서 '404 에러'를 가장 많이 일으킨 상위 5개 IP 주소를 뽑아내는 스크립트입니다.

```bash

#!/bin/bash

LOG_PATH="/var/log/nginx/access.log"

if [[ ! -f $LOG_PATH ]]; then
    echo "로그 파일을 찾을 수 없습니다: $LOG_PATH"
    exit 1
fi

echo "--- 404 에러를 유발한 상위 5개 IP ---"

# 1. awk로 IP($1)와 상태코드($9) 추출
# 2. 404 코드만 필터링
# 3. 정렬 후 중복 카운트
# 4. 숫자 순으로 역정렬하여 상위 5개 출력
awk '$9 == 404 {print $1}' "$LOG_PATH" | sort | uniq -c | sort -nr | head -n 5
```


## 일괄 파일 백업 및 정리 (Batch Job)

특정 폴더의 파일들을 백업 폴더로 복사하고, 7일이 지난 오래된 백업은 자동으로 삭제하는 스크립트입니다.

```bash

#!/bin/bash

SOURCE_DIR="/home/user/workspace"
BACKUP_DIR="/mnt/backup/$(date +%Y%m%d)"
OLD_DAYS=7

# 백업 폴더 생성
mkdir -p "$BACKUP_DIR"

# 1. 파일 복사
echo "백업 시작: $SOURCE_DIR -> $BACKUP_DIR"
cp -r "$SOURCE_DIR"/* "$BACKUP_DIR"

# 2. 결과 확인 ($? 활용)
if [ $? -eq 0 ]; then
    echo "백업 성공!"
else
    echo "백업 실패!"
    exit 1
fi

# 3. 7일 지난 오래된 백업 삭제 (find 명령어 활용)
# /mnt/backup 폴더 내에서 7일 이상 된 디렉토리를 찾아 삭제
find /mnt/backup -type d -mtime +$OLD_DAYS -exec rm -rf {} \;
echo "오래된 백업 정리 완료."
```