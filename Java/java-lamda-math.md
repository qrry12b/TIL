자바 - 람다를 이용한 연산 (Lambda)

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

```
package common;
@FunctionalInterface
interface Num{
	int exe(int x,int y);
}
public class Test {
	public static void main(String[] args) {
		Test t = new Test();
		int add  = t.exe((x,y)->{return x+y;});
		int sub = t.exe((x,y)->{return x-y;});
		int mul = t.exe((x,y)->{return x*y;});
		int div = t.exe((x,y)->{return x/y;});
		StringBuffer sf = new StringBuffer();
		sf.append("add : "+add+"\n");
		sf.append("sub : "+sub+"\n");
		sf.append("mul : "+mul+"\n");
		sf.append("div : "+div+"\n");
		System.out.println(sf);
	}
	
	public int exe(Num num) {
		return num.exe(10,2);
	}
}
```

```
package common;
@FunctionalInterface
interface Num{
	int exe(int x,int y);
}
public class Test {
	public static void main(String[] args) {
		Num add = (x,y) -> x+y;
		Num sub = (x,y) -> x-y;
		Num mul = (x,y) -> x*y;
		Num div = (x,y) -> x-y;
		Num max = (x,y) -> x>y?x:y;
		Num min = (x,y) -> x>y?y:x;
		System.out.println("add:"+add.exe(10,2));
		System.out.println("sub:"+sub.exe(10,2));
		System.out.println("mul:"+mul.exe(10,2));
		System.out.println("div:"+div.exe(10,2));
		System.out.println("max:"+max.exe(10,2));
		System.out.println("min:"+min.exe(10,2));
	}
}
```

람다식이란 "식별자없이 실행가능한 함수"
함수적 프로그래밍을 위해 자바 8부터 람다식 (Lambda Expressions)을 지원
기본형태 (매개변수, ...) -> { 실행문 }

기본적으로 람다식을 위한 인터페이스에서 추상 메소드는 단 하나여야 함.

그래서 이 인터페이스가 람다식을 위한 것이다라는 표현을 위해 어노테이션
@FunctionalInterface를 사용합니다. 이 어노테이션을 사용할 경우 해당 인터페이스에 메소드를 두 개 이상 선언하면 유효하지 않다는 오류가 발생.

즉, 컴파일러 수준에서 오류를 확인할 수 있음.
