# 🏢 스마트 아파트 통합관리 플랫폼
> 아파트 입주민, 관리인 위한 App / Web 서비스
<br />

## 1. 📅 제작 기간 & 인원 별 역할
> **2026.01.14 - 02.28**  
> **5인 팀 프로젝트**  
> Front/Back 분야 별 구분 없이 기능 별로 역할을 나누어 **API, 페이지, IoT** 풀스택 분담
> ### 핵심 역할
> - 기능 구현: **입주민 로그인, 커뮤니티 시설 예약, QR 출입 제어, FCM 알림**
> - 서비스 배포: **On-Premise 기반 서버 인프라 배포 및 운영 관리**


## 2. 🛠 Tech Stack
> ### BackEnd
> ![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
> ![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-6DB33F?logo=springboot&logoColor=white)
> ![Gradle](https://img.shields.io/badge/Gradle-02303A?logo=gradle&logoColor=white)
> ![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?logo=springsecurity&logoColor=white)
> ![JWT](https://img.shields.io/badge/JWT-000000?logo=jsonwebtokens&logoColor=white)
> ![OAUTH](https://img.shields.io/badge/OAuth-2.0-4285F4?logo=googleauthenticator&logoColor=white)
> ![MariaDB](https://img.shields.io/badge/MariaDB-10.11-003545?logo=mariadb&logoColor=white)
> ![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
> ![MQTT](https://img.shields.io/badge/Mosquitto-3C5280?logo=eclipsemosquitto&logoColor=white)

> ### Mobile App
> ![React Native](https://img.shields.io/badge/React_Native-0.81-61DAFB?logo=react&logoColor=white)
> ![Expo](https://img.shields.io/badge/Expo-54-000020?logo=expo&logoColor=white)
> ![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)
> ![Axios](https://img.shields.io/badge/Axios-5A29E4?logo=axios&logoColor=white)
> ![Firebase](https://img.shields.io/badge/Firebase-DD2C00?logo=firebase&logoColor=white)

> ### IoT Device
> ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
> ![RaspberryPi](https://img.shields.io/badge/Raspberry_Pi-4B-A22846?logo=raspberrypi&logoColor=white)

> ### DevOps / Infra
> ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white)
> ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
> ![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)
> ![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?logo=prometheus&logoColor=white)
> ![Grafana](https://img.shields.io/badge/Grafana-F46800?logo=grafana&logoColor=white)
> ![k6](https://img.shields.io/badge/k6-7D64FF?logo=k6&logoColor=white)
> ![influxDB](https://img.shields.io/badge/influxDB-22ADF6?logo=influxdb&logoColor=white)

## 3. 🏗 System Architecture & CI/CD

<img width="1095" height="602" src="https://github.com/user-attachments/assets/15d59f28-63a9-4854-995c-c6c7a48aa72d">

---


## 4. 📊 ERD (담당 도메인)
<img width="793" height="491" alt="fs102" src="https://github.com/user-attachments/assets/058c2d55-4491-49fe-8c1b-481249db80dd" />

> 전체 서비스 중 **입주민(Member/Resident)** 과 **시설 예약(Facility/Reservation)** 핵심 도메인을 설계했습니다.  
> **입주민** : JWT 기반 인증 및 권한 관리  
> **시설 예약**: 시설별 운영 시간, 예약 가능 인원, 실시간 상태값 관리  

## 5. 🚀 핵심 기능 & 흐름(Flow)
### 🔐 1. JWT & Redis 기반 인증 시스템
보안성과 사용자 경험을 모두 잡기 위해 Access Token(단기) + Refresh Token(장기) 구조를 채택했습니다.

코드 스니펫
sequenceDiagram
    participant User as 입주민 (App)
    participant Server as Spring Boot
    participant DB as MariaDB/Redis

    User->>Server: 로그인 요청 (ID/PW)
    Server->>DB: 사용자 정보 확인
    Server->>Server: JWT 생성 (Access/Refresh)
    Server->>DB: Refresh Token 저장 (Redis)
    Server-->>User: 토큰 발급
### 📅 2. QR 기반 시설 예약 및 체크인
QueryDSL을 사용하여 동적인 예약 상태 조회를 구현하고, 실시간성 확보를 위해 QR 로직을 연동했습니다.

코드 스니펫
flowchart LR
    A[시설 및 시간 선택] --> B{예약 가능 여부 확인}
    B -- 가능 --> C[예약 DB 등록]
    B -- 불가 --> D[에러 메시지]
    C --> E[고유 ID 기반 QR 생성]
    E --> F[IoT 리더기 스캔]
    F --> G[서버 검증 및 문 개방]
## 6. 🛠 핵심 트러블 슈팅
⚡ Redis 도입을 통한 인증 성능 및 보안 강화
문제: JWT만 사용 시 토큰 만료 전 탈취 위험이 있고, DB(RDB)에 Refresh Token을 저장할 경우 매 요청마다 발생하는 조회 쿼리로 인해 오버헤드 발생.

해결:

Redis를 도입하여 Token 저장소로 활용.

메모리 기반 조회로 응답 속도 개선 및 TTL 설정을 통해 만료된 토큰 자동 폐기.

로그아웃 시 Redis 내 토큰을 즉시 삭제하는 'Blacklist' 로직을 추가하여 보안성 극대화.

🔄 QueryDSL을 이용한 예약 동시성 및 검색 최적화
문제: 여러 입주민이 동시에 특정 시설을 예약할 때 발생하는 데이터 무결성 이슈와 복잡한 필터 조건(시설명, 날짜, 예약상태 등) 검색 시 쿼리 복잡도 증가.

해결:

QueryDSL 도입으로 가독성 있는 동적 쿼리 작성 및 컴파일 시점 에러 방지.

비즈니스 로직 레벨에서 BooleanExpression을 활용해 예약 가능 여부를 1차 검증하고, DB 유니크 제약 조건을 통해 중복 예약을 원천 차단.

📂 부록
팀 전체 README: LINK_TO_TEAM_README

데모 영상: LINK_TO_VIDEO
