# 시작전 해야할 것 
- 1. 검색창에 개발자 설정 들어가서 개발자 모드 켜기
- 2. window 기능 켜기/ 끄기 에 들어가서 "linux용 window 하위 시스템 체크 활성화 후 재부팅
- 3. 재부팅시 DEL키 연타하여 bios모드로 진입하여  cpu 가상화 활성화하기 
- 4. 실행창(윈도우키 +r)을 켜서 winver 실행후 버전 확인하기(2004이상)(버전이 낮은경우 업데이트)

# 시작
- 1. powershell 을 관리자 권한으로 실행 후 아래코드 작성
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
#wsl 시스템 활성화단계
```
- 2. 마이크로소프트 스토어에 들어가서 ubuntu 설치
- 3. powershell 에 아래코드 작성
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
- 4. ubuntu 실행 후 사용자명과 비밀번호 작성 (사용자명은 소문자 +비밀번호는 타자시 보이지 않음 참고)

- 5. powershell에서 아래코드 작성
```
wsl -l -v   
#여기서 ubuntu 의 ver1을 2로 변경해야한다]
wsl --set-version Ubuntu 2
#wsl 1에서 2 로 변경
```
- 추가로 다운받을 여러 배포판을 wsl에 추가할 때 마다 wsl2 버전으로 다운받도록 성정하기위해 powershell에 아래코드 작성
```
wsl --set-default-version 2
wsl --set-default Ubuntu
```
- ubuntu실행후 윈도우 탐색기에 "\\wsl#" 검색시 ubuntu폴더 생성확인가능

# 사용중 vmmem이라는 프로그램이 엄청난 메모리를 차지할 때!
- 1. 실행창 (window+r)
- 2. %userprofile% 이동
- 3. ".wslconfig" 메모 파일 생성후 아래코드 작성한 뒤 재부팅(메모리는 유도리것)
```
[wsl2]
memory=2GB
swap=0
localhostForwarding= true
```

- 추가로 메모리를 줄인다해도 한번 실행시 linux서버에서 점점 더 많은 메모리를 원하기 때문에 강제 제한하는 것이고 다른 용도로 pc를 사용할시에는 켜지않는것을 권장