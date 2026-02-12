# NODEJS 설치

## WSL 환경에서 설치

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

# 패키지매니저란?

패키지 매니저란 컴퓨터에서 소프트웨어를 설치, 업데이트, 설정, 삭제하는 과정을 자동화해주는 **관리 도구**입니다.

패키지 매니저가 하는 일

단순히 파일을 내려받는 것을 넘어, 다음과 같은 복잡한 작업을 대신 처리합니다.

- **의존성 관리(Dependency Management):** 어떤 프로그램(A)을 실행하기 위해 다른 프로그램(B)이 꼭 필요할 때, 이를 감지하고 함께 설치해 줍니다.
    
- **버전 제어:** 소프트웨어의 특정 버전을 선택해 설치하거나, 최신 버전으로 안전하게 업데이트합니다.
    
- **보안 및 검증:** 공식 저장소(Repository)를 통해 배포되므로, 파일의 변조 여부를 확인하여 보안성을 높입니다.
    
- **환경 설정:** 소프트웨어가 실행될 수 있도록 경로(Path)를 지정하거나 필요한 설정을 자동으로 구성합니다.

# npm - 패키지 매니저

npm은 말 그대로 패키지를 **관리(Manage)**하는 도구입니다.

## 동작방식 - npm 저장소에서 영구적 download

`npm`은 이름 그대로 **관리(Manager)**에 집중합니다. 도서관에서 책을 빌려 내 책상에 꽂아두고 언제든 꺼내 보는 방식입니다.

- **저장소 검색:** 원격 npm 저장소(Registry)에서 해당 패키지를 찾습니다.
- **물리적 다운로드:** 패키지 파일 전체를 내 컴퓨터의 특정 경로(`node_modules` 또는 전역 폴더)에 **영구적으로 저장**합니다.
- **의존성 설치:** 해당 패키지가 실행되는 데 필요한 다른 라이브러리들도 함께 설치합니다.
- **사용:** 이후 사용자가 명령어를 입력하면, 내 컴퓨터에 저장된 파일을 찾아 실행합니다.
- **잔류:** 사용이 끝나도 파일은 삭제되지 않고 용량을 차지하며 그대로 남습니다

## npm 설치방법

만약 설치가 되어있다면, npm을 upgrade합니다. 
```
npm install -g npm
```

버전확인
```
npm -v
```


## npm project 생성 - npm init
```
npm init : 새프로젝트 생성 package.json파일 생성 
npm init -y : 질문들에대해서 기본값설정으로 초기화진행
```


## package.json
npm init을 하면 package.json파일 생성된다.  package.json파일을 통해 프로젝트의 package들을 관리한다. 

- name : 프로젝트이름 (기본값 현재폴더명) `name : vite-project`
- version : 기본값(1.0.0) `version : 1.0.0`
- description : 프로로젝트설명 `description : app for vite testing`
- entry point :  `main : index.js`
	- 여러분이 만든 코드 폴더 안에 파일이 100개 있다고 가정해 봅시다. 다른 사람이 이 폴더를 가져다 쓸 때, **"그래서 제일 먼저 실행해야 할 파일이 뭐야?"** 라고 물어보겠죠? 그때 "이 파일부터 읽으면 돼!"라고 답해주는 설정이 바로 진입점입니다.
	- 보통 `main`이라는 항목으로 표시합니다.
	- "누가 내 폴더를 불러오면 `index.js` 파일을 실행해줘." 라는 말은 , 다른 코드에서 `require('./내폴더')`라고만 써도, 컴퓨터가 알아서 `index.js`를 찾아 실행합니다.
	- 만약 내 시작 파일이 `app.js`라면, `main: "app.js"`라고 꼭 적어줘야 합니다.
	- **실행 버튼:** `npm start` 같은 명령어를 칠 때 컴퓨터가 어디서부터 코드를 읽기 시작할지 결정하는 기준이 됩니다.

- test command : 입력값 → `script :{test : “입력값 “ }
	- 한마디로 **'명령어 단축키'** 입니다. 복잡하고 긴 명령어를 매번 터미널에 치기 귀찮으니까, 짧은 별명을 지어주는 거라고 생각하면 쉬워요.
	- **원래 치던 말:** `node server.js` (서버 실행해줘)
	- 명령어 단축키 등록 : `"start": "node server.js"`
	- **사용법:** 터미널에 **`npm run start`** (또는 간단히 **`npm start`**)라고 치면 끝!
	- **기억하기 쉬워요:** 실제 명령어는 `nodemon --exec babel-node index.js`처럼 엄청 길고 복잡할 수 있습니다. 이걸 다 외울 순 없죠? 그냥 `"dev"`라고 별명을 붙여두면 **`npm run dev`** 한 줄로 실행됩니다.
	- **팀원들과 약속:** 새로 들어온 팀원이 "이 프로젝트 어떻게 실행해요?"라고 물어볼 필요가 없습니다. 그냥 `package.json`의 `scripts`만 보면 "아, `npm start` 치면 되는구나!" 하고 바로 알 수 있죠.
	- 테스트하기, 코드 예쁘게 정리하기, 배포하기 등 여러 단계를 하나의 단축키로 묶을 수 있습니다.

| **별명 (Key)** | **실제 하는 일**                      | **실행 명령어**      |
| ------------ | -------------------------------- | --------------- |
| **`start`**  | 실제 서비스를 시작할 때                    | `npm start`     |
| **`dev`**    | 개발하면서 코드 수정할 때 (보통 자동 재시작 기능 포함) | `npm run dev`   |
| **`test`**   | 코드가 잘 돌아가는지 검사할 때                | `npm run test`  |
| **`build`**  | 배포하기 위해 코드를 압축/변환할 때             | `npm run build` |
	- **주의!** `start`와 `test`는 워낙 자주 써서 `run`을 빼고 `npm start`라고만 쳐도 되지만, 그 외의 별명(dev, build 등)은 꼭 중간에 **`run`**을 넣어서 **`npm run dev`**라고 쳐야 합니다.


- repository : 입력값 → {type : git, url : 입력값} - git repository
    - git저장소가 생성되어있고 github 이 연결되어있다면 자동으로 입력
    
- keywords : 프로젝트키워드
    
- author : 저자
    
- license : ISC

## 패키지 설치 및 명령어들

`npm install` : packages.json파일에 dependencies에 명시된 패키지들을 설치
`npm install -production` : packages.json파일에 devDependencies 패키지를 제외하고 depencencies에 기록된 패키지만 설치한다.
`npm install 패키지이름@버전` : 프로젝트폴더의 node_modules에 파일을 설치한다.
- `npm root` 명령어를 통해서 node-modules폴더의 위치를 알수있다.

`npm install 패키지이름 -g` → 패키지를 global패키지에 설치 어디서든사용가능
    - 사용자명/appdata/roaming/npm/node_modules에 설치된며 컴퓨터 어디서든 사용가능하도록 설치된다.
    - 시스템의 node modules의 경로는 `npm root -g`를 통해서 찾을수 있다.
    - -g 플래그를 통해 설치한경우 package.json의 dependencies목록에 기록되지않는다.
`npm install -P 패키지명` / npm install 패키지명과 동일한 명령어다. (Production의 약자)
`npm install -S 패키지명` / SAVE의 약자, 패키지를 설치하고 dependencies에 추가하기위한 flag였다. npm@5버전이상부터는 -s붙이지 않아도 설치시 자동으로 dependencies에 추가가된다.
`npm install -D` 패키지명 : 패키지를 설치하고 devDependencies의 목록에 추가한다.
    - 프로젝트폴더의 node_modules에 설치합니다. 다만 패키지명을 devDependencies에 기록합니다. 이는 프로젝트를 구동할때는 필요치 않고 **개발단계에서만 필요한 프로젝트를 설치**할때 사용합니다.
`npm install 깃허브주소` → 특정저장소에있는 패키지설치 : 주로 깃허브에만 있는 패키지를 설치할때 사용


업데이트
`npm update` → 설치한 패키지를 전체 업데이트
`npm outdated` : 업데이트 가능한 패키지 있는지 체크
`npm update 패키지명` : 패키지를 최신버전으로 업데이트

패키지 삭제
`npm uninstall 패키지명` : 패키지를 삭제. (축약 버전 npm rm 패키지명)

기타
- `npm search 키워드` : npm 라이브러리 저장소에서 다운가능한 패키지검색
- `npm info 패키지명` : 패키지의 상세정보
- `npm ls` : 설치된 패키지의 구조를 볼수있다.
- `npm ll` : 설치된 패키지의 상세정보




# npx
npx는 패키지를 **실행(Execute)**하는 도구입니다. npm 5.2 버전부터 기본 포함되었습니다.

- **주요 역할:** 패키지를 내 컴퓨터에 영구적으로 설치하지 않고, **임시로 다운로드하여 실행한 뒤 바로 삭제**합니다.
- **작동 방식:** 실행하려는 도구가 내 컴퓨터에 있는지 확인한 후, 없으면 최신 버전을 내려받아 실행하고 끝냅니다.
- **사용 예시:**

- `npx create-react-app my-app`: 리액트 초기 설정 도구는 자주 쓰지 않으므로 설치 없이 바로 실행만 함.
- `npx http-server`: 현재 폴더를 웹 서버로 잠깐 띄우고 싶을 때 사용.

### 동작방식
`npx`는 **실행(Execute)**에 집중하는 도구입니다. 책을 사서 소장하는 게 아니라, 필요할 때만 도서관에서 잠시 빌려 읽고 바로 반납하는 방식입니다.

 💡 작동 프로세스 (예: `npx create-react-app my-app`)

1. **로컬 확인:** 먼저 내 컴퓨터에 해당 패키지가 이미 깔려 있는지 확인합니다.
2. **임시 다운로드:** 없다면, npm 저장소에서 패키지를 **임시 폴더(Cache)**에 다운로드합니다.
3. **즉시 실행:** 다운로드가 완료되는 즉시 메모리상에서 패키지의 실행 파일을 구동합니다.
4. **자동 삭제:** 실행이 종료되면, 임시로 받았던 패키지 파일들을 **자동으로 삭제(Clean up)** 합니다. (정확히는 캐시를 비우거나 무시합니다.)


### npm vs npx
|**특징**|**npm**|**npx**|
|---|---|---|
|**설치 여부**|`node_modules`에 영구 저장|실행 후 사라짐 (일회성)|
|**저장 공간**|디스크 용량을 계속 차지함|용량을 거의 차지하지 않음|
|**버전 관리**|특정 버전을 고정해서 설치함|항상 **최신 버전**을 가져와 실행함|
|**사용 목적**|프로젝트 의존성(라이브러리) 관리|일회성 도구 실행, 실행 파일 호출|


---

|**구분**|**npm (Install)**|**npx (Execute)**|
|---|---|---|
|**저장 위치**|로컬 프로젝트 폴더 또는 전역(Global)|임시 캐시 공간|
|**용량 관리**|계속 쌓이므로 주기적인 삭제 필요|실행 후 사라지므로 용량 부담 없음|
|**버전 관리**|설치 당시 버전이 고정됨|항상 **최신 버전**을 가져와 실행 가능|
|**주 사용처**|자주 쓰는 라이브러리 (React, Express 등)|한 번만 실행할 도구 (보일러플레이트 생성 등)|
# 패키지 프로젝트에설치 vs 전역에 설치
패키지를 **내 프로젝트에 설치(Local)**할지, **시스템 전역에 설치(Global)**할지는 해당 패키지를 '코드의 부품'으로 쓸 것인지 '독립된 도구'로 쓸 것인지에 따라 결정됩니다.

### 1. 내 프로젝트에 설치 (Local Install)

가장 권장되는 방식이며, 실무 프로젝트의 99%는 이 방식을 사용합니다.

- **명령어:** `npm install <패키지명>`
    
- **저장 위치:** 현재 폴더의 `node_modules/` 폴더 안.
    
- **특징:**
    
    - **프로젝트별 독립성:** A 프로젝트는 React v17, B 프로젝트는 React v18을 써도 서로 간섭이 없습니다.
        
    - **공유 용이성:** `package.json`에 기록되므로, 다른 개발자가 내 코드를 받았을 때 `npm install`만 치면 똑같은 환경이 구축됩니다.
        
- **주요 대상:** `express`, `react`, `lodash`, `axios` 등 내 코드에서 `import`하여 사용하는 라이브러리.
    

---

### 2. 시스템 전역에 설치 (Global Install)

내 컴퓨터 어디에서나 명령어로 실행하고 싶은 도구를 설치할 때 사용합니다.

- **명령어:** `npm install -g <패키지명>`
    
- **저장 위치:** (NVM 사용 시) `~/.nvm/versions/node/.../lib/node_modules`.
    
- **특징:**
    
    - **시스템 도구화:** 터미널 어디에서나 해당 이름을 명령어로 바로 실행할 수 있습니다.
        
    - **코드에서 사용 불가:** 전역 설치된 패키지는 내 코드에서 `require`나 `import`로 불러올 수 없습니다.
        
- **주요 대상:** `nodemon`(자동 재시작 도구), `typescript`(컴파일러), `pm2`(프로세스 관리자) 등 개발 편의 도구.

비교 요약

|**구분**|**프로젝트 내 설치 (Local)**|**시스템 전역 설치 (Global)**|
|---|---|---|
|**명령어**|`npm install`|`npm install -g`|
|**관리 파일**|`package.json`에 기록됨|기록 안 됨 (컴퓨터에 귀속)|
|**사용 방식**|코드 내부에서 `import`|터미널에서 명령어(`CLI`)로 실행|
|**공동 작업**|팀원과 동일한 버전 유지 가능|팀원마다 직접 설치해야 함|
💡 실무자의 선택 기준

**"웬만하면 로컬(프로젝트 내)에 설치하세요."**

전역 설치를 하면 시간이 지나면서 내 컴퓨터의 패키지 버전들이 꼬이거나, "내 컴퓨터에선 되는데 왜 네 컴퓨터에선 안 돼?"라는 문제가 발생하기 쉽습니다.

- **팁:** 전역 도구가 필요할 때는 전역 설치 대신 앞에서 배운 **`npx`**를 사용하면 설치 없이 필요할 때만 최신 버전 도구를 쓸 수 있어 더 안전합니다.

# pnpm - 진화된 패키지 매니저
pnpm은 **performant npm**의 약자로, npm과 yarn의 고질적인 문제인 **속도와 중복 저장 공간** 문제를 해결한 도구입니다.

핵심 특징

- **저장 공간 절약 (Content-addressable storage):** 프로젝트가 100개라도 동일한 버전의 라이브러리는 내 컴퓨터에 **딱 한 번만 저장**됩니다. 나머지는 하드 링크(Hard link)로 연결하여 용량을 거의 차지하지 않습니다.
    
- **압도적인 속도:** 이미 설치된 패키지를 재사용하기 때문에 `install` 속도가 npm보다 훨씬 빠릅니다.
    
- **엄격한 의존성 관리:** 내가 설치하지 않은 "유령 의존성(Phantom dependencies)"을 코드에서 몰래 사용하는 것을 방지하여 예기치 못한 에러를 차단합니다.


pnpm은 하드 링크와 심볼릭 링크를 사용하여 디스크 공간을 절약하고 설치 속도를 높입니다. 모든 패키지는 `~/.pnpm-store`에 한 번만 저장되며, 프로젝트에서는 이를 링크로 참조합니다.

## 설치 
```
# NVM 환경에서 전역 설치
npm install -g pnpm

# 설치확인
pnpm -v

```


## 사용법

|**기능**|**npm 명령어**|**pnpm 명령어**|
|---|---|---|
|**의존성 설치** (전체)|`npm install`|**`pnpm install`** (또는 `pnpm i`)|
|**패키지 추가**|`npm install <pkg>`|**`pnpm add <pkg>`**|
|**개발용 패키지 추가**|`npm install -D <pkg>`|**`pnpm add -D <pkg>`**|
|**패키지 제거**|`npm uninstall <pkg>`|**`pnpm remove <pkg>`**|
|**스크립트 실행**|`npm run <script>`|**`pnpm <script>`** (`run` 생략 가능)|
|**전역 설치**|`npm install -g <pkg>`|**`pnpm add -g <pkg>`**|

## 주요 명령어

**패키지 설치**

bash

```bash
pnpm install              # package.json의 모든 의존성 설치
pnpm add <패키지>         # 프로덕션 의존성 추가
pnpm add -D <패키지>      # 개발 의존성 추가
pnpm add -g <패키지>      # 전역 설치
```

**패키지 제거**

bash

```bash
pnpm remove <패키지>      # 패키지 제거
pnpm remove -g <패키지>   # 전역 패키지 제거
```

**스크립트 실행**

bash

```bash
pnpm run <스크립트>       # package.json의 스크립트 실행
pnpm start               # npm start와 동일
pnpm test                # npm test와 동일
```

**기타 유용한 명령어**

bash

```bash
pnpm update              # 패키지 업데이트
pnpm list                # 설치된 패키지 목록
pnpm outdated            # 업데이트 가능한 패키지 확인
pnpm store prune         # 사용하지 않는 패키지 정리
```

## npx 대신 사용하는 `pnpm dlx`
앞서 배운 `npx`처럼 패키지를 설치하지 않고 일회성으로 실행하고 싶을 때는 `dlx`(Download and Execute) 명령어를 사용합니다.

```
pnpm dlx create-react-app my-app
```


## 기존 npm 프로젝트 전환하기 to pnpm
기존에 `npm`이나 `yarn`을 쓰던 프로젝트를 `pnpm`으로 바꾸고 싶다면 다음 순서를 따르세요.

1. **기존 파일 삭제:** 기존의 `node_modules`와 lock 파일(`package-lock.json` 또는 `yarn.lock`)을 삭제합니다.
    
    - _주의: `package.json`은 절대 지우면 안 됩니다!_
        
2. **pnpm으로 재설치:** 해당 폴더에서 아래 명령어를 입력합니다.
    
    Bash
    
    ```
    pnpm install
    ```
    
1. **결과:** `pnpm-lock.yaml` 파일이 생성되며, 훨씬 적은 용량을 차지하는 `node_modules`가 구성됩니다.

## 강력한 기능 - workspace
pnpm이 현업에서 사랑받는 가장 큰 이유 중 하나는 **'워크스페이스(Workspace)'** 기능입니다. 여러 개의 프로젝트를 하나의 폴더에서 관리할 때, 공통 라이브러리를 중복 없이 공유할 수 있습니다.
- `pnpm-workspace.yaml` 파일을 만듭니다.
    
- 각 프로젝트 폴더를 등록합니다.
    
- 프로젝트 A에서 프로젝트 B를 참조할 때, 실제 배포된 라이브러리처럼 자유롭게 연결해서 쓸 수 있습니다.

### 1. 왜 pnpm workspace를 쓰나요?

일반적인 멀티레포(프로젝트마다 저장소가 따로 있는 경우)와 비교했을 때 다음과 같은 강력한 장점이 있습니다.

- **중복 설치 방지:** A 프로젝트와 B 프로젝트가 같은 `lodash`를 쓴다면, pnpm의 전역 저장소 덕분에 용량을 거의 차지하지 않고 공유합니다.
    
- **로컬 패키지 참조:** 내가 만든 `shared-utils` 패키지를 npm에 배포하지 않고도, 같은 워크스페이스 내의 `web-app` 프로젝트에서 `import` 해서 쓸 수 있습니다.
    
- **한 번에 관리:** 모든 프로젝트의 의존성을 한 번의 명령(`pnpm install`)으로 설치하고 업데이트할 수 있습니다.
    

---

### 2. 핵심 파일: `pnpm-workspace.yaml`

워크스페이스를 설정하는 가장 중요한 파일입니다. 루트(최상단) 폴더에 위치하며, 어떤 폴더들을 워크스페이스로 관리할지 명시합니다.

YAML

```
# pnpm-workspace.yaml
packages:
  - 'apps/*'      # 실행 가능한 앱 (예: web, admin, mobile)
  - 'packages/*'  # 공유 라이브러리 (예: ui-kit, utils, types)
  - 'services/*'  # 백엔드 서비스 등
```

---

### 3. 내부 동작 원리: `workspace:` 프로토콜

pnpm workspace의 진가는 패키지 간의 연결 방식에 있습니다.

- **동작 방식:** `apps/web`에서 `packages/ui-kit`을 사용하고 싶다면, `package.json`에 다음과 같이 기록합니다.
    
    JSON
    
    ```
    "dependencies": {
      "ui-kit": "workspace:*"
    }
    ```
    
- **결과:** pnpm은 외부 npm 서버에서 `ui-kit`을 찾는 대신, **내 컴퓨터의 `packages/ui-kit` 폴더를 가리키는 심볼릭 링크**를 생성합니다. 덕분에 `ui-kit` 코드를 수정하면 `web` 앱에 즉시 반영됩니다.
    

---

### 4. 주요 명령어 사용법

워크스페이스에서는 "누가, 어디서" 명령을 실행하는지가 중요합니다.

|**명령어**|**설명**|
|---|---|
|**`pnpm install`**|워크스페이스 전체의 모든 의존성을 한 번에 설치합니다.|
|**`pnpm -F <name> <cmd>`**|**Filter**의 약자로, 특정 패키지(`name`)에서만 명령을 실행합니다.|
|**`pnpm -r <cmd>`**|**Recursive**의 약자로, 모든 패키지에서 해당 명령을 실행합니다. (예: `pnpm -r build`)|

> **예시:** `web` 프로젝트에만 `axios`를 설치하고 싶을 때:
> 
> `pnpm add axios --filter web`

---

### 5. 폴더 구조 예시

보통 다음과 같은 구조로 구성합니다.

Plaintext

```
my-monorepo/
├── pnpm-workspace.yaml
├── package.json
├── node_modules/
├── apps/
│   ├── web-project/ (React)
│   └── admin-project/ (Vue)
└── packages/
    ├── ui-components/ (공통 버튼, 입력창 등)
    └── utils/ (공통 함수들)
```

---

### 💡 실무 팁: 의존성 격리

pnpm은 `npm`과 달리 **유령 의존성**을 허용하지 않기 때문에, 워크스페이스 구조에서도 매우 안전합니다. `apps/web`에서 사용하고 싶은 라이브러리는 반드시 해당 폴더의 `package.json`에 명시되어야만 합니다. 루트에 설치되었다고 해서 자동으로 불러와지지 않습니다.

## workspace 설명2

pnpm workspace는 모노레포(monorepo)를 관리하기 위한 기능입니다. 여러 패키지를 하나의 저장소에서 효율적으로 관리할 수 있습니다.

### 설정 방법

**1. pnpm-workspace.yaml 파일 생성**

프로젝트 루트에 `pnpm-workspace.yaml` 파일을 만듭니다:

yaml

````yaml
packages:
  - 'packages/*'
  - 'apps/*'
  # 특정 패키지 제외
  - '!**/test/**'
```

**2. 디렉토리 구조 예시**
```
my-monorepo/
├── pnpm-workspace.yaml
├── package.json
├── packages/
│   ├── package-a/
│   │   └── package.json
│   └── package-b/
│       └── package.json
└── apps/
    └── web/
        └── package.json
````

### 주요 명령어

**전체 워크스페이스에서 설치**

bash

```bash
pnpm install                    # 모든 패키지의 의존성 설치
```

**특정 워크스페이스에서 명령 실행**

bash

```bash
pnpm --filter <패키지명> <명령어>
pnpm --filter package-a build
pnpm --filter web dev
```

**여러 패키지에서 동시 실행**

bash

```bash
pnpm -r run build              # 모든 패키지에서 build 실행
pnpm -r run test               # 모든 패키지에서 test 실행
pnpm --parallel -r run dev     # 병렬로 dev 실행
```

**의존성 추가**

bash

```bash
# 루트에 의존성 추가
pnpm add -w <패키지>

# 특정 워크스페이스에 추가
pnpm --filter package-a add lodash

# 워크스페이스 간 의존성 추가
pnpm --filter package-b add package-a@workspace:*
```

### 워크스페이스 프로토콜

패키지 간 의존성 설정 시 `workspace:` 프로토콜 사용:

json

```json
{
  "dependencies": {
    "package-a": "workspace:*",
    "package-b": "workspace:^1.0.0"
  }
}
```

- `workspace:*` - 현재 버전 사용
- `workspace:^` - semver 범위 사용

### 유용한 팁

**필터 패턴**

bash

```bash
pnpm --filter "./packages/**" build  # packages 폴더 전체
pnpm --filter "!package-a" test      # package-a 제외
pnpm --filter "...[origin/main]"     # 변경된 패키지만
```

**루트 package.json 예시**

json

```json
{
  "scripts": {
    "build": "pnpm -r run build",
    "dev": "pnpm --parallel -r run dev",
    "test": "pnpm -r run test"
  }
}
```

모노레포 구조나 특정 사용 사례에 대해 더 자세히 알고 싶으신가요?



## 💡 팁: 저장소(Store) 관리

pnpm은 모든 파일을 한곳에 모은다고 말씀드렸죠? 시간이 지나 안 쓰는 파일들이 쌓였다면 아래 명령어로 청소할 수 있습니다.

Bash

```
pnpm store prune
```

이 명령어를 실행하면 어떤 프로젝트에서도 참조하지 않는 '고립된 파일'들을 삭제하여 용량을 확보합니다.

지금 바로 테스트해 볼 **연습용 프로젝트**가 있으신가요? 없다면 `pnpm add lodash` 같은 간단한 명령어로 얼마나 빨리 설치되는지 직접 확인해 보시는 것을 추천드려요!

## 동작방식
pnpm의 핵심은 **"내 컴퓨터에 똑같은 라이브러리는 딱 하나만 저장한다"**는 철학에 있습니다. 이를 가능하게 하는 3가지 핵심 동작 방식을 설명해 드릴게요.

### 1. 콘텐츠 주소 지정 저장소 (Content-addressable Storage)

가장 큰 특징은 라이브러리를 프로젝트 폴더(`node_modules`)가 아닌, 컴퓨터 전체의 **단일 저장소(Global Store)**에 저장한다는 점입니다.

- **동작 방식:** 어떤 프로젝트에서든 같은 버전의 라이브러리를 사용하면, pnpm은 이를 다시 다운로드하지 않고 이미 저장된 파일을 가리키기만 합니다.
    
- **효율성:** 만약 100개의 리액트 프로젝트가 있다면, `npm`은 100번 다운로드하지만, `pnpm`은 단 1번만 저장합니다. 하드 디스크 용량을 획기적으로 아낄 수 있습니다.
    

---

### 2. 하드 링크 (Hard Link)와 심볼릭 링크 (Symbolic Link)

pnpm은 실제 파일을 복사하는 대신, 파일 시스템의 **'링크'** 기능을 사용합니다.

- **하드 링크:** 글로벌 저장소에 있는 실제 파일과 프로젝트의 `node_modules`를 물리적으로 연결합니다. 겉으로 보기엔 폴더 안에 파일이 있는 것 같지만, 실제로는 저장소의 파일을 바라보고 있는 것입니다.
    
- **심볼릭 링크:** 패키지 간의 복잡한 구조를 연결할 때 사용합니다.
    

---

### 3. 유령 의존성(Phantom Dependency) 방지

`npm`이나 `yarn v1`은 설치 속도를 높이기 위해 의존성 구조를 평평하게 만드는 '호이스팅(Hoisting)'을 수행합니다. 이 과정에서 내가 직접 설치하지 않은 라이브러리를 불러와 쓸 수 있는 보안 취약점(유령 의존성)이 생깁니다.

- **pnpm의 해결책:** pnpm은 `node_modules` 내부 구조를 엄격하게 관리합니다.
    
- 내가 `package.json`에 명시한 패키지만 최상단에 노출하고, 그 패키지가 사용하는 의존성들은 별도의 숨겨진 경로(`.pnpm` 폴더)에 링크로 격리해 둡니다.
    
- 결과적으로 **"내가 선언한 것만 쓸 수 있는"** 안전한 환경을 만듭니다.


## 사용예시
### 1단계: 프로젝트 초기화 및 워크스페이스 설정

먼저 전체 프로젝트를 담을 폴더를 만들고 pnpm 설정을 합니다.

Bash

```
mkdir pnpm-monorepo && cd pnpm-monorepo
pnpm init
```

그다음, 루트 폴더에 **`pnpm-workspace.yaml`** 파일을 만들고 아래 내용을 적어주세요.

YAML

```
packages:
  - 'apps/*'
  - 'packages/*'
```

---

### 2단계: 패키지(공유 라이브러리) 만들기

모든 프로젝트에서 공통으로 쓸 `utils` 패키지를 만듭니다.

Bash

```
mkdir -p packages/utils
cd packages/utils
pnpm init
```

`packages/utils/index.js` 파일을 만들고 간단한 함수를 넣습니다.

JavaScript

```
// packages/utils/index.js
export const sayHello = (name) => `Hello, ${name}! This is from shared utils.`;
```

---

### 3단계: 앱(App) 만들기

이제 위에서 만든 `utils`를 가져다 쓸 실제 앱을 만듭니다.

Bash

```
cd ../../ # 다시 루트 폴더로 이동
mkdir -p apps/web
cd apps/web
pnpm init
```

이제 **가장 중요한 연결 작업**입니다. `web` 앱에서 `utils`를 쓰겠다고 선언합니다.

Bash

```
pnpm add utils --filter web --workspace
```

> **참고:** `--workspace` 옵션은 외부 npm 저장소가 아닌 현재 워크스페이스 내에서 패키지를 찾으라는 뜻입니다.

---

### 4단계: 연결 확인하기

`apps/web/index.js` 파일을 만들고 공통 함수를 실행해 봅니다.

JavaScript

```
// apps/web/index.js
import { sayHello } from 'utils';

console.log(sayHello('Gemini'));
```

이제 루트 폴더로 돌아와서 `web` 앱을 실행해 보세요.

Bash

```
cd ../../
pnpm --filter web node apps/web/index.js
```

# 빌드툴 - 소스코드를 실행가능한 파일로 자동변환

개발자가 작성한 소스 코드를 컴퓨터나 브라우저가 실행할 수 있는 **최종 소프트웨어 결과물로 변환하는 과정을 자동화**해주는 도구입니다.

개발자가 코드만 짠다고 바로 프로그램이 돌아가는 것은 아닙니다. 코드를 검사하고, 합치고, 압축하고, 테스트하는 수많은 뒷작업이 필요한데 이를 빌드 툴이 대신해 줍니다.

우리가 작성한 최신 자바스크립트나 리액트 코드는 웹 브라우저가 바로 읽기에 너무 복잡하거나 무겁습니다. 
빌드 도구는 이 소스 코드를 **브라우저가 가장 좋아하는 형태(표준 JS, CSS, HTML)로 가공하고 묶어주는 '공장'** 역할을 합니다.

## 빌드툴의 주요역활


빌드 툴은 마치 **공장의 자동화 라인**처럼 다음과 같은 복잡한 공정을 처리합니다.

- **컴파일(Compile):** 사람이 읽기 쉬운 코드(Java, TypeScript 등)를 컴퓨터가 이해하는 언어로 번역합니다.
    
- **번들링(Bundling):** 수백 개의 파일로 쪼개진 소스 코드를 하나 혹은 몇 개의 파일로 묶어줍니다.
    
- **코드 압축(Minification):** 파일 용량을 줄이기 위해 공백을 제거하고 변수명을 짧게 바꿉니다.
    
- **리소스 최적화:** 이미지 크기를 줄이거나, CSS/JS 파일을 웹 브라우저가 빨리 읽을 수 있게 가공합니다.
    
- **테스트 자동화:** 코드가 작성된 대로 잘 작동하는지 테스트 코드를 실행합니다.


## 주요 빌드툴 
|**분야**|**주요 빌드 툴 및 번들러**|**특징**|
|---|---|---|
|**Frontend (JS/TS)**|**Webpack**, **Vite**, **esbuild**|브라우저 환경에 최적화된 파일(HTML, CSS, JS) 생성|
|**Java**|**Gradle**, **Maven**|대규모 프로젝트의 의존성 관리와 컴파일 특화|
|**Android**|**Gradle**|안드로이드 앱 설치 파일(APK) 생성|
|**C/C++**|**Make**, **CMake**|운영체제에 맞는 실행 파일 빌드|




## 웹팩 - 전통의 강자, 꼼꼼한 포장술" deprecated

웹팩은 오랫동안 업계 표준이었던 **번들러(Bundler)**입니다.

- **방식:** 모든 파일을 하나하나 분석해서 거대한 하나의 덩어리(Bundle)로 묶습니다.
- **장점:** 매우 세밀한 설정이 가능하고, 오래된 만큼 플러그인이 방대합니다.
- **단점:** 파일이 많아지면 분석하고 묶는 시간이 **너무 오래 걸립니다.** 개발 서버를 켤 때 커피 한 잔 마시고 와야 할 정도로 느려질 수 있습니다.



## vite "차세대 주자, 빛처럼 빠른 속도"

Vite는 프랑스어로 '빠르다'는 뜻입니다. 웹팩의 느린 속도를 해결하기 위해 등장했습니다.
- **방식:** 파일을 미리 묶지 않습니다! 브라우저가 "이 파일 줘"라고 요청할 때마다 **그 즉시 해당 파일만 보내줍니다.**
    
- **장점:**
    - **즉시 시작:** 서버를 켜자마자 바로 뜹니다.
    - **초고속 수정:** 코드 한 줄 바꾸면 화면에 0.1초 만에 반영됩니다(HMR).
- **단점:** 아주 구형 브라우저(IE 등) 지원 설정이 웹팩보다 까다로울 수 있습니다.

## 웹팩 vs vite
### 1. 개발 서버 기동 방식: "필요할 때만 가져온다"

기존의 빌드 툴(Webpack)과 Vite의 가장 큰 차이점은 **'언제 번들링을 하느냐'**입니다.

 Webpack (기존 방식)

- **방식:** 서버를 띄우기 전에 프로젝트의 모든 파일(수백~수천 개)을 하나로 합치는(Bundling) 과정을 거칩니다.
    
- **문제점:** 프로젝트 규모가 커질수록 이 '합치는 시간'이 길어져서, 명령어 입력 후 실제 화면이 뜨기까지 몇 분씩 걸리기도 합니다.
    

 Vite (최신 방식)

- **방식:** **Native ESM(ES Modules)**을 활용합니다. 서버를 띄울 때 번들링을 하지 않고 바로 실행합니다.
    
- **원리:** 브라우저가 "A 파일 줘!"라고 요청하면, Vite는 그 순간 그 파일만 가공해서 전달합니다. 즉, **브라우저가 번들러 역할의 일부를 대신 수행**하는 것입니다.
    

---

### 2. 의존성 사전 빌드 (Dependency Pre-bundling)

"그럼 라이브러리가 엄청 많아지면 브라우저가 수천 번 요청하느라 느려지지 않나요?"라는 의문이 생길 수 있습니다. 이를 해결하기 위해 Vite는 **esbuild**를 사용합니다.

- **사전 빌드:** 자주 바뀌지 않는 외부 라이브러리(`lodash`, `react` 등)는 서버를 처음 켤 때 **Go 언어로 작성된 매우 빠른 esbuild**로 미리 합쳐버립니다.
    
- **효과:** 수많은 HTTP 요청이 발생하는 것을 방지하고, 이후에는 캐시된 파일을 사용하여 로딩 속도를 극대화합니다.
    

---

### 3. 소스 코드 업데이트 (HMR)

코드를 수정했을 때 화면에 반영되는 **HMR(Hot Module Replacement)** 방식도 다릅니다.

- **기존:** 파일 하나만 바꿔도 관련 있는 모듈들을 다시 번들링해야 해서 프로젝트가 클수록 느려졌습니다.
    
- **Vite:** 수정한 파일과 가장 가까운 브라우저의 ESM 연결 고리만 교체합니다. 프로젝트 크기와 상관없이 **항상 즉각적으로 반영**됩니다.



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

