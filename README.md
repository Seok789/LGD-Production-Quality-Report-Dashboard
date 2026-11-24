# LGD-Production-Quality-Report-Dashboard

<img width="1369" height="1050" alt="스크린샷 2026-11-24 오후 2 32 23" src="https://github.com/user-attachments/assets/b43524ee-f996-4706-b628-a9b08eddbf42" />


**날짜:** 09월 14일

## 사용 라이브러리

### Backend (Python)
* **Flask (`3.1.2`)**: 경량 웹 서버 및 REST API(`/api/view`, `/api/report`, `/api/download` 등) 구축
* **Werkzeug**: 백그라운드 스레드 기반 로컬 서버(`make_server`) 구동
* **IPython (`IPython.display`)**: Colab 환경 내 접속 링크 HTML 렌더링
* **Python 표준 라이브러리**: `asyncio`, `threading`, `json`, `pathlib`, `socket`, `subprocess`, `urllib.request`, `datetime`, `re`, `copy`

### Frontend (Web & 3D)
* **Three.js (`0.170.0`)**: WebGL 기반 3D 패널 검사·이송 설비 모형 렌더링
* **OrbitControls (`three/addons/OrbitControls.js`)**: 3D 뷰포트 마우스 드래그 회전, 휠 확대/축소 및 시점 제어
* **HTML5 / CSS3 / Vanilla JavaScript**: 대시보드 UI 구성, Fetch API 비동기 통신 및 동적 DOM 렌더링

### Tunneling & Runtime
* **Cloudflare Tunnel (`cloudflared`)**: 로컬 Flask 서버를 외부에서 접속 가능한 임시 HTTPS URL(`trycloudflare.com`)로 포트 포워딩

---

## 설명

Google Colab 환경에서 실행되는 **생산·품질 데이터 조회 및 근거 기반 보고서 작성 웹 애플리케이션**입니다. 교육용 가상 제조 데이터를 바탕으로 기간·라인별 지표와 설비 이력을 대조하고, 3D 설비 모형으로 현재 모의 상태를 확인한 뒤 보고서를 작성·보관할 수 있습니다.

### 주요 기능
1. **생산·품질 지표 및 근거 조회**
   * 시작일, 종료일, 생산라인(전체 / A / B) 조건에 따라 생산량·목표량·달성률·검사 수·불량 수·불량률을 집계합니다.
   * LOT별 상세 기록 테이블, 일자별 불량률(0~10% 눈금) 막대 차트, 조회 기간 내 설비 사건(해제/미해제) 이력을 함께 제공합니다.
2. **3D 패널 검사·이송 설비 시각화**
   * Three.js로 구현한 컨베이어, 검사 챔버, 작업대, 타워 램프 및 이동하는 디스플레이 패널을 브라우저에서 3D로 조작할 수 있습니다.
   * 선택된 라인의 현재 모의 상태(라인 A: 정상 28°C / 라인 B: 온도 주의 42°C)에 따라 설비 상단 타워 램프 색상이 실시간으로 연동됩니다.
3. **보고서 초안 생성, 검토 및 내보내기**
   * 현재 조회 범위의 수치와 사건 이력을 근거로 고정한 정형 보고서 초안을 자동 생성합니다(별도 LLM API 미사용).
   * 본문 수정 시 검토 체크박스가 자동 해제되어 재검토를 유도하며, 작성된 보고서는 `04_reports.json`에 영구 저장됩니다.
   * 저장된 보고서 본문(`.txt`)과 근거 데이터(`.json`)를 파일로 내려받을 수 있습니다.
4. **원클릭 실행 및 외부 접속**
   * 실행 시 필요한 패키지(Flask, Three.js, cloudflared)를 자동으로 내려받고, 사용 가능한 로컬 포트(8000~8020)를 탐지해 서버와 터널을 자동 실행합니다.

---

## 참고 문헌

* **Flask 공식 문서**: https://flask.palletsprojects.com/
* **Three.js 공식 문서 (WebGLRenderer, OrbitControls)**: https://threejs.org/docs/
* **Cloudflare Tunnel (`cloudflared`) 공식 문서**: https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/
* **MDN Web Docs (Fetch API, ES Import Maps, ResizeObserver)**: https://developer.mozilla.org/
* **실습 교재**: LG디스플레이 직무 이해 교육 과정 (LET’S GROW WITH LG DISPLAY · DAY 04)
