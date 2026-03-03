---

# HDC랩스 NOVA 3rd

HDC랩스 NOVA 3rd 레포지토리입니다.


## 🛠 Tech Stack

### Frontend & App

### Backend

### Database & Real-time

### AI & Infra

---

## 훈련생들에게 보여주기 위한 템플릿

---

# 프로젝트 계획서

## 1. 프로젝트 개요

* **프로젝트명**: IoT와 AI를 결합한 스마트 아파트 통합관리 플랫폼 (NOVA)
* **목표**: 입주민 앱과 관리자 웹을 연동하여 주거 환경 제어 및 단지 운영을 효율화하는 올인원 시스템 구축
* **기간**: 2026년 1월 - 2026년 3월

## 2. 프로젝트 일정

* **분석 및 설계**: 1주차 - 요구사항 수집 및 시스템 아키텍처 설계
* **개발**: 2~5주차 - 기능별 스프린트 개발 및 하드웨어(Arduino) 연동
* **테스트 및 배포**: 6주차 - Redis 기반 성능 최적화 및 CI/CD 배포 자동화

---

## 3. 팀 구성

## 👥 TEAM 파이브가이즈

<p align="center">
  Backend · Frontend · IoT · Infra · AI Pipeline
</p>

---

### 안창석 | Team Leader
<p align="center">
  <a href="https://github.com/아이디">
    <img src="https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true&theme=default" height="150"/>
  </a>
</p>

- 🔹 관리자 인증 시스템 및 보안 로직 수립  
- 🔹 민원 및 관리비 결제(PDF/Excel) 구현  
- 🔹 QueryDSL 기반 DB 조회 성능 최적화  
- 🔹 관리자 전용 웹 대시보드 구현  

---

### 이희원 | AI & Design
<p align="center">
  <a href="https://github.com/아이디">
    <img src="https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true&theme=default" height="150"/>
  </a>
</p>

- 🔹 AI 파이프라인 설계 및 총괄 관리  
- 🔹 Gemini, OpenRouter 기반 챗봇 구축  
- 🔹 Pinecone 활용 RAG 파이프라인 구축  
- 🔹 앱 챗봇 UI 구현  
- 🔹 서비스 GUI 및 물리 구조물 설계  

---

### 양준길 | Infra & Backend
<p align="center">
  <a href="https://github.com/아이디">
    <img src="https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true&theme=default" height="150"/>
  </a>
</p>

- 🔹 CI/CD 파이프라인 및 Docker Compose 구축  
- 🔹 JWT 및 OAuth 2.0 기반 로그인 시스템 구현  
- 🔹 시설 예약, QR 출입 시스템, FCM 푸시 알림 구현  
- 🔹 부하 테스트 및 API 성능 병목 개선  

---

### 천경신 | AI & IoT
<p align="center">
  <a href="https://github.com/아이디">
    <img src="https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true&theme=default" height="150"/>
  </a>
</p>

- 🔹 화재 및 가스 감지 로직 및 MQTT 통신 구축  
- 🔹 Hugging Face 기반 음성인식 아키텍처 구현  
- 🔹 통합 관제 대시보드 및 공지사항 웹/앱 기능 개발
- 🔹 MQTT 디바이스 제어 연동
- 🔹 웹 인터페이스 디자인

---

### 최우영 | IoT & Hardware
<p align="center">
  <a href="https://github.com/아이디">
    <img src="https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true&theme=default" height="150"/>
  </a>
</p>

- 🔹 디바이스 상태 API 및 환경 센서 데이터 연동  
- 🔹 사용자 설정 모드 스케줄러(Automation) 구현  
- 🔹 홈/디바이스 제어 UI 및 OpenWeather API 호출  
- 🔹 IoT 물리 구조물 설계 및 하드웨어 구축

---

# 요구사항 정의서

## 1. 기능적 요구사항

* **사용자 관리**: 관리자 세대 등록 기반 입주민 회원가입 및 OAuth 간편 로그인
* **홈 IoT 제어**: MQTT 기반 실시간 기기 제어(LED, FAN) 및 온·습도 스케줄링
* **커뮤니티 예약**: 시설 예약 알림, QR 스캔을 통한 물리적 출입 통제 연동
* **지능형 인터페이스**: "하이 노바" 호출어 기반 음성 제어 및 RAG 챗봇 연동
* **안전 대응**: 가스/온도 센서 화재 감지 시 관리자 관제 알림 및 시설 자동 차단

## 2. 비기능적 요구사항

* **응답 시간**: 외부 API 호출 구간 Redis 캐싱 적용으로 로그인 응답성 확보
* **성능**: 1,000명 이상의 동시 접속 환경에서 실시간 기기 상태 동기화 유지
* **보안**: 관리자 시스템 접근을 위한 Stateless OTP 및 권한 분리 적용

---

# WBS

## 1. 프로젝트 분석

* 요구사항 수집 및 유즈케이스 정의
* 클라우드 인프라 및 네트워크 토폴로지 설계
* 하드웨어 제어 프로토콜(MQTT) 정의

## 2. 시스템 개발

* **Backend**: Spring Boot 기반 API 서버 및 QueryDSL 검색 최적화
* **Frontend**: React 기반 관리자 대시보드 및 React Native 입주민 전용 앱
* **AI**: Gemini 기반 RAG 파이프라인 및 Whisper STT 음성 비서 구현

## 3. 테스트 및 배포

* **성능 테스트**: k6/JMeter를 활용한 부하 테스트 및 Redis 지표 확인
* **배포**: GitHub Actions를 통한 Docker 이미지 빌드 및 서버 자동 배포
* **모니터링**: Prometheus/Grafana를 활용한 가용성 모니터링 구축

---

# 모델 정의서

## 1. 데이터 모델

* **사용자 정보 (User Entity)**
* `user_id` (PK), `email`, `role` (ADMIN, USER), `apt_unit`


* **기기 제어 (Device Entity)**
* `device_id` (PK), `type`, `current_status`, `target_value`


* **예약 로그 (Reservation Entity)**
* `res_id` (PK), `facility_id`, `user_id`, `qr_hash`



## 2. 객체 모델

* **AI 챗봇 객체**
* 속성: `query_text`, `embedding_vector`, `retrieved_docs`
* 메소드: `searchVectorDB()`, `generateNaturalResponse()`



---

# 성능 평가 결과서

## 1. 테스트 환경

* **인프라**: AWS EC2 c5.large (Docker 가상화)
* **DB**: MariaDB 10.6, Redis 7.0, Pinecone
* **도구**: Grafana Dashboard

## 2. 테스트 결과

* **응답 시간 (p95)**: 외부 날씨 API 지연 문제를 Redis 캐싱으로 극복
* 개선 전: **476ms** → 개선 후: **53ms** (**약 9배 향상**)


* **실시간성**: MQTT 제어 명령 지연 100ms 이내 유지 성공
* **AI 정확도**: RAG 도입 후 아파트 수칙 답변 정확도 95% 달성

## 3. 성능 개선 필요 사항

* 대규모 트래픽 대비 MQTT 클러스터링 구성 필요
* 지속적인 DB 인덱싱 최적화 및 슬로우 쿼리 모니터링

---

# 최종 보고서

## 1. 프로젝트 개요

* **목표**: IoT 실시간 제어와 AI 지능형 상담이 결합된 차세대 스마트 아파트 관리 플랫폼
* **기간**: 2026년 1월 - 2026년 3월

## 2. 주요 성과

* 입주민 앱과 관리자 웹의 실시간 데이터 연동 및 동기화 완성
* STT/TTS 기반 음성 인터페이스 및 RAG 챗봇 서비스 상용 수준 구현
* 화재 센서-알림-시설 차단으로 이어지는 안전 자동화 시나리오 구축

## 3. 향후 개선 사항

* 확장성을 고려한 MSA(Microservices Architecture)로의 점진적 전환
* 입주민 편의를 위한 관리비 자동 정산 및 단지 내 중고거래 기능 추가

---

# 회고

## 1. 잘된 점

* **협업**: Git Flow와 PR 체크리스트를 통해 코드 품질 및 개발 속도 균형 확보
* **최적화**: 데이터 기반으로 병목 지점을 파악하고 Redis로 성능을 직접 개선한 경험

## 2. 개선할 점

* **초기 설계**: 하드웨어 연동 시의 물리적 예외 상황을 설계 단계에서 더 보완할 필요 있음
* **테스트**: 하드웨어 실기기 연동 테스트 시나리오를 더 체계화해야 함

## 3. 교훈

* **명확한 목표**: 사용자 여정(User Journey)을 먼저 정의한 것이 효율적인 개발의 핵심이었음
* **기술 적합성**: 실시간성은 MQTT, 고성능은 Redis 등 문제 해결에 최적인 기술 스택 선정의 중요성 체감

---



