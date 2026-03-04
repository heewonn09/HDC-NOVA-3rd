<div align="center">

# 🏠 NOVA - 스마트 아파트 통합관리 플랫폼
### Team Five Guys

**IoT 센서 + AI 음성비서 기반 스마트홈 통합 관리 시스템**

입주민 모바일 앱과 관리자 웹을 통해  
주거 환경 제어 · 시설 예약 · 안전 관제를 제공하는 **스마트 아파트 플랫폼**

📅 **개발 기간**  
2026.01.14 ~ 2026.02.28  

🎓 **HDC Labs 스마트 IoT 풀스택 개발자 과정**

</div>

---

# 📌 Project Overview

**NOVA**는 IoT 디바이스와 AI 기술을 결합하여  
아파트 관리와 입주민 편의를 동시에 제공하는 **스마트 아파트 통합 플랫폼**입니다.

입주민은 모바일 앱을 통해

- 🏠 홈 IoT 제어
- 📅 커뮤니티 시설 예약
- 🤖 AI 챗봇 문의
- 📢 공지사항 확인
- 💳 관리비 조회

등의 기능을 사용할 수 있으며,

관리자는 웹 대시보드를 통해

- 세대 관리
- 민원 관리
- 시설 관리
- 안전 관제

등 단지 운영을 관리할 수 있습니다.

---

# 🚀 Key Features

| 기능 | 설명 |
|-----|-----|
| 🏠 Home IoT | MQTT 기반 LED / FAN 실시간 제어 |
| 🤖 AI Chatbot | Pinecone 기반 RAG 챗봇 |
| 🎤 Voice Assistant | WakeWord 기반 음성 디바이스 제어 |
| 📅 Facility Reservation | QR 기반 시설 출입 시스템 |
| 🔥 Fire Detection | 가스 / 온도 센서 기반 자동 관제 |
| 📊 Admin Dashboard | 세대 / 민원 / 관리비 관리 |

---

# 🎬 Demo Scenario

### 1️⃣ 화재 감지


Gas Sensor → MQTT → Safety Service
→ 관리자 관제 시스템
→ FCM Push 알림


센서 임계값 초과 시  
실시간 관제 화면에서 **DANGER 상태 표시 및 푸시 알림**

---

### 2️⃣ 음성 디바이스 제어


WakeWord ("하이 노바")
→ STT (Whisper)
→ LLM Intent 분석
→ MQTT
→ IoT Device Control


예시


"하이 노바, 거실 불 켜줘"


→ LED ON

---

### 3️⃣ RAG 챗봇


User Question
↓
Embedding
↓
Pinecone Vector Search
↓
Relevant Document Retrieval
↓
LLM Response


아파트 규칙 및 시설 정보를 기반으로  
**맥락 기반 답변 제공**

---

# 🏗 System Architecture

<img width="1095" height="602" src="https://github.com/user-attachments/assets/15d59f28-63a9-4854-995c-c6c7a48aa72d">

---

# 📱 Service Screens

<img width="2396" height="1287" src="https://github.com/user-attachments/assets/3dc8e47e-50e3-45d0-a1d2-9aa54c1045f5">

<img width="2415" height="1284" src="https://github.com/user-attachments/assets/8057825c-6e6d-4d57-adce-8269d653a64a">

---

# 🛠 Tech Stack

## Frontend

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![React Native](https://img.shields.io/badge/React_Native-0.81-61DAFB?logo=react&logoColor=white)
![Expo](https://img.shields.io/badge/Expo-54-000020?logo=expo&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-7-646CFF?logo=vite&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)

---

## Backend

![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-6DB33F?logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?logo=springsecurity&logoColor=white)
![QueryDSL](https://img.shields.io/badge/QueryDSL-0769AD)

---

## Database & Messaging

![MariaDB](https://img.shields.io/badge/MariaDB-003545?logo=mariadb&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT-Mosquitto-3C5280)

---

## AI

![Gemini](https://img.shields.io/badge/Gemini-AI-4285F4)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-black)
![Whisper](https://img.shields.io/badge/Whisper-STT-green)

---

## DevOps / Infra

![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?logo=grafana&logoColor=white)

---

# ⚙ Key Technical Implementation

## RAG 기반 AI 챗봇

- OpenAI Embedding 기반 벡터 생성
- Pinecone Top-K 유사도 검색
- Gemini LLM 응답 생성

---

## MQTT 기반 IoT 실시간 제어

기존 Polling 방식의 한계를 해결하기 위해


REST Polling → MQTT WebSocket


구조로 변경하여  
**실시간 디바이스 제어 시스템 구축**

---

## QueryDSL 동적 쿼리

복잡한 검색 조건을 처리하기 위해

- QueryDSL 기반 동적 쿼리 구현
- 관리자 대시보드 검색 성능 개선

---

# 📊 Performance Optimization

외부 API 호출 병목 문제 해결

| 구분 | 응답시간 |
|-----|-----|
| Before | 476ms |
| After | 53ms |

➡ **약 9배 성능 개선**

Redis 캐싱을 적용하여  
OpenWeather API 호출 성능을 개선했습니다.

---

# 🧩 Infrastructure

Docker Compose 기반 통합 인프라 구성


React
Spring Boot
Mosquitto
Redis
Prometheus
Grafana
InfluxDB
Loki
Promtail
Nginx


CI/CD


GitHub Push
↓
Build & Test
↓
Docker Compose Deploy


---

# 👨‍👩‍👧‍👦 Team

| 이름 | 역할 |
|-----|-----|
| 안창석 | 관리자 인증 · 보안 · 민원 시스템 |
| 이희원 | AI 챗봇 · RAG 파이프라인 · 앱 챗봇 UI |
| 양준길 | CI/CD · 로그인 인증 · 시설 예약 |
| 천경신 | 화재 감지 시스템 · 음성 인식 |
| 최우영 | IoT 디바이스 제어 · 센서 연동 |

---

# 🏗 Hardware Prototype

### 전체

<img width="1332" height="644" src="https://github.com/user-attachments/assets/b35429ef-e9cc-4a6c-aafa-c3c71e6165da">

### AI 스피커 & 시설 출입

<img width="837" height="699" src="https://github.com/user-attachments/assets/2f442a9d-7a86-46e5-9372-8db8942f9235">

### 집 내부 환경

<img width="1509" height="674" src="https://github.com/user-attachments/assets/ce199543-6589-427e-8b24-04970d20f27e">

---

# 🎬 Demo Video

[![Demo Video](https://github.com/user-attachments/assets/aa4c51b1-60d9-4927-bcee-78aff0781712)](https://drive.google.com/file/d/1b-vz5VNmS-BF1424KBxGbMl939a8d0pX/view)
