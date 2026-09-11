# Away_1

`Away_1` is the active candidate for the real COMET avionics version.
It is currently under development.

This board inherits the core avionics/logger intent from the earlier ROK2
concept, but it is treated as the current development target rather than a
completed flight-validated design.

The avionics design intent and development rules apply to the whole
COMET_Avionics board family and are documented in the root `README.md`.

## 전원 구성

`Away_1`은 배터리 2개를 사용하는 전원 구조를 기준으로 개발한다.

- 로거용 배터리
- 대전류 구동용 배터리

두 배터리는 모두 2~3S LiPo를 기준으로 하며, 용량은 약 500mAh급을 상정한다.

## 로거용 전원

로거용 배터리는 비행 판단, 기록, 통신 계통을 담당한다.

주요 공급 대상은 다음과 같다.

- ESP
- 내부 센서
- LoRa
- GPS
- LTE 모듈용 5V 2A 전원

로거용 전원은 센서 감시, 로그 기록, 위치 송신, 지상국 통신이 유지되도록 하는
쪽을 우선한다. 대전류 부하의 순간 전류나 노이즈가 로거 계통을 불안정하게
만들지 않도록 분리하는 것을 기본 방향으로 둔다.

## 대전류 구동용 전원

대전류 구동용 배터리는 직접 구동 부하를 담당한다.

주요 공급 대상은 다음과 같다.

- 30kg급 서보 2개
- 니크롬 코일 점화기

이 전원 계통은 서보 구동과 점화기 동작처럼 순간 전류가 큰 부하를 처리하기
위한 것이다. 로거용 전원과 역할을 분리해, 구동 부하가 동작할 때도 ESP와 센서,
통신 장치가 가능한 한 안정적으로 유지되도록 한다.

## 개발 상태

`Away_1`은 실제 사용을 목표로 하는 에비오닉스 후보 버전이지만, 아직 개발 중인
보드다. 전체 설계 의도 규약은 루트 `README.md`를 기준으로 한다.
