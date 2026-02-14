
# WSL 환경에서 설치

WSL(Windows Subsystem for Linux) 환경에서 Node.js를 설정할 때 가장 권장되는 방법은 **NVM(Node Version Manager)** 을 사용하는 것입니다. 
윈도우용 설치 파일을 쓰는 대신 NVM을 사용해야 나중에 권한 문제(sudo) 없이 여러 버전을 자유롭게 바꿀 수 있습니다.

### NVM(Node Version Manager) 설치

터미널(Ubuntu 등)을 열고 아래 명령어를 입력하여 NVM 설치 스크립트를 다운로드하고 실행합니다.


```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

설치가 끝나면 변경 사항을 적용하기 위해 터미널을 완전히 껐다 켜거나 아래 명령어를 입력하세요.


```
source ~/.bashrc
```




### Node.js 설치 및 확인

이제 NVM을 통해 원하는 버전의 Node.js를 설치합니다. 실무에서는 안정적인 **LTS(Long Term Support)** 버전을 주로 사용합니다.

- **최신 LTS 버전 설치:**
    
    ```
    nvm install --lts
    ```
    
- **설치 확인 (버전 체크):**
```
    node -v  # v20.x.x 와 같은 결과 출력
    npm -v   # npm 버전 출력
```

### 일반설치 설치방식 sudo apt install nodejs의 문제점

일반적인 설치 방식 (`sudo apt install nodejs`)

- **설치 경로:** `/usr/local/bin` 또는 `/usr/bin`
    
- **권한 문제:** 이 경로는 **시스템 관리자(root)**만 수정할 수 있는 공용 영역입니다.
    
- **결과:** 글로벌 패키지(`npm install -g`)를 설치할 때 시스템 영역에 파일을 써야 하므로 반드시 `sudo` 권한이 필요합니다. 만약 권한 없이 실행하면 `EACCES` 에러가 발생합니다.

### NVM을 통한 설치 방식

- **설치 경로:** `~/.nvm/versions/node/v20.x.x/bin` (사용자 홈 디렉토리)
    
- **권한 문제:** 홈 디렉토리(`~`)는 현재 로그인한 **사용자 본인**이 모든 권한을 가진 개인 영역입니다.
    
- **결과:** 내 방에 가구를 들여놓는 것과 같아서 관리자의 허가(`sudo`) 없이도 마음껏 패키지를 설치하거나 삭제할 수 있습니다.

### sudo를 쓰지않는것이 안전한 이유

왜 `sudo`를 쓰지 않는 것이 "안전"한가요?

실무에서 `npm install -g` 시 `sudo`를 남발하면 다음과 같은 위험이 있습니다.

1. **보안 위험:** npm 패키지 중에는 악성 스크립트가 포함된 경우가 있을 수 있습니다. `sudo`로 설치하면 이 스크립트가 내 시스템 전체(root)를 장악할 권한을 갖게 됩니다.
    
2. **권한 꼬임:** 특정 패키지를 `sudo`로 설치하면 그 과정에서 생성된 파일들의 소유권이 `root`로 바뀝니다. 나중에 일반 사용자로 실행하려고 할 때 "권한 없음" 오류가 계속 발생하여 개발 환경이 엉망이 될 수 있습니다.
    
3. **시스템 안정성:** 시스템 기본 경로의 파일을 건드리지 않으므로, Node.js 설정이 OS 자체의 중요 파일과 충돌할 일이 없습니다.

### NVM 작동원리
NVM은 Node.js를 사용자의 홈 디렉토리에 몰래(?) 설치한 뒤, 터미널이 실행될 때 **환경 변수(`PATH`)**를 조작하여 "시스템 기본 Node 대신 내 폴더에 있는 Node를 먼저 써라!"라고 지시합니다.

- **관리자 영역:** `/usr/bin` (건드리지 않음)
- **사용자 영역:** `~/.nvm` (여기서 모든 작업 발생)

```
which node
# 결과 예시: /home/사용자명/.nvm/versions/node/v20.11.0/bin/node
```
### 실무 효율을 높이는 추가 설정
💡 글로벌 패키지 설치 위치 해결

NVM을 쓰면 자동으로 사용자 폴더에 설치되므로, `npm install -g` 시 `sudo`를 붙이지 않아도 되어 매우 안전하고 편리합니다.

💡 패키지 매니저 pnpm 또는 yarn (선택)

최근 실무에서는 `npm`보다 빠르고 저장 공간을 아껴주는 `pnpm`을 많이 씁니다.

# VS CODE 연동
WSL에서 작업할 때는 윈도우의 VS Code를 그대로 사용하되, **'WSL' 확장 프로그램**을 설치해야 합니다.

1. VS Code 실행 후 왼쪽 확장(Extensions) 탭에서 **WSL** 검색 후 설치.
2. 터미널에서 프로젝트 폴더로 이동 후 `code .` 입력.
3. VS Code 하단 바에 **WSL: Ubuntu**라고 뜨면 성공입니다.

### ⚠️ 주의사항

- **파일 저장 위치:** 소스 코드는 반드시 WSL 내부 경로(예: `~/projects/...`)에 두세요. 윈도우 경로(`/mnt/c/...`)에서 작업하면 파일 감시 기능이나 성능이 매우 떨어집니다.
    
- **Vim 유저라면:** 앞서 설정한 `.vimrc`에 Javascript 문법 강조 플러그인을 추가하면 WSL 환경에서 환상적인 개발 환경이 완성됩니다!


## hello world

```
console.log("hello node world");
```

```
node helloworld.js
hello node world
```







# REACT 프로젝트 생성 - pnpm create vite

`create-react-app`(CRA)은 이제 구식(Legacy) 방식

npx create-react-app my-app (전통적 방식)
- - **도구**: Webpack 기반의 리액트 공식 초기화 도구입니다.
    
- **작동 방식**: `npx`를 사용해 일회성으로 도구를 내려받아 실행합니다.
    
- **특징**:
    
    - 모든 설정을 내부에 숨겨두어 초보자가 쓰기 편하지만, 수정이 어렵습니다.
        
    - **치명적 단점**: Webpack 기반이라 프로젝트가 커질수록 빌드와 수정 반영(HMR) 속도가 매우 느려집니다. 현재는 리액트 공식 문서에서도 이 방식보다 Vite나 Next.js를 권장합니다


pnpm create vite (최신 권장 방식) 🚀

- **도구**: 차세대 빌드 도구인 **Vite**를 사용합니다.
    
- **작동 방식**: `pnpm`의 효율적인 패키지 관리와 `Vite`의 압도적인 속도를 결합합니다.
    
- **특징**:
    
    - **속도**: `CRA`보다 개발 서버 구동이 최대 10~100배 빠릅니다.
        
    - **유연성**: 리액트뿐만 아니라 Vue, Svelte, Vanilla JS 등 다양한 템플릿을 선택할 수 있습니다.
        
    - **최신 표준**: 브라우저의 기본 기능을 활용하여 불필요한 번들링 과정을 생략합니다.


# NEXT JS 프로젝트 생성 - pnpm create next-app@latest

Next.js는 `create-next-app`이라는 공식 CLI 도구를 제공합니다. 터미널에서 아래 명령어를 실행하세요.

Bash

```
pnpm create next-app@latest
```

실행 후 나타나는 대화형 설정에서 다음과 같이 선택하는 것을 추천합니다 (대부분 엔터):

- **Project name:** 프로젝트 이름 (예: `my-next-app`)
    
- **TypeScript:** Yes (실무 표준)
    
- **ESLint:** Yes (코드 품질 관리)
    
- **Tailwind CSS:** Yes (스타일링 표준)
    
- **src/ directory:** Yes (코드 정리 용이)
    
- **App Router:** **Yes** (Next.js의 핵심이자 최신 방식)
    
- **Import alias:** Yes (`@/*` 경로 사용)

