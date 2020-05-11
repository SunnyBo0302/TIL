https://offbyone.tistory.com/33
https://offbyone.tistory.com/34

Interceptor: Dispatcher 서블릿이 컨트롤러를 호출할 때 전, 후에 실행
instance of 로 구분해야하는 것 같음.
```
  @Override 
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception 
  { 
    logger.info("preHandle call......"); 
    if (handler instanceof HandlerMethod) 
    { // HandlerMethod 는 후에 실행할 컨트롤러의 메소드 입니다. 
      // 메소드명, 인스턴스, 타입, 사용된 Annotation 들을 확인할 수 있습니다. 
      HandlerMethod method = (HandlerMethod) handler; 
      logger.info("handler method name : " + method.getMethod().getName()); 
    } 
    return true; 
  }

```

AOP: Controller의 method 동작 절차중에 삽입 가능

Annotation으로 동작하는 controller, 함수를 지정할 수 있음

```
// AOP 임을 알리는 annotation 입니다. 
@Aspect 
public class TestAdvice {

  private static final Logger logger = LoggerFactory.getLogger(TestAdvice.class);

  // 공통으로 사용될 포인트컷을 지정합니다. 
  // com.tisotry.pentode 패키지 안의 Controller 로 끝나는 클래스의 모든 메소드에 적용됩니다. 
  @Pointcut("execution(* com.tistory.pentode.*Controller.*(..))")
  public void commonPointcut() {

  }

  // Before Advice 입니다. 위에서 정의한 공통 포인터 컷을 사용합니다. 
  @Before("commonPointcut()")
  public void beforeMethod(JoinPoint jp) throws Exception {
    logger.info("beforeMethod() called.....");
    // 호출될 메소드의 인자들으 얻을 수 있습니다. 
    Object arg[] = jp.getArgs();
    // 인자의 갯수 출력 
    logger.info("args length : " + arg.length);
    // 첫 번재 인자의 클래스 명 출력 
    logger.info("arg0 name : " + arg[0].getClass().getName());
    // 호출될 메소드 명 출력 
    logger.info(jp.getSignature().getName());
  }
  ...
}
```


가독성은 AOP가 좀 더 좋아보임. instance of 로 구분하면 읽기가 힘들지 않을듯