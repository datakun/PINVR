# PINVR
- 라즈베리 파이에 카메라 영상 녹화 및 분배 서버 구현

## Pi Camera 모듈 연결 및 활성화
  1. 카메라 모듈을 라즈베리 파이에 설치
  2. uv4l 설치
    - 0번, 1.1, 1.2, 1.3번만 참고
  3. 디바이스에 등록
    - ``` uv4l --driver raspicam --auto-video_nr --width 1280 --height 720 --framerate 20 --vflip --hflip --encoding h264 --quantisation-parameter 35 ```
    - 장치 이름은 자동으로 설정(/dev/video0), 가로 1280, 세로 720, 초당 프레임 20, 세로 가로 화면 반전, h264 인코딩, 품질 35(최상 10 ~ 최저 40) 설정
  4. 시작 프로그램에 등록하기 위해 다음 링크 참고
    - http://www.rasplay.org/?p=4854
    - 스크립트 내용은 다음을 참고
      ```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          camera-enable-uv4l
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: camera enable by uv4l
### END INIT INFO

uv4l --driver raspicam --auto-video_nr --width 1280 --height 720 --framerate 20 --vflip --hflip --encoding h264 --quantisation-parameter 35

sudo chrt -a -r -p 99 `pgrep uv4l`
```

## 원격 RTSP 영상 파일로 저장하기 (녹화 서버)
  1. 필요한 기능
    - 스트리밍 받는 형태 그대로(h264 인코딩) 파일 저장
    - 프로세스 생성 할 때 관리서버로부터 인수 전달 받음
      - RTSP 주소, 파일 저장 위치
    - 관리 서버에 의해 프로세스 종료(녹화 종료) 및 생성(녹화 시작) 가능
  2. 참고 예제
    - Python
      - http://blog.mikemccandless.com/2013/11/pulling-h264-video-from-ip-camera-using.html
    - C / C++
      - https://ffmpeg.org/doxygen/trunk/doc_2examples_2remuxing_8c-example.html
  3. 

## 분배 서버
  1. 필요한 기능
    - 클라이언트에서 요청 시, RTSP로 영상 분배
    - 녹화 영상, 실시간 영상을 분배
    - 주소형식 : rtsp://주소:포트/카메라이름/yyyyMMdd_hhmmss
  2. 참고 예제
  3. 

## 관리 서버
  1. 필요한 기능
    - 카메라 등록
    - 저장경로 설정
    - 할당 공간 크기 설정
    - 녹화 스케쥴 설정
    - 북마크 관리
    - 녹화 시간 확인
    - 카메라 상태 확인
    - 웹에서 설정 변경 및 확인 가능
  2. 참고 예제
