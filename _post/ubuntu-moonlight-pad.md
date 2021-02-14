# 우분투 20.04에서 문라이트에 게임패드 연결 하기

우분투 20.04 에서 ipega PG_9068 usb로 연결해서 문라이트 인풋으로 사용하려고 한다.  
PG-9068을 블루투스로 연결하면 Dinput으로 바로 인식이 되는데, 이상하게 usb로 붙이면 바로 인식이 안되다.  
일단 붙긴 붙었는지 확인부터 해보자  
Dmesg로 긁어보니, 연결해서 input아래로 들어가긴 했다.  

```
[   54.174230] input: ShanWan     PS(R) Gamepad as /devices/pci0000:00/0000:00:14.0/usb3/3-4/3-4:1.0/0003:2563:0623.0005/input/input21  
```
dev 아래를 보니 js0가 정상적으로 보인다.  
```
C:~$ ls /dev/input/ | grep js
js0
```
그리고 문라이트를 켰더니 등록이 안되있다고 gamepad tool 사이트로 리다이렉트 해준다.  
가서 설명 일고 일단 gamepad tool을 받았다.  
그리고 실행  

<p align="center">
<img style="max-height:40%; max-width:40%;" align="center" src="https://raw.githubusercontent.com/csj0202/csj0202.github.io/master/img/gamepad_tool_gui.png">
</p>  

다행이 pad인식은 됐고, create map으로 키맵핑을 해준다.  
마지막에 이름도 그냥 붙은대로 해주고 (저장했던가? 기억이 안남) 나면 아래 로그창에 키맵 텍스트가 뜬다.  
요걸 바로 긁어도 되고, 아니면 gamecontroolerdb 라는데 저 내용이 저장되있는데 거기서 긁어도 된다.  
여튼 키맵 정보를 복사한 후  

```
C:~$ find . -name '*controll*'
./.local/share/gamecontrollerdb.local.txt
./snap/moonlight/common/.cache/Moonlight Game Streaming Project/Moonlight/gamecontrollerdb.txt
```
```
C:~$ cat ./.local/share/gamecontrollerdb.local.txt 
03000000632500002306000010010000,ShanWan     PS(R) Gamepad,platform:Linux,a:b0,b:b1,x:b3,y:b4,back:b11,start:b10,leftstick:b13,rightstick:b14,leftshoulder:b6,rightshoulder:b7,dpup:h0.1,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,leftx:a0,lefty:a1,rightx:a2,righty:a3,lefttrigger:b8,righttrigger:b9,
```
문라이트 설치 폴더 아래있는 gamecontrollerdb.txt에 업데이트를 해주면 된다.  
vi로 열어서 맨 아래다 붙이자. 이렇게  
```
C:~$ cat ./snap/moonlight/common/.cache/Moonlight\ Game\ Streaming\ Project/Moonlight/gamecontrollerdb.txt  | tail -n 2
050000005e040000e0020000ff070000,Xbox Wireless Controller,a:b0,b:b1,back:b8,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,dpup:h0.1,guide:b9,leftshoulder:b4,leftstick:b6,lefttrigger:a2,leftx:a0,lefty:a1,rightshoulder:b5,rightstick:b7,righttrigger:a5,rightx:a3,righty:a4,start:b10,x:b2,y:b3,platform:iOS,
03000000632500002306000010010000,ShanWan     PS(R) Gamepad,platform:Linux,a:b0,b:b1,x:b3,y:b4,back:b11,start:b10,leftstick:b13,rightstick:b14,leftshoulder:b6,rightshoulder:b7,dpup:h0.1,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,leftx:a0,lefty:a1,rightx:a2,righty:a3,lefttrigger:b8,righttrigger:b9,
```
나는 snap 으로 설치해서 snap 아래에 있었다.  
업데이트를 해주고 문라이트를 다시 켜보면 에러창 없이 정상적으로 패드가 붙어있다.  
