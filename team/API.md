## RESTful API의 설계 원칙

자원의 표현과 상태를 HTTP 기반으로 전달하는 아키텍처 스타일입니다.

- Uniform interface: 일관된 URI와 HTTP 메서드(GET, POST, PUT, DELETE)를 사용
- Stateless: 서버는 클라이언트의 상태를 저장하지 않음
- Client-Server 분리: 클라이언트와 서버의 역할을 명확히 구분
- Cacheable: 응답을 캐싱 가능하도록 설계
- Layered System: 여러 계층으로 구분하여 확장성과 보안 강화

## REST와 SOAP의 차이점

- 프로토콜: REST는 HTTP 기반, SOAP는 XML 기반 메시지를 사용하는 프로토콜
- 데이터 형식: REST는 JSON, XML 등 다양한 형식을 지원하지만, SOAP는 XML만 사용
- 성능: REST는 경량화된 구조로 속도가 빠르고 확장성이 뛰어남, SOAP는 메시지 크기가 커 성능이 떨어짐
- 보안: SOAP는 WS-Security 등 보안 기능이 강력하지만 REST는 기본적으로 TLS/SSL을 활용

> REST는 웹 서비스와 모바일 API에 적합하며, SOAP는 금융 서비스나 트랜잭션 보장이 필요한 시스템에서 주로 사용됩니다.

## API 버전 관리의 중요성과 방법

API 버전 관리는 하위 호환성을 유지하면서 새로운 기능을 추가하기 위해 필수적입니다.

- URI 기반 버전 관리: /api/v1/resource (직관적이지만 URI 변경 필요)
- 헤더 기반 버전 관리: Accept: application/vnd.company.v1+json (유연하지만 클라이언트 설정 필요)
- 쿼리 파라미터 기반: /api/resource?version=1 (캐싱에 불리함)

> 실무에서는 URI 방식과 헤더 방식을 혼합하여 사용하는 경우가 많음.

## GraphQL과 REST API의 차이점

- 데이터 요청 방식: REST는 여러 엔드포인트 호출 필요, GraphQL은 단일 엔드포인트에서 필요한 데이터만 요청 가능
- 오버페칭/언더페칭 해결: REST는 불필요한 데이터까지 가져올 수 있지만, GraphQL은 필요한 필드만 선택 가능
- 버전 관리: REST는 버전 관리를 해야 하지만, GraphQL은 스키마 확장을 통해 버전 없이 관리 가능
  > REST: 단순한 CRUD API, 캐싱 활용이 중요한 경우  
  > GraphQL: Front-End에서 다양한 데이터를 한 번에 요청할 때

## OAuth 2.0과 인증 방식

권한 위임 프로토콜로, 클라이언트가 사용자 인증을 직접 처리하지 않고 액세스 토큰을 발급받아 API를 호출하는 방식

- Authorization Code Flow: 웹/모바일 애플리케이션에서 보안이 중요한 경우 사용  
> Authorization Code Flow 방식이 가장 많이 사용됩니다.

## HTTP 상태 코드(200, 404, 500)

- 200: 정상적인 요청에 대한 성공 응답
- 404: Not Found: 요청한 리소스를 찾을 수 없음
- 500: 서버 내부에서 예기치 않은 오류 발생

> API 설계 시, 클라이언트가 요청 결과를 명확히 인지할 수 있도록 적절한 상태 코드를 반환하는 것이 중요합니다.

## 멱등성과 API 설계

멱등성이란 같은 요청을 여러 번 보내더라도 결과가 동일한 속성을 의미합니다.

- GET, DELETE, PUT 요청은 멱등성을 가져야 함
- POST는 기본적으로 멱등성을 보장하지 않음()

> 결제 API에서 동일한 결제 요청이 중복되지 않도록 멱등성을 고려하여 설계합니다.

## HATEOAS(헤이티오스)와 RESTful API

API 응답에 관련 링크를 포함하여 클라이언트가 동적으로 API를 탐색할 수 있도록 하는 원칙

```json
{
  "id": 1,
  "name": "Lecture",
  "_links": {
    "self": {
      "href": "/lectures/1"
    },
    "register": {
      "href": "/lectures/1/register"
    }
  }
}
```

REST API의 확장성과 유지보수를 고려할 때 HATEOAS를 적용할 수 있다.

## HTTP/2와 HTTP/1.1의 차이

멀티플렉싱 지원: HTTP/1.1은 한 번에 하나의 요청만 가능하지만, HTTP/2는 여러 요청을 동시에 처리 가능
헤더 압축: HTTP/2는 HPACK을 사용해 헤더 크기를 줄여 네트워크 효율성을 개선
서버 푸시: 서버가 클라이언트 요청 전에 관련 리소스를 미리 전송 가능


