# [ETC] Server 관련 명령어들 모음 

[Mac 명령어 바로가기](#mac-명령어)

[Linux 명령어 바로가기](#linux-명령어)



---

### Mac 명령어

* 현재 열려있는 포트 확인
  * 현재 열린 전체 포트 목록 확인 : `sudo lsof -PiTCP -sTCP:LISTEN`
  * 특정 포트 확인(e.g. 8080) : sudo lsof -i :8080
  * 포트 닫기 sudo kill -9 PID



### Linux 명령어

