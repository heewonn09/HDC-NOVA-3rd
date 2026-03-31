# 🤖 AI 챗봇 & IoT 제어 시스템

> 자연어 기반 질의응답 + IoT 디바이스 제어를 통합한 AI 시스템  
> Rule 기반 Intent Router + RAG 구조를 결합하여  
> **LLM 호출 비용 절감 + 응답 속도 최적화**를 달성

<br />

## 1. 📅 제작 기간 & 역할
> **2026.01.14 - 02.28**  
> **5인 팀 프로젝트**

### 🔑 핵심 역할
- 챗봇 Core Engine (ChatService) 단독 설계 및 구현  
- Rule 기반 Intent 분기 시스템 구축  
- RAG + Pinecone 벡터 검색 파이프라인 설계  
- 자연어 → IoT 제어(MQTT) end-to-end 흐름 구현  
- LLM 호출 최적화 및 캐싱 전략 설계  

---

## 2. 🛠 Tech Stack

### 🔧 BackEnd
![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-6DB33F?logo=springboot&logoColor=white)
![Spring AI](https://img.shields.io/badge/Spring_AI-LLM-green)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?logo=mariadb&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT-Mosquitto-3C5280)

### 🤖 AI
![RAG](https://img.shields.io/badge/RAG-Architecture-blue)
![Gemini](https://img.shields.io/badge/Gemini-LLM-4285F4)
![Embedding](https://img.shields.io/badge/Text_Embedding-Vector-green)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-black)

### ⚡ IoT
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![RaspberryPi](https://img.shields.io/badge/Raspberry_Pi-A22846?logo=raspberrypi)

---

### ⚡ IoT
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![RaspberryPi](https://img.shields.io/badge/Raspberry_Pi-A22846?logo=raspberrypi)

---

## 3. 🏗 시스템 아키텍처


User Input
↓
Rule-Based Intent Parser (0ms ~ 5ms)
↓
[단순 요청] → DB / API / MQTT 처리 (No LLM)
[복잡 질의] → RAG (Embedding → Pinecone → LLM)
↓
Response 반환
↓
IoT 제어 (MQTT)


---

## 4. ⚙️ 핵심 설계 (Core Architecture)

### 🔹 1. Rule 기반 Intent Router (LLM 우회 구조)

👉 전체 요청 중 **약 70~85%를 LLM 없이 처리**

- 문자열 패턴 기반 Intent 추출  
- Slot Parsing (room, device_type, action, value)  
- 약 **30+ Intent 직접 처리**


---

### 📌 처리 가능한 주요 Intent:

DEVICE_CONTROL
ENV_STATUS
NOTICE / COMPLAINT / RESERVATION
BILL 조회
FACILITY / SPACE 조회

👉 효과

LLM 호출 횟수 대폭 감소
평균 응답속도 ~1.2s → ~80ms

### 🔹 2. LLM 호출 최적화 구조
ConcurrentHashMap<String, CacheEntry> llmCache;
동일 질문 캐싱 (TTL 기반)
시스템 프롬프트 메모리 캐싱
대화 히스토리 제한 (20개)

📊 최적화 결과

LLM 호출량 60~80% 감소
토큰 비용 약 70% 절감
🔹 3. RAG 기반 질의응답

구성:

Embedding → OpenRouter API
Vector DB → Pinecone
ragAnswerService.tryAnswer(...)

동작:

사용자 질문 임베딩
Pinecone Top-K (기본 5) 검색
LLM 응답 생성

👉 특징

아파트 전용 지식 (공지, 규칙, FAQ)
sourceType 기반 필터링 (GUIDE / RULE / FAQ)
🔹 4. IoT 제어 시스템 (MQTT 기반)

핵심 흐름:

handleDeviceControl(...)

📌 처리 과정:

자연어 → Intent + Slots 변환
Room / Device DB 조회
MQTT 메시지 생성
디바이스 제어 요청 전송
DB 상태 즉시 동기화
topic = "hdc/{hoId}/room/{roomId}/device/execute/req"

📊 특징:

TraceId 기반 명령 추적
DeviceCommandLog 기록
상태 동기화 (eventual consistency 방지)

👉 응답 속도: 50~100ms 수준

🔹 5. 대화 흐름 관리 (Session + Pending)
ChatSession / ChatMessage 구조

기능:

세션 기반 대화 유지
pending intent 상태 관리
follow-up 질문 처리

예:

"온도 알려줘"
→ "어느 방?"

"거실"
→ 이어서 처리
🔹 6. 대화 히스토리 최적화
HISTORY_LIMIT = 20;

👉 이유

토큰 비용 제한
LLM 컨텍스트 최적화

👉 효과

응답 속도 안정화
불필요한 토큰 사용 감소


## 5. 📊 성능 개선 결과
항목	개선 전	개선 후

LLM 호출 비율	100%	20~40%
평균 응답 속도	1~3초	50~200ms
토큰 비용	기준 100%	약 30%
캐시 히트율	없음	최대 60%

## 6. 🛠 트러블 슈팅
⚡ 문제: LLM 의존 구조
모든 요청을 LLM 처리
비용 증가 + 응답 지연
✅ 해결
Rule 기반 분기
→ 약 30개 Intent 직접 처리
RAG 분리
→ 지식형 질문만 LLM 사용
캐싱 전략
→ 동일 질문 LLM 호출 방지
🎯 결과
응답 속도 최대 90% 개선
LLM 호출량 최대 80% 감소
사용자 체감 속도 대폭 개선
