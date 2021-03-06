---
layout: post
title: "[Java] Statement & PreparedStatement"
subtitle: "Statement & PreparedStatement"
categories: study
tags: java
---

> Statement & PreparedStatement ๋น๊ต

### ๐Statement
1. Statement ๊ฐ์ฒด๋ Connection ํด๋์ค์ `createStatement()` ๋ฉ์๋ ํธ์ถ์ ํตํด ์์ฑ.
2. Statement ๊ฐ์ฒด๊ฐ ์์ฑ๋๋ฉด executeQuery() ๋ฉ์๋๋ฅผ ํธ์ถํด SQL๋ฌธ์ ์คํ์ํฌ ์ ์๋ค. ๋ฉ์๋์ ์ธ์๋ก SQL๋ฌธ์ ๋ด์ String ๊ฐ์ฒด๋ฅผ ์ ๋ฌํ๋ค.
3. Statement๋ `์ ์ `์ธ ์ฟผ๋ฆฌ๋ฌธ์ ์ฒ๋ฆฌํ  ์ ์๋ค. ์ฆ ์ฟผ๋ฆฌ๋ฌธ์ ๊ฐ์ด ๋ฏธ๋ฆฌ ์๋ ฅ๋์ด ์์ด์ผ ํ๋ค.

### ๐PreparedStatement
1. PreparedStatement ๊ฐ์ฒด๋ Connection ๊ฐ์ฒด์ `prepareStatement()` ๋ฉ์๋๋ฅผ ์ฌ์ฉํด์ ์์ฑ. ๋ฉ์๋ ์ธ์๋ก SQL๋ฌธ์ ๋ด์ String ๊ฐ์ฒด๊ฐ ํ์
2. SQL ๋ฌธ์ฅ์ด **๋ฏธ๋ฆฌ ์ปดํ์ผ**๋๊ณ , ์คํ ์๊ฐ๋์ ์ธ์๊ฐ์ ์ํ ๊ณต๊ฐ์ ํ๋ณดํ  ์ ์๋ค๋ ์ ์์ Statement ๊ฐ์ฒด์ ๋ค๋ฆ
3. Statement ๊ฐ์ฒด์ SQL์ ์คํ๋  ๋ ๋งค ๋ฒ ์๋ฒ์์ ๋ถ์ํด์ผ ํ๋ ๋ฐ๋ฉด, PreparedStatement ๊ฐ์ฒด๋ ํ ๋ฒ ๋ถ์๋๋ฉด `์ฌ์ฌ์ฉ` ์ฉ์ด
4. ๊ฐ๊ฐ์ ์ธ์์ ๋ํด ์์นํ๋(placeholder)๋ฅผ ์ฌ์ฉํด SQL ๋ฌธ์ฅ์ ์ ์ํ  ์ ์๊ฒ ํด์ค๋ค. ์์นํ๋๋ **?** ๋ก ํํ๋จ
5. ๋์ผํ SQL๋ฌธ์ ํน์  ๊ฐ๋ง ๋ฐ๊พธ์ด์ **์ฌ๋ฌ ๋ฒ ์คํํด์ผ ํ  ๋**, ์ธ์๊ฐ ๋ง์์ SQL๋ฌธ์ ์ ๋ฆฌํด์ผ ๋  ํ์๊ฐ ์์ ๋ ์ฌ์ฉํ๋ฉด ์ ์ฉ


![prepared-statement-performance-1](/assets/img/etc/prepared-statement-performance-1.png)

- PreparedStatement์ ์ฌ์ฌ์ฉ ์์ค์ ๋ ๊ฐ์ง
    1. JDBC ๋๋ผ์ด๋ฒ์ ์ํ PreparedStatement ์ฌ์ฌ์ฉ
    2. DB์ ์ํ PreparedStatement ์ฌ์ฌ์ฉ

### ๐์ด๋ค ๊ฒ์ ์ฌ์ฉํด์ผ ํ๋๊ฐ?
Statement์ ๊ฒฝ์ฐ ๋งค ๋ฒ ์ฟผ๋ฆฌ์ ๋ํ ์ปดํ์ผ์ ์ํํ๋ค. PreparedStatement์ ๊ฒฝ์ฐ ๋ฏธ๋ฆฌ ์ปดํ์ผ๋ ์ฟผ๋ฆฌ๋ฅผ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ์ํ ์๋๊ฐ ๋ ๋น ๋ฅด๋ค๋ ์ฅ์ ์ด ์๋ค.  
SQL Injection ์ ๋ง๊ณ , ์ฟผ๋ฆฌ๋ฌธ ์ฌ์ฌ์ฉ์ ํตํ ์ฑ๋ฅ์์ ์ด์ ์ ์ํด PreparedStatement๋ฅผ ์ฌ์ฉํ๋ค.  
๋ฌด์กฐ๊ฑด PreparedStatement๋ฅผ ์ฌ์ฉํ  ๊ฒฝ์ฐ DB์ ์บ์ฑ๋ SQL๋ฌธ์ด ๊ณผ๋ํด์ง ์ ์๋ค. ์ด๋ ์ฑ๋ฅ์ ๋ฎ์ถ ์ ์์ผ๋, ์์ฃผ ์ฌ์ฉ๋๋ ๊ตฌ๋ฌธ๋ง PreparedStatement๋ก ํ๋ ๊ฒ์ด ์ข์ ๊ฒฝ์ฐ๋ ์๋ค.

### ๐4.3. ์ฐธ์กฐ
- [http://tutorials.jenkov.com/jdbc/preparedstatement.html](http://tutorials.jenkov.com/jdbc/preparedstatement.html)
- [https://devbox.tistory.com/entry/Comporison](https://devbox.tistory.com/entry/Comporison)
