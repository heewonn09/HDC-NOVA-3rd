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

<img width="1408" height="768" alt="Gemini_Generated_Image_l9orral9orral9or" src="https://github.com/user-attachments/assets/9755075a-2b86-4f91-accc-c9afcc141ac0" />

---

### 🔍 Architecture Overview

본 시스템은 **Rule-Based Intent Routing + RAG 구조**를 결합하여  
응답 속도, 비용, 정확도를 동시에 개선한 AI 챗봇 & IoT 제어 아키텍처입니다.

---

### ⚙️ 전체 처리 흐름

---

### 🚀 핵심 구성 요소

#### 1. Intent Routing Layer
- 사용자 입력을 **5ms 이내로 분류**
- 요청을 단순 요청 / 복잡 질의로 분기
- LLM 호출 여부를 사전에 결정하여 비용 최적화

---

#### 2. Fast Path (Non-LLM 처리)
- 단순 명령은 LLM을 거치지 않고 처리

**처리 방식**
- DB 조회  
- 외부 API 호출  
- MQTT 기반 IoT 제어  

**효과**
- ⚡ 응답 속도: 수십 ms  
- 💰 비용: 0 (LLM 미사용)

---

#### 3. RAG Pipeline (복잡 질의 처리)

**처리 흐름**
1. 사용자 질문 → Embedding 생성  
2. Vector DB (Pinecone)에서 유사 문서 검색  
3. Context + 질문 → LLM 전달  
4. 응답 생성  

**특징**
- Hallucination 감소  
- 도메인 기반 정확도 향상  

---

#### 4. Response & IoT Execution
- 💬 챗봇 응답 반환  
- 📡 MQTT 기반 IoT 디바이스 제어  

---

### ⚡ Key Design Decisions

- **LLM 최소 호출 구조**
  - 불필요한 비용 제거
  - 응답 속도 개선

- **Rule-Based + RAG Hybrid**
  - 단순 요청 → 빠르게 처리
  - 복잡 질의 → 정확하게 처리

- **IoT 제어 분리**
  - 챗봇 응답과 디바이스 실행을 독립적으로 처리 가능

---

### 📈 Performance Improvement

| 항목 | 기존 (LLM Only) | 개선 구조 |
|------|----------------|----------|
| 응답 속도 | 약 13초 | 약 80ms |
| 비용 | 높음 | 최소화 |
| 안정성 | 낮음 | 높음 |

---


<table>
<tr>
<td width="50%" valign="top">

### 🔹 1. Rule 기반 Intent Router  
(LLM 우회 구조)

👉 전체 요청 중 **약 70~85%를 LLM 없이 처리**

- 문자열 패턴 기반 Intent 추출  
- Slot Parsing (room, device_type, action, value)  
- 약 **30+ Intent 직접 처리**

</td>

<td width="50%" valign="top">

### 📌 처리 가능한 주요 Intent

- DEVICE_CONTROL  
- ENV_STATUS  
- NOTICE / COMPLAINT / RESERVATION  
- BILL 조회  
- FACILITY / SPACE 조회  
- 아파트 단지 데이터 약 30개 API Intent  

---

### 👍 효과

- LLM 호출 횟수 대폭 감소  
- 평균 응답속도 **~1.2s → ~80ms**

</td>
</tr>
</table>

### 🔹 2. LLM 호출 최적화 구조
ConcurrentHashMap<String, CacheEntry> llmCache;
동일 질문 캐싱 (TTL 기반)
시스템 프롬프트 메모리 캐싱
대화 히스토리 제한 (20개)

📊 최적화 결과

LLM 호출량 60~80% 감소
토큰 비용 약 70% 절감

---

### 🔹 3. RAG 기반 질의응답

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

---

### 🔹 4. IoT 제어 시스템 (MQTT 기반)

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

---

### 🔹 5. 대화 흐름 관리 (Session + Pending)

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

---

### 🔹 6. 대화 히스토리 최적화
HISTORY_LIMIT = 20;

👉 이유

토큰 비용 제한
LLM 컨텍스트 최적화

👉 효과

응답 속도 안정화
불필요한 토큰 사용 감소

---


### 5. 📊 성능 개선 결과
항목	개선 전	개선 후

LLM 호출 비율	100%	20~40%

평균 응답 속도	1~3초	50~200ms

토큰 비용	기준 100%	약 30%
캐시 히트율	없음	최대 60%

---

### 6. 🛠 트러블 슈팅
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
