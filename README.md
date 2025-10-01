# VGA_Turret
🔴 VGA 영상처리 방산형 터렛 (Basys3 + OV7670 + STM32)
Vision + FPGA + MCU + PC HUD: OV7670 카메라로 빨간색(RED)을 감지해 3×3 구역 타겟을 만들고, Basys3가 9비트 패턴을 송신 → STM32가 서보·모터로 터렛 시퀀스를 수행 → PC(PyQt HUD)가 상황을 시각화합니다.

    

✨ 개요
주제: RED 색상 자동 식별 및 3×3 구역 기반 터렛 제어
영상처리: RGB565 프레임버퍼에서 9구역 ROI별 RED 우세성(5×5 윈도 평균) 판단
표시: Basys3 VGA 640×480(×2 스케일) + GUI 오버레이 옵션
통신: Basys3 → STM32 9N1(9-bit) UART, PC HUD는 시리얼 패킷(저/고/\n) 수신 표시
제어: STM32가 최소 이동 경로 기반으로 목표 구역을 순차 방문·발사, 부드러운 서보 이동
🧭 시스템 아키텍처
image
🎥 피피티 데모 영상
<영상: https://drive.google.com/file/d/1BzQ-j-We_yZgZU4CgEkCghIwKlL28Flg/view?usp=sharing>

🧾 9비트 UART 프로토콜
프레임: 9N1 (9 data, no parity, 1 stop)
비트 맵(MSB→LSB): [TL, TM, TR, ML, MM, MR, BL, BM, BR]
예: 9'b101_101_101 (1,3,4,6,7,9 구역)
STM32는 USART1 RX 전용(9-bit) 로 수신, PC HUD는 (저/고/\n) 3바이트 패킷을 읽어 9비트로 재조합(테스트/모니터링용).

🧑‍🤝‍🧑 팀원 소개
image image
프로젝트 전반에 걸쳐 각자의 역할을 맡아 협력하여 진행하였습니다.

📅 일정 계획
image image
🏆 결과
image
🛠️ 빌드 & 실행
1) FPGA (Vivado)
보드: Digilent Basys3 (Artix‑7)
소스 추가: .sv + Basys-3-Master.xdc
합성/구현/비트스트림 → 프로그래밍
2) STM32 (STM32CubeIDE)
보드: Nucleo‑F401RE/F411RE 등
USART1: 9-bit, Parity None, Stop 1, RX only, Baud 115200 권장
PWM 타이머: Pan/Tilt/Fire 서보, 모터 채널 설정
3) PC HUD (Python 3.9+)
pip install pyqt5 opencv-python pyserial numpy
python main.py
