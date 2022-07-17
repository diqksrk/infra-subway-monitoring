<p align="center">
    <img width="200px;" src="https://raw.githubusercontent.com/woowacourse/atdd-subway-admin-frontend/master/images/main_logo.png"/>
</p>
<p align="center">
  <img alt="npm" src="https://img.shields.io/badge/npm-%3E%3D%205.5.0-blue">
  <img alt="node" src="https://img.shields.io/badge/node-%3E%3D%209.3.0-blue">
  <a href="https://edu.nextstep.camp/c/R89PYi5H" alt="nextstep atdd">
    <img alt="Website" src="https://img.shields.io/website?url=https%3A%2F%2Fedu.nextstep.camp%2Fc%2FR89PYi5H">
  </a>
  <img alt="GitHub" src="https://img.shields.io/github/license/next-step/atdd-subway-service">
</p>

<br>

# 인프라공방 샘플 서비스 - 지하철 노선도

<br>

## 🚀 Getting Started

### Install
#### npm 설치
```
cd frontend
npm install
```
> `frontend` 디렉토리에서 수행해야 합니다.

### Usage
#### webpack server 구동
```
npm run dev
```
#### application 구동
```
./gradlew clean build
```
<br>

예비 분석
* 사용자 트래픽이 많은 페이지 등 가장 중요한 페이지가 무엇인지 파악
* Pagespeed를 활용하여 FCP, TTI등의 지표를 확인

### 1단계 - 웹 성능 테스트
1. 웹 성능예산은 어느정도가 적당하다고 생각하시나요

      | 경쟁사 | FCP      | TTI  | SI   | TBT    | LCP   | CLS   |
      |----------|-------|------|--------|-------|-------| ----- |
      | 서울교통공사 | 1.4s     | 2.0s | 2.1s | 180 ms | 3.7 s | 0     |
      | 네이버지도 | 0.4s     | 6.0s | 3.6s | 840ms  | 2.6s  | 0.019 |
      | 카카오맵  | 0.6s     | 2.6s | 2.7s | 620ms  | 0.7s  | 0.018 |

      | 내 서비스 | FCP  | TTI  | SI   | TBT    | LCP  | CLS   |
      |------|------|------|--------|------|-------| ----- |
      | 현재    | 2.7s | 2.8s | 2.7s | 50ms   | 2.8s | 0.004 |
      | 목표     | 0.5s | 2.8s | 2.7s | 50ms   | 1.6s | 0.004 |
* 목표
  * 경쟁사 대비 성능이 떨어지는 지표인 FCP, LCP 개선을 목표로 했습니다.
  * 경쟁사 중 주로 사용하는 서비스인 네이버지도, 카카오맵을 기준으로 목표를 산정했습니다.
  * FCP를 네이버지도, 카카오맵의 평균인 0.5s로 목표를 설정했습니다.
  * LCP은 네이버지도, 카카오맵의 평균인 1.6s로 목표를 설정했습니다.

2. 웹 성능예산을 바탕으로 현재 지하철 노선도 서비스는 어떤 부분을 개선하면 좋을까요
* 텍스트 압축 사용
* 사용하지 않는 자바스크립트 줄이기
* 캐시 정책 사용하기


---

### 2단계 - 부하 테스트 
1. 부하테스트 전제조건은 어느정도로 설정하셨나요

- 대상
    - 로그인
    - 즐겨찾기 조회
    - 경로 조회

- 목표값
    - 지하철 앱 사용자 수의 10%를 일 사용자로 계산했습니다.
      5,000,000 * 0.1 = 500,0000명
    - 1일 인당 평균 요청 횟수
      3 * 2 (출퇴근) = 6
    - 1일 총 접속 수
        - 500,000 * 6 = 3,000,000
    - 1일 평균 rps
        - 3,000,000 / 86,400 ~= 35
    - 1일 최대 rps
        - 35 * 3 = 105
    - Throughput : 35 ~ 105 rps

- VUSER
  - http_req_duration : 0.5s
  - T (VU iteration)
    - ( 6 * 0.5 ) + 1s = 4s
  - 목표 VUSER
    - 평균 : ( 35 * 4 ) / 6 = 23
    - 최대 : ( 105 * 4 ) / 6 = 70

2. Smoke, Load, Stress 테스트 스크립트와 결과를 공유해주세요

---

### 3단계 - 로깅, 모니터링
1. 각 서버내 로깅 경로를 알려주세요

2. Cloudwatch 대시보드 URL을 알려주세요
