- 모션 설치
```
sudo apt-get install motion
```

- 웹캠 인식 확인

```
lsusb
```

- 모션설정
```
sudo nano /etc/motion/motion.conf
# daemon off -> on (백그라운드 작동 유무)
control+w ->search
#width 검색 (해상도 ) ->대충 w640*h480 +웹캠 가능 해상도보다 높게 설정하면 스트리밍 불가능
#framerate ->초당 화면 ex 100
#ffmpeg 검색
#ffmpeg_output_movies off (자동녹화)
#stream_maxrate검색->100
#stream local host off
#webcontrol_localhost검색
컨트롤 x 시프트 y

```

- motion 서비스 등록
```
sudo nano /etc/default/motion
#start_motion_daemon=yes
```

- motion 자동실행 설정
```
sudo nano /etc/rc.local
# 맨 아랫줄 exit 0 바로 위에
sudo motion 추가 
```

- motion 폴더 권한 설정
```
sudo chmod 777 /var/lib/motion #chmod 파일이나 폴더의 권한설정 777은  개방설정 \
```

- 같은 와이파이를 쓰는 공간에서 주소창에 할당 ip:8081 

# 외부에서 하려면
- 공유기 홈페이지
- 포트포워딩 한번더 
- 외부포트 1만번대 아무거나 
- 내부포트  8081