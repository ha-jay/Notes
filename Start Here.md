# Note GIT address

https://github.com/ha-jay/Notes.git

```bash
# git 
git init
git add .
git commit "first commit"
git branch -m main
git add remote origin https://github.com/ha-jay/Notes.git
git push -u origin main

#git clone
git clone https://github.com/ha-jay/Notes.git
git add .
git commit
git push -u origin main

#git 최신코드가져오기
git pull origin main

#pull할때 저장소의 변경사항과 깃허브의 변경사항이 충돌시 
#1. 그냥 무시하고 깃허브 내용으로 적용
git fetch --all 
git reset --hard origin/main

#2. 내코드는 잠시 stash해놓고, 일단 서버내용을 받기 
git stash          # 내 수정사항을 임시 저장소에 보관
git pull           # 서버 내용 내려받기
git stash pop      # 보관했던 내 수정사항을 다시 꺼내오기 (이때 다시 충돌이 날 수 있음)

#3. 충돌해결하기

```

# DEV Environment
도구가 좋아야 작업이 즐겁습니다. 가장 표준적이고 강력한 도구들입니다.

- **OS**: macOS(Unix 기반, 개발 표준) 또는 Linux(Ubuntu). Windows라면 **WSL2** 설치가 필수입니다.
    
- **터미널 및 스크립팅**: iTerm2(Mac) 또는 Oh My Zsh 설정으로 가독성과 생산성을 높이세요. 스크립팅을 통해 반복작업을 자동화하고, 개발환경을 정밀히 제어하여 생산성을 높이기 위해 터미널을 이해하세요.
    
- **언어설치** - 언어(java, node)환경 설치
    
- **IDE (에디터)**: **VS Code**가 기본이며, 최근에는 AI 네이티브 에디터인 **Cursor**가 대세입니다. IDE를 얼마나 잘 다루느냐는 개발 생산성과 직결됩니다. 
      
- **버전 관리**: **Git**은 선택이 아닌 생존입니다. GitHub 계정을 만들고 CLI 명령어를 익히세요.
      
- **패키지 매니저**: Homebrew(Mac), 가상환경 관리(pyenv, nvm 등)가 필수입니다.  
    
- **가상화** : 가상화 기술을 배우는 것은, APP의 이식성, 확장성, 그리고 효율적 자원을 관리할수 있도록 해줍니다. 

개발환경설정을 완료하고 helloworld를 출력해보세요. 환경설정에 관한 내용은 우측 링크에서 확인하세요. --> [[DEV ENvironment]]

# Computer Science

기초가 없으면 '복사-붙여넣기' 개발자가 됩니다. AI 시대일수록 원리가 중요합니다.

- **자료구조 & 알고리즘**: Array, Linked List, Stack, Queue, Tree, Graph, 정렬 및 탐색.
    
- **운영체제 (OS)**: 프로세스 vs 스레드, 메모리 관리(Virtual Memory), 컨텍스트 스위칭.  내프로그램을 어느 OS에서 구동할것인지, 구동중 왜 느려지는지, 왜 갑자기 종료되는지를 이해하려면 OS의 작동원리를 아는 것이 필수적입니다.
    
- **네트워크**: HTTP/HTTPS 프로토콜, TCP/UDP, DNS, CORS, **RESTful API** 설계 원칙.
    
- **데이터베이스**: SQL(관계형) vs NoSQL(비관계형)의 차이, 트랜잭션, 인덱싱.

컴퓨터 과학에 대한 내용은 우측 링크에서 확인하세요. --> [[Computer Science]]

# Learn Lang

개발을 시작하기전에 기초언어를 학습하고 작은 프로젝트를 진행해보세요.
- java
- javascript / node.js
- python

개발언어에 대한 내용은 우측 링크를 확인하세요. --> [[Learn lang]]


# DEV

## web 개발

- REACT / SPRING BOOT 학습
- REACT / NODE.JS 학습
- TYPESCRIPT학습
- MICROSERVICE학습
- **메시징 & 이벤트 기반** <RabbitMQ 또는 Kafka / Spring AMQP / Spring Kafka, 비동기 처리 패턴> - **프로젝트**: 주문-결제-배송 시스템 (이벤트 기반)


# CLOUD
- EC2 인스턴스 설정
- RDS (PostgreSQL)
- S3 + CloudFront
- Elastic Beanstalk (Spring Boot 배포)
- **실습**: Spring Boot 앱 AWS 배포


# CI/CD

GitHub Actions

# Kubernetes & 모니터링
**Week 29-30: Kubernetes**

- Pod, Deployment, Service
- ConfigMap, Secret
- Ingress Controller
- Helm Charts
- **실습**: Spring Boot + React 앱 K8s 배포


**Week 31-32: 모니터링 & 로깅**

- **애플리케이션 모니터링**
    - Spring Boot Actuator
    - Micrometer + Prometheus
    - Grafana 대시보드
- **로그 관리**
    - Logback 설정
    - ELK Stack (Elasticsearch, Logstash, Kibana)
    - 또는 Loki + Grafana
- **APM**
    - Sentry (에러 트래킹)
    - New Relic 또는 Datadog (선택)
- **프로젝트**: 모니터링 대시보드 구축


# 성능최적화
**Week 33-34: 성능 최적화**

- **백엔드**
    - JPA N+1 문제 해결 (fetch join, @EntityGraph)
    - 쿼리 최적화, 인덱싱
    - Redis 캐싱 (@Cacheable)
    - 커넥션 풀 튜닝 (HikariCP)
- **프론트엔드**
    - Code Splitting, Lazy Loading
    - React Query (서버 상태 캐싱)
    - 이미지 최적화, CDN


# 보안
**보안**

- OWASP Top 10 대응
- SQL Injection, XSS 방지
- HTTPS, CORS 심화


# 테스팅
- - JUnit 5, Mockito
    - Spring Boot Test (@WebMvcTest, @DataJpaTest)
    - React Testing Library
    - E2E 테스트 (Playwright)
- **프로젝트**: 테스트 커버리지 80% 이상


# 최종 프로젝트 & 취업 준비

- **주제 예시**
    - 전자상거래 플랫폼
    - 예약/티켓팅 시스템
    - 소셜 미디어 플랫폼
    - 실시간 협업 도구
- **기술 스택**
    - Frontend: React + TypeScript + React Query
    - Backend: Spring Boot + JPA + Redis
    - Database: PostgreSQL
    - Messaging: Kafka
    - Infra: Docker + Kubernetes + AWS
    - CI/CD: GitHub Actions
- **구현 기능**
    - 인증/인가 (OAuth2, JWT)
    - 실시간 기능 (WebSocket)
    - 파일 업로드 (S3)
    - 결제 연동 (PG사 API)
    - 관리자 대시보드


# 면접준비

**포트폴리오 & 면접 준비**

- GitHub 정리 (README, 문서화)
- 포트폴리오 웹사이트
- 기술 블로그 작성
    - 트러블슈팅 사례
    - 아키텍처 설계 과정
    - 성능 개선 사례
- **면접 준비**
    - 자료구조/알고리즘 (LeetCode, 백준)
    - Java/Spring 면접 질문
    - 시스템 설계 면접
    - 포트폴리오 발표 연습