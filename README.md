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

**동작 방식**

* **로그인**
  * 사용자가 `loginId`, `password` 정보를 전달하면서 서버에서 해당 사용자가 맞는지 확인 

![img_5.png](img_5.png)

* **세션 생성**
  * 세션 ID를 생성(추정 불가능 해야함)
  * UUID 사용
    * `Cookie: mySessionId=zz0101xx-bab9-4b92-9b32-dadb280f4b61`

![img_6.png](img_6.png)

* **세션 id를 쿠키로 전달**
  * 클라이언트롸 서버는 결국 쿠키로 연결되어야함.(세션ID를 사용하여 느슨한 관계 유지)
  * 서버는 클라리언트에 `mySessionId` 라는 이름으로 세션 ID만 `쿠키`에 담아서 전달
  * 클라이언트는 `쿠키` 저장소에 `mySessionId` 쿠키를 보관

![img_7.png](img_7.png)

* **클라이언트의 세션 id 쿠키 전달**
  * 클라이언트는 요청시 항상 `mySessionId` 쿠키를 전달한다.
  * 서버에서는 클라이언트가 전달한 `mySessionId` 쿠키 정보로 세션 저장소를 조회해서 로그인시 보관한 세션 정보를 사용

![img_8.png](img_8.png)

* 정리
  * 쿠키 값을 변조 가능 
    * 예상 불가능한 복잡한 세션 Id를 사용
  * 쿠키에 보관하는 정보는 클라이언트 해킹시 훔칠 가능성
    * 세션 Id를 도둑 맞아도 여기에는 중요한 정보가 없음
  * 쿠키 탈취 후 사용 
    * 해커가 토큰을 훔쳐가도 시간이 지나면 사용할 수 없도록 서버에서 세션의 만료시간을 짧게 유지
    * 해킹이 의심되는 경우 서버에서 해당 세션을 강제로 제거

### HTTPSession 사용하기
서블릿이 제공하는 `HttpSession` 도 결국 우리가 직접 만든 `SessionManager` 와 같은 방식으로 동작한다.

서블릿을 통해 `HttpSession` 을 생성하면 다음과 같은 쿠키를 생성한다.

쿠키의 이름이 `JSESSIONID` 이고, 값은 추정 불가능한 랜덤 값이다.

`Cookie: JSESSIONID=5B78E23B513F50164D6FDD8C97B0AD05`

![img_9.png](img_9.png)

```java
public class SessionConst {
    public static final String LOGIN_MEMBER = "loginMember";
}
```

```java
@PostMapping("/login")
public String loginV3(@Valid @ModelAttribute LoginForm loginForm, BindingResult bindingResult, HttpServletRequest request) {

      if (bindingResult.hasErrors()) {
          return "login/loginForm";
      }
      
      Member loginMember = loginService.login(loginForm.getLoginId(), loginForm.getPassword());
      
      if (loginMember == null) {
          bindingResult.reject("loginFail", "아이디 또는 비밀번호가 맞지 않습니다.");
          return "login/loginForm";
      }
      
      //로그인 성공 처리 TODO
      //세션이 있으면 있는 세션 반환, 없으면 신규 세션을 생성
      HttpSession session = request.getSession(true);
      //세션에 로그인 회원 정보 보관
      session.setAttribute(SessionConst.LOGIN_MEMBER, loginMember);
      
      return "redirect:/";
}
```

```java
@GetMapping("/")
public String homLoginV3Spring(@SessionAttribute(name = SessionConst.LOGIN_MEMBER, required = false) Member member, Model model) {

    //세션에 회원 데이터가 없으면 home
    if(member == null) {
        return "home";
    }

    model.addAttribute("member", member);
    return "loginHome";
}
```

