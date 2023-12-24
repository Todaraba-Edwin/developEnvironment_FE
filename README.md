# Github Settings

<details>
<summary>Permisstion Denied Error</summary>

---
2022년 09월 이후 코딩을 하기 시작한 후, MacOS에 많은 설정을 때려박았다. 이와함께 GITHUB는 잡다한 레포지토리가 올라가며 더러워졌다. 

2023년 10월 취뽀를 했고, 이때다 싶어서 컴퓨터를 쏵 밀었다. 그리고 GITHUB-ID를 새로 만들어서 앞으로 정리된 레포지토리들을 관리하기로 했다. SSH를 새롭게 만들었고, 새로운 GITHUB-ID에 SSH를 할당했지만 기존에 남아있던 키체인으로 인해서 이전의 SSH가 등록되어 있어 문제가 되었다. 

### 1. SSH 삭제하기 
```bash
rm ~/.ssh/id* 
# .ssh 하위에 있는 모든 SSH-KEY를 삭제한다. 
```

### 2. MacOS 기준, 키체인에 등록된 github 관련 암호들 삭제하기
나의 경우에는, 기존에 사용했던 아이디에 대한 키체인이 남아있었고, 해당 아이디에 대한 Permisstion Denied Error 가 계속해서 발생했었다. 어디에 남아있는지 몰랐던 해당 아이디의 흔적을 키체인에서 확인한 것이다. 이를 삭제했다. 

### 3. 다시 SSH 를 생성하고, github에 등록하기 
```bash
ssh-keygen -t rsa -b 4096 -C "User@email.com" 
# -t rsa : RSA 알고리즘을 사용하여 키를 생성하도록 지정하는데, RSA는 공개 키 암호화 및 디지털 서명에 널리 사용되는 하나의 알고리즘

# -b 4096 : 생성할 키의 비트 수를 나타햅니다. 이 경우 4096 비트로 키를 생성하라는 의미이지만, 보안이 강화되고 안전한 연결을 위해서는 더 긴 키를 사용하는 것이 일반적이다. 

# -C : 키에 대한 주석 또는 식별자 이름을 입력하며, 보통 이메일을 입력한다. 

# 위의 명령어를 실행하면 개인 키와 해당 공개 키(id_rsa.pub)가 현재 작업 디렉터리에 생성되며, 주석은 공개키 파일에 추가된다. 

ssh-keygen
# 위의 명령어로도 공개 키를 생성할 수 있지만, 높은 보안 수준이 요구되는 경우에는 취약할 수 있다. 그러므로 -b 8192 또는 -b 16384 로 설정할 수 있지만, 생성 및 사용이 느려질 수 있다는 단점이 있다. 일반적으로 4096 비트로도 충분한다. 
```

[GITHUB 공식문서](https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)를 보면 `-t ed25519` 알고리즘을 제안하면서, 이를 지원하지 않는 레거시 시스템의 경우 `-t rsa -b 4096 -C`를 사용할 것을 권한다. 

```bash
Enter a file in which to save the key
# 키를 저장할 파일을 입력하세요라는 문구가 뜬다. 그냥 Enter를 입력하면 해당 폴더에 SSH가 생성된다. 
Enter passphrase (empty for no passphrase): [Type a passphrase]
# 그리고 패스워드를 등록하면 끝이다. 
cat ~/.ssh/id_rsa.pub
# 루트 디렉토리 아래에서 id_rsa.pub 를 읽으라는 명령을 하면 SSh-KEY를 주는데 이른 깃 허브에 등록하면 된다. 
```

### 4. 정리하면
기존의 SSH를 삭제하고 그래도 Permission Error가 발생한다면, MacOS의 키체인을 확인하고 등록된 정보가 있는지 확인해보자. 

---
</details>

<details>
<summary>github에 올라간 파일에 대한 캐시제거</summary>

```bash 
git rm --cached -r "[folder or file Name]"
```
</details>

<br/>

# Github 저장소 관리 

<details>
<summary>[<strong>Inflearn : 2023.12~</strong>] jeonghwan-kim: github sorce code</summary>

- [첫째, NPM](#프론트엔드-개발환경의-이해--npm)<br/>
- [둘째, WEBPACK](#프론트엔드-개발환경의-이해--웹팩기본)<br/>

### [프론트엔드 개발환경의 이해 : NPM](https://jeonghwan-kim.github.io/series/2019/12/09/frontend-dev-env-npm.html)
⭐️⭐️ jeonghwan-kim 님의 블로그를 요약 정리 했음을 밝힘<br/> 
⭐️⭐️ 링크는 상단의 타이들을 클릭하면 이동합니다. 

#### 1. Node.js 와 NPM
Node.js 라고 하면 백엔드를 구현하는 기술로 알려져 있다. 그러나 프론트엔드 개발자가 웹 어플리케이션 개발을 하다보면 개발 환경을 이해하고 구성하는 부분에 있어서 한계에 부딪히게 된다. 이때 Node.js 를 프론트엔드 개발자는 필요로하게 된다. 

(1) 최신 스펙으로 개발할 수 있다. <br/>
자바스크립트 스펙의 빠른 발전에 비해 `브라우저의 지원 속도는 항상 뒤쳐진다`. 편리한 스펙이 나오더라도, 이를 구현해주는 징검다리(바벨)의 도움이 없이는 부족하다.

(2) 빌드 자동화 <br/>
과거에는 코딩의 결과물을 브라우저에 바로 올리는 경우가 흔지 않았다. 파일 >> 압축 >> 코드의 난독화 >> 폴리필 추가 >> 배포. Node.js는 이러한 일련의 빌드 과정을 이해하는데 적지 않는 역할을 수행한다. 라이브러리들의 의존성을 해결하고, 각종 테스트를 자동화하는데 도움을 준다. 

(3) 개발환경 커스터마이징<br/>
각 프레임워크에서 제공하는 도구를 사용하면 손쉽게 개발환경을 갖출 수 있다. 대표적인 사례가 React.js의 `CRA; create-react-app`이다. 그러나 개발 프로젝트는 각자의 형편이라는 것이 있기에 해당 툴을 그대로 사용할 수 없는 경우들도 발생한다. 이때 커스터마이징을 하려면 Node.js 지식이 요구된다. 이러한 배경하에 Node.js는 프론트엔드 개발에서 필수 기술로 자리매김하고 있다. 

(4) Node.js 설치, NPM(Node Package Manage) & Yarn <br/>
Node.js를 설치하면, Node의 설치매니저인 NPM이 함께 설치된다. 이를 통해서 각종 서드파티 라이브러리들을 다운로드 받을 수 있다. NPM보다 빠른 설치매니저를 요한다면, `yarn`을 설치하여 진행할 수도 있다. 

    현재적시점에서 npm과 yarn은 둘다 발전하고 있으며 프로젝트에 따라 어떤 도구를 설정할 것인지는 사용자의 선호도 및 프로젝트의 특정 요구에 따라 다르다. 

    npm은 Node.js가 처음 소개되면서 등장하였다. 초기에 npm은 JS 프로젝트의 종속성을 관리하고 패키지를 설치하는 주요 도구로 자리잡았다. 

    yarn은 npm이 가진 몇가지 문제, 패키지 설치속도 및 의존성 관리로 인해 등장하였다. yarn 은 현 meta(facebook)에서 소개하며 등장하였다. yarn은 npm과 호환성을 유지하면서도 빠른 패키지 설치 및 보다 효과적인 의존성 해결을 제공하며, 오프라인 상태에서도 패키지 설치가 가능하도록 캐시를 지원하고, 보안 및 안정성을 강조하여 사용자들에게 새로운 선택지를 제공하였다. 

    npm(ver.5)은 패키지 잠금파일(package-lock.json)을 도입하여 의존성 버전을 더욱 정확하게 관리하는 방향으로 발전하며 나아가고 있다. 

- `종속성`(dependencies) : 프로그램에서 특정 프로그램이나 패키지가 동작할 때 필요한 외부 모듈이나 라이브러리를 가리킨다. 이는 프로그램이나 패키지를 구성하는데 필요한 다른 소프트웨어 요소들을 나타난다. 
- `의존성`(devDependencies) : 의존성은 하나의 소프트웨어 모듈이 다른 모듈의 기능을 이용하거나 다른 모듈과 협력하여 동작할 때 발생한다. 즉 한 모듈이 다른 모듈에 의존한다는 것은 다른 모듈의 기능, 인터페이스, 혹은 자원을 필요로 한다는 것을 뜻한다. 
- 사례 : `React` : 종속성에는 `styled-components`와 같이 스타일링을 돕는 라이브러리들이 해당된다. React 안에서 특정 부분이 이 라이브러리에 의존하고 있기 때문이다. 의존성에는 `typescript`, `eslint`, `prettier`와 같이 개발 및 빌드 프로세스에서 사용되는 도구들이 해당된다. 이러한 도구들은 개발 시간에만 필요하며, 프로덕션 환경에서는 직접적으로 애플리케이션의 기능에 영향을 미치지 않기 때문이다. 

```bash
# npm - 종속성 설치 
npm install package-name

# npm - 의존성 설치
npm install --save-dev package-name
npm install -D package-name

# yarn - 종속성 설치 
yarn add package-name

# yarn - 의존성 설치 
yarn add package-name -D
yarn add package-name --dev

# 또는 CDN을 통해서 직접 다운로드 할 수 있지만, 최신 버전을 관리하기 위해서는 위의 방법이 적합하다. 이는 구체적인 버전의 버전이 요구될 때 사용된다. 
```

(5) Package.json<br/>
`npm init`를 통해 프로젝트 초기설정을 할 수 있다. 패키지 이름, 버전 등 프로젝트와 관련환 정보들이 기록되는 파일을 생성한다. `npm init -y`는 질문들을 생략하고 package.json 파일을 생성한다. 

```json
{
  "name": "프로젝트 이름",
  "version": "1.0.0", // 프로젝트의 버전 정보 
  "description": "프로젝트 설명",
//   "main": "index.js", // 노드 어플리케이션일 경어 진입점 경로, 프론트엔드는 사용하지 않는다. 
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }, // 프로젝트 명령어를 등록할 수 있으며, test는 샘플명령어이다. 
  "author": "프로그램 작성자", 
  "license": "ISC" // 라이센스 
}
```

(6) Package.json과 유의적 버전표시
```json
{
  "dependencies": {
    "react": "^16.12.0"
  }
}
```

설치한 패키지들의 버전을 관리하기 위한 규칙, 이를 `유의적 버전`(Sementic Version)이라고 한다. 
- Maior(주버전) : 기존 버전과 호환되지 않게 변경
- Minor(부버전) : 기존 버전과 호환되면서 기능이 추가된 경우
- patch(수버전) : 기존 버전과 호환되면서 버그를 수정한 경우 

```json
// 특정버전 
"react": "16.12.0"

// 부등호
"react": ">16.12.0" // 해당 버전보다 크면 허용
"react": ">=16.12.0" // 해당 버전이상 이면 허용
"react": "<16.12.0" // 해당 버전보다 작은 경우
"react": "<=16.12.0" // 해당 버전이하 이면 허용 

// 틸트(~)와 캐럿(^)
"react": "~16.12.0" // 해당 버전의 miror 버전 안에서 허용 16.12.a ~ 16.12.z
"react": "^16.12.0" // 해당 버전의 major 버전 안에서 허용 16.a.0 ~ 16.z.0
```

보통 라이브러리가 정식 릴리즈 되기 전에는 패키지 버전이 수시로 변한다. 이때 주버전이 변할 때 하위 호환성이 지켜지지 않는 경우가 빈번하다. 이러한 경우의 호환성을 위해서 유의적 버전이 활용된다. 



### [프론트엔드 개발환경의 이해 : 웹팩(기본)](https://jeonghwan-kim.github.io/series/2019/12/10/frontend-dev-env-webpack-basic.html)

문법 수준에서 모듈이 지원된 것은 ES2015(import && export)부터이다. ES2015 이전에 모듈을 구현하는 방식에는 `AMD`와 `CommnonJS`가 대표적이다. 그 가운데 CommonJS는 exports && require() 함수로 자바스크립트를 불러들인다. AMD는 `Asynchronous` 비동기로 로딩되는 브라우져의 환경에서의 자바스크립트를 불러들이는 방식이다. 

---
### 1. 웹팩 : 앤트리와 아웃풋 실습
```bash 
# TODO: 웹팩으로 빌드한 자바스크립트를 여기에 로딩하세요
# branch : 1-webpack/1-entry
# 1) index.html 을 npm 환경으로 세팅해야 한다. 
```


</details>