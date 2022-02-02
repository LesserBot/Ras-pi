- 윈도우나 라즈베리파이에 있는 ubuntu 에서 ros나 terminator을 사용하려면 일단 display에 연결하여 시각화 해야한다. 그 때 사용하는 것이 xming 과 x-server이다 .
- 1. xming 설치
    - https://sourceforge.net/projects/xming/
    - 위 주소나 구글에 xming을 검색하여 sourceforge.net 애 올라온 자신의 컴퓨터os와 맞는 버전을 설치하가

    - additional icons: 에서 상단 1,2 번째와 마지막 5번째 체크하기 :바탕화면에 아이콘 

- 2. 환경변수 설정
    - 일단 ubuntu로 접속하여 
    ```
    cat /etc/resolv.conf
    ``` 
    - 코드를 작성하여 ip를 확인한다. raspberry pi의 경우에는 할당한 고정 ip를 사용하면 되겠지만 window에서 wsl을 사용하여 할 경우에는 따로 ip를 배정받기 때문에 위 작업으로 확인해야한다.

    - 매 접속마다 이 이 아이피를 받기위해 코드 추가
    ```
    vi ~/.bashrc
    ```
    - 맨 아래에 다음코드를 추가하자 vi 이므로 vim 사용법을 익히자: 에디터에서 shift+} 나 키보드 등으로 맨 아래로 내려가 i 를 눌러 입력모드로 변경한뒤 아래코드를 작성하고 esc를 눌러 단축어 모드로 변경해 :wq를 입력하여 저장한뒤 창을 닫는다.
    ```
    export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
    ```

    - 저장 후 환경변수를 적용시킨다
    ```
    source ~/.bashrc
    ```
    - 이후 정상적으로 ip가 출력되는지 확인
    ```
    echo $DISPLAY
    ```
- 3. xluncher을 이용한 옵션 설정
    - multiple window 설정후 다음 (display num 0 )
    - start no client
    - additional parameters for xming 에 "-ac"입력
    - save configuration 을 선택하여 바탕화면에 저장
    -  기존 xming이 실행중이라면 끄고  다음 config를 실행

- +. 추가로 매번 실행시 자동으로 config 를 실행하고싶으면
    - windows+r 을 눌러 실행창을 열어 " shell:startup"입력
    - 해당폴더에 config 파일 넣기

- 4. windows 방화벽 예외설정
    - powershell 을 관리자권한으로 실행
    -아래 코드 입력
    ``` New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -LocalAddress $myIp -Action Allow
    ```
    - 재부팅

- 5. 확인
    - ubuntu에 아래 코드를 하여 할아버지를 소환해보자
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install build-essential
    sudo apt install imagemagick
    
    -설치후 
    display
    ```
    - 만약 작동하지 않다면 방화벽을 의심해봐야한다
    - windows설정 -> 업데이트 및 보안 -> windows 보안-> windows 보안 열기 -> 방화벽 및 네트워크 보호-> 방화벽에서 앱 허용-> 설정 변경 클릭 
    - 이후 xming x server 체크박수중 공용으로 되어있는 부분에 체크


- 6. terminator 설치
    ```
    sudo apt-get install terminator
    ```
    - 둘중 아무 코드로 실행
    ```
    terminator 
    or
    DISPLAY=:0 terminator &
    ```

- +. ZSH 설치 희망시 
    - sudo apt-get install curl wget git zsh
     -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash

    - vi .zshrc 진입
    - 항상 bash를 실행하는 문제를 해결하기위해 
    ```
    if [ -t 1]; then
        exec zsh
    fi
    ```
    - 추가

- terminator 색 구성용(solarized DArk로 변경) -base16-builder 사용
    ```
    sudo apt-get install nodejs-legacy
    sudo npm install --global base16-builder
    mkdir -p .config/terminator
    base16-builder -s solarized -t terminator -b dark > .config/terminator/config
    ``
- Dircolors
    -github 에서 solarize dircolors 를 찾아 .dir_coors
    ```
    wget https://raw.githubusercontent.com/seebi/dircolors-solarized/master/dircolors.256dark
    mv dircolors.256dark .dir_colors
    ```
    - vi.zshrc
    ```
    if [ -f ~/.dir_colors ]; then
    eval `dircolors ~/.dir_colors`
    fi
    ```
- 터미네이터 직접실행
- https://blog.naver.com/changbab/222097716430  블로그 참조 본 내용은 이 블로그와 다른 블로그 내용을 참조 했음
- vscode에서 wsl 사용은 https://velog.io/@gidskql6671/WSL-WSL2-%EC%84%A4%EC%B9%98-VSCode-%EC%97%B0%EB%8F%99 참조
