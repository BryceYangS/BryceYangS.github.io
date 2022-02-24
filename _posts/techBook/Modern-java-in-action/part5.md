---
layout: post
title: "[Book] 모던 자바 인 액션 - 파트5. 개선된 자바 동시성"
subtitle: "모던 자바 인 액션 챕터 5"
categories: various
tags: techBook
---
> 개선된 자바 동시성

## 챕터 15. CompetableFuture & 리액티브 프로그래밍 컨셉의 기초

### 동기 API와 비동기 API

```java
int f(int x){
  return x;
}

int g(int x){
  return x;
}

System.out.println(f(x) + g(x));
```



f와 g를 실행하는데 오랜 시간이 걸릴 경우, 별도의 스레드로 실행해 작업 시간을 단축할 수 있음

```java
class ThreadExample {
  public static void main(String[] args) throws InterruptedException {
    int x = 1557;
    Result result = new Result();
    
    Thread t1 = new Thread(() -> {result.left = f(x);});
    Thread t2 = new Thread(() -> {result.right = g(x);});
    t1.start();
    t2.start();
    t1.join();
    t2.join();
    System.out.println(result.left + result.right); // 3114
  }
  
  private static class Result {
    private int left;
    private int right;
  }
}
```



Runnable 대신 Future API 사용해 단순화 가능.

- submit 메서드와 같은 호출로 코드가 오염됨 → **비동기 API**를 통해 해결

```java
public class ExecutorServiceExample {
  public static void main(String[] args) throws ExecutionException, InterruptedException {
    int x = 1557;
    
    ExecutorService es = Executors.newFixedThreadPool(2);
    Future<Integer> y = es.submit(() -> f(x));
    Future<Integer> z = es.submit(() -> g(x));
    System.out.println(y.get() + z.get()); // 3114
    es.shutdown();
  }
}
```



### Future 형식 API

```java
Future<Integer> f(int x);
Future<Integer> g(int x);
System.out.println(f(x).get() + g(x).get());
```



### 리액티브 형식 API

f, g의 시그니처를 바꿔 `콜백 형식`의 프로그래밍 이용

- 호출 합계 정확하게 출력 X, 상황에 따라 먼저 계산된 결과를 출력
- 락을 사용하지 않아 두 번 출력할 수 있을 뿐더러 때로는 +에 제공된 피연산자가 println이 호출되기 전에 업데이트될 수 있음

해결방법

- If-then-else를 이용해 적절할 락을 이용. 두 콜백이 모두 호출되었는지 확인한 다음 println을 호출
- 리액티브 형식의 API는 보통 한 결과가 아니라 일련의 이벤트에 반응하도록 설계되었으므로 Future를 이용하는 것이 더 적절

```java
public class CallbackStyleExample {

    public static void main(String[] args) {
        int x = 1337;
        Result result = new Result();

        f(x, (int y) -> {
            result.left = y;
            System.out.println(result.left + result.right);
        });

        g(x, (int y) -> {
            result.right = y;
            System.out.println(result.left + result.right);
        });
    }

    static void f(int x, IntConsumer dealWithResult) {
        dealWithResult.accept(x);
    }

    static void g(int x, IntConsumer dealWithResult) {
        dealWithResult.accept(x);
    }
}
```



### 비동기 API에서의 예외 처리

Future를 구현한 `CompletableFuture`에서는 get() 메서드에 **예외를 처리할 수 있는 기능 제공**, 예외에서 회복할 수 있도록 **exceptionally()** 같은 메서드 제공

리액티브 형식의 비동기 API : return 대신 기존 콜백이 호출되므로 예외가 발생했을 때 실행될 <u>**추가 콜백**</u>을 만들어 인터페이스를 변경해야 함

```java
void f(int x, Consumer<Integer> dealWithResult, Consumer<Throwable> dealWithException);

// f의 바디는 다음을 수행 : dealWithException(e);
```

콜백이 여러 개인 경우 한 객체로 이 메서드를 감싸는 것이 좋음.(eg. Java 9의 Flow API : Subscriber\<T> 클래스로 감쌈)

**Subscriber\<T>**

- void **onComlete**() : 값을 다 소진했거나 에러가 발생해서 더 이상 처리할 데이터가 없을 때
- void **onError**(Throwable throwable) : 도중에 에러가 발생했을 때
- void **onNext**(T item) : 값이 있을 때