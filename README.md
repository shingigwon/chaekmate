# 📚 ChaekMate - 온라인 도서 쇼핑몰

> NHN 아카데미 Java Backend 7기 팀 프로젝트 | 2025.10.15 ~ 2025.12.05 (8주)  
> 🔗 [Organization](https://github.com/nhnacademy-be11-1)

Spring Cloud 기반 MSA 구조로 설계한 온라인 도서 쇼핑몰입니다.  
분산 인증(JWT/Redis), Spring AI(Gemini) 벡터 검색, RabbitMQ 비동기 처리, ELK 로그 중앙화를 적용했습니다.

---

## 🏗️ 아키텍처

![architecture](https://github.com/user-attachments/assets/2db4e84e-12c3-4a3d-8143-b34f1fdf1faf)

---

## 📊 ERD

![erd](https://github.com/user-attachments/assets/3f9bf5f4-9d8e-4383-b2b8-78151d328356)

---

## 🛠️ 기술 스택

`Java 21` `Spring Boot 3.5.6` `Spring Cloud 2025.0.0`  
`MySQL 8.0` `Redis 6.0` `Elasticsearch 8.x` `RabbitMQ 3.x`  
`Docker` `GitHub Actions` `ELK Stack` `Spring AI (Gemini)`

---

## 🙋 담당: 주문 · 결제

### 주문
- 회원/비회원 주문 생성, 재고 검증, 주문번호 생성
- 포장지 선택, 배송 날짜 지정
- 쿠폰·포인트 적용
- 회원/비회원 주문 내역 조회
- 개별 취소·반품 처리, 재고 복구

### 결제
- TossPayments API 연동 (결제 승인/실패/취소)
- 사유별 반품비 차감
- 관리자 배송 상태 관리
- 배송 시작·도착 시 Dooray 메시지 알림 연동

### 💡 주요 구현 포인트
- 주문 상태 전이(`주문생성 → 결제완료 → 배송중 → 완료 / 취소 / 반품`)를 도메인 엔티티 메서드로 캡슐화해 트랜잭션 일관성 유지
- 취소/반품 시 쿠폰·포인트 환원 및 재고 복구를 하나의 트랜잭션으로 처리
- TossPayments 결제 취소 API 연동으로 배송 전 결제 취소 지원

---

## 📁 Organization 레포지토리

| 레포 | 설명 |
|---|---|
| [chaekmate-front](https://github.com/nhnacademy-be11-1/chaekmate-front) | 프론트엔드 서버 |
| [chaekmate-gateway](https://github.com/nhnacademy-be11-1/chaekmate-gateway) | API Gateway |
| [chaekmate-auth](https://github.com/nhnacademy-be11-1/chaekmate-auth) | 인증 서버 |
| [chaekmate-core](https://github.com/nhnacademy-be11-1/chaekmate-core) | 핵심 비즈니스 로직 |
| [chaekmate-coupon](https://github.com/nhnacademy-be11-1/chaekmate-coupon) | 쿠폰 서비스 |
| [chaekmate-search](https://github.com/nhnacademy-be11-1/chaekmate-search) | 검색 서비스 |
| [chaekmate-batch](https://github.com/nhnacademy-be11-1/chaekmate-batch) | 배치 처리 |
| [chaekmate-eureka](https://github.com/nhnacademy-be11-1/chaekmate-eureka) | 서비스 디스커버리 |
| [chaekmate-config](https://github.com/nhnacademy-be11-1/chaekmate-config) | Config 서버 |
| [chaekmate-logging-starter](https://github.com/nhnacademy-be11-1/chaekmate-logging-starter) | 공통 로그 설정 |
