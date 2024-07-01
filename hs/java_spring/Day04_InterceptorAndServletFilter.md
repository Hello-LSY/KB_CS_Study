## Spring 에서 Interceptor와 Servlet Filter에 대해 설명해 주세요.

__개념__<br/>

Filter는 J2EE 표준 스펙 기능으로 Dispatcher Servlet에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능을 제공합니다. 즉, 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리가 되는 것이므로 스프링 범위 밖에서 처리됩니다. 따라서 웹 애플리케이션 전체에 공통된 처리가 필요할 때에 주로 사용합니다. Filter 는 Interceptor와 달리 Request와 Response를 조작할 수 있습니다.

Interceptor는 Dispatcher Servlet이 Controller를 호출하기 전/후에 인터셉터가 가로채는 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공합니다. 필터와 달리 Spring 내부 컨텍스트에서 처리됩니다. 따라서 필터와 달리 요청 처리의 세부 단계별로 처리 로직을 적용해야 할 때 주로 사용합니다. 인터셉터는 필터와 다르게 HttpServletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없습니다. 그러나 해당 객체가 내부적으로 갖는 값은 조작할 수 있으므로, 컨트롤러로 넘겨주기 위한 정보를 가공하기에 용이합니다. 

__Filter 사용법__<br/>
Filter를 사용하기 위해서는 javax.servlet의 Filter 인터페이스를 implements 해야 합니다.

```java
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}
```

- init()<br/>
  필터 객체를 초기화하고 서비스에 추가하기 위한 메소드입니다. 
- doFilter()<br/>
  url 패턴에 맞는 HTTP 요청이 Dispatcher Servlet으로 전달되기 전에 실행되는 메소드입니다. 여러 필터를 chaining 해서 순차적으로 처리할 수 있습니다. 
- destroy()<br/>
  필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드입니다. 

__Interceptor 사용법__<br/>
org.springframework.web.servlet의 HandlerInterceptor 인터페이스를 implements 해서 사용한다.

```java
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println("Pre Handle method is Calling");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           ModelAndView modelAndView) throws Exception {
        System.out.println("Post Handle method is Calling");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        System.out.println("Request and Response is completed");
    }
}

```

- preHandle()<br/>
  컨트롤러가 호출되기 전에 실행됩니다. preHandle의 반환 값이 true이면 다음 단계로 진행되지만, false라면 작업을 중단하여 다음으로 넘어가지 않습니다.
- postHandle()<br/>
  컨트롤러를 호출된 후에 실행됩니다. 컨트롤러 하위 계층에서 작업을 진행하다가 중간에 예외가 발생하면 postHandle()은 호출되지 않습니다.
- afterCompletion()<br/>
  모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행됩니다. 컨트롤러 하위 계층에서 예외가 발생해도 afterCompletion()은 반드시 호출됩니다. 

> 톰캣(Servlet Container)은 자바 웹 애플리케이션에서 서블릿과 JSP를 실행할 수 있도록 지원합니다. 주요 기능은 다음과 같습니다. 
> 1. 서블릿 생명주기 관리
> 2. HTTP 요청 및 응답 처리
> 3. 스레드 관리

> 스프링부트는 기본적으로 내장 톰캣을 사용하여 애플리케이션을 실행합니다. 톰캣이 제공하는 내장 서블릿 컨테이너를 사용하여 별도의 외부 서버를 설정하거나 배포하지 않고도 스프링 웹 애플리케이션을 실행할 수 있게 해줍니다. 


### 설명만 들어보면 인터셉터만 쓰는게 나아보이는데, 아닌가요? 필터는 어떤 상황에 사용 해야 하나요?
스프링과 무관하게 전역적으로 처리해야 하는 보안 공통 작업, 이미지나 데이터의 압축과 문자열 인코딩 등 웹 애플리케이션에 전반적으로 사용되는 기능을 구현할 때 사용할 수 있습니다. 대표적으로 SpringSecurity가 필터로 구현되어 있습니다. 

### 참고
[[Spring] 필터(Filter) vs 인터셉터(Interceptor) 차이 및 용도 - (1)
출처: https://mangkyu.tistory.com/173 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/173)
