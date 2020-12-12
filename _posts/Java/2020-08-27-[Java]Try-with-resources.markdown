---
layout: post
title: "[Java] try-with-resources"
subtitle: "try-with-resources"
categories: study
tags: java
---

# Try-with-resources

- JDK 1.7부터 적용
- 기존 try-catch 문을 사용할 경우 코드가 복잡해지는 경우 다수 발생

```java
try{
    fis = new FileInputStream("file.txt");
    dis = new DataInputStream(fis);
} catch (IOException ie){
    ie.printStackTrace();
} finally {
    try{
        if(dis != null){
            dis.close();
        }
    } catch (IOException ie){
        ie.printStackTrace();
    }
}
```

**try-with-resources 적용 후**

```java
try(FileInputStream fis = new FileInputStream("file.txt");
    DataInputStream dis = new DataInputStream(fis)) {
        while(true){
            score = dis.readInt();
            Sytem.out.println(score);
            sum += score;
        }
} catch (EOFException e) {
    System.out.println("점수의 총합은 " + sum + "입니다.");
} catch (IOException ie) {
    ie.printStackTrace();
}
```

- try 괄호()안에 변수 선언 가능, 선언 변수 try 블럭 내에서만 사용

```java
public class Application {

    public static void main(String[] args) throws SQLException {
        String url = "jdbc:postgresql://localhost:5432/springdata";
        String username = "hosuk";
        String password = "pass";

        try(Connection connection = DriverManager.getConnection(url, username, password)){
            String sql = "INSERT INTO ACCOUNT VALUES(1, 'hosuk', 'pass')";
            try(PreparedStatement statement = connection.prepareStatement(sql)){
                statement.execute();
            }
        }
    }
}
```
