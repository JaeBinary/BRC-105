# [2024년 제7회 전국 대학교 인공지능 드론 경진대회](https://www.theieie.org/board/?_0000_method=view&ncode=a016&num=45&page=1)
    - 4차 산업을 대비하고 인공지능(AI) 드론 관련 기술의 미래인재 교육 및 육성
    
    - 향후 도래되는 ICT 융합기술의 핵심 기반 기술이 되는 인공지능과 임베디드 컴퓨팅
      플랫폼을 응용하는 소프트웨어 및 하드웨어 관련 기술교육 및 육성
### 장소: [중소벤처기업진흥공단](http://sbti.sbc.or.kr) 연수원 봉사관, 1층 112호
    경기 안산시 단원구 원수원로 87 (초지역 1번 출구에서 도보 983m)

### 일시: 2024년 6월 22일(토) 10:00 ~ 17:00
### 시상: 대상 1팀, 금상 1팀, 은상 2팀, 동상 3팀
### 기술보고서상 별도 시상 (금상 1팀, 은상 1팀, 동상 1팀)

#### [경진대회 참가 접수 링크](https://forms.gle/WFev5tbmiYSakLUU6)

# e_drone for python
## 1. Intro

---
### 1) e_drone for python 소개
***e_drone for python***은 python에서 **E-DRONE**을 쉽게 사용할 수 있도록 도와주는 라이브러리이다.

### 2) 설치
아래의 명령을 실행하면 *e_drone*이 설치된다.
> pip install e_drone

최신 버전이 설치되지 않는다면 아래의 명령을 사용한다.
> pip --no-cache-dir install e_drone

### 3) 업그레이드
최신 버전으로 업글이드 하려면 아래의 명령을 실행하면 된다.
> pip install --upgrade e_drone

### 4) 삭제
아래의 명령을 실행하면 *e_drone*이 삭제된다.
> pip uninstall e_drone

### 5) 드론, 조종기 펌웨어 업그레이드
펌웨어를 최신 버전으로 업그레이드 하려면 드론 또는 조종기를 부트로더 모드로 USB에 연결한 후 아래의 명령을 실행한다.
> python -m e_drone upgrade

상세한 설명은 아래의 문서를 참고한다.<br>
[e_drone 파이썬 라이브러리를 사용한 펌웨어 업그레이드](https://dev.byrobot.co.kr/documents/kr/products/e_drone/manual/update/python/)

### 6) 시리얼 포트 검색
- Drone 클래스 내부에서 pyserial을 사용하여 시리얼 포트에 연결한다.
- 시리얼 포트에 연결하려면 장치 이름을 알고 있어야 한다.
- 이때, 필요한 것이 컴퓨터에 연결된 시리얼 통신 장치들을 검색할 수 있는 명령이다.
- 이 명령은 pyserial에서 제공하고 있다. (pyserial은 e_drone을 설치한 경우 함께 설치된다.)

아래는 컴퓨에 연결된 시리얼 통신 장치들의 이름을 확인하는 코드이다.
```python
from serial.tools.list_ports import comports

for port, desc, hwid in sorted(comports()):
    print("%s" %(port))
```

장치에 대한 상세한 정보를 확인하려면 아래의 코드를 실행해본다.
```python
from serial.tools.list_ports import comports

nodes = comports()

count = 0
for node in nodes:
    print("[{0}]".format(count))
    print("         device: ", node.device)
    print("    description: ", node.description)
    print("   manufacturer: ", node.manufacturer)
    print("           hwid: ", node.hwid)
    print("      interface: ", node.interface)
    print("       location: ", node.location)
    print("           name: ", node.name)
    count += 1
```

### 7) 응용 프로젝트 예제
**[일반적인 진행 순서]**
1. 드론 객체 생성
2. 시리얼 포트 연결
3. 명령
4. 시리얼 포트 닫기

```python
from time import sleep

from e_drone.drone import *
from e_drone.protocol import *

if __name__ == '__main__':

    drone = Drone()       # 드론 객체 생성
    drone.open("COM22")   # 시리얼 포트 연결

    drone.sendBuzzer(BuzzerMode.Scale, BuzzerScale.C4.value, 500)   # 버저에 4옥타브 도 소리를 500ms 동안 내라고 명령하기
    sleep(1)              # 1초간 sleep

    drone.close()         # 시리얼 포트 닫기 및 내부 데이터 수신 스레드 종료
```

open() 함수 사용 시 인자를 넣지 않으면, 내부에서 시리얼 포트를 검색하여 가장 마지막에 검색된 장치에 연결을 시도한다.
```python
from time import sleep

from e_drone.drone import *
from e_drone.protocol import *

if __name__ == '__main__':

    drone = Drone()       # 드론 객체 생성
    drone.open()          # 시리얼 포트 연결(내부에서 시리얼 포트 리스트를 읽어서 마지막 장치에 연결)

    drone.sendBuzzer(BuzzerMode.Scale, BuzzerScale.C4.value, 500)   # 버저에 4옥타브 도 소리를 500ms 동안 내라고 명령하기
    sleep(1)              # 1초간 sleep

    drone.close()         # 시리얼 포트 닫기 및 내부 데이터 수신 스레드 종료
```

## 2. Command Line

---
### Command Line 명령어
e_drone 라이브러리는 소스 코드 작성 없이 원하는 명령을 실행하거나 데이터를 확인할 수 있는 command line 명령어를 지원하고 있다.<br>

아래에서 소개하는 명령어를 실행하여 간단하게 데이터를 확인하거나 작동할 수 있다.
> python -m e_drone

![실행 가능한 명령어 리스트](https://dev.byrobot.co.kr/documents/kr/products/e_drone/library/python/e_drone/images/02_commandline_commandlist.png)

### 1) Request Data
#### (1) state 데이터 요청
State 데이터를 10회 0.2초 주기로 요청하는 명령은 다음과 같다.
> python -m e_drone request State 10 0.2

![State 요청 실행 결과](https://dev.byrobot.co.kr/documents/kr/products/e_drone/library/python/e_drone/images/02_commandline_request_state.png)

#### (2) Motion 데이터 요청
Motion 데이터를 10회 0.2초 주기로 요청하는 명령은 다음과 같다.
> python -m e_drone request Motion 10 0.2

![Motion 요청 실행 결과](https://dev.byrobot.co.kr/documents/kr/products/e_drone/library/python/e_drone/images/02_commandline_request_motion.png)

### 2) Command
#### (1) 이륙
> python -m e_drone takeoff

#### (2) 착륙
> python -m e_drone landing

#### (3) 정지
> python -m e_drone stop

### 3) Control
#### (1) 조종 명령
> python -m e_drone control [roll] [pitch] [yaw] [throttle] [time(ms)]

##### 조종 명령 예시 - Roll, Pitch, Yaw, Throttle
> python -m e_drone control 0 30 0 0 5000

#### (2) 조종 명령(위치, 방향)
> python -m e_drone position [x(meter)] [y(meter)] [z(meter)] [speed(m/sec)] [heading(degree)] [rotational velocity(deg/sec)]

##### 조종 명령 예시 - 위치, 방향
> python -m e_drone position 5 0 0 2 90 45

#### (3) 조종 명령(위치)
> python -m e_drone position [x(meter)] [y(meter)] [z(meter)] [speed(m/sec)]

##### 조종 명령 예시 - 위치
> python -m e_drone position 5 0 0 2

#### (4) 조종 명령(방향)
>python -m e_drone heading [heading(degree)] [rotational velocity(deg/sec)]

##### 조종 명령 예시 - 방향
> python -m e_drone heading 90 45

