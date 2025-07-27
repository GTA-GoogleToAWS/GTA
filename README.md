# 🌩️ 멀티 클라우드 DR 아키텍처

> **AWS + GCP 기반 자동 장애 전환 시스템** | **신세계아이앤씨 클라우드 엔지니어 양성과정**

<div align="center">

![Multi-Cloud](https://img.shields.io/badge/Multi--Cloud-AWS%20%2B%20GCP-orange?style=flat-square)
![DR](https://img.shields.io/badge/DR-Active--Passive-red?style=flat-square)
![Flask](https://img.shields.io/badge/Flask-Python-blue?style=flat-square)

[🎥 **시연 영상**](https://youtu.be/lxeubdhbO1k?si=wts_6042jry7hWL1) • [📊 **발표자료**](./docs/presentation.pdf)

</div>

## 📋 프로젝트 개요

**Azure 대규모 장애(2024.7) 사건을 계기로 멀티 클라우드 DR 시스템 구축**

- **목표**: 99.9% 가용성, RTO 15분, RPO 1분
- **구성**: GCP(Primary) + AWS(DR Site) + 자동 Failover
- **결과**: 응답속도 84% 개선, 비용 50% 절감

## 🏗️ 아키텍처

```
Route 53 DNS Failover
        │
   ┌────┴────┐
GCP(Active) AWS(Passive)
   Primary    DR Site
```

**핵심 구성요소**
- **DNS**: Route 53 Health Check 기반 자동 전환
- **Network**: GCP HA VPN ↔ AWS Site-to-Site VPN
- **Security**: Cloud Armor + AWS WAF 이중 보안
- **DB**: MySQL + Redis 실시간 동기화

## 🛠️ 기술 스택

| 분야 | 기술 |
|------|------|
| **Backend** | Python, Flask |
| **Cloud** | Google Cloud Platform, Amazon Web Services |
| **Database** | MySQL, Redis |
| **Infrastructure** | Auto Scaling, Load Balancer, CDN |

## 🚀 주요 성과

### 📊 성능 개선
- **응답속도**: CloudFront 적용 시 **84% 향상** (3.8초 → 0.6초)
- **가용성**: 99.9% 목표 달성
- **복구시간**: RTO 15분 이내

### 💰 비용 최적화
- **Active-Passive 구성**으로 DR 비용 50% 절감
- **Auto Scaling**으로 리소스 효율성 극대화
- **Lambda 기반 DR**로 서버리스 운영

## 🔧 핵심 기능

- ✅ **자동 장애 전환**: Route 53 Health Check 기반
- ✅ **실시간 모니터링**: GCP + AWS 통합 모니터링
- ✅ **데이터 동기화**: AWS DMS 활용
- ✅ **보안 강화**: 이중 WAF + IAM 관리

## 🚀 빠른 실행

```bash
# 로컬 실행
git clone [repository-url]
pip install -r requirements_local.txt
mysql -u root -p < init_database.sql
python app.py

# 접속: http://localhost:8080
```

## 📁 주요 파일

```
flask_app-main/
├── app.py              # 메인 애플리케이션 (GCP)
├── app_dr.py           # DR 애플리케이션 (AWS)
├── speed.py            # 성능 테스트
├── requirements_*.txt  # 환경별 의존성
└── templates/          # HTML 템플릿
```

## 👥 팀 구성

**Team GTA (4명)**
- **이충민**: 코드 개발, AWS 3-tier, DR 시스템
- **최윤하**: 팀장, AWS 인프라, VPN 연결
- **백지영**: GCP 인프라, 모니터링
- **한승규**: GCP 구축, Lambda, DMS

## 🎯 활용 방안

**적용 대상**: 전자상거래, 금융, 헬스케어 등 서비스 중단 민감 산업  
**확장성**: 중소기업부터 대기업까지 단계적 적용 가능

---

<div align="center">

**🎬 [시연 영상 보기](https://youtu.be/lxeubdhbO1k?si=wts_6042jry7hWL1)**

*Made with ❤️ by Team GTA*

</div>
