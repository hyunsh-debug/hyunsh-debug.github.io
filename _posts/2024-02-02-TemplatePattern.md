## Template Method Pattern


**정의**
- 템플릿 메서드 패턴은 상위 클래스에서 알고리즘의 골격을 정의하고, 일부 단계를 하위 클래스로 위임합니다.  
- 하위 클래스에서는 특정 단계를 재정의하여 알고리즘의 동작을 개별적으로 조정할 수 있습니다.  
- 알고리즘의 구조를 변경하지 않으면서도 일부 단계를 다시 정의함으로써 유연성을 제공합니다.   


---


**사용 예시**
```java
//부모 클래스
public abstract class AbstractTemplate<T> {

    private final LogTrace trace;

    public AbstractTemplate(LogTrace trace) {
        this.trace = trace;
    }

    public T execute(String message) {
        TraceStatus status = null;

        try {
            status = trace.begin(message);

            T result = call();

            trace.end(status);

            return result;
        } catch (Exception e) {
            trace.exception(status, e);
            throw e;
        }
    }

    protected abstract T call();
}

//자식 클래스
public class SubClassLogic extends AbstractTemplate {

    public Test(LogTrace trace) {
        super(trace);
    }	

    @Override
    protected Object call() {
        System.out.println("비지니스 로직 실행");
		return null;
    }
}

// 클라이언트 코드(1)-자식 클래스 사용
public class Client {
    public static void main(String[] args) {
        LogTrace trace = new LogTrace();
        AbstractTemplate template = new SubClassLogic(trace);
        template.execute("Template");
    }
}

// 클라이언트 코드(2)-익명 내부 클래스 사용
public class Client {
    public static void main(String[] args) {
        AbstractTemplate template = new AbstractTemplate<>(trace) {
            @Override
            protected Object call() {
                System.out.println("비지니스 로직 실행");
                return null;
            }
        };

        template.execute("Template");
    }
}
```

---


**정리**
- 템플릿 메서트 패턴은 상속을 사용하기 때문에 상속에서 오는 단점들을 그대로 안고간다.
- 컴파일 시점에 자식 클래스가 부모 클래스와 강하게 결합되는 문제가 있다.   
  (부모 클래스를 수정하면, 자식 클래스에도 영향을 줄 수 있다)
- 상속 구조를 사용하기 때문에, 별도의 클래스나 익명 내부 클래스를 만들어야 하는 부분도 복잡하다.
- ***상속의 단점을 개선하기 위해 `Strategy Pattern` 사용한다.***



