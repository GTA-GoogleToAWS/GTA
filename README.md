# 🌩️ 멀티 클라우드 DR 아키텍처 설계 및 자동 장애 전환 구현

<div align="center">

![Multi-Cloud](https://img.shields.io/badge/Multi--Cloud-AWS%20%2B%20GCP-orange?style=flat-square)
![DR](https://img.shields.io/badge/DR-Active--Passive-red?style=flat-square)
![Flask](https://img.shields.io/badge/Flask-Python-blue?style=flat-square)

[🎥 **시연 영상**](https://www.youtube.com/watch?v=vzg01HwdsLw) • [📊 **발표자료**](https://github.com/user-attachments/files/21451998/02._4.pdf)

</div>

## 🏗️ 아키텍처

<img width="100%" alt="project2_architecture drawio" src="https://github.com/user-attachments/assets/cce9d1f7-d2a7-41b0-a3a8-3b0058b23946" />


## 📋 프로젝트 개요

### 주제 선정 배경: Azure 대규모 장애 사건

**1차 장애 - Azure 자체 시스템 (24년 7월 18일)**
- 원인: Azure Central US 리전 네트워크 설정 오류
- 지속시간: 12시간 ~ 24시간
- 영향: 스토리지 및 Microsoft 365 서비스 중단

**2차 장애 - 크라우드스트라이크 (24년 7월 19일)**
- 원인: 보안 소프트웨어 업데이트 오류로 Windows 시스템 충돌
- 지속시간: 원인 제거 6시간, 전체 복구 몇 주
- 영향: Azure 가상머신 대량 블루스크린

### 피해 규모
**전 세계적 영향**
- 항공업계: 5천편 이상 항공기 운항 중단
- 금융업계: 은행, 거래소 시스템 마비
- 의료업계: 병원 응급 시스템 중단
- **경제적 손실: 1조 4천억원 이상**

### 프로젝트 목표
- **GCP와 AWS를 활용한 고가용성 멀티 클라우드 환경 구축**
- **자동 장애 전환 시스템 구현**
- **운영 비용 최적화를 위한 Active-Passive DR 구성**
- **통합 모니터링 및 보안 강화**

## 🛠️ 기술 스택

| 분야 | 기술 |
|------|------|
| **개발** | Python, Flask, HTML, CSS |
| **클라우드** | Google Cloud Platform, Amazon Web Services |
| **데이터베이스** | MySQL, Redis |
| **도구** | Git, GitHub, Slack |

## 📅 프로젝트 일정

- **6/27**: 팀 구성 및 아키텍처 설계
- **6/28**: 핵심 기술 학습 및 코드 작성
- **6/29**: AWS 기본 인프라 구축
- **6/30**: GCP 환경 구성 및 VPN 연결
- **7/1**: DR 시스템 및 보안 설정
- **7/2**: 모니터링 시스템 구축 및 발표 준비
- **7/3**: 최종 발표 및 시연

## 🏗️ 핵심 아키텍처

### 네트워크 구성
- **글로벌 DNS**: GCP Cloud DNS ↔ AWS Route 53
- **CDN**: AWS CloudFront
- **WAF**: GCP Cloud Armor ↔ AWS WAF
- **VPN**: GCP HA VPN ↔ AWS Site-to-Site VPN

### 인프라 구성
- **3-Tier 아키텍처**: Web, App, Database 계층 분리
- **Auto Scaling**: 자동 확장/축소 시스템
- **고가용성 DB**: RDS Multi-AZ, ElastiCache
- **보안 관리**: Secrets Manager, IAM 역할 기반 접근 제어

### DR 구성
- **Active-Passive 구성**으로 비용 최적화
- **Route 53 Failover**를 통한 자동 트래픽 전환
- **AWS DMS**를 활용한 데이터 연속성 유지


**Route 53 DNS Failover 구성**
- Primary: GCP (35.201.106.123)
- Secondary: AWS (d36vqg3xcdb804.cloudfront.net)
- 라우팅 정책: Failover
- 상태 확인: Health Check 기반 자동 전환

## 🔧 Trouble Shooting

### AWS DRS → AWS Lambda 선택 이유

**기존 AWS DRS의 한계점**
- 동일 이미지 중복 복제: Auto Scaling으로 생성된 인스턴스 모두 개별 복제
- 불필요한 리소스 낭비: 같은 애플리케이션 코드를 여러 번 복제
- 높은 비용: 지속적인 볼륨 동기화로 인한 네트워크 비용 증가

| 항목 | AWS DRS | AWS Lambda |
|------|---------|------------|
| **복제 방식** | VM 디스크 × 인스턴스 수 | 함수 코드 (경량) |
| **비용** | 지속적 동기화 | 실행 시에만 과금 |
| **복구 시간** | 중간 (부팅 필요) | 빠름 |
| **관리 복잡도** | 높음 | 낮음 (서버리스) |

### 성능 최적화 결과

**AWS CloudFront vs WEB ALB Direct 응답 속도 비교**
- CloudFront 성능 테스트: 평균 **615.60ms** (최소/최대: 467.16ms / 1182.10ms)
- ALB Direct 성능 테스트: 평균 **3848.84ms** (최소/최대: 1273.96ms / 8385.45ms)
- **성능 향상**: **84% 개선**

**GCP Cloud CDN vs Cloud Storage Direct**
- CDN 응답시간: **0.041s ~ 0.058s**
- Direct 응답시간: **0.300s ~ 0.325s**

## 🚀 활용방안 및 기대효과

### 적용 대상
- **전자상거래, 금융, 헬스케어, 게임** 등 서비스 중단 민감 산업
- **중소기업 → 대기업** 단계별 확장 가능한 아키텍처

### 도입 전략
1. **1단계**: Active-Passive DR 구성으로 시작
2. **2단계**: 무중단 서비스 필요 시 Active-Active 확장

### 기대효과

#### 비즈니스 연속성
- **RTO 15분 이내, RPO 1분 이내** 목표 달성
- 자동 장애 전환으로 서비스 중단 최소화

#### 비용 최적화
- Active-Passive 구성으로 DR 리소스 최소화
- Auto Scaling으로 리소스 효율성 극대화
- 기존 대비 비용 절감

#### 보안 강화
- 이중 보안 체계 (Cloud Armor + AWS WAF)
- 통합 권한 관리 (Secrets Manager + IAM)

#### 성능 향상
- CDN 활용으로 글로벌 콘텐츠 전송 최적화
- 지연 시간 **40-60% 단축**

#### 운영 효율성
- 통합 모니터링 (Cloud Monitoring + CloudWatch)
- 장애 감지 시간 **70% 단축**
- 사전 예방적 운영 가능

#### 벤더 종속성 탈피
- 멀티 클라우드로 단일 업체 의존도 감소
- 운영 유연성 확보 및 서비스 안정성 동시 확보

## 👥 팀 구성 및 역할

**Team GTA (4명)**

|           이충민           |           최윤하           |           한승규           |           백지영           |
| :------------------------: | :------------------------: | :------------------------: | :------------------------: |
| <img src="https://github.com/user-attachments/assets/baa1e756-2962-4b99-9cd7-5ad1f2944437" width="120px;" alt=""/> | <img src="https://github.com/user-attachments/assets/2f605565-1b76-4a79-a5e8-01f3680d9e8e" width="120px;" alt=""/> | <img src="https://github.com/user-attachments/assets/88ea13ff-4e2a-4a08-a8ea-04d5b549e730" width="120px;" alt=""/> | <img src="https://github.com/user-attachments/assets/3a2b1e6b-3f8c-4c7a-8652-9f15a9f1855e" width="120px;" alt=""/> |
| **코드 담당** | **팀장** | **GCP 전문** | **GCP 전문** |
| AWS 3-tier 구축 | AWS 3-tier 구축 | GCP 3-tier 구축 | GCP 3-tier 구축 |
| DR 시스템 | GCP CDN, Armor | GCP Monitoring | GCP Monitoring |
| CloudFront, WAF | VPN 터널링 | Lambda 및 DR | AWS Lambda |
| | | AWS DMS | |


---

<div align="center">

**🎬 [시연 영상 보기](https://www.youtube.com/watch?v=vzg01HwdsLw)**

*Made with ❤️ by Team GTA*
