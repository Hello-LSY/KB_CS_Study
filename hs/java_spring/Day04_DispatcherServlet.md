## DispatcherServlet 의 역할에 대해 설명해 주세요.

__개념 및 역할__<br/>
DispatcherServlet은 웹 애플리케이션의 요청을 받아 처리하는 프론트 컨트롤러 역할을 합니다. DispatcherServlet은 요청을 받아서 적절한 핸들러로 배분하고, 핸들러가 처리한 결과를 적절한 뷰로 렌더링하여 응답을 생성합니다. 

__처리 과정__<br/>
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbcff5H%2FbtstbdRuSr9%2FpNKnGdMwftSWmiGLHA7yL0%2Fimg.png)

- 클라이언트의 요청을 디스패처 서블릿이 받음 
- 요청 url을 기반으로 적절한 핸들러를 찾기 위해 HadlerMapping 조회
- (인터셉터의 preHandle 호출)
- 찾은 핸들러를 실행하기 위해 HandlerAdapter를 호출하여 핸들러(컨트롤러) 메서드 실행 후 
  - (뷰를 직접 반환하는 경우) ModelAndView 객체 반환
  - (Rest API인 경우) ResponseEntity나 도메인 객체 반환(Rest API인 경우)
- (인터셉터의 postHandle 호출)
- 응답 변환
  - (뷰를 직접 반환하는 경우) ModelAndView 객체에서 뷰 이름을 추출하고 ViewResolver를 사용하여 실제 뷰 객체를 찾음
  - (RestAPI인 경우) 핸들러 메서드가 반환한 객체(ResponseEntity)를 HTTP 응답 본문으로 변환하기 위해 HttpMessageConverter를 사용
- 응답 반환
  - (뷰를 직접 반환하는 경우) 뷰 객체를 사용하여 모델 데이터를 바탕으로 응답 생성, 클라이언트에 반환
  - (RestAPI인 경우) 변환된 응답 데이터는 HTTP 응답 본문에 작성되어 클라이언트에게 반환 
- (인터셉터의 afterCompletion 호출)

### 여러 요청이 들어온다고 가정할 때, DispatcherServlet은 한번에 여러 요청을 모두 받을 수 있나요?

가능합니다. 서블릿 컨테이너는 각 HTTP 요청을 처리하기 위해 새로운 스레드를 생성하거나, 스레드 풀에서 사용 가능한 스레드를 할당합니다. 각 요청을 독립된 스레드에서 DispatcherServlet을 통해 처리됩니다. 
dispatcherServlet은 단일 인스턴스로 생성되며, 할당된 스레드는 DispatcherServlet 인스턴스의 service 메서드를 호출하여 요청을 처리합니다. 각 요청에 대한 상태는 ThreadLocal 또는 메서드 로컬 변수를 통해 관리되며, 다른 요청과 격리됩니다.

### 수많은 @Controller 를 DispatcherServlet은 어떻게 구분 할까요?
HandlerMapping을 조회하여 @RequestMapping 또는 특정 HTTP 메서드 매핑 어노테이션이 붙은 메서드 즉, 적절한 핸들러(컨트롤러)를 찾습니다.

이후 HandlerExecutionChain 객체로 반환합니다. 이 객체에는 선택된 핸들러와 함께 실행할 인터셉터 목록을 포함합니다.

이후 인터셉터의 preHandle을 호출하고 HandlerAdapter에서 실제 핸들러를 호출합니다.

