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



## 편집

SHEBANG - 이파일을 어떤 쉘로 해석할지 지정
```
#!/bin/bash
```

