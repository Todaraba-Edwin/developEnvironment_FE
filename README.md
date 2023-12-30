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

- [TODARABA-EDWIN, 관련 저장소, READMD.md](https://github.com/Todaraba-Edwin/Inflearn_frontend-dev-env)
</details>