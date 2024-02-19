---
title: Strategy Pattern
author: hyunsh
date: 2024-02-03 13:23:00 +0800
categories: [Spring, StrategyPattern]
pin: true
---

## Strategy Pattern


**정의**
- 알고리즘을 정의하고 각각을 캡슐화하여 교체가능한 컨텍스트를 만든다.
- 클라이언트와 독립적으로 알고리즘을 변경할 수 있다. 


---


**사용 예시#1** (Context 필드에 Strategy 저장)
```java
//Strategy
public interface CorpStrategy {
	void saveSkey();
}

//A전략
public class AStrategy implements CorpStrategy {

	@Override
	public void saveSkey() {
		//...
		System.out.println("A전략");
	}
}

//B전략
public class BStrategy implements CorpStrategy {

	@Override
	public void saveSkey() {
		//...
		System.out.println("B전략");
	}
}

//Context
public class CorpContextV1 {

	private CorpStrategy corpStrategy;
	
	public CorpContextV1(CorpStrategy corpStrategy) {
		this.corpStrategy = corpStrategy;		
	}
	
	public void execute() {
		System.out.println("공통 로직 실행");
		corpStrategy.saveSkey();
	}
}

//Client
public class StrategyPatternTest {

    @Test
    void corpStrategyTestV1(){
        CorpStrategy strategy = new AStrategy();
        CorpContextV1 context = new CorpContextV1(strategy);
        context.execute();

//      ##### output #####
//      공통 로직 실행
//      A전략
    }
}
``` 
  
<br/>

**정리#1**
- `CorpContextV1` 은 필드에 `CorpStrategy` 를 저장하는 방식으로 전략 패턴을 구사했다. 
- 선 조립, 후 실행 방법.
- Context 실행 시점에는 이미 조립이 끝났기 때문에 전략을 신경쓰지 않고 단순히 실행만 하면 된다.


---

**사용 예시#2** (Context 파라미터에 Strategy 전달)
```java
//Strategy
public interface CorpStrategy {
	void saveSkey();
}

//Context
public class CorpContextV2 {

	public void execute(CorpStrategy corpStrategy) {
		System.out.println("공통 로직 실행");
		corpStrategy.saveSkey();
	}
}

//Client
public class StrategyPatternTest {

    @Test
    void corpStrategyTestV2(){
        CorpContextV2 context = new CorpContextV2();
        context.execute(() -> System.out.prinln("A전략"));

//      ##### output #####
//      공통 로직 실행
//      A전략
    }
}
``` 
  
<br/>

**정리#2**
- `CorpContextV2` 은 파라미터에 `CorpStrategy` 를 전달하는 방식으로 전략 패턴을 구사했다. 
- 실행할 때 마다 전략을 변경할 수 있다. (실행할 때 마다 전략을 지정해줘야 하는 단점이 있다.)

---


## Template Collback Pattern    
 
**정의**
- 스프링에서는 `ContextV2` 와 같은 방식의 전략 패턴을 템플릿 콜백 패턴이라 한다. 전략 패턴에서 `Context`가 ***템플릿*** 역할을 하고, `Strategy` 부분이 ***콜백***으로 넘어온다 생각하면 된다.
- 템플릿 콜백 패턴은 GOF 패턴은 아니고, 스프링 내부에서 이런 방식을 자주 사용하기 때문에, 스프링 안에서만 이렇게 부른다.
> 콜백 정의   
> 다른 코드의 인수로서 넘겨주는 실행 가능한 코드.


**사용 예시**
```java
//callback
public interface Callback {
	void saveSkey();
}

//template
public class TestTemplate {

	public void execute(Callback callback) {
		System.out.println("공통 로직 실행");
		callback.saveSkey();
	}
}

//Client
public class TemplateCallbackPatternTest {

    @Test
    void templateCallbackTest(){
        TestTemplate template = new TestTemplate();
        template.execute(() -> System.out.prinln("Template Collback"));

//      ##### output #####
//      공통 로직 실행
//      Template Collback
    }
}
``` 
  
<br/>

**정리**
- 변하는 코드와 변하지 않는 코드를 분리하여 코드 사용 최소화.
- 하지만, 템플릿을 적용하기 위해서 결국 원본 코드를 수정해야 한다.    
  이를 해결하기 위해서는 [프록시]()에 대한 개념이 필요하다.

