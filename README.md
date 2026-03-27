# 📚 ChaekMate - 온라인 도서 쇼핑몰

> 2025.10.15 ~ 2025.12.05 <br>
> 🔗 [NHN 아카데미 Java Backend 7기 팀 프로젝트](https://github.com/nhnacademy-be11-1)

Spring Cloud 기반 유사 MSA 구조로 설계한 온라인 도서 쇼핑몰입니다.  
분산 인증(JWT/Redis), Spring AI(Gemini) 벡터 검색, RabbitMQ 비동기 처리, ELK 로그 중앙화를 적용했습니다.

---

## 🏗️ 아키텍처

![architecture](https://github.com/user-attachments/assets/2db4e84e-12c3-4a3d-8143-b34f1fdf1faf)

---

## 📊 ERD

![erd](https://github.com/user-attachments/assets/3f9bf5f4-9d8e-4383-b2b8-78151d328356)

---

## 🛠️ 기술 스택

**언어 & 빌드**  
`Java 21` `Maven`

**프레임워크**  
`Spring Boot` `Spring Cloud` `Spring Data JPA` `QueryDSL`

**데이터베이스 & 인프라**  
`MySQL` `Redis` `Elasticsearch` `RabbitMQ`  
`Docker` `GitHub Actions` `ELK Stack`

**테스트**  
`JUnit5` `AssertJ` `Mockito` `SonarQube`

---

## 📅 프로젝트 관리

### WBS / 로드맵

전체 기능을 이슈 단위로 분리하고 로드맵으로 일정을 관리했습니다.

![wbs](https://github.com/user-attachments/assets/6206f5be-3da9-4d44-9557-b17053a903eb)


### 데일리 스크럼

매일 오전 09시, 아래 항목을 중심으로 스크럼 회의를 진행했습니다.

- ✅ 전날 진행 사항 공유
- 🚧 회의가 필요한 내용 논의
- 🙋 도움이 필요한 사항 공유
- 📌 다음 진행할 사항 계획

![scrum](https://github.com/user-attachments/assets/1a8c8d17-93f7-44a5-bfd9-7513c5a32c55)

---

## 🙋 담당: 주문 · 결제

### 주문

- 회원/비회원 주문을 동일한 방식으로 처리하도록 구현
- 주문 생성 시 각 상품의 재고를 확인하고, 주문과 함께 처리하여 데이터 오류가 발생하지 않도록 설계
- 하나의 주문에서도 상품별로 취소 및 반품이 가능하도록 상품 단위로 상태를 관리
- 결제 결과에 따라 상품 상태를 변경하고, 이에 맞춰 재고 감소 및 복구가 이루어지도록 구현
- 주문 취소 및 반품 시 상품 상태에 따라 재고를 복구하도록 처리
- 결제 실패, 취소 등 다양한 상황에서도 데이터가 일관되게 유지되도록 로직 구성

### 결제

- TossPayments API를 연동하여 결제 승인 및 취소 기능 구현
- 결제 결과에 따라 주문 상태를 변경하고, 실패 시 재고를 복구하도록 처리
- 관리자가 설정한 배송 정책 중 현재 활성화된 정책을 기준으로 배송비 및 반품 비용이 적용되도록 구현
- 반품 사유에 따라 비용이 다르게 차감되도록 로직 구현
- 배송 시작·도착 /상품 취소·반품 시 이벤트를 발생 Dooray 메시지 알림 연동

---

## 📁 Organization 레포지토리

| 레포 | 설명 |
|---|---|
| [chaekmate-front](https://github.com/nhnacademy-be11-1/chaekmate-front) | 프론트엔드 서버 |
| [chaekmate-gateway](https://github.com/nhnacademy-be11-1/chaekmate-gateway) | API Gateway |
| [chaekmate-auth](https://github.com/nhnacademy-be11-1/chaekmate-auth) | 인증 서버 |
| [chaekmate-core](https://github.com/nhnacademy-be11-1/chaekmate-core) | 핵심 도메인 서비스 |
| [chaekmate-coupon](https://github.com/nhnacademy-be11-1/chaekmate-coupon) | 쿠폰 서비스 |
| [chaekmate-search](https://github.com/nhnacademy-be11-1/chaekmate-search) | 검색 서비스 |
| [chaekmate-batch](https://github.com/nhnacademy-be11-1/chaekmate-batch) | 배치 처리 |
| [chaekmate-eureka](https://github.com/nhnacademy-be11-1/chaekmate-eureka) | 서비스 디스커버리 |
| [chaekmate-config](https://github.com/nhnacademy-be11-1/chaekmate-config) | Config 서버 |
| [chaekmate-logging-starter](https://github.com/nhnacademy-be11-1/chaekmate-logging-starter) | 공통 로그 설정 |
