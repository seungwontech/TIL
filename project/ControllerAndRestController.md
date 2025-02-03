# @Controller와 RestController
Spring MVC에서 컨트롤러 클래스를 정의할 때 사용하는 어노테이션입니다.

## Controller
웹 페이지 (View)를 반환하는 컨트롤러를 만들 때 사용합니다.   
주로 `Thymeleaf`, `jsp` 와 같은 템플릿 엔젠과 함께 서버 사이드 렌더링(SSR) 할 때 활용합니다.
- 메서드의 반환값은 뷰 이름이 되고, 해당 뷰를 렌더링
- 데이터를 템플릿에 전달하려면 `Model` 객체를 사용
- @ResponseBody 없이도 HTML 뷰를 반환 가능

## Controller 예제
```java
@Controller
public class HomeController {
    
    @GetMapping("/home")
    public String home(Model model) {
        model.addAttribute("message", "Hello, Spring!");
        return "home";
    }
}
```
## 동작 방식
- /home 경로로 요청이 들어오면 home() 메서드 실행
- "home"이라는 뷰 이름을 반환 → templates/home.html을 찾아서 렌더링
- Model 객체를 사용해 템플릿 엔진에 데이터 전달 가능

## @Controller를 사용하는 경우
- `Html`을 반환하는 웹 애플리케이션을 만들 때 사용
- 템플릿 엔진(`Thymeleaf`, `jsp`)과 함께 서버 사이드 렌더링(SSR)을 할 때 사용

## RestController
@RestController는 REST API를 만들 때 사용하는 컨트롤러입니다.  
모든 메서드에 `@ResponseBody`가 자동 포함 되어 JSON이나 XML 등의 데이터를 반환합니다.
- `@ResponseBody`가 기본적으로 적용됨 → 뷰를 렌더링 하지 않고 데이터를 직접 반환
- 주로 JSON 형식의 데이터를 반환하는 API 개발에 사용
- ResponseEntity<T> 를 활용해 HTTP 상태 코드와 함께 데이터를 반환할 수 있음
```java
@RequiredArgsConstructor
@RestController
@RequestMapping("/api/balances")
public class BalanceController {
    private final BalanceService balanceService;
    @PutMapping("/balance/charge")
    public ResponseEntity<BalanceRes> charge(@RequestHeader("user-id") Long userId, @RequestBody int chargeAmount) {
        Balance res = balanceService.charge(userId, chargeAmount);
        return ResponseEntity.ok(res);
    }
}
```
## 동작 방식
- 클라이언트가 PUT /api/balances/charge 요청을 보냄
- @RequestBody로 JSON 데이터를 받아 서비스 계층에 전달
- 처리 후 ResponseEntity.ok(response)로 응답 데이터를 JSON으로 반환

## @RestController를 사용하는 경우
- React, Vue.js, 모바일 앱과 연동할 REST API 개발 시
- JSON 데이터만 반환하는 API를 만들 때
- 뷰(View) 없이 데이터를 직접 응답해야 하는 경우

## 결론
- 웹 애플리케이션의 UI를 서버에서 렌더링할 때 (Thymeleaf, JSP) → @Controller
- React/Vue/모바일 앱과 REST API로 통신할 때 → @RestController