# Discovery Server

PLAN&WITH 서비스에서 사용하는 Eureka 기반 서비스 디스커버리 서버입니다.

## 기술 스택

- Java 17
- Spring Boot 4.0.7
- Spring Cloud 2025.1.2
- Netflix Eureka Server
- Gradle Wrapper 9.5.1

## 사전 요구사항

- JDK 17
- 기본 포트 `8761` 사용 가능

별도의 데이터베이스나 외부 서비스 설정은 필요하지 않습니다.

## 서버 실행

### Windows

```powershell
.\gradlew.bat bootRun
```

### macOS / Linux

```bash
./gradlew bootRun
```

서버가 시작되면 다음 주소에서 Eureka 대시보드를 확인할 수 있습니다.

```text
http://localhost:8761
```

## 포트 변경

기본 포트는 `8761`이며 `SERVER_PORT` 환경 변수로 변경할 수 있습니다.

### Windows PowerShell

```powershell
$env:SERVER_PORT=8762
.\gradlew.bat bootRun
```

### macOS / Linux

```bash
SERVER_PORT=8762 ./gradlew bootRun
```

## 빌드 및 테스트

### Windows

```powershell
.\gradlew.bat clean test
.\gradlew.bat bootJar
```

### macOS / Linux

```bash
./gradlew clean test
./gradlew bootJar
```

실행 가능한 JAR 파일은 `build/libs` 디렉터리에 생성됩니다.

```powershell
java -jar build/libs/discovery-0.0.1-SNAPSHOT.jar
```

## 주요 설정

```yaml
server:
  port: ${SERVER_PORT:8761}

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

Discovery Server 자체는 다른 Eureka 서버에 등록하지 않으며, 외부 레지스트리도 조회하지 않습니다.

## 서비스 등록 예시

Discovery Server를 사용하는 각 서비스의 설정에 다음 내용을 추가합니다.

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

컨테이너나 원격 환경에서는 `localhost`를 Discovery Server의 실제 호스트 이름으로 변경해야 합니다.

## 문제 해결

### 8761 포트가 이미 사용 중인 경우

`SERVER_PORT` 환경 변수로 다른 포트를 지정합니다. 포트를 변경했다면 등록할 서비스의 `defaultZone`도 같은 포트로 변경해야 합니다.

### Windows에서 Gradle 실행이 안 되는 경우

다음 명령으로 Java 버전을 확인합니다.

```powershell
java -version
```

Java 17이 출력되지 않으면 JDK 17을 설치하고 `JAVA_HOME`을 올바르게 설정해야 합니다.
