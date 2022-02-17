# Java - Iterator, Iterable 인터페이스 - For Each Loop

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

자바 For Each Loop 를 사용하기 위해서 구현해야 할 인터페이스로 Iterator, Iterable 가 있다.

Iterable 에는 Iterator 를 리턴하는 iterator 메소드를 구현해야 한다.

만약 클래스가 Iterator 인터페이스를 구현하고 있다면 this 를 리턴해도 되고

아니면 iterator 인터페이스를 구현한 다른 클래스 객체를 리턴할 수도 있다.

간단하게 생성자로 입력받은 숫자까지만 반복하는 클래스를 만들자.
```
public class Test implements Iterable<Integer> , Iterator<Integer> {
 private int idx=0, max=0;
 public Test(int max) { this.max = max; }
 @Override public Iterator<Integer> iterator() { return this; }
 @Override public boolean hasNext() { return max>idx; }
 @Override public Integer next() { return cnt++; }
}
```


크게 두 가지 방법으로 많이 사용된다.   
방법1. for (int cnt : new Test(10)) { /* cnt */ }   
방법2. while( t.hasNext( )) { /* t.next( ) */ }