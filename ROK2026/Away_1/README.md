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

## 메인 IMU

`Away_1`의 메인 IMU는 STMicroelectronics `LSM6DSO32XTR`를 채택한다.

이 부품은 3축 가속도, 3축 자이로, 온도 센서를 포함하는 6축 IMU이며, ESP와는
I2C 또는 SPI로 연결할 수 있다. 비행 로그와 자세/회전 측정에는 SPI 연결을
우선 검토한다.

기본 운용 목표는 다음과 같다.

- 가속도 범위: ±32g
- 자이로 범위: ±2000dps
- 가속도 민감도: 0.976mg/LSB at ±32g
- 자이로 민감도: 70mdps/LSB at ±2000dps
- 전원 범위: 1.71V~3.6V
- 패키지: LGA-14

`LSM6DSO32XTR`는 MPU6500이나 ICM-42688-P의 ±16g 범위보다 넓은 ±32g
가속도 범위를 제공하므로, `Away_1`의 메인 IMU로 사용한다. 다만 점화 충격,
사출 충격, 착지 충격처럼 ±32g를 넘을 수 있는 이벤트까지 모두 보장하는
고G 전용 센서는 아니므로, 필요하면 별도 고G 가속도센서를 보조로 검토한다.

## 어포지 판단

`Away_1`의 어포지 판단은 루트 `README.md`의 어포지 판단 예비 기준을 따른다.
정상 상승만 가정하지 않고, 발사 실패나 중간 고도 이후 뒤집힘, 과격한 회전,
불안정 비행 가능성을 함께 고려한다.

## 개발 상태

`Away_1`은 실제 사용을 목표로 하는 에비오닉스 후보 버전이지만, 아직 개발 중인
보드다. 전체 설계 의도 규약은 루트 `README.md`를 기준으로 한다.
