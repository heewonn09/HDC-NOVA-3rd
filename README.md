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

> **Client & Server:** React Native App의 요청을 Nginx를 거쳐 Spring 서버에서 로직 처리.    
> **Data & Cache:** RDB로 MariaDB / JWT 토큰 및 OpenWeather API 데이터 캐싱으로 Redis 활용.    
> **External Service:** 예약 관련 알림을 위해 FCM과 연동한 실시간 푸시 알림 제공.    

#### 🔄 CI/CD Pipeline & Monitoring
> **CI/CD 자동화:** GitHub Actions 활용 Merge 시 자동 빌드 및 테스트 수행 **(CI)**, On-Premise 서버로 자동 배포 **(CD)**.    
> **컨테이너 오케스트레이션:** Docker Compose를 활용 API 서버, DB, Nginx, 모니터링 툴을 컨테이너 단위 관리 및 배포합니다.    
> **모니터링 및 성능 측정:**    
>  - Prometheus, Grafana 기반 서버 상태 및 리소스 시각화 & 모니터링.    
>  - k6 활용 부하 테스트 환경 구축, p95, p99 응답 지표를 주기적으로 측정하고 병목 탐색.    
---


## 4. 📊 ERD (담당 도메인)
<img width="793" height="491" alt="fs102" src="https://github.com/user-attachments/assets/058c2d55-4491-49fe-8c1b-481249db80dd" />

> 전체 서비스 중 **입주민(Member/Resident)** 과 **시설 예약(Facility/Reservation)** 핵심 도메인을 설계했습니다.  
> **Member** : 모바일 App 이용자 / 로그인 시 사용되며, JWT 기반 인증 및 권한 부여    
> **Resident** :  입주민이 회원가입 시 인증되는 정보, 관리자가 관리하는 실 거주자 Table    
> **Facility** : 입주민이 사용하는 커뮤니티 시설, 예약 가능 여부 / 이용 시간    
> **Reservation** : 입주민이 예약한 시설 예약 정보, 예약 비용 / 이용 시간 / 예약자 정보 / QR Token 등

## 5. 🚀 핵심 기능 & 흐름(Flow)
### 🔐 1. JWT & Redis 기반 입주민 로그인 기능
**로그인 구조**    
   로그인 인증을 위한 계정 정보(Member) 와 아파트 거주 증명 정보(Resident)로 나누어진 구조로,    
   아파트 거주 인증을 마친 후에 로그인을 할 수 있도록 설계했습니다.    
이후 로그인 관리는 Access Token 과 Refresh Token 를 사용하여 인증 정보를 관리했습니다.    

**인증 흐름 (Authentication Flows)**    
  A. 일반 로그인 (ID/PW)
   1. 사용자가 loginId와 password로 로그인 요청을 보냅니다.
   2. MemberAuthenticationProvider에서 자격 증명을 검증합니다.
   3. MemberDetailsService가 Member 정보와 해당 회원이 소속된 apartmentId, hoId를 함께 조회하여 MemberDetails를 생성합니다.
   4. 검증 성공 시, JwtProvider를 통해 Access/Refresh 토큰을 발급합니다.

  B. 소셜 로그인 (OAuth2)
   1. 외부 플랫폼(Google, Naver 등)을 통해 인증을 진행합니다.
   2. CustomOAuth2UserService 에서 사용자 정보를 수신하고, 기존 회원 여부를 확인합니다.
   3. OAuthSuccessHandler에서 인증 성공 후 JWT 토큰을 발급하고 클라이언트로 리다이렉트합니다.

코드로 인해 생성된 Access/Refresh 토큰은 Redis에 기간을 두고 저장되며, Provider 검증 시 비교에 활용됩니다.

### 📅 2. QR 기반 시설 예약 및 체크인
아파트 단지 내 공용 시설(체육시설, 독서실 등)을 예약하고, 발급된 QR 코드를 시설 카메라에 인식해 체크인하는 시스템입니다.

**도메인 모델 구조 (Domain Hierarchy)**    
  시설과 예약 데이터는 다음과 같은 계층 구조로 설계되어 있습니다.


   - `Facility`: 아파트 전용 시설 (예: 헬스장, 커뮤니티 센터, 도서관)
   - `Space`: 시설 내 개별 공간 (예: 헬스장 GX룸, 도서관 1번 좌석, 테니스 1번 코트)
   - `Reservation`: 특정 사용자의 공간 이용 예약 정보 및 QR 토큰 관리


**QR 체크인 흐름**    
  예약한 시설의 이용을 위해 qr_token 기반의 체크인 시스템을 운영합니다.

   - 예약 생성: 예약 시 고유한 qr_token이 생성되며 클라이언트에 전달됩니다.
   - QR 인증: 시설 카메라를 통해 qr_token을 서버에 전달 후 검증여부에 따라 이용자의 출입을 제어합니다.
   - 예약 상태 : CONFIRMED(확정) → IN_USE(이용 중) → COMPLETED(완료)의 생명주기를 가집니다.


**자동 스케줄러 및 알림 (Scheduler & Notification)**    
  Scheduler를 통해 예약 상태가 자동적으로 변하게 하고, 상태에 따라 사용자에게 알림을 보내도록 했습니다.

   - 자동 상태 업데이트: ReservationScheduler가 2분 간격으로 실행되며 다음 작업을 수행합니다.
       - 입장 시작: 시작 시간 10분 전 예약 상태를 IN_USE로 자동 변경하고, 알림을 보내 입실을 안내합니다.
       - 종료 예정 알림: 이용 종료 10분 전 사용자의 푸시 알림(NotificationService)을 통해 퇴실을 안내합니다.
       - 만료 처리: 이용 종료 10분 후 예약을 COMPLETED로 변경하고 QR 토큰을 만료시킵니다.
   - 취소 및 환불: 사용자가 직접 취소할 경우 상태를 즉시 CANCELLED로 변경하고 리소스를 즉시 해제합니다.


**기술적 최적화 (Technical Highlights)**    
   - 인덱싱 전략: 대량의 예약 데이터에도 조회 성능을 확보하기 위해 status와 start_time/end_time의 복합 인덱스를 설정했습니다.
   - 비동기 알림: 알림 발송 시 @Async 설정을 활용하여 스케줄러의 메인 로직이 차단되지 않도록 설계했습니다.

## 6. 🛠 핵심 트러블 슈팅
### ⚡ JWT 보안 및 관리 구조 최적화: XSS/CSRF 방어 및 RTR 기법 도입
**[문제 상황]**   
사용자 편의성을 위해 JWT 기반 인증을 도입했으나, 초기 설계 시 토큰 저장 및 전달 방식에 따른 보안 취약점이 발견되었습니다.

- XSS 공격 가능성: 웹 환경에서 토큰을 localStorage에 저장할 경우, 악성 스크립트 탈취에 무방비함.

- CSRF 공격 가능성: 보안을 위해 Cookie를 사용할 경우, 사용자의 의도와 상관없이 토큰이 전송되는 CSRF 공격에 노출될 위험성 존재.

- 토큰 탈취 시 대응 부재: 만료 시간이 긴 Refresh Token이 탈취될 경우, 탈취된 토큰에 대한 대응방안이 없어 탈취자가 Access Token을 발급받을 수 있는 구조적 한계.

**[분석 및 해결]**   
**1. 보안 위협에 따른 토큰 분리**   

- CSRF 방어: 모든 API 요청 시 토큰을 Authorization Header에 담아 전송하도록 했습니다.

- XSS 방어 (Android App): 모바일 클라이언트에서는 OS 레벨의 암호화 저장소인 SecureStore를 통해 Refresh Token을 저장했습니다. 이를 통해 메모리 덤프나 파일 시스템 접근을 통한 토큰 탈취 가능성을 최소화했습니다.

**2. RTR(Refresh Token Rotation) 기법을 통한 피해 최소화**   

- Access Token 수명 단축: 유효 기간을 5분으로 설정하여, 토큰이 탈취되더라도 공격자가 이용할 수 있는 시간을 최소화했습니다.

- 일회용 Refresh Token: RTR 방식을 도입하여 토큰 재발급 시 기존 Refresh Token을 즉시 무효화하고, 새로운 Access/Refresh 토큰 쌍을 발급하도록 변경했습니다.

- 이상 징후 감지 및 즉시 무효화: Redis에 저장된 '이미 사용된 Refresh Token'이 다시 사용될 경우, 이를 토큰 탈취 및 재사용 시도로 간주합니다. 이 시점에 서버는 즉시 해당 사용자의 모든 세션을 만료시켜 공격자의 접근을 차단하는 자가 방어 로직을 구축했습니다.

**[결과 및 성과]**   
- 보안성 강화: 인증 정보 저장 방식 최적화를 통해 XSS 및 CSRF 공격에 대한 방어 체계를 구축했습니다.

- 로그인 성능 효율성 증대: Redis 기반의 토큰 관리를 통해 DB 접근을 최소화 했습니다.

- 신뢰성 확보: RTR 기법 도입으로 Refresh Token 탈취 상황에서도 시스템이 대응할 수 있도록 구조를 완성했습니다.


### 🔄 외부 API 병목 탐색 및 Redis 캐싱을 통한 88% 성능 개선
**[문제 상황]**   
  앱 실행 후 메인 화면 진입 시, 로그인 초기 화면을 불러오는 과정에서 총 1초 ~ 3초 이상의 로딩 지연의 문제가 발견되었습니다.

**[분석 과정]**   
- AOP 기반 성능 모니터링: 특정 API의 문제인지 파악하기 위해 AOP를 구현하여 모든 컨트롤러의 실행 시간을 측정했습니다.
   - [API PERF] GET /api/v1/weather - OpenWeatherService.getOpenWeather :    
     812ms 로그를 통해 외부 API(OpenWeatherMap) 호출 구간이 전체 응답 시간의 50% 이상 차지하는 병목 지점임을 확정했습니다.
- 비동기 처리 최적화: 기존의 순차적인 호출 방식을 WebClient와 Mono.zip을 활용한 Non-blocking IO로 전환하여 외부 API 응답 대기 시간을
 단축했습니다.


**[최종 해결: Redis 캐싱 전략]**   
- 날씨 데이터의 특성상 실시간성이 중요하지만, 한 번 검색하면 1-2시간 동안 변하지 않는 특성이 있어 Redis 캐싱을 도입했습니다.
- 동일한 위도/경도 좌표에 대해 30분간의 캐시 유효시간을 설정하여, 불필요한 외부 API 호출을 줄였습니다.


**[결과]**   
50명 이상의 사용자가 날씨 호출 API를 불러온다는 가정 하에 나온 평균 속도를 측정했습니다.
- p95 응답 속도: 476ms → 53ms (약 88.8% 개선)
- 외부 API 호출 비용 절감 및 네트워크 장애 시에도 캐시된 데이터를 통해 안정적인 서비스 제공이 가능해졌습니다.


### 부록
[README_TEAM 버전 문서](https://github.com/wnsrlf0721/HDC-NOVA-3rd/blob/main/README_TEAM.md)
