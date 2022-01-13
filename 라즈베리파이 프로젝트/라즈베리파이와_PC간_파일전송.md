# 동영상 전송
- 1. USB전송  (파란색 3.0 포트로 전송속도가 빠름)->암호입력 (원격설정시 했던 암호)
- 파일관리자에서 가져오기

- 2. Samba 설치
- 라즈베리파이 웹브라우저에서 공유기 관리 사이트 이동
- 고급설정 -> 네트워크 설정->고정아이피 -> 주소 기억
- Samba설치 
```
sudo apt install samba samba-common-bin

```
- WINS 설정 수락

- 위 과정은 공유폴더를 만들어주는 과정이므로 폴더 이름과 비밀번호를 설정해야한다. (보안)
```
sudo smbpasswd -a pi (이름)
패스워드 입력 (커서 안 움직임) 

# 설정파일 쓰기

sudo nano /etc/samba/smb.conf

쭉 내리기

[pi]
  comment = my rpi folder
  path = home/pi
  valid users = pi (이건 아이디쓰는거임)
  browseable = yes
  guest ok = no
  read only = no
  creat mask = 0777

콘트롤 x +시프트 y + 엔터

sudo reboot

```

# 컴퓨터 파일 탐색기 주소창에 
- \\(원화표시)

- \\\\라즈베리파이의 공유기 내부 아이피\pi(아이디)

- 네트워크 자격 증명 (패스워드 입력)



