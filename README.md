# 🤖 AI 챗봇 & IoT 제어 시스템
> 자연어 기반 질의응답 + IoT 디바이스 제어를 통합한 AI 시스템  
> Rule 기반 Intent Router + RAG 구조를 결합하여  
> **LLM 호출 비용 절감 + 응답 속도 최적화**를 달성

<br />

## 1. 📅 제작 기간 & 인원 별 역할
> **2026.01.14 - 02.28**  
> **5인 팀 프로젝트**

### 핵심 역할
- 챗봇 Core Engine (ChatService) 단독 설계 및 구현
- Rule 기반 Intent 분기 및 패턴매칭 시스템 구축
- RAG + Pinecone 벡터 검색 파이프라인 설계 및 최적화
- 자연어 → IoT 제어(MQTT) end-to-end 흐름 구현
- LLM 호출 최적화 (캐싱 전략, History 제한, System Prompt 관리)

---

## 2. 🛠 Tech Stack

> ### 🔧 BackEnd
> ![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
> ![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-6DB33F?logo=springboot&logoColor=white)
> ![Spring AI](https://img.shields.io/badge/Spring_AI-LLM-green)
> ![Gradle](https://img.shields.io/badge/Gradle-02303A?logo=gradle&logoColor=white)
> ![MariaDB](https://img.shields.io/badge/MariaDB-003545?logo=mariadb&logoColor=white)
> ![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
> ![MQTT](https://img.shields.io/badge/MQTT-Mosquitto-3C5280?logo=eclipsemosquitto&logoColor=white)

> ### 🤖 AI
> ![RAG](https://img.shields.io/badge/RAG-Architecture-blue)
> ![Gemini](https://img.shields.io/badge/Gemini-LLM-4285F4?logo=google-gemini&logoColor=white)
> ![Embedding](https://img.shields.io/badge/Text_Embedding-Vector-green)
> ![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-black)

> ### ⚡ IoT
> ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
> ![RaspberryPi](https://img.shields.io/badge/Raspberry_Pi-A22846?logo=raspberrypi)

---

## 3. 🏗 시스템 아키텍처

<img width="1408" height="768" alt="Gemini_Generated_Image_l9orral9orral9or" src="https://github.com/user-attachments/assets/735c55f8-c8eb-4e51-bbaa-48c0aec23931" />

---

### 🔍 Architecture Overview

### 🎯 설계 목표

- LLM 의존도 최소화
- 실시간 IoT 제어 (100ms 이하)
- 도메인 특화 응답 정확도 확보

본 시스템은 **Rule-Based Intent Routing + RAG 구조**를 결합하여 응답 속도, 비용, 정확도를 동시에 개선했습니다.

---

### 🗂️ ERD (Entity Relationship Diagram)

<img width="723" height="593" alt="image" src="https://github.com/user-attachments/assets/53cbf262-5da3-4556-8302-312a1786c055" />


## 4. 🚀 핵심 기능 & 흐름(Flow)

### 🔹 1. Rule 기반 Intent Router 및 Fast Path 처리

👉 전체 요청 중 **약 70~85%를 LLM 없이 처리**하는 LLM 우회 구조

- **문자열 패턴 매칭:** 사용자 입력에서 사전 정의된 문자열 패턴을 기반으로 Intent를 빠르게 추출 (5ms 미만).
- **Slot Parsing:** 추출된 Intent에 따라 필수 인자 (room, device_type, action, value 등)를 파싱.
- **주요 Intent 처리:** DEVICE_CONTROL (전등 켜줘, 온도 낮춰줘 등), ENV_STATUS (현재 거실 온도 알려줘 등), NOTICE/COMPLAINT (공지사항 조회, 민원 접수), BILL (관리비 조회), FACILITY/SPACE (체육시설 운영시간 등) 등 약 30+가지 아파트 관련 API Intent를 직접 처리.
- **동작 방식 (Fast Path):** 단순 패턴 매칭된 요청은 LLM을 거치지 않고 DB 조회, 외부 API 호출, MQTT 기반 IoT 제어 로직으로 즉시 분기.
- **효과:** 평균 응답속도를 **~1.2s에서 ~80ms로 단축**하고 LLM 호출 비용을 제거했습니다.

### 🔹 2. LLM 호출 최적화 및 캐싱 전략

👉 LLM 성능은 유지하되 비용과 대기 시간을 줄이는 구조

- **TTL 기반 동일 질문 캐싱:** ConcurrentHashMap<String, CacheEntry>를 활용하여 동일 질문에 대한 LLM 응답을 TTL 기간 동안 캐싱.
- **System Prompt 캐싱:** 빈번하게 사용되는 시스템 프롬프트를 메모리에 캐싱하여 토큰 사용 최적화.
- **대화 히스토리 제한:** 문맥 유지를 위한 대화 히스토리를 최근 20개로 제한하여 불필요한 토큰 사용과 응답 지연 방지.
- **효과:** LLM 호출량을 60~80% 감소시키고 전체 토큰 비용을 약 70% 절감했습니다.

### 🔹 3. RAG 기반 질의응답 (복잡 질의 처리)

👉 단순 패턴 매칭으로 처리할 수 없는 아파트 전용 지식에 대한 정확한 응답 제공

- **동작 흐름:** 사용자 질문 임베딩 -> Vector DB (Pinecone) Top-K 검색 -> Source Filtered Context (GUIDE/RULE/FAQ 등) 추출 -> Context + 질문 기반 LLM 응답 생성.
- **핵심 기술:** OpenRouter API 기반 Embedding, Pinecone Vector DB, ragAnswerService 활용.
- **특징:** 아파트 전용 공지사항, 생활 규칙, FAQ 데이터를 활용하여 정확한 도메인 기반 지식을 제공하며 Hallucination을 대폭 감소시켰습니다.

### 🔹 4. MQTT 기반 IoT 제어 시스템

👉 자연어 명령을 IoT 디바이스 실행으로 End-to-End 연결

- **handleDeviceControl Flow:** 파싱된 Intent + Slots (예: 거실, 전등, 끄기) 활용 -> Room/Device DB 조회 및 권한 검증 -> MQTT 메시지 생성 -> 디바이스 제어 요청 전송 (topic = "hdc/{hoId}/room/{roomId}/device/execute/req") -> DB 상태 즉시 동기화.
- **안정성 확보:** TraceId 기반 명령 추적 및 DeviceCommandLog 기록으로 eventual consistency 문제 방지 및 이력 관리.
- **효과:** 자연어 명령 후 50~100ms 이내에 실제 디바이스 제어가 이루어지는 고속 처리 환경 구현.

### 🔹 5. 대화 흐름 및 Session 관리

- **구조:** ChatSession / ChatMessage 구조 설계.
- **핵심 기능:** 세션 기반 대화 context 유지, **Pending Intent 상태 관리**를 통한 Follow-up 질문 처리.
- **예시:** "온도 알려줘" (Pending: "어느 방?") -> "거실" (처리 완료) 형태로 대화형 사용자 경험 제공.

---

## 5. 📊 성능 개선 결과

| 항목 | 개선 전 (LLM Only) | 개선 후 (Hybrid 구조) | 개선율 |
|:---|:---|:---|:---|
| **평균 응답 속도** | 약 1~3초 (LLM 처리 대기) | **약 50~200ms** | **최대 93.3% 단축** |
| **토큰 비용** | 기준 100% | **약 30%** | **약 70% 절감** |
| **LLM 호출 비율** | 100% | **20~40%** | **최대 80% 감소** |
| **캐시 히트율** | 없음 | **최대 60%** | - |
| **안정성** | 낮음 (LLM 장애 시 마비) | **높음** (단순 기능 상시 작동) | - |

---

### 부록
[TEAM 버전 통합 README 문서](https://github.com/wnsrlf0721/HDC-NOVA-3rd/blob/main/README_TEAM.md)
