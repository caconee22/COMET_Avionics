# COMET_Avionics

COMET rocket avionics hardware project archive.

This repository replaces the previous `COMET_ROK_2` repository with the full
COMET avionics workspace layout.

## Structure

- `ROK2025/`: ROK2-era module board and reference files.
- `ROK2026/`: Current ROK2026 KiCad projects, shared libraries, and revisions.
- `ROK2026/ROK_3/`: ROK3 small-form-factor and SMD-chip test board.
- `ROK2026/ROK_4/`: ROK4 power-design research test board.
- `ROK2026/Away_1/`: Current active avionics candidate version under development.

## Project History

- `ROK2025/COMET_ROK_2`: First ROK2 circuit and module board. It was not used
  because of software development staffing issues.
- `ROK2026/ROK_3`: Miniaturized ROK3 version with SMD chips. It was not used
  because of power-section issues, but it may still be recoverable and remains
  a test version.
- `ROK2026/ROK_4`: A further test version focused on power-section design. It
  is a research version for a small buck-converter power design.
- `ROK2026/Away_1`: The avionics version candidate intended for actual use. It
  is currently under development.

## Avionics Design Intent

This design intent applies to the whole COMET avionics board family.

## 설계 목적

COMET_Avionics의 목적은 단순한 데이터 로거가 아니라, 로켓 비행 중 필요한
항공전자 기능을 안정적으로 수행하는 것이다.

가장 중요한 기능은 최고점 판단, 지연 사출 판단, 그리고 실제 사출 작동이다.
로그 저장, 통신, GPS 전송, LED, 부저, 콘솔 출력은 모두 보조 기능이며,
이 기능들이 사출 판단과 작동을 막아서는 안 된다.

## 우선순위 규약

1. 사출 판단과 사출 작동을 최우선으로 둔다.
2. SD카드 기록, LoRa, MQTT, Wi-Fi/LTE, GPS 텔레메트리, LED, 부저, 콘솔은
   사출 경로를 블로킹하지 않아야 한다.
3. 센서 감시는 100Hz 이상을 기본 목표로 한다.
4. 저장과 통신은 큐나 별도 작업으로 분리해 지연이 생겨도 비행 판단에 영향을
   주지 않게 한다.
5. 실제 비행 적용 전에는 회로, 펌웨어, 전원부, 사출부를 별도로 검증해야 한다.

## 센서와 비행 판단

기본 설계 방향은 압력 센서, IMU, 배터리 전압, 사출 감지, GPS 최신값을 함께
다루는 것이다.

비행 중에는 발사 감지, 최고점 판단, 지연 사출, 착지 판단, 회수 모드 진입을
이벤트로 남긴다. 고도, 필터링된 속도, 가속도 크기, 발사/최고점 판단 점수,
GPS 속도와 방향처럼 후처리가 가능한 값은 가능한 한 뷰어나 분석 도구에서
계산한다.

## 로그 규약

로그는 텍스트보다 바이너리 형식을 우선한다.

- `LOG`: 고정 길이 센서 샘플
- `EVENT`: 발사, 최고점, 사출, 착지, 전압 변화, SD 오류, 파일 회전, 통신 상태,
  GPS fix/loss, 회수 모드 진입 같은 이벤트 기록

`LOG`에는 시간 코드, 시퀀스 번호, 상태 플래그, 압력, 온도, IMU 가속도/자이로,
최신 GPS 위도/경도/고도, GPS 플래그, 배터리 전압, 사출 센서 상태 같은
raw/minimal 값을 저장하는 방향으로 둔다.

## 로그 동작 흐름

패드 대기 또는 armed 상태에서는 최근 약 3초 분량의 100Hz RAM prebuffer를
유지한다. 평상시 저장은 10Hz 정도로 제한할 수 있다.

발사가 감지되면 발사 직전 prebuffer와 발사 이후 데이터를 100Hz로 저장한다.
최고점/사출 이후 하강 구간은 짧은 시간 단위 파일로 분리하는 방향을 둔다.

착지가 10초 이상 안정적으로 확인되면 고속 로그를 닫고, 회수/저전력 모드로
전환한다. 이 상태에서는 1Hz 수준으로 로그를 줄이고 GPS 위치를 LoRa와 MQTT로
전송하는 것을 목표로 한다.

## 통신과 회수

LoRa 433MHz는 지상국과 회수용 통신을 위한 기본 방향으로 둔다.
MQTT는 온보드 Wi-Fi/LTE 라우터를 통한 원격 모니터링 또는 회수 보조 용도로
사용할 수 있다.

통신 실패는 비행 판단 실패로 이어지면 안 된다. 통신은 가능한 경우 보내고,
안 되는 경우에는 로그와 사출 판단이 계속 살아 있어야 한다.

## 검증 기준

이 문서는 COMET_Avionics 전체 보드에 적용되는 설계 의도와 개발 규약이다.
각 보드는 실제 비행 적용 전 회로, 펌웨어, 전원부, 사출부를 별도로 검증해야
하며, README에 적힌 내용만으로 비행 검증 완료를 의미하지 않는다.

Temporary extraction/work folders such as `tmp/` and `tmp_pdf_kmg/` are not
tracked.
