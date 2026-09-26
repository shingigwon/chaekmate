# 📚 ChaekMate - 온라인 도서 쇼핑몰

> 2025.10.15 ~ 2025.12.05 <br>
> 🔗 [NHN 아카데미 Java Backend 7기 팀 프로젝트](https://github.com/nhnacademy-be11-1)

Spring Cloud 기반 유사 MSA 구조로 설계한 온라인 도서 쇼핑몰입니다.  
분산 인증(JWT/Redis), Spring AI(Gemini) 벡터 검색, RabbitMQ 비동기 처리, ELK 로그 중앙화를 적용했습니다.

## 목차
- [아키텍처](#️-아키텍처)
- [ERD](#-erd)
- [기술 스택](#️-기술-스택)
- [프로젝트 관리](#-프로젝트-관리)
- [담당: 주문/결제](#-담당-주문결제)
- [Organization 레포지토리](#-organization-레포지토리)

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

## 🙋 담당: 주문/결제

> 팀 프로젝트이며, 아래 주문/결제 도메인 설계 및 구현은 본인이 단독으로 담당했습니다.

### 주문

- 주문과 주문 상품의 상태를 각각 별도로 관리해, 하나의 주문 안에서도 상품별로 개별 취소/반품이 가능하도록 설계
- 재고는 주문 생성이 아닌 결제 승인 시점에 차감하고, 승인 직전 한 번 더 재검증해 동시에 여러 주문이 들어올 때 재고가 초과 판매되는 문제를 방지
- 주문 취소/반품 시 취소/반품된 수량만큼 재고를 원상 복구하고, 상품이 전부 취소/반품되면 대표 주문 상태도 함께 변경
- 여러 조건(상태, 기간, 날짜 등)별로 나뉘어 있던 유사 조회 API를 조건 조합에 따라 동작하는 QueryDSL 동적 쿼리 하나로 통합

### 결제

- TossPayments 연동으로 결제 승인/취소 구현
- 회원은 현금과 포인트를 함께 사용하는 혼합 결제도 가능하도록, 하나의 결제 건 안에서 결제 금액과 사용 포인트를 함께 관리
- 결제 실패 시 오류 코드와 설명을 분리해 저장하고, 실패 이력 저장은 별도 트랜잭션으로 분리해 결제 트랜잭션이 롤백되더라도 실패 원인은 남도록 처리
- 취소/반품 후 남은 주문 금액이 무료배송 기준보다 낮아지면 배송비를 추가로 차감하도록 구현, 차감 시 현금을 우선 사용하고 부족분은 포인트로 처리
- 반품은 배송 완료 이후, 정해진 기간 이내에만 가능하도록 검증
- 관리자가 설정한 배송 정책 중 활성화된 정책 기준으로 배송비/반품비를 적용하고, 반품 사유별로 차감 비용을 다르게 처리
- 배송 시작/완료, 취소/반품 시점마다 이벤트를 발생시켜 Dooray 메시지 알림 연동

### 공통 에러 설계

- 상태/코드/메시지를 갖는 공통 에러 인터페이스를 도메인별 에러코드 Enum이 구현하도록 설계해, 도메인이 늘어나도 동일한 규격으로 에러 코드를 확장 가능하게 함
- 모든 비즈니스 예외를 하나의 커스텀 예외로 통일해서 던지고, 전역 예외 핸들러가 이를 공통 처리해 어떤 도메인에서 발생한 예외든 동일한 형식의 에러 응답을 반환
- 입력값 검증 실패, 인증 실패, 예상치 못한 예외까지 각각 전용 핸들러로 잡아, 도메인 예외가 아닌 프레임워크 레벨 예외까지 일관된 응답 포맷을 유지

---

## 📁 Organization 레포지토리

| 레포 | 설명 |
|---|---|
| [chaekmate-core](https://github.com/nhnacademy-be11-1/chaekmate-core/tree/develop) | 핵심 도메인 서비스 |
| [chaekmate-front](https://github.com/nhnacademy-be11-1/chaekmate-front/tree/develop) | 프론트엔드 서버 |
| [chaekmate-gateway](https://github.com/nhnacademy-be11-1/chaekmate-gateway/tree/develop) | API Gateway |
| [chaekmate-auth](https://github.com/nhnacademy-be11-1/chaekmate-auth) | 인증 서버 |
| [chaekmate-coupon](https://github.com/nhnacademy-be11-1/chaekmate-coupon/tree/develop) | 쿠폰 서비스 |
| [chaekmate-search](https://github.com/nhnacademy-be11-1/chaekmate-search) | 검색 서비스 |
| [chaekmate-batch](https://github.com/nhnacademy-be11-1/chaekmate-batch) | 배치 처리 |
| [chaekmate-eureka](https://github.com/nhnacademy-be11-1/chaekmate-eureka) | 서비스 디스커버리 |
| [chaekmate-config](https://github.com/nhnacademy-be11-1/chaekmate-config/tree/develop) | Config 서버 |
| [chaekmate-logging-starter](https://github.com/nhnacademy-be11-1/chaekmate-logging-starter) | 공통 로그 설정 |
