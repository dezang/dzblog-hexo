---
title: cors
date: 2016-03-08 01:23:06
tags:
---

> XMLHttpRequest cannot load http://192.168.0.103:8080/somthing. A wildcard '*' cannot be used in the 'Access-Control-Allow-Origin' header when the credentials flag is true. Origin 'http://localhost:9000' is therefore not allowed access. The credentials mode of an XMLHttpRequest is controlled by the withCredentials attribute.

## 동일 출처 정책 (Same-Origin Policy)
이 정책에 의해서 자바스크립트(XMLHttpRequest)로 다른 웹페이지에 접근할때는 같은 출처(same origin)의 페이지에만 접근이 가능하다. 같은 출처라는 것은 프로토콜, 호스트명, 포트가 같다는 것을 의미한다. 즉 쉽게 말하면 웹페이지의 스크립트는 그 페이지와 같은 서버에 있는 주소로만 ajax 요청을 할 수 있다는 것이다.
 
추가 정책이 CORS(Cross-Origin Resource Sharing) 이다. 이 정책의 특징은 서버에서 외부 요청을 허용할 경우 ajax요청이 가능해지는 방식이다. CORS에 대해서 설명하기전에 서버의 도움없이 동일 출처 정책(same-origin policy)을 회피하여 외부 서버로 요청을 날릴 수 있는 방법을 몇가지 소개 한다.
 
- http://adrenal.tistory.com/16
- http://ooz.co.kr/232

### 터미널 실행
```sh
open -a Google\ Chrome --args --disable-web-security
```

## Using Filter
```java
@Slf4j
@Component
public class CORSFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse httpServletResponse = (HttpServletResponse) response;
        String method = req.getMethod();

        httpServletResponse.setHeader("Access-Control-Allow-Origin", "*");
        httpServletResponse.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE, PUT");
        httpServletResponse.setHeader("Access-Control-Max-Age", "3600");
        httpServletResponse.setHeader("Access-Control-Allow-Headers", "MOBILE, ajax, Origin, X-Requested-With, Content-Type, Accept, Key, Authorization");

        chain.doFilter(request, response);

    }

    @Override
    public void destroy() {

    }
}
```

## CORS-support-in-Spring-Framework
기본값
> By default all origins and GET, HEAD and POST methods are allowed.

- https://spring.io/guides/gs/rest-service-cors/
- https://spring.io/blog/2015/06/08/cors-support-in-spring-framework
- http://devtrans.tistory.com/entry/CORS-support-in-Spring-Framework
- http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/html/cors.html

## Override Spring Framework version in Spring Boot
- http://blog.codeleak.pl/2015/09/override-spring-framework-version-in.html
- http://java.ihoney.pe.kr/402
 
#### 참고

- http://devtrans.tistory.com/entry/CORS-support-in-Spring-Framework