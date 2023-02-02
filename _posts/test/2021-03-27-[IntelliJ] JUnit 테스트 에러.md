---
layout: post
title: "[IntelliJ] JUnit 테스트 에러"
subtitle: "IntelliJ JUnit 에러"
categories: study
tags: test
---

> IntelliJ 에서 JUnit 테스트 에러 발생 시


```
FAILURE: Build failed with an exception.

* What went wrong:

Execution failed for task ':test'.

> No tests found for given includes: [com.test.TestService](filter.includeTestsMatching)

* Try:

Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 4s
```
위와 같은 에러가 발생 시 

![intelliJ-JUnit](/assets/img/etc/IntelliJ_JUnit_error.png)

Run tests using 을 `IntelliJ IDEA`로 변경하면 해결된다.

https://stackoverflow.com/questions/55405441/intelij-2019-1-update-breaks-junit-tests