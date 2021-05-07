### 개발자를 위한 터미널 입문

개발을 하다보면 원치않아도 터미널을 사용해야하는 경우를 마주하게 된다. Git이나 PackageManager를 다룰 때에도 터미널이 필요하다.  
터미널을 쓰는 것에 두려움을 느끼지 말고 익혀두자. 터미널을 잘 다루는 것은 개발효율성을 높이는 방법중에 하나이다.

### 터미널에서 할 수 있는 것
터미널은 텍스트 기반이며 명령어를 입력할 수 있는 CLI(command-line interface)역할을 한다.  깃(GUI)버젼을 사용해본 사용자라면
왜 터미널을 사용해야 하는가에 의문을 가질 수 있다.  GUI는 명령어를 버튼과 같은 시각적인 도구로 입력하는 방법이다. 그렇기 때문 모든 명령어를 구현하지 않는다. 터미널을 사용해 명령어를 입력할 수 있다면, 원하는 결과를 이끌어 내기 위한 입력을 할 수 있다.

### 터미널 실행하기
Mac OS는 Spotlight (`⌘ + Space`)를 불러 “Terminal”을 찾아 실행할 수 있다.\
Linux는 “terminal” 을 찾아 실행하면 된다. 단축기 `Ctrl + Alt + T`를 이용해 실행할 수 있다.
Windows 10은, `PowerShell`이라는 터미널을 실행한다.

### 기본 명령어
터미널의 기본 명령어는 다음과 같다. 
``` 
ls     # 파일 list 나열  
cat    # 파일의 내용을 보여주기  
cd     # 디렉토리 변경  
mv     # 파일, 디렉토리 이동과 이름변경  
cp     # 파일, 디렉토리 복사  
mkdir  # 디렉토리 생성  
rm     # 파일, 디렉토리 제거  
pwd    # 현재 디렉토리 경로   
```
위 명령어들의 사용 방법은 다양하다. 사용방법 중 인자들을 입력해야 하기도 하다.
cp 명령어를 입력해보자.  
```
LT1066ui-MacBook-Pro:test hyunho.lee$ cp  
usage: cp [-R [-H | -L | -P]] [-fi | -n] [-apvXc] source_file target_file
       cp [-R [-H | -L | -P]] [-fi | -n] [-apvXc] source_file ... target_directory
```
좀 더 자세한 설명을 살펴보려면, 터미널 창에 `man cp`를 입력하면 정보가 나온다.

### 패키지 매니져 (PackageManager)
여러 패키지 매니져 중 mac에서 사용하는 `brew`를 사용해보자. brew의 사용법은 [이 곳](https://brew.sh/index_ko)에서 확인 할 수 있다.  
> brew install wget   

wget 이라는 패키지를 설치했다. 


### Git
프로젝트의 버젼을 관리하려면 git과 같은 VCS를 사용해야한다. 
git을 설치해보자
```
- macOS(Homebrew): brew install git
- Linux(e.g. Debian, Ubuntu): apt-get install git
- Windows(Git for Windows installer or Chocolatey): choco install git
```
git의 개념과 명령어를 다루는 방법은 ['git 다루기'](https://dev200ok.blogspot.com/2020/04/git.html) 포스팅에서 다루기로 한다.

### 팁과 트릭
터미널을 사용하면서 도움이 되는 팁을 소개한다.  

1. 테마변경  
테마를 변경해 터미널 사용에 도움이 되는 [기능](https://medium.com/harrythegreat/oh-my-zsh-iterm2%EB%A1%9C-%ED%84%B0%EB%AF%B8%EB%84%90%EC%9D%84-%EB%8D%94-%EA%B0%95%EB%A0%A5%ED%95%98%EA%B2%8C-a105f2c01bec)들을 추가해 주자.

2. 단축키  

`^ (caret)`는 맥북 키보드의 `Control`키를 나타낸다 :
`^a` : 커서를 줄의 맨 처음으로 이동시킨다
`^e` : 커서를 줄의 맨 마지막으로 이동시킨
`⇥ ( tab)`: 파일명과 경로의 자동완성
`!!` : 직전에 수행했던 명령어 수행
`^l` : 화면을 비운다 (clear 명령어)
`^c` : 현재 실행중인 프로세스 중지
`^d` : 터미널 종료 (or exit)

3. 별칭(Aliases)과 functions(함수)  
매번 긴 명령어를 조금씩 바꿔서 실행하는 것 보다 줄여서 별칭으로 사용하면 생산성을 향상시킬 수 있다. 
> alias ll="ls -lA --color"  

`ll` 이라는 명령어로 `ls -lA --color`과 같은 명령을 내릴 수 있다.

함수를 정의 해서 명령어를 실행해보자.
> say_hi () {
  echo Hello, $1
}
위와 같이 함수를 정의해놓고, `say_hi John`을 입력하면, `Hello, John`이라는 출력 결과물을 볼 수 있다.  

alias tutorial : https://www.digitalocean.com/community/tutorials/an-introduction-to-useful-bash-aliases-and-functions  
functions and alias : http://scriptingosx.com/2017/05/configuring-bash-with-aliases-and-functions/

생산성 증가의 방법에는 여러가지 도메인을 공부하는 방법도 있지만 도구의 사용법을 공부하는 것도 하나의 방법이라고 생각된다.  
단축키와 툴사용법을 익히는 것도 중요하다.  


참고 : https://medium.com/@webprolific/introducing-the-terminal-for-developers-1f86dfbcd623






