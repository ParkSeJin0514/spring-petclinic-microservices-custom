# PetClinic Dev

Spring PetClinic Microservices 소스 코드 및 CI/CD 파이프라인

## 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway                            │
└─────────────────────────┬───────────────────────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
┌────────────┐     ┌────────────┐       ┌────────────┐
│ Customers  │     │   Visits   │       │    Vets    │
│  Service   │     │  Service   │       │  Service   │
└────────────┘     └────────────┘       └────────────┘
         │                │                    │
         └────────────────┼────────────────────┘
                          │
                   ┌──────▼──────┐
                   │   MySQL     │
                   └─────────────┘
```

## 서비스 구성

| 서비스 | 포트 | 설명 |
|--------|------|------|
| config-server | 8888 | 중앙 설정 관리 |
| discovery-server | 8761 | Eureka 서비스 디스커버리 |
| api-gateway | 8080 | API 라우팅 |
| customers-service | 8081 | 고객/펫 관리 |
| visits-service | 8082 | 방문 기록 관리 |
| vets-service | 8083 | 수의사 정보 관리 |
| admin-server | 9090 | Spring Boot Admin |

## 디렉토리 구조

```
├── spring-petclinic-*/     # 각 마이크로서비스 소스
├── docker/                 # Dockerfile들
├── scripts/                # 빌드/배포 스크립트
├── Jenkinsfile             # CI/CD 파이프라인
├── docker-compose.yml      # 로컬 개발용
└── pom.xml                 # Maven 루트 설정
```

## 로컬 실행

```bash
# 전체 빌드
./mvnw clean install -DskipTests

# Docker Compose로 실행
docker-compose up -d

# 개별 서비스 실행
./mvnw spring-boot:run -pl spring-petclinic-config-server
```

## CI/CD (Jenkins)

Jenkinsfile이 포함되어 있으며 다음 단계를 수행:
1. 변경된 서비스 감지
2. Maven 빌드 & 테스트
3. Docker 이미지 빌드
4. ECR Push
5. GitOps 저장소 업데이트 (petclinic-gitops)

## 기술 스택

- Java 17, Spring Boot 3.x, Spring Cloud
- Maven, Docker
- MySQL (RDS)
- Jenkins CI/CD
