# π login
## MVC 2νΈ(κΉμν)
**λͺ©ν: Login κΈ°λ₯μ κ΅¬ν**

### λ‘κ·ΈμΈ μκ΅¬μ¬ν­
* ν νλ©΄ - λ‘κ·ΈμΈ μ 
  * νμ κ°μ
  * λ‘κ·ΈμΈ
* ν νλ©΄ - λ‘κ·ΈμΈ ν
  * λ³ΈμΈ μ΄λ¦(λκ΅¬λ νμν©λλ€.)
  * μν κ΄λ¦¬
  * λ‘κ·Έ μμ
* λ³΄μ μκ΅¬μ¬ν­
  * λ‘κ·ΈμΈ μ¬μ©μλ§ μνμ μ κ·Όνκ³ , κ΄λ¦¬ν  μ μμ
  * λ‘κ·ΈμΈ νμ§ μμ μ¬μ©μκ° μν κ΄λ¦¬μ μ κ·Όνλ©΄ λ‘κ·ΈμΈ νλ©΄μΌλ‘ μ΄λ
* νμ κ°μ, μν κ΄λ¦¬

### ννλ©΄
![img.png](img.png)

### λ‘κ·ΈμΈ μ±κ³΅
![img_1.png](img_1.png)

### νμ κ°μ
![img_2.png](img_2.png)

### λ‘κ·ΈμΈ
![img_3.png](img_3.png)

### μν λͺ©λ‘
![img_4.png](img_4.png)


### ν¨ν€μ§ κ΅¬μ‘° μ€κ³
**package κ΅¬μ‘°**
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
### λλ©μΈμ΄ κ°μ₯ μ€μνλ€!!!!

> **`web`μ μ£½μμ `domain`μκ² μλ¦¬μ§ λ§λΌ..!**

`domain`μ νλ©΄, UI, κΈ°μ  μΈνλΌ λ±λ±μ μμ­μ μ μΈν μμ€νμ΄ κ΅¬νν΄μΌνλ `ν΅μ¬ λΉμ¦λμ€ μλ¬΄ μμ­`μ λ§ν¨

ν₯ν `web`μ λ€λ₯Έ κΈ°μ λ‘ λ°κΎΈμ΄λ `domain`μ κ·Έλλ‘ μ μ§ν  μ μμ΄μΌ νλ€.

μ΄λ κ² νλ €λ©΄ `web`μ `domain`μ μκ³ μμ§λ§, `domain`μ `web`μ λͺ¨λ₯΄λλ‘ μ€κ³ ν΄μΌνλ€.

μ΄κ²μ `web`μ `domain`μ μμ‘΄νμ§λ§, `domain`μ `web`μ μμ‘΄νμ§ μλλ€κ³  νννλ€.

μλ₯Ό λ€μ΄ `web` ν¨ν€μ§λ₯Ό λͺ¨λ μ­μ ν΄λ `domain`μλ μ ν μν₯μ΄ μλλ‘ μμ‘΄κ΄κ³λ₯Ό μ€κ³νλ κ²μ΄ μ€μνλ€.

μ¦, **`domain`μ `web`μ μ°Έμ‘°νλ©΄ μλλ€.!**

---

### λ‘κ·ΈμΈ μ²λ¦¬νκΈ° -μΈμ μ§μ  λ§λ€κΈ°
π§βπ» μΈμμ μ§μ  κ°λ°ν΄μ μ μ©ν΄λ³΄μ!!

μΈμ κ΄λ¦¬λ ν¬κ² λ€μ 3κ°μ§ κΈ°λ₯μ μ κ³΅νλ©΄ λλ€.

* μΈμ μμ±
  * `sessionId` μμ±(μμμ μΆμ²­ λΆκ°λ₯ν π²λλ€ κ°)
  * μΈμ μ μ₯μμ `sessionId` μ λ³΄κ΄ν  κ° μ μ₯
  * `sessionId` λ‘ μλ΅ μΏ ν€λ₯Ό μμ±ν΄μ ν΄λΌμ΄μΈνΈμ μ λ¬
* μΈμ μ‘°ν
  * ν΄λΌμ΄μΈνΈκ° μμ²­ν `sessionId` μΏ ν€μ κ°μΌλ‘, μΈμ μ μ₯μμ λ³΄κ΄ν κ° μ‘°ν
* μΈμ λ§λ£
  * ν΄λΌμ΄μΈνΈκ° μμ²­ν `sessionId` μΏ ν€μ κ°μΌλ‘, μΈμ μ μ₯μμ λ³΄κ΄ν `sessionId` μ κ° μ κ±°

**SessionManager -μΈμκ΄λ¦¬**
```java
@Component
public class SessionManager {
    private Map<String, Object> sessionStore = new ConcurrentHashMap<>();
    public static final String SESSION_COOKIE_NAME = "mySessionId";

    /**
     * μΈμ μμ±
     * * sessionId μμ± (μμμ μΆμ  λΆκ°λ₯ν λλ€ κ°)
     * * μΈμ μ μ₯μμ sessionIdμ λ³΄κ΄ν  κ° μ μ₯
     * * sessionIdλ‘ μλ΅ μΏ ν€λ₯Ό μμ±ν΄μ ν΄λΌμ΄μΈνΈμ μ λ¬
     */
    public void createSession(Object value, HttpServletResponse response) {

        //sessionId μμ±, κ°μ μΈμμ μ μ₯
        String sessionId = UUID.randomUUID().toString();
        sessionStore.put(sessionId, value);

        //μΏ ν€ μμ±
        Cookie mySessionCookie = new Cookie(SESSION_COOKIE_NAME, sessionId);
        response.addCookie(mySessionCookie);
    }

    /**
     * μΈμ μ‘°ν
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
     * μΈμ λ§λ£
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

**SessionManagerTest -μΈμκ΄λ¦¬ νμ€νΈ**

`HttpServletRequest`, `HttpServletResponse` κ°μ²΄λ₯Ό μ§μ  μ¬μ©ν  μ μκΈ° λλ¬Έμ,

νμ€νΈμμ λΉμ·ν μ­ν μ ν΄μ£Όλ κ°μ§ `MockHttpServletRequest`, `MockHttpServletResponse` λ₯Ό μ¬μ©
```java
class SessionManagerTest {
    SessionManager sessionManager = new SessionManager();

    @Test
    void sessionTest() {
        
        //μΈμ μμ±(μλ²μμ ν΄λΌμ΄μΈνΈλ‘)
        MockHttpServletResponse response = new MockHttpServletResponse();
        Member member = new Member();
        sessionManager.createSession(member, response);

        //μμ²­μ μλ΅ μΏ ν€ μ μ₯(ν΄λΌμ΄μΈνΈμμ μλ²λ‘)
        MockHttpServletRequest request = new MockHttpServletRequest();
        request.setCookies(response.getCookies());

        //μΈμ μ‘°ν
        Object result = sessionManager.getSession(request);
        Assertions.assertThat(result).isEqualTo(member);

        //μΈμ λ§λ£
        sessionManager.expire(request);
        Object expired = sessionManager.getSession(request);
        Assertions.assertThat(expired).isNull();
    }
}
```

**λμ λ°©μ**

* **λ‘κ·ΈμΈ**
  * μ¬μ©μκ° `loginId`, `password` μ λ³΄λ₯Ό μ λ¬νλ©΄μ μλ²μμ ν΄λΉ μ¬μ©μκ° λ§λμ§ νμΈ 

![img_5.png](img_5.png)

* **μΈμ μμ±**
  * μΈμ IDλ₯Ό μμ±(μΆμ  λΆκ°λ₯ ν΄μΌν¨)
  * UUID μ¬μ©
    * `Cookie: mySessionId=zz0101xx-bab9-4b92-9b32-dadb280f4b61`

![img_6.png](img_6.png)

* **μΈμ idλ₯Ό μΏ ν€λ‘ μ λ¬**
  * ν΄λΌμ΄μΈνΈλ‘Έ μλ²λ κ²°κ΅­ μΏ ν€λ‘ μ°κ²°λμ΄μΌν¨.(μΈμIDλ₯Ό μ¬μ©νμ¬ λμ¨ν κ΄κ³ μ μ§)
  * μλ²λ ν΄λΌλ¦¬μΈνΈμ `mySessionId` λΌλ μ΄λ¦μΌλ‘ μΈμ IDλ§ `μΏ ν€`μ λ΄μμ μ λ¬
  * ν΄λΌμ΄μΈνΈλ `μΏ ν€` μ μ₯μμ `mySessionId` μΏ ν€λ₯Ό λ³΄κ΄

![img_7.png](img_7.png)

* **ν΄λΌμ΄μΈνΈμ μΈμ id μΏ ν€ μ λ¬**
  * ν΄λΌμ΄μΈνΈλ μμ²­μ ν­μ `mySessionId` μΏ ν€λ₯Ό μ λ¬νλ€.
  * μλ²μμλ ν΄λΌμ΄μΈνΈκ° μ λ¬ν `mySessionId` μΏ ν€ μ λ³΄λ‘ μΈμ μ μ₯μλ₯Ό μ‘°νν΄μ λ‘κ·ΈμΈμ λ³΄κ΄ν μΈμ μ λ³΄λ₯Ό μ¬μ©

![img_8.png](img_8.png)

* μ λ¦¬
  * μΏ ν€ κ°μ λ³μ‘° κ°λ₯ 
    * μμ λΆκ°λ₯ν λ³΅μ‘ν μΈμ Idλ₯Ό μ¬μ©
  * μΏ ν€μ λ³΄κ΄νλ μ λ³΄λ ν΄λΌμ΄μΈνΈ ν΄νΉμ νμΉ  κ°λ₯μ±
    * μΈμ Idλ₯Ό λλ λ§μλ μ¬κΈ°μλ μ€μν μ λ³΄κ° μμ
  * μΏ ν€ νμ·¨ ν μ¬μ© 
    * ν΄μ»€κ° ν ν°μ νμ³κ°λ μκ°μ΄ μ§λλ©΄ μ¬μ©ν  μ μλλ‘ μλ²μμ μΈμμ λ§λ£μκ°μ μ§§κ² μ μ§
    * ν΄νΉμ΄ μμ¬λλ κ²½μ° μλ²μμ ν΄λΉ μΈμμ κ°μ λ‘ μ κ±°

### HTTPSession μ¬μ©νκΈ°
μλΈλ¦Ώμ΄ μ κ³΅νλ `HttpSession` λ κ²°κ΅­ μ°λ¦¬κ° μ§μ  λ§λ  `SessionManager` μ κ°μ λ°©μμΌλ‘ λμνλ€.

μλΈλ¦Ώμ ν΅ν΄ `HttpSession` μ μμ±νλ©΄ λ€μκ³Ό κ°μ μΏ ν€λ₯Ό μμ±νλ€.

μΏ ν€μ μ΄λ¦μ΄ `JSESSIONID` μ΄κ³ , κ°μ μΆμ  λΆκ°λ₯ν λλ€ κ°μ΄λ€.

`Cookie: JSESSIONID=5B78E23B513F50164D6FDD8C97B0AD05`

![img_9.png](img_9.png)

**SessionConst**

`HttpSession` μ λ°μ΄ν°λ₯Ό λ³΄κ΄νκ³  μ‘°νν  λ, κ°μ μ΄λ¦μ΄ μ€λ³΅λμ΄ μ¬μ©λλ―λ‘, μμλ₯Ό νλ μ μνλ€.
```java
public class SessionConst {
    public static final String LOGIN_MEMBER = "loginMember";
}
```

**LoginController - loginV3()**

* μΈμ μμ± `request.getSession()` `default: true`
  * `request.getSession(true)`
    * μΈμμ΄ μμΌλ©΄ κΈ°μ‘΄ μΈμ λ°ν
    * μΈμμ΄ μμΌλ©΄ μλ‘μ΄ μΈμ μμ± ν λ°ν
  * `request.getSession(false)`
    * μΈμμ΄ μμΌλ©΄ κΈ°μ‘΄ μΈμ λ°ν
    * μΈμμ΄ μμΌλ©΄ `null` λ°ν
```java
@PostMapping("/login")
public String loginV3(@Valid @ModelAttribute LoginForm loginForm, BindingResult bindingResult, HttpServletRequest request) {

      if (bindingResult.hasErrors()) {
          return "login/loginForm";
      }
      
      Member loginMember = loginService.login(loginForm.getLoginId(), loginForm.getPassword());
      
      if (loginMember == null) {
          bindingResult.reject("loginFail", "μμ΄λ λλ λΉλ°λ²νΈκ° λ§μ§ μμ΅λλ€.");
          return "login/loginForm";
      }
      
      //λ‘κ·ΈμΈ μ±κ³΅ μ²λ¦¬ TODO
      //μΈμμ΄ μμΌλ©΄ μλ μΈμ λ°ν, μμΌλ©΄ μ κ· μΈμμ μμ±
      HttpSession session = request.getSession(true);
      //μΈμμ λ‘κ·ΈμΈ νμ μ λ³΄ λ³΄κ΄
      session.setAttribute(SessionConst.LOGIN_MEMBER, loginMember);
      return "redirect:/";
}
```

**LoginController - logoutV3()**

`session.invalidate()` μΈμμ μ κ±°νλ€.
```java
@PostMapping("/logout")
public String logoutV3(HttpServletRequest request) {
      HttpSession session = request.getSession(false);
      if(session != null){
          session.invalidate();
      }
      return "redirect:/";
}
```

**HomeController - homLoginV3()**

`session.getAttribute(SessionConst.LOGIN_MEMBER)` : λ‘κ·ΈμΈ μμ μ μΈμμ λ³΄κ΄ν νμ κ°μ²΄λ₯Ό
μ°Ύλλ€.
```java
@GetMapping("/")
public String homLoginV3(HttpServletRequest request, Model model) {

    HttpSession session = request.getSession(false);
    if (session == null) {
        return "home";
    }

    Member loginMember = (Member) session.getAttribute(SessionConst.LOGIN_MEMBER);
    //μΈμμ νμ λ°μ΄ν°κ° μμΌλ©΄ home
    if(loginMember == null) {
        return "home";
    }

    model.addAttribute("member", loginMember);
    return "loginHome";
}
```

**HomeController - homLoginV3Spring()**

`@SessionAttribute`

μ€νλ§μ μΈμμ λ νΈλ¦¬νκ² μ¬μ©ν  μ μλλ‘ `@SessionAttribute` μ μ§μνλ€.

μ΄λ―Έ λ‘κ·ΈμΈ λ μ¬μ©μλ₯Ό μ°Ύμ λλ μλμ κ°μ΄ μ¬μ©νλ©΄ λλ€.

_μ°Έκ³ λ‘ μ΄ κΈ°λ₯μ μΈμμ μμ± νμ§ μλλ€._

`@SessionAttribute(name = SessionConst.LOGIN_MEMBER, required = false)`

```java
@GetMapping("/")
public String homLoginV3Spring(@SessionAttribute(name = SessionConst.LOGIN_MEMBER, required = false) Member member, Model model) {

    //μΈμμ νμ λ°μ΄ν°κ° μμΌλ©΄ home
    if(member == null) {
        return "home";
    }

    model.addAttribute("member", member);
    return "loginHome";
}
```

### TrackingModes
λ‘κ·ΈμΈμ μ²μ μλνλ©΄ URLμ΄ λ€μκ³Ό κ°μ΄ `jsessionid` λ₯Ό ν¬ν¨νκ³  μλ κ²μ νμΈν  μ μλ€.
```text
http://localhost:8080/;jsessionid=F59911518B921DF62D09F0DF8F83F872
```
**μ΄κ²μ μΉ λΈλΌμ°μ κ° μΏ ν€λ₯Ό μ§μνμ§ μμ λ μΏ ν€ λμ  URLμ ν΅ν΄μ μΈμμ μ μ§νλ λ°©λ²!**

νμλ¦¬ν κ°μ ννλ¦Ώμ μμ§μ ν΅ν΄μ λ§ν¬λ₯Ό κ±Έλ©΄ μλμΌλ‘ ν¬ν¨ν΄μ€λ€.

μλ² μμ₯μμ μΉ λΈλΌμ°μ κ° μΏ ν€λ₯Ό μ§μνλμ§ μλμ§ μ΅μ΄μλ ννλ€μ§ λͺ»νλ―λ‘, μΏ ν€ κ°λ μ λ¬νκ³ , URLμ jsessionid λ ν¨κ» μ λ¬νλ€.

**URL μ λ¬ λ°©μμ λκ³  ν­μ μΏ ν€λ₯Ό ν΅ν΄μλ§ μΈμμ μ μ§**νκ³  μΆμΌλ©΄ λ€μ μ΅μμ λ£μ΄μ£Όλ©΄ λλ€. 

μ΄λ κ² νλ©΄ URLμ `jsessionid` κ° λΈμΆλμ§ μλλ€.
```properties
server.servlet.session.tracking-modes=cookie
```

### μΈμ νμμμ

κΈλ‘λ² μ€μ 
```properties
# 60μ΄
server.servlet.session.timeout=60
```

νΉμ  μΈμ λ¨μλ‘ μκ° μ€μ 
```java
session.setMaxInactiveInterval(1800); //1800μ΄
```
μΈμμ νμμμ μκ°μ ν΄λΉ μΈμκ³Ό κ΄λ ¨λ `JSESSIONID` λ₯Ό μ λ¬νλ `HTTP μμ²­`μ΄ μμΌλ©΄ νμ¬ μκ°μΌλ‘ λ€μ μ΄κΈ°ν λλ€.
μ΄λ κ² μ΄κΈ°ν λλ©΄ μΈμ νμμμμΌλ‘ μ€μ ν μκ°λμ μΈμμ μΆκ°λ‘ μ¬μ©ν  μ μλ€.
`session.getLastAccessedTime()` : μ΅κ·Ό μΈμ μ κ·Ό μκ°
LastAccessedTime μ΄νλ‘ timeout μκ°μ΄ μ§λλ©΄, WASκ° λ΄λΆμμ ν΄λΉ μΈμμ μ κ±°

### μΈμ μ λ¦¬

**μΈμμλ μ΅μνμ λ°μ΄ν°λ§ λ³΄κ΄**ν΄μΌ νλ€λ μ μ΄λ€. 

λ³΄κ΄ν `λ°μ΄ν° μ©λ * μ¬μ©μ μ`λ‘ μΈμμ λ©λͺ¨λ¦¬ μ¬μ©λμ΄ κΈκ²©νκ² λμ΄λμ μ₯μ λ‘ μ΄μ΄μ§ μ μλ€. 

μΆκ°λ‘ μΈμμ μκ°μ λλ¬΄ κΈΈκ² κ°μ Έκ°λ©΄ λ©λͺ¨λ¦¬ μ¬μ©μ΄ κ³μ λμ  λ  μ μμΌλ―λ‘ μ λΉν μκ°μ μ ννλ κ²μ΄ νμνλ€. 

`κΈ°λ³Έμ΄ 30λΆ`μ΄λΌλ κ²μ κΈ°μ€μΌλ‘ κ³ λ―Όνλ©΄ λλ€.


## μλΈλ¦Ώ νν°
μκ΅¬μ¬ν­μ λ³΄λ©΄ λ‘κ·ΈμΈ ν μ¬μ©μλ§ μν κ΄λ¦¬ νμ΄μ§μ λ€μ΄κ° μ μμ΄μΌ νλ€.

λ¬Έμ λ λ‘κ·ΈμΈ νμ§ μμ μ¬μ©μλ URLμ μ§μ  νΈμΆνλ©΄ μν κ΄λ¦¬ νλ©΄μ λ€μ΄κ° μ μλ€λ μ μ΄λ€.

`http://localhost:8080/items`

μν κ΄λ¦¬ μ»¨νΈλ‘€λ¬μμ λ‘κ·ΈμΈ μ¬λΆλ₯Ό μ²΄ν¬νλ λ‘μ§μ νλνλ μμ±νλ©΄ λκ² μ§λ§, 
λ±λ‘, μμ , μ­μ , μ‘°ν λ±λ± μν κ΄λ¦¬μ λͺ¨λ  μ»¨νΈλ‘€λ¬ λ‘μ§μ κ³΅ν΅μΌλ‘ λ‘κ·ΈμΈ μ¬λΆλ₯Ό νμ΄ν΄μΌ νλ€.
λ ν° λ¬Έμ λ **ν₯ν λ‘κ·ΈμΈκ³Ό κ΄λ ¨λ λ‘μ§μ΄ λ³κ²½λ  λ λ§λ€ μμ±ν λͺ¨λ  λ‘μ§μ μμ ν΄μΌ ν μ μλ€....**

μ΄λ κ² μ νλ¦¬μΌμ΄μ μ¬λ¬ λ‘μ§μμ κ³΅ν΅μΌλ‘ κ΄μ¬μ΄ μλ κ²μ κ³΅ν΅ κ΄μ¬μ¬(cross-cutting concern)λΌκ³  νλ€.

μ΄λ¬ν κ³΅ν΅ κ΄μ¬μ¬λ `μ€νλ§μ AOP`λ‘λ ν΄κ²° ν  μ μμ§λ§, μΉκ³Ό κ΄λ ¨λ κ³΅ν΅ κ΄μ¬μ¬λ μ§κΈλΆν° μ€λͺν  μλΈλ¦Ώ νν° λλ μ€νλ§ μΈν°μν°λ₯Ό μ¬μ©νλ κ²μ΄ μ’λ€.
μΉκ³Ό κ΄λ ¨λ κ³΅ν΅ κ΄μ¬μ¬λ₯Ό μ²λ¦¬ν  λλ HTTPμ ν€λλ URLμ μ λ³΄λ€μ΄ νμνλ°, μλΈλ¦Ώ νν°λ μ€νλ§ μΈν°μν°λ `HttpServletRequest` λ₯Ό μ κ³΅νλ€.

### νν°μ νλ¦
νν°λ μλΈλ¦Ώ(μ€νλ§μ λμ€ν¨μ² μλΈλ¦Ώ)μ΄ μ§μνλ **μλ¬Έμ₯**μ΄λ€.

**νν° νλ¦**
```text
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μ»¨νΈλ‘€λ¬
```
νν°λ₯Ό μ μ©νλ©΄ νν°κ° νΈμΆ λ λ€μμ μλΈλ¦Ώμ΄ νΈμΆ.

λͺ¨λ  κ³ κ°μ μμ²­ λ‘κ·Έλ₯Ό λ¨κΈ°λ μκ΅¬μ¬ν­μ΄ μλ€λ©΄ νν°λ₯Ό μ¬μ©νλ©΄λλ€.

μ°Έκ³ λ‘ νν°λ νΉμ  URL ν¨ν΄μ μ μ©ν  μ μλ€. `*/` μ΄λΌκ³  μ μ©νλ©΄ λͺ¨λ  μμ²­μ νν°κ° μ μ©λλ€.


**νν° μ ν**
```text
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μ»¨νΈλ‘€λ¬ //λ‘κ·ΈμΈ μ¬μ©μ
HTTP μμ²­ -> WAS -> νν°(μ μ νμ§ μμ μμ²­μ΄λΌ νλ¨, μλΈλ¦Ώ νΈμΆ X) //λΉ λ‘κ·ΈμΈ μ¬μ©μ
```
νν°μμ μ μ νμ§ μμ μμ²­μ΄λΌκ³  νλ¨νλ©΄ κ±°κΈ°μ `return;` νλ©΄ μ’λ€.

κ·Έλμ **λ‘κ·ΈμΈ μ¬λΆλ₯Ό μ²΄ν¬**νκΈ°μ λ± μ’λ€! π

**νν° μ²΄μΈ**
```text
HTTP μμ²­ -> WAS -> νν°1(λ‘κ·Έ1) -> νν°2(λ‘κ·Έ2) -> νν°3(λ‘κ·ΈμΈ μ¬λΆ) -> μλΈλ¦Ώ -> μ»¨νΈλ‘€λ¬
```
**νν°λ μ²΄μΈμΌλ‘ κ΅¬μ±**λλλ°, μ€κ°μ νν°λ₯Ό μμ λ‘­κ² μΆκ°ν  μ μλ€.

μλ₯Ό λ€μ΄μ λ‘κ·Έλ₯Ό λ¨κΈ°λ νν°λ₯Ό λ¨Όμ  μ μ©νκ³ , κ·Έ λ€μμ λ‘κ·ΈμΈ μ¬λΆλ₯Ό μ²΄ν¬νλ νν°λ₯Ό λ§λ€ μ μλ€.

**νν° μΈν°νμ΄μ€**

νν° μΈν°νμ΄μ€λ₯Ό κ΅¬ννκ³  λ±λ‘νλ©΄ 

μλΈλ¦Ώ μ»¨νμ΄λκ° νν°λ₯Ό μ±κΈν€ κ°μ²΄λ‘ μμ±νκ³ , κ΄λ¦¬νλ€.
```java
public interface Filter { 
    default void init(FilterConfig filterConfig) throws ServletException;
    
    void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;
    
    default void destroy();
}
```
* `init()`: νν° μ΄κΈ°ν λ©μλ, μλΈλ¦Ώ μ»¨νμ΄λκ° μμ±λ  λ νΈμΆλλ€.
* `doFilter()`: κ³ κ°μ μμ²­μ΄ μ¬ λ λ§λ€ ν΄λΉ λ©μλκ° νΈμΆλλ€. νν°μ λ‘μ§μ κ΅¬ννλ©΄ λλ€.
* `destroy()`: νν° μ’λ£ λ©μλ, μλΈλ¦Ώ μ»¨νμ΄λκ° μ’λ£λ  λ νΈμΆλλ€.

### μλΈλ¦Ώ νν° -μΈμ¦ μ²΄ν¬
λλμ΄ μΈμ¦ μ²΄ν¬ νν°λ₯Ό κ°λ°ν΄λ³΄μ. 

λ‘κ·ΈμΈ λμ§ μμ μ¬μ©μλ μν κ΄λ¦¬ λΏλ§ μλλΌ λ―Έλμ κ°λ°λ  νμ΄μ§μλ μ κ·Όνμ§ λͺ»νλλ‘ νμ.!

**LoginCheckFilter -μΈμ¦ μ²΄ν¬ νν°**
```java
@Slf4j
public class LoginCheckFilter implements Filter {

    private static final String[] whitelist = {"/", "/members/add", "/login", "/logout", "/css/*"};

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String requestURI = httpRequest.getRequestURI();

        HttpServletResponse httpResponse = (HttpServletResponse) response;

        try {
          log.info("μΈμ¦ μ²΄ν¬ νν° μμ{}", requestURI);
          if(isLoginCheckPath(requestURI)) {
              log.info("μΈμ¦ μ²΄ν¬ λ‘μ§ μ€ν {}", requestURI);
              HttpSession session = httpRequest.getSession(false);
              if(session == null || session.getAttribute(SessionConst.LOGIN_MEMBER) == null) {
                  log.info("λ―ΈμΈμ¦ μ¬μ©μ μμ²­ {}", requestURI);
                  // λ‘κ·ΈμΈμΌλ‘ redirect
                  httpResponse.sendRedirect("/login?redirectURL="+requestURI);
                  return;
              }
          }
          /**
           * λ€μ νν°κ° μμΌλ©΄ νν°λ₯Ό νΈμΆνκ³ , νν°κ° μμΌλ©΄ μλΈλ¦Ώμ νΈμΆ!
           * μ΄ λ‘μ§μ΄ μλ€λ©΄ λ€μ λ¨κ³λ‘ μ§ν λΆκ°λ₯..
           */
          chain.doFilter(request, response);
          
        } catch (Exception e) {
            throw e; //μμΈ λ‘κΉ κ°λ₯νμ§λ§, ν°μΊ£κΉμ§ μμΈλ₯Ό λ³΄λ΄μ£Όμ΄μΌ ν¨
        } finally {
            log.info("μΈμ¦ μ²΄ν¬ νν° μ’λ£ {}", requestURI);
        }
    }

    /**
     * νμ΄νΈ λ¦¬μ€νΈμ κ²½μ° μΈμ¦ μ²΄ν¬ X
     */
    private boolean isLoginCheckPath(String requestURI) {
        return !PatternMatchUtils.simpleMatch(whitelist, requestURI);
    }
}
```
* `httpResponse.sendRedirect("/login?redirectURL=" + requestURI);`
  * **λ―ΈμΈμ¦ μ¬μ©μλ λ‘κ·ΈμΈ νλ©΄μΌλ‘ λ¦¬λ€μ΄λ νΈ** νλ€. κ·Έλ°λ° **λ‘κ·ΈμΈ μ΄νμ λ€μ νμΌλ‘ μ΄λν΄λ²λ¦¬λ©΄,
  μνλ κ²½λ‘λ₯Ό λ€μ μ°Ύμκ°μΌ νλ λΆνΈν¨**μ΄ μλ€. μλ₯Ό λ€μ΄μ μν κ΄λ¦¬ νλ©΄μ λ³΄λ €κ³  λ€μ΄κ°λ€κ°
  λ‘κ·ΈμΈ νλ©΄μΌλ‘ μ΄λνλ©΄, λ‘κ·ΈμΈ μ΄νμ λ€μ μν κ΄λ¦¬ νλ©΄μΌλ‘ λ€μ΄κ°λ κ²μ΄ μ’λ€. μ΄λ° λΆλΆμ΄
  κ°λ°μ μμ₯μμλ μ’ κ·μ°?μ μ μμ΄λ μ¬μ©μ μμ₯μΌλ‘ λ³΄λ©΄ νΈλ¦¬ν κΈ°λ₯μ΄λ€. μ΄λ¬ν κΈ°λ₯μ μν΄
  νμ¬ **μμ²­ν κ²½λ‘μΈ requestURI λ₯Ό /login μ μΏΌλ¦¬ νλΌλ―Έν°λ‘ ν¨κ» μ λ¬**νλ€. λ¬Όλ‘  /login
  μ»¨νΈλ‘€λ¬μμ λ‘κ·ΈμΈ μ±κ³΅μ ν΄λΉ κ²½λ‘λ‘ μ΄λνλ κΈ°λ₯μ μΆκ°λ‘ κ°λ°ν΄μΌ νλ€.
* `return;`
  * μ¬κΈ°κ° μ€μνλ€. νν°λ₯Ό λλ μ§ννμ§ μλλ€. μ΄ν νν°λ λ¬Όλ‘  μλΈλ¦Ώ, μ»¨νΈλ‘€λ¬κ° λλ
  νΈμΆλμ§ μλλ€. **μμ redirect λ₯Ό μ¬μ©νκΈ° λλ¬Έμ redirect κ° μλ΅μΌλ‘ μ μ©λκ³  μμ²­μ΄ λλλ€.**

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

  @Bean
  public FilterRegistrationBean loginCheckFilter() {
    FilterRegistrationBean<Filter> filterFilterRegistrationBean = new FilterRegistrationBean<>();
    filterFilterRegistrationBean.setFilter(new LoginCheckFilter());
    filterFilterRegistrationBean.setOrder(2);
    filterFilterRegistrationBean.addUrlPatterns("/*");

    return filterFilterRegistrationBean;
  }
}
```
* `setFilter(new LoginCheckFilter())` : λ‘κ·ΈμΈ νν°λ₯Ό λ±λ‘νλ€.
* `setOrder(2)` : μμλ₯Ό 2λ²μΌλ‘ μ‘μλ€.
* `addUrlPatterns("/*")` : λͺ¨λ  μμ²­μ λ‘κ·ΈμΈ νν°λ₯Ό μ μ©νλ€. 

LoginController - loginV4()
```java
@PostMapping("/login")
public String loginV4(@Valid @ModelAttribute LoginForm loginForm, BindingResult bindingResult,
                      @RequestParam(defaultValue = "/") String redirectURL,
                      HttpServletRequest request) {

    if (bindingResult.hasErrors()) {
        return "login/loginForm";
    }

    Member loginMember = loginService.login(loginForm.getLoginId(), loginForm.getPassword());

    if (loginMember == null) {
        bindingResult.reject("loginFail", "μμ΄λ λλ λΉλ°λ²νΈκ° λ§μ§ μμ΅λλ€.");
        return "login/loginForm";
    }

    //λ‘κ·ΈμΈ μ±κ³΅ μ²λ¦¬ TODO
    //μΈμμ΄ μμΌλ©΄ μλ μΈμ λ°ν, μμΌλ©΄ μ κ· μΈμμ μμ±
    HttpSession session = request.getSession(true);
    //μΈμμ λ‘κ·ΈμΈ νμ μ λ³΄ λ³΄κ΄
    session.setAttribute(SessionConst.LOGIN_MEMBER, loginMember);

    return "redirect:"+redirectURL;
}
```
λ‘κ·ΈμΈ μ²΄ν¬ νν°μμ, λ―ΈμΈμ¦ μ¬μ©μλ μμ²­ κ²½λ‘λ₯Ό ν¬ν¨ν΄μ `/login` μ `redirectURL` μμ²­ νλΌλ―Έν°λ₯Ό
μΆκ°ν΄μ μμ²­νλ€. 

μ΄ κ°μ μ¬μ©ν΄μ λ‘κ·ΈμΈ μ±κ³΅μ ν΄λΉ κ²½λ‘λ‘ κ³ κ°μ `redirect` νλ€.

**μλΈλ¦Ώ νν°**λ₯Ό μ μ¬μ©ν λλΆμ λ‘κ·ΈμΈ νμ§ μμ μ¬μ©μλ λλ¨Έμ§ κ²½λ‘μ λ€μ΄κ° μ μκ² λμλ€. κ³΅ν΅
κ΄μ¬μ¬λ₯Ό μλΈλ¦Ώ νν°λ₯Ό μ¬μ©ν΄μ ν΄κ²°ν λλΆμ **ν₯ν λ‘κ·ΈμΈ κ΄λ ¨ μ μ±μ΄ λ³κ²½λμ΄λ μ΄ λΆλΆλ§ λ³κ²½νλ©΄
λλ€.**

### μ€νλ§ μΈν°μν°
`μ€νλ§ μΈν°μν°`λ μλΈλ¦Ώ νν°μ κ°μ΄ **μΉκ³Ό κ΄λ ¨λ κ³΅ν΅ κ΄μ¬μ¬ν­μ ν¨κ³Όμ μΌλ‘ ν΄κ²°**ν  μ μλ κΈ°μ 
* μλΈλ¦Ώμ΄ μ κ³΅
  * μλΈλ¦Ώ νν°
* μ€νλ§ MVC μ κ³΅
  * μ€νλ§ μΈν°μν°

**μ€νλ§ μΈν°μν° νλ¦**
```text
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μ€νλ§ μΈν°μν° -> μ»¨νΈλ‘€λ¬
```
* μ€νλ§ μΈν°μν°λ λμ€ν¨μ² μλΈλ¦Ώκ³Ό μ»¨νΈλ‘€λ¬ μ¬μ΄μμ μ»¨νΈλ‘€λ¬ νΈμΆ μ§μ μ νΈμΆ
* μ€νλ§ μΈν°μν°λ μ€νλ§ MVCκ° μ κ³΅νλ κΈ°λ₯μ΄κΈ° λλ¬Έμ κ²°κ΅­ λμ€ν¨μ² μλΈλ¦Ώ μ΄νμ λ±μ₯
  * μ€νλ§ MVCμ μμμ μ΄ λμ€ν¨μ² μλΈλ¦Ώμ΄λΌκ³  μκ°ν΄λ³΄λ©΄ μ΄ν΄κ° λ κ²μ΄λ€.!
* μ€νλ§ μΈν°μν°μλ URL ν¨ν΄μ μ μ©ν  μ μλλ°, μλΈλ¦Ώ URL ν¨ν΄κ³Όλ λ€λ₯΄κ³ , λ§€μ° μ λ°νκ² μ€μ  ν  μ μλ€.

**μ€νλ§ μΈν°μν° μ ν**

**μΈν°μν°μμ μ μ νμ§ μμ μμ²­μ΄λΌκ³  νλ¨νλ©΄ κ±°κΈ°μμ λ**μ λΌ μλ μλ€. κ·Έλμ λ‘κ·ΈμΈ μ¬λΆλ₯Ό
μ²΄ν¬νκΈ°μ λ± μ’λ€.
```text
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μ€νλ§ μΈν°μν° -> μ»¨νΈλ‘€λ¬ //λ‘κ·ΈμΈ μ¬μ©μ
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μ€νλ§ μΈν°μν°(μ μ νμ§ μμ μμ²­μ΄λΌ νλ¨, μ»¨νΈλ‘€λ¬ νΈμΆ X) // λΉ λ‘κ·ΈμΈ μ¬μ©μ
```

**μ€νλ§ μΈν°μν° μ²΄μΈ**

μ€νλ§ μΈν°μν°λ μ²΄μΈμΌλ‘ κ΅¬μ±λλλ°, μ€κ°μ μΈν°μν°λ₯Ό μμ λ‘­κ² μΆκ°ν  μ μλ€. 

μλ₯Ό λ€μ΄μ λ‘κ·Έλ₯Ό λ¨κΈ°λ μΈν°μν°λ₯Ό λ¨Όμ  μ μ©νκ³ , κ·Έ λ€μμ λ‘κ·ΈμΈ μ¬λΆλ₯Ό μ²΄ν¬νλ μΈν°μν°λ₯Ό λ§λ€ μ μλ€.
```text
HTTP μμ²­ -> WAS -> νν° -> μλΈλ¦Ώ -> μΈν°μν°1 -> μΈν°μν°2 -> μ»¨νΈλ‘€λ¬
```

> μ§κΈκΉμ§ λ΄μ©μ λ³΄λ©΄ μλΈλ¦Ώ νν°μ νΈμΆ λλ μμλ§ λ€λ₯΄κ³ , μ κ³΅νλ κΈ°λ₯μ λΉμ·ν΄ λ³΄μΈλ€. μμΌλ‘
μ€λͺνκ² μ§λ§, **μ€νλ§ μΈν°μν°λ μλΈλ¦Ώ νν°λ³΄λ€ νΈλ¦¬νκ³ , λ μ κ΅νκ³  λ€μν κΈ°λ₯μ μ§μ**νλ€.

**μ€νλ§ μΈν°μν° μΈν°νμ΄μ€**

μ€νλ§μ μΈν°μν°λ₯Ό μ¬μ©νλ €λ©΄ `HandlerInterceptor` μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ©΄ λλ€.
```java
public interface HandlerInterceptor {
    
	default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		return true;
	}
    
	default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable ModelAndView modelAndView) throws Exception {
	}
    
	default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable Exception ex) throws Exception {
	}

}
```
* μλΈλ¦Ώ νν°μ κ²½μ° λ¨μνκ² `doFilter()` νλλ§ μ κ³΅λλ€. 
* μΈν°μν°λ μ»¨νΈλ‘€λ¬ νΈμΆ μ ( `preHandle` ), νΈμΆ ν( `postHandle` ), μμ²­ μλ£ μ΄ν( `afterCompletion` )μ κ°μ΄ λ¨κ³μ μΌλ‘ μ μΈλΆν λμ΄ μλ€.
* μλΈλ¦Ώ νν°μ κ²½μ° λ¨μν `request` , `response` λ§ μ κ³΅νμ§λ§, μΈν°μν°λ μ΄λ€ μ»¨νΈλ‘€λ¬( `handler` )κ°
νΈμΆλλμ§ νΈμΆ μ λ³΄λ λ°μ μ μλ€. κ·Έλ¦¬κ³  μ΄λ€ `modelAndView` κ° λ°νλλμ§ μλ΅ μ λ³΄λ λ°μ μ
μλ€.

**μ€νλ§ μΈν°μν° νΈμΆ νλ¦**

![img_10.png](img_10.png)

**μ μ νλ¦**

* `preHandle` : μ»¨νΈλ‘€λ¬ νΈμΆ μ μ νΈμΆλλ€. (λ μ ννλ νΈλ€λ¬ μ΄λν° νΈμΆ μ μ νΈμΆλλ€.)
  * `preHandle` μ μλ΅κ°μ΄ `true` μ΄λ©΄ λ€μμΌλ‘ μ§ννκ³ , `false` μ΄λ©΄ λλ μ§ννμ§ μλλ€. 
  `false` μΈ κ²½μ° λλ¨Έμ§ μΈν°μν°λ λ¬Όλ‘ μ΄κ³ , νΈλ€λ¬ μ΄λν°λ νΈμΆλμ§ μλλ€. κ·Έλ¦Όμμ 1λ²μμ λμ΄
  λλ²λ¦°λ€.
* `postHandle` : μ»¨νΈλ‘€λ¬ νΈμΆ νμ νΈμΆλλ€. (λ μ ννλ νΈλ€λ¬ μ΄λν° νΈμΆ νμ νΈμΆλλ€.)
* `afterCompletion` : λ·°κ° λ λλ§ λ μ΄νμ νΈμΆλλ€.

**μ€νλ§ μΈν°μν° μμΈ μν©**

![img_11.png](img_11.png)

**μμΈκ° λ°μμ**

* `preHandle` : μ»¨νΈλ‘€λ¬ νΈμΆ μ μ νΈμΆλλ€.
* `postHandle` : μ»¨νΈλ‘€λ¬μμ μμΈκ° λ°μνλ©΄ `postHandle` μ νΈμΆλμ§ μλλ€.
* `afterCompletion` : `afterCompletion` μ ν­μ νΈμΆλλ€. μ΄ κ²½μ° μμΈ( `ex` )λ₯Ό νλΌλ―Έν°λ‘ λ°μμ μ΄λ€
μμΈκ° λ°μνλμ§ λ‘κ·Έλ‘ μΆλ ₯ν  μ μλ€.

**`afterCompletion` μ μμΈκ° λ°μν΄λ νΈμΆλλ€.**
* μμΈκ° λ°μνλ©΄ `postHandle()` λ νΈμΆλμ§ μμΌλ―λ‘ **μμΈμ λ¬΄κ΄νκ² κ³΅ν΅ μ²λ¦¬λ₯Ό νλ €λ©΄ `afterCompletion()` μ μ¬μ©**ν΄μΌ νλ€.
* μμΈκ° λ°μνλ©΄ `afterCompletion()` μ μμΈ μ λ³΄( `ex` )λ₯Ό ν¬ν¨ν΄μ νΈμΆλλ€.

> μΈν°μν°λ μ€νλ§ MVC κ΅¬μ‘°μ νΉνλ νν° κΈ°λ₯μ μ κ³΅νλ€.
> νΉλ³ν νν°λ₯Ό λ°λμ μ¬μ©ν΄μΌ νλ μν©μ΄ μλλΌλ©΄ μΈν°μν°λ₯Ό μ¬μ©νλ κ²μ΄ λ νΈλ¦¬νλ€.

### μ€νλ§ μΈν°μν° - μμ²­ λ‘κ·Έ
**LogInterceptor - μμ²­ λ‘κ·Έ μΈν°μν°**
```java
@Slf4j
public class LogInterceptor implements HandlerInterceptor {

    public static final String LOG_ID = "logId";

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        String requestURI = request.getRequestURI();
        String uuid = UUID.randomUUID().toString();

        request.setAttribute(LOG_ID, uuid);

        //@RequestMapping: HandlerMethod
        //μ μ  λ¦¬μμ€: ResourceHttpRequestHandler
        if(handler instanceof HandlerMethod) {
            HandlerMethod hm = (HandlerMethod) handler; //νΈμΆν  μ»¨νΈλ‘€λ¬ λ©μλμ λͺ¨λ  μ λ³΄κ° ν¬ν¨λμ΄ μλ€.
        }

        log.info("REQUEST [{}] [{}] [{}]",uuid, requestURI, handler);

        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandle=[{}]", modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        String requestURI = request.getRequestURI();
        Object uuid = request.getAttribute(LOG_ID);
        log.info("REQUEST [{}] [{}] [{}]",uuid, requestURI, handler);

        if(ex != null) {
            log.error("afterCompletion error!!", ex);
        }
    }
}
```
* `request.setAttribute(LOG_ID, uuid)`
  * μλΈλ¦Ώ νν°μ κ²½μ° μ§μ­λ³μλ‘ ν΄κ²°μ΄ κ°λ₯νμ§λ§, μ€νλ§ μΈν°μν°λ νΈμΆ μμ μ΄ μμ ν λΆλ¦¬λμ΄
  μλ€. λ°λΌμ `preHandle` μμ μ§μ ν κ°μ `postHandle` , `afterCompletion` μμ ν¨κ» μ¬μ©νλ €λ©΄
  μ΄λκ°μ λ΄μλμ΄μΌ νλ€. `LogInterceptor` λ μ±κΈν€ μ²λΌ μ¬μ©λκΈ° λλ¬Έμ λ§΄λ²λ³μλ₯Ό μ¬μ©νλ©΄
  μννλ€. λ°λΌμ request μ λ΄μλμλ€. μ΄ κ°μ `afterCompletion` μμ
  .request.getAttribute(LOG_ID)` λ‘ μ°Ύμμ μ¬μ©νλ€.
* `return true`
  * `true` λ©΄ μ μ νΈμΆμ΄λ€. λ€μ μΈν°μν°λ μ»¨νΈλ‘€λ¬κ° νΈμΆλλ€

**HandlerMethod**

**νΈλ€λ¬ μ λ³΄λ μ΄λ€ νΈλ€λ¬ λ§€νμ μ¬μ©νλκ°μ λ°λΌ λ¬λΌμ§λ€.** μ€νλ§μ μ¬μ©νλ©΄ μΌλ°μ μΌλ‘
`@Controller` , `@RequestMapping` μ νμ©ν νΈλ€λ¬ λ§€νμ μ¬μ©νλλ°, μ΄ κ²½μ° νΈλ€λ¬ μ λ³΄λ‘
`HandlerMethod` κ° λμ΄μ¨λ€.

**ResourceHttpRequestHandler**

`@Controller` κ° μλλΌ `/resources/static` μ κ°μ μ μ  λ¦¬μμ€κ° νΈμΆ λλ κ²½μ°
`ResourceHttpRequestHandler` κ° νΈλ€λ¬ μ λ³΄λ‘ λμ΄μ€κΈ° λλ¬Έμ νμμ λ°λΌμ μ²λ¦¬κ° νμνλ€.

**postHandle, afterCompletion**

μ’λ£ λ‘κ·Έλ₯Ό `postHandle` μ΄ μλλΌ `afterCompletion` μμ μ€νν μ΄μ λ, **μμΈκ° λ°μν κ²½μ°
`postHandle` κ° νΈμΆλμ§ μκΈ° λλ¬Έ**μ΄λ€. `afterCompletion` μ μμΈκ° λ°μν΄λ νΈμΆ λλ κ²μ λ³΄μ₯νλ€.

### μ€νλ§ μΈν°μν° - μΈμ¦ μ²΄ν¬
μλΈλ¦Ώ νν°μμ μ¬μ©νλ μΈμ¦ μ²΄ν¬ κΈ°λ₯μ μ€νλ§ μΈν°μν°λ‘ κ°λ°ν΄λ³΄μ.

**LoginCheckInterceptor**

μΈμ¦μ΄λΌλ κ²μ μ»¨νΈλ‘€λ¬ νΈμΆ μ μλ§ νΈμΆλλ©΄ λλ€. 
λ°λΌμ `preHandle` λ§ κ΅¬ννλ©΄ λλ€.
```java
@Slf4j
public class LoginCheckInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
  
      String requestURI = request.getRequestURI();
  
      log.info("μΈμ¦ μ²΄ν¬ μΈν°μν° μ€ν {}", requestURI);
  
      HttpSession session = request.getSession();
  
      if (session == null || session.getAttribute(SessionConst.LOGIN_MEMBER) == null) {
        log.info("λ―ΈμΈμ¦ μ¬μ©μ μμ²­");
  
        //λ‘κ·ΈμΈμΌλ‘ redirect
        response.sendRedirect("/login?redirectURL=" + requestURI);
        return false;
      }
      return true;
    }
}
```

**WebConfig - μΈν°μν° λ±λ‘**
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LogInterceptor())
                .order(1)
                .addPathPatterns("/**")
                .excludePathPatterns("/css/**", "/*.ico", "/error");
    
        registry.addInterceptor(new LoginCheckInterceptor())
                .order(2)
                .addPathPatterns("/**")
                .excludePathPatterns("/", "/members/add", "/login", "/logout",
                        "/css/**", "/*.ico", "/error");
    }
}
```
* `WebMvcConfigurer` κ° μ κ³΅νλ `addInterceptors()` λ₯Ό μ¬μ©ν΄μ μΈν°μν°λ₯Ό λ±λ‘ν  μ μλ€.
* `registry.addInterceptor(new LogInterceptor())` : μΈν°μν°λ₯Ό λ±λ‘νλ€.
* `order(μ«μ)` : μΈν°μν°μ νΈμΆ μμλ₯Ό μ§μ νλ€. λ?μ μλ‘ λ¨Όμ  νΈμΆλλ€.
* `addPathPatterns("/**")` : μΈν°μν°λ₯Ό μ μ©ν  URL ν¨ν΄μ μ§μ νλ€.
* `excludePathPatterns("/css/**", "/*.ico", "/error")` : μΈν°μν°μμ μ μΈν  ν¨ν΄μ μ§μ νλ€.

**PathPattern κ³΅μ λ¬Έμ**

* https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/pattern/PathPattern.html
```text
? ν λ¬Έμ μΌμΉ
* κ²½λ‘(/) μμμ 0κ° μ΄μμ λ¬Έμ μΌμΉ
** κ²½λ‘ λκΉμ§ 0κ° μ΄μμ κ²½λ‘(/) μΌμΉ
{spring} κ²½λ‘(/)μ μΌμΉνκ³  springμ΄λΌλ λ³μλ‘ μΊ‘μ²
{spring:[a-z]+} matches the regexp [a-z]+ as a path variable named "spring"
{spring:[a-z]+} regexp [a-z]+ μ μΌμΉνκ³ , "spring" κ²½λ‘ λ³μλ‘ μΊ‘μ²
{*spring} κ²½λ‘κ° λλ  λ κΉμ§ 0κ° μ΄μμ κ²½λ‘(/)μ μΌμΉνκ³  springμ΄λΌλ λ³μλ‘ μΊ‘μ²

/pages/t?st.html β matches /pages/test.html, /pages/tXst.html but not /pages/toast.html
/resources/*.png β matches all .png files in the resources directory
/resources/** β matches all files underneath the /resources/ path, including /
resources/image.png and /resources/css/spring.css
/resources/{*path} β matches all files underneath the /resources/ path and
captures their relative path in a variable named "path"; /resources/image.png
will match with "path" β "/image.png", and /resources/css/spring.css will match
with "path" β "/css/spring.css"
/resources/{filename:\\w+}.dat will match /resources/spring.dat and assign the
value "spring" to the filename variable
```

> `μ€νλ§ μΈν°μν°`κ° κ°λ°μ μμ₯μμ ν¨μ¬ νΈλ¦¬νλ€λ κ²μ μ½λλ‘ μ΄ν΄νμ κ²μ΄λ€. 
> νΉλ³ν λ¬Έμ κ° μλ€λ©΄ μΈν°μν°λ₯Ό μ¬μ©νλ κ²μ΄ μ’λ€.


### ArgumentResolver νμ©
<a href="https://github.com/ssosee/springmvc#%EC%9A%94%EC%B2%AD-%EB%A7%A4%ED%95%91-%ED%95%B8%EB%93%A4%EB%9F%AC-%EC%96%B4%EB%8C%91%ED%84%B0-%EA%B5%AC%EC%A1%B0">μμ²­ λ§€μΉ­ νΈλ€λ¬ μ΄λν° κ΅¬μ‘°</a>

**@Login μ λΈνμ΄μ μμ±**
```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface Login {
}
```
* `@Target(ElementType.PARAMETER)` : νλΌλ―Έν°μλ§ μ¬μ©
* `@Retention(RetentionPolicy.RUNTIME)` : λ¦¬νλ μ λ±μ νμ©ν  μ μλλ‘ λ°νμκΉμ§ μ λΈνμ΄μ μ λ³΄κ° λ¨μμμ

**HomeController - μΆκ°**
```java
@GetMapping("/")
public String homLoginV3ArgumentResolver(@Login Member loginMember, Model model) {

    //μΈμμ νμ λ°μ΄ν°κ° μμΌλ©΄ home
    if(loginMember == null) {
        return "home";
    }

    model.addAttribute("member", loginMember);
    return "loginHome";
}
```

**LoginMemberArgumentResolver μμ±**
```java
@Slf4j
public class LoginMemberArgumentResolver implements HandlerMethodArgumentResolver {

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        log.info("supportParameter μ€ν");

        boolean hasLoginAnnotation = parameter.hasParameterAnnotation(Login.class);
        boolean hasMemberType = Member.class.isAssignableFrom(parameter.getParameterType());

        return hasLoginAnnotation && hasMemberType;
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        log.info("resolveArgument μ€ν");

        HttpServletRequest request = (HttpServletRequest) webRequest.getNativeRequest();
        HttpSession session = request.getSession(false);

        if (session == null) {
            return null;
        }

        return session.getAttribute(SessionConst.LOGIN_MEMBER);
    }
}
```
* `supportsParameter()` : `@Login` μ λΈνμ΄μμ΄ μμΌλ©΄μ `Member` νμμ΄λ©΄ ν΄λΉ `ArgumentResolver` κ° μ¬μ©λλ€.
* `resolveArgument()` : μ»¨νΈλ‘€λ¬ νΈμΆ μ§μ μ νΈμΆ λμ΄μ νμν νλΌλ―Έν° μ λ³΄λ₯Ό μμ±ν΄μ€λ€. 
μ¬κΈ°μλ μΈμμ μλ λ‘κ·ΈμΈ νμ μ λ³΄μΈ `member` κ°μ²΄λ₯Ό μ°Ύμμ λ°νν΄μ€λ€. 
μ΄ν μ€νλ§MVCλ μ»¨νΈλ‘€λ¬μ λ©μλλ₯Ό νΈμΆνλ©΄μ μ¬κΈ°μμ λ°νλ `member` κ°μ²΄λ₯Ό νλΌλ―Έν°μ μ λ¬ν΄μ€λ€.

**WebMvcConfigurerμ μ€μ  μΆκ°**
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
   @Override
   public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
      resolvers.add(new LoginMemberArgumentResolver());
   }
 //...
}
```

μ€νν΄λ³΄λ©΄, κ²°κ³Όλ λμΌνμ§λ§, λ νΈλ¦¬νκ² λ‘κ·ΈμΈ νμ μ λ³΄λ₯Ό μ‘°νν  μ μλ€. μ΄λ κ²
`ArgumentResolver` λ₯Ό νμ©νλ©΄ κ³΅ν΅ μμμ΄ νμν  λ μ»¨νΈλ‘€λ¬λ₯Ό λμ± νΈλ¦¬νκ² μ¬μ©ν  μ μλ€.
