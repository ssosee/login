# 🔐 login
## MVC 2편(김영한)
**목표: Login 기능을 구현**

### 로그인 요구사항
* 홈 화면 - 로그인 전
  * 회원 가입
  * 로그인
* 홈 화면 - 로그인 후
  * 본인 이름(누구님 환영합니다.)
  * 상품 관리
  * 로그 아웃
* 보안 요구사항
  * 로그인 사용자만 상품에 접근하고, 관리할 수 있음
  * 로그인 하지 않은 사용자가 상품 관리에 접근하면 로그인 화면으로 이동
* 회원 가입, 상품 관리

### 홈화면
![img.png](img.png)

### 로그인 성공
![img_1.png](img_1.png)

### 회원 가입
![img_2.png](img_2.png)

### 로그인
![img_3.png](img_3.png)

### 상품 목록
![img_4.png](img_4.png)


### 패키지 구조 설계
**package 구조**
* hello.login
  * domain
    * item
    * member
    * login
  * web
    * item
    * member
    * login
---
### 도메인이 가장 중요하다!!!!

> **`web`의 죽음을 `domain`에게 알리지 마라..!**

`domain`은 화면, UI, 기술 인프라 등등의 영역을 제외한 시스템이 구현해야하는 `핵심 비즈니스 업무 영역`을 말함

향후 `web`을 다른 기술로 바꾸어도 `domain`은 그대로 유지할 수 있어야 한다.

이렇게 하려면 `web`은 `domain`을 알고있지만, `domain`은 `web`을 모르도록 설계 해야한다.

이것을 `web`은 `domain`을 의존하지만, `domain`은 `web`을 의존하지 않는다고 표현한다.

예를 들어 `web` 패키지를 모드 삭제해도 `domain`에는 전혀 영향이 없도록 의존관계를 설계하는 것이 중요하다.

즉, **`domain`은 `web`을 참조하면 안된다.!**

---

### 로그인 처리하기 -세션 직접 만들기
세션을 직접 개발해서 적용해보자!!

세션 관리는 크게 다음 3가지 기능을 제공하면 된다.

* 세션 생성
  * `sessionId` 생성(임의의 추청 불가능한 🎲랜덤 값)
  * 세션 저장소에 `sessionId` 와 보관할 값 저장
  * `sessionId` 로 응답 쿠키를 생성해서 클라이언트에 전달
* 세션 조회
  * 클라이언트가 요청한 `sessionId` 쿠키의 값으로, 세션 저장소에 보관한 값 조회
* 세션 만료
  * 클라이언트가 요청한 `sessionId` 쿠키의 값으로, 세션 저장소에 보관한 `sessionId` 와 값 제거

**SessionManager -세션관리**
```java
@Component
public class SessionManager {
    private Map<String, Object> sessionStore = new ConcurrentHashMap<>();
    public static final String SESSION_COOKIE_NAME = "mySessionId";

    /**
     * 세션 생성
     * * sessionId 생성 (임의의 추정 불가능한 랜덤 값)
     * * 세션 저장소에 sessionId와 보관할 값 저장
     * * sessionId로 응답 쿠키를 생성해서 클라이언트에 전달
     */
    public void createSession(Object value, HttpServletResponse response) {

        //sessionId 생성, 값을 세션에 저장
        String sessionId = UUID.randomUUID().toString();
        sessionStore.put(sessionId, value);

        //쿠키 생성
        Cookie mySessionCookie = new Cookie(SESSION_COOKIE_NAME, sessionId);
        response.addCookie(mySessionCookie);
    }

    /**
     * 세션 조회
     */
    public Object getSession(HttpServletRequest request) {
//        Cookie[] cookies = request.getCookies();
//        if (cookies == null) {
//            return null;
//        }
//        for(Cookie cookie : cookies) {
//            if(cookie.getName().equals(SESSION_COOKIE_NAME)) {
//                return sessionStore.get(cookie.getValue());
//            }
//        }
//        return null;
        Cookie sessionCookie = findCookie(request, SESSION_COOKIE_NAME);
        if(sessionCookie == null) {
            return null;
        }
        return sessionStore.get(sessionCookie.getValue());
    }

    /**
     * 세션 만료
     */
    public void expire(HttpServletRequest request) {
        Cookie sessionCookie = findCookie(request, SESSION_COOKIE_NAME);
        if(sessionCookie != null) {
            sessionStore.remove(sessionCookie.getValue());
        }
    }

    public Cookie findCookie(HttpServletRequest request, String cookieName) {
        Cookie[] cookies = request.getCookies();
        if (cookies == null) {
            return null;
        }
        return Arrays.stream(cookies)
                .filter(c -> c.getName().equals(cookieName))
                .findAny()
                .orElse(null);
    }
}
```

**SessionManagerTest -세션관리 테스트**

`HttpServletRequest`, `HttpServletResponse` 객체를 직접 사용할 수 없기 때문에,

테스트에서 비슷한 역할을 해주는 가짜 `MockHttpServletRequest`, `MockHttpServletResponse` 를 사용
```java
class SessionManagerTest {
    SessionManager sessionManager = new SessionManager();

    @Test
    void sessionTest() {
        
        //세션 생성(서버에서 클라이언트로)
        MockHttpServletResponse response = new MockHttpServletResponse();
        Member member = new Member();
        sessionManager.createSession(member, response);

        //요청에 응답 쿠키 저장(클라이언트에서 서버로)
        MockHttpServletRequest request = new MockHttpServletRequest();
        request.setCookies(response.getCookies());

        //세션 조회
        Object result = sessionManager.getSession(request);
        Assertions.assertThat(result).isEqualTo(member);

        //세션 만료
        sessionManager.expire(request);
        Object expired = sessionManager.getSession(request);
        Assertions.assertThat(expired).isNull();
    }
}
```

