# demo-aop
Demo of using Spring AOP

Sample:

```java
package demo.demoaop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

import lombok.extern.slf4j.Slf4j;

@Aspect
@Component
@Slf4j
public class AOPComponent {
	
	// apply to the methods with LogExecutionTime Annotation
	@Around("@annotation(LogExecutionTime)")
	public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
	    long start = System.currentTimeMillis();
	    
	    Object proceed = joinPoint.proceed();
	    
	    long executionTime = System.currentTimeMillis() - start;
	 
	    log.info(joinPoint.getSignature() + " executed in " + executionTime + "ms");
	    
	    return proceed;
	}
	
	// execute before the target method
	@Before("execution(* demo.demoaop.FirstController.*(..))")
	public void before(JoinPoint joinPoint) {
		log.info("AOP Log before....");
	}
	
	// execute after the target method
	@After("execution(* demo.demoaop.FirstController.*(..))")
	public void after(JoinPoint joinPoint) {
		log.info("AOP Log after....");
	}
	
	// executed only when a method successfully and get the result
	@AfterReturning(value = "execution(* demo.demoaop.FirstController.*(..))", 
			returning = "result")
	public void afterReturning(JoinPoint joinPoint, Object result) {
		log.info("{} returned with value {}", joinPoint, result);
	}
	
	// define point cut for better maintenance
	@Pointcut("execution(* demo.demoaop.FirstController.*(..))")
	public void firstController() {}
	
	// apply pointcut
	@Before("firstController()")
	public void beforeUsingPointCut(JoinPoint joinPoint) {
		log.info("AOP Log before by using point cut....");
	}	
	
}

```
