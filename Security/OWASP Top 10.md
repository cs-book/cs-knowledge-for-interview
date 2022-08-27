### Contents

- [A01:2021-Broken Access Control](#a012021-broken-access-control)
- [A02:2021-Cryptographic Failures](#a022021-cryptographic-failures)
- [A03:2021-Injection](#a032021-injection)
- [A04:2021-Insecure Design](#a042021-insecure-design)
- [A05:2021-Security Misconfiguration](#a052021-security-misconfiguration)
- [A06:2021-Vulnerable and Outdated Components](#a062021-vulnerable-and-outdated-components)
- [A07:2021-Identification and Authentication Failures](#a072021-identification-and-authentication-failures)
- [A08:2021-Software and Data Integrity Failures](#a082021-software-and-data-integrity-failures)
- [A09:2021-Security Logging and Monitoring Failures](#a092021-security-logging-and-monitoring-failures)
- [A10:2021-Server-Side Request Forgery](#a102021-server-side-request-forgery)

# OWASP Top 10

OWASP는 `Open Web Application Security Project`라는 비영리 재단입니다. 2004년부터 10개의 보안 위협을 선정하여 공개하고 있으며, 최근에는 2021년에 공개하였습니다.

[OWASP Top 10 Link](https://owasp.org/Top10/)

## A01:2021-Broken Access Control

가장 영향이 큰 보안 위협으로 `취약한 접근 제어`가 선정되었습니다.

### 취약점 예시

- 인증되지 않은 사용자가 리소스에 접근할 수 있는 경우
- 권한이 없는 사용자가 리소스에 접근할 수 있는 경우
- 쿼리스트링이나 쿠키 등의 값을 변조하여 권한을 상승시킬 수 있는 경우

### 예방 방법

- 화이트 리스트 기반으로 접근 제어 정책 수립
- 리소스 별 접근 제어 정책 적용

## A02:2021-Cryptographic Failures

두 번째 보안 위협은 `암호화 실패`입니다.

### 취약점 예시

- 암호키 관리가 미흡한 경우(eg. 암호키가 코드에 하드코딩된 경우)
- DES, RC4, MD5 등 취약한 암호 알고리즘을 사용하는 경우
- SSL v2.0, SSL v3.0, TLS v1.0, TLS v1.1 등 취약한 암호화 프로토콜을 사용하는 경우
- HTTP, FTP, TELNET 등 데이터를 평문으로 전송하는 프로토콜을 사용하는 경우

### 예방 방법

- 최신 암호 알고리즘 및 프로토콜 사용
- 암호키 관리

## A03:2021-Injection

세 번째 보안 위협은 `인젝션`입니다.

### 취약점 예시

- SQL 인젝션
- OS Command 인젝션
- XSS

### 예방 방법

- 사용자 입력에 대한 검증
- 사용자 입력이 그대로 사용되지 않도록 처리

## A04:2021-Insecure Design

네 번째 보안 위협은 `취약한 설계`입니다.

### 취약점 예시

- 설계 단계에서 보안을 고려하지 않은 상태

### 예방 방법

- 설계 단계에서부터 보안을 고려하여 설계

### KISA의 소프트웨어 개발보안 방법론

1. 요구사항 분석

   - 요구사항 중 보안 항목 식별

2. 설계

   - 위협모델링
   - 보안설계 검토 및 보안설계서 작성
   - 보안 통제 수립

3. 구현

   - 표준 코딩 정의서 및 SW개발보안 가이드 준수하여 개발
   - 소스코드 보안약점 진단 및 개선

4. 테스트

   - 모의침투 테스트 또는 동적분석을 통한 보안취약점 진단 및 개선

5. 유지보수

   - 지속적인 개선
   - 보안패치

## A05:2021-Security Misconfiguration

다섯 번째 보안 위협은 `미흡한 보안 설정`입니다.

### 취약점 예시

- 웹서버 보안 설정 미흡
- 애플리케이션 보안 설정 미흡
- 오픈소스 보안 설정 미흡
- 서버 보안 설정 미흡
- 방화벽 설정 미흡

### 예방 방법

- 사용하는 모든 소프트웨어의 보안 설정 및 보안성 검토 후 사용

## A06:2021-Vulnerable and Outdated Components

여섯 번째 보안 위협은 `취약하고 지원이 종료된 요소`입니다.

### 취약점 예시

- Windows XP, Internet Explorer 지원이 종료된 소프트웨어 버전 사용
- Nginx, Spring, OpenSSL 등 취약점이 존재하는 소프트웨어 버전 사용

### 예방 방법

- 취약점이 존재하지 않는 최신 소프트웨어 사용
- 보안 패치

## A07:2021-Identification and Authentication Failures

일곱 번째 보안 위협은 `식별 및 인증 실패`입니다.

### 취약점 예시

- 세션 등의 인증 정보 타팀아웃이 없는 경우
- 취약한 패스워드를 사용할 수 있는 경우
- 인증 실패에 대한 제한이 없어 brute force 공격이 가능한 경우
- 2차 인증이 없는 경우

### 예방 방법

- 2차 인증 도입
- 높은 수준의 패스워드 정책 설정
- 인증 정보 타임아웃 설정

## A08:2021-Software and Data Integrity Failures

여덟 번째 보안 위협은 `소프트웨어 및 데이터 무결성 오류`입니다.

### 취약점 예시

- 애플리케이션에서 사용하는 라이브러리나 모듈의 무결성 검증이 없는 경우
- 직렬화된 데이터의 무결성 검증이 없는 경우
- CI/CD 파이프라인에 무결성, 보안성 검토가 없는 경우

### 예방 방법

- 직렬화 라이브러리를 사용하는 경우 직렬화된 데이터에 대해 무결성 검증
- CI/CD 파이프라인을 정기적으로 무결성, 보안성 검토
- 해싱을 활용해 애플리케이션 무결성 검증

## A09:2021-Security Logging and Monitoring Failures

아홉번째 보안 위협은 `보안 로깅 및 모니터링 실패`입니다.

### 취약점 예시

- 로그인, 로그아웃, 로그인 실패, 권한 부여 등 중요 로직에 대한 로그를 남기지 않는 경우
- 일정 주기로 로그를 백업하지 않는 경우
- 불필요한 로깅이나 모니터링을 하는 경우

### 예방 방법

- 중요 기능에 대해 로깅
- 로그를 정기적으로 백업
- 소프트웨어, 애플리케이션, 서버 등 모니터링 진행

## A10:2021-Server-Side Request Forgery

마지막 보안 위협은 `서버 측 요청 변조`입니다.

### 취약점 예시

- 적절한 검증 없이 사용자가 리소스에 접근할 수 있는 경우

### 예방 방법

- 모든 사용자 요청, 입력 데이터를 검증

[참고(넷마블 보안팁)](https://netmarble.engineering/owasp-top-10-2021-1/)
