---
layout: post
title:  "Sample Post"
date: "2017-07-02"
categories: category
tags: tag tag1 tag2
main-class: 'misc'
color: '#2DA0C9'
introduction: "introduction"
description: "description"
---

## Hello
sample posting

### 3장. HTTP 메시지
#### HTTP 메시지 소개
* HTTP 메시지는 애플리케이션 간 주고받는 텍스트 메타 정보와 데이터로 이루어진 **데이터 블록** 들이다.
* 메시지는 클라이언트, 서버, 프락시 사이를 흐르며 메시지 방향에 따라 아래와 같이 규정한다.
    * 인바운드 : 서버 방향으로 흐르는 메시지
    * 아웃바운드 : 사용자 에이전트 방향으로 흐르는 메시지
    * 모든 메시지는 다운스트림이고, 메시지 발송자는 수신자의 업스트림이다.

#### 메시지 형식
* 메시지 = 시작줄 + 헤더 블록 + 본문
    * 시작줄과 헤더는 줄 단위로 분리된 아스키 문자열이다.
    * 각 줄은 캐리지 리턴, 즉 줄바꿈 문자열(CRLF)로 끝난다.
    * 본문은 선택 사항이다.
* 요청 메시지
    ```java
    public static void main() {

    }
    <Method> <Request URL> <Version>
    <Header>

    <Entity Body>
    ```

    * 시작줄(start-line, request-line)
        * `Method`
            * `POST`, `GET`, `PUT`, `DELETE`, `HEAD`, `TRACE`, `OPTIONS`
                > [HTTP 1.0 에서는 GET, POST, HEAD만 사용 가능했다.](https://www.w3.org/Protocols/HTTP/1.0/spec.html#Method)

        * `Version`
            * `HTTP/<Major>.<Minor>`
                > HTTP 1.0 이전에는 버전이 필수사항이 아니었다.

* 응답 메시지
    ```java
    <Version> <Status Code> <Reason-Phrase>
    <Header>

    <Entity Body>
    ```

    * 시작줄(start-line, response-line)
        * `Status Code`
            * [HTTP 상태 코드](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
        * `Reason-Phrase`
            * 상태 코드의 의미를 이해할 수 있도록 설명한 짧은 구절

* 공통
    * `Header`
        * 0개 이상의 헤더들이 위치할 수 있다.
        * 헤더 집합은 항상 빈 줄(CRLF)로 끝나야 한다.
        * 이름, 콜론, 공백(없어도 된다), 필드 값, CRLF순으로 위치한다.
            ```
            Content-Length: 51
            Content-Type: text/plain
            Date: Sun, 04 Jun 2017 19:49:00 GMT

            <Entity Body>
            ```
        * 헤더는 다음과 같이 분류할 수 있다.
            * `일반 헤더(General-Header)`
                * 일반적인 정보 제공
                * `Date`, `Cache-Control`
            * `요청 헤더(Request-Header)`
                * 요청에 대한 부가 정보 제공
                * 때때로 조건부 요청 헤더를 넣기도 한다.
                * `Accept`, `Host`, `Uset-Agent`, `If-Match`, `Authorization` 등등
            * `응답 헤더(Response-Header)`
                * 응답에 대한 부가 정보 제공
                * `Server`, `Set-Cookie` 등등
            * `엔티티 헤더(Entity-Header)`
                * 본문 크기와 콘텐츠, 혹은 리소스 그 자체를 서술
                * `Content-Type`, `Allow`, `Location`, `Last-Modified` 등등
            * `확장 헤더(Extended-Header)`
                * 명세에 정의되지 않은 새로운 헤더

#### 상태 코드
* 상태코드, 어떻게 정의할까?
    * [RESTful API Design: what about errors?](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)
    * 권장코드
        * 200 - OK
        * 400 - Bad Request
        * 401 - Unauthorized
        * 403 - Forbidden
        * 404 - Not Found
        * 500 - Internal Server Error

#### 메서드
* [Safe Methods](https://tools.ietf.org/html/rfc7231#section-4.2.1)
    * HTTP 요청의 결과로 서버에 어떤 작용도 없는 메서드, 즉 `GET`, `HEAD`, `OPTIONS` 등을 말한다.
    * 대개 `idempotent` 성질을 갖고 있으면 `Safe Method`라고 할 수 있다.
        > 단, `DELETE` 메서드는 `idempotent` 성질을 갖고 있으나 리소스를 변경하므로 `Safe Method`라고 부를 수 없다.

* [Idempotent Methods](https://tools.ietf.org/html/rfc7231#section-4.2.2)
    * 멱등성이란 동일한 연산을 여러 번 적용할 수 있는 성질을 말한다.
    * `POST`, `LOCK`, `PATCH` 등의 메서드는 `non-idempotent` 메서드다.


#### HTTP 1.0 vs HTTP 1.1
* `Host Header`
    * 요청 헤더에 반드시! `Host`를 사용해서 프록시를 지원해야 한다.
        > [HTTP version 1.1 에서 "host" 필드가 필요한 이유?](https://bluestarblogkr.blogspot.kr/2011/10/http10-11.html)

* `Persistent Connection`
    * 매 요청마다 연결에 필요한 부가 작업으로 대역폭과 성능 저하가 발생하여 이를 해결하기 위해 나왔다. `Keep-Alive` 등의 커넥션 활용 메커니즘은 4장을 참고하자.
* 그 밖에 **추가적인 헤더, 분할 전송, 인증 및 캐시, 향상된 압축 지원** 등의 기능이 추가됐다.
* 자세한 사항은 [RFC 2616 - Changes from HTTP/1.0](http://greenbytes.de/tech/webdav/rfc2616.html#rfc.section.19.6.1) 문서를 참고하자.

##### 커넥션 끊기
* HTTP 애플리케이션은 언제라도 커넥션을 끊을 수 있음을 기억하자.
    * 즉, 애플리케이션은 에러를 방지하기 위한 `Retry` 로직이나 데이터 전송에 대한 순서 보장 등을 고려해야 한다.
    * 또한 커넥션 절반 끊기 등의 우아한 커넥션 끊기 동작도 고려할 수 있다.
* `Content-Length` 값이 일치하지 않거나 존재하지 않으면 수신자는 서버에게 데이터의 정확한 길이를 물어봐야 한다.
    > 일부 HTTP 서버의 커넥션을 끊는다는 행위는 데이터 전송이 끝났음을 의미하기 때문에 `Content-Length` 헤더를 생략하거나 잘못된 길이로 응답하는 경우가 있기 때문이다.
