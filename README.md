> **포트폴리오 보관용 사본입니다.** 원본 저장소: https://github.com/Taegyu-Lee1117/Rocycle
> 커밋 기록은 원본 그대로 옮겼습니다. 팀 프로젝트이며, 아래는 제(김도윤) 담당 범위입니다.
>
> **담당:** 로봇 제어 · 추적 및 파지 — 캘리브레이션, 칼만 추적, M0609·RG2·컨베이어 연동, 분류 정책

---

# ROCYCLE — 재활용품 분류 협동로봇

컨베이어로 흘러오는 재활용품을 카메라로 인식하고, 로봇팔이 **도착 시점의 위치를 예측해**
움직이는 상태 그대로 집어 분류 통에 넣는 협동로봇 시스템.

두산로보틱스 ROKEY 9기 협동 프로젝트 2 · 2026.08~09 · 4인

`Doosan M0609` · `OnRobot RG2` · `ROS 2` · `YOLO` · `OpenCV` · `Kalman Filter` ·
`Docker` · `PostgreSQL` · `FastAPI` · `Python`

## 시연

**[시연 영상 (YouTube)](https://youtu.be/MHQZOGPDLWQ)**

| | |
|---|---|
| 작업 셀 | [hardware.png](https://raw.githubusercontent.com/tnduaehowl12/portfolio-assets/main/rocycle/hardware.png) |
| 시스템 구조 | [system_architecture.png](https://raw.githubusercontent.com/tnduaehowl12/portfolio-assets/main/rocycle/system_architecture.png) |
| 처리 파이프라인 | [pipeline_8steps.png](https://raw.githubusercontent.com/tnduaehowl12/portfolio-assets/main/rocycle/pipeline_8steps.png) |
| 공정도 | [process_chart.png](https://raw.githubusercontent.com/tnduaehowl12/portfolio-assets/main/rocycle/process_chart.png) |
| 캘리브레이션 개선 | [calibration_improvement.png](https://raw.githubusercontent.com/tnduaehowl12/portfolio-assets/main/rocycle/calibration_improvement.png) |

## 최종 코드는 `Final/` 입니다

제출 시점의 배포본을 `Final/` 에 따로 담았습니다. 루트에도 같은 구조가 있지만,
**완성된 상태로 보시려면 [`Final/`](Final) 을 보세요.**

```text
Final/
├── rocycle_robot/     ROS 2 패키지 — 추적 · 예측 파지 · 분류 정책
├── control_pc/        관제 PC 쪽 노드          (루트에만 있음)
├── calib_capture/     카메라–로봇 캘리브레이션
├── models/            YOLO 가중치
├── DB/                PostgreSQL 스키마 (rocycle_project_schema.sql)
├── web_api/           FastAPI — 상태·통계 API
├── web_ui/            브라우저 UI (대시보드 · 통계)
├── docker/            실행 환경
├── config/            좌표 · 임계값 설정
├── docs/              기획 · 명세 · 인터페이스 규격
└── handover/          인수인계 문서
```

## 담당 부분에서 다룬 것

- **실측 기반 캘리브레이션** — 솔버 버그 수정 + 실측 5점 평면 보정 → 벨트 평면 RMS 4.5 mm(적합 5점 기준)
- **칼만 추적과 예측 파지** — 컨베이어 위 물체의 도착 시점 위치를 예측해 멈추지 않고 집기
- **예측 파지 결함 수정** — 파지 동작 중에 쌓인 검출 결과가 예측을 밀어내던 문제를 걸러냄
- **판단 불가 물체 처리** — 무게 초과 · 재질 불명 · 미학습 물체는 확인 통이나 사람에게 전달

7종 물체 연속 완주 시험에서 파지 실패 · 배치 오탐 0건,
실패 경로(무게 차단 · 미수령 · 미학습 통과)도 설계대로 동작했습니다.

## 실행

환경 변수는 `.env.example` 을 복사해 채웁니다. DB 스키마는 `DB/` 에 있습니다.
자세한 실행 순서와 인터페이스 규격은 `docs/` 와 `handover/` 를 보세요.
