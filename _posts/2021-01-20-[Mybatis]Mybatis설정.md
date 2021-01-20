---
layout: post
title: "[Mybatis]Mybatis설정"
subtitle: "Mybatis 설정"
categories: study
tags: etc
---

> Mybatis 설정


application.yml 파일

    ```yml
    mybatis:
       enabled: true
       config-location: sql/configuration.xml # 설정 파일
       mapper-locations: classpath:/sql/mysql/**/sql-*.xml # mapper XML 매핑
       type-aliases-package: com.bryce.yang.test # type 클래스의 경로 설정
    ```

## type-aliases-package

sql xml 파일의 parameterType 또는 resultType에 DTO, Entity 등의 경로를 com.** 부터 쓰는 것이 공수가 드는 경우가 많음.  
type-aliases-package의 값으로 `패키지 경로`를 설정하면 모든 path를 명시하지 않고 `클래스명`만 적어도 매핑됨.


### 적용 전
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<mapper namespace="TestXml">
    <select id="selectTest" parameterType="com.bryce.yang.test.dto.TestSearchDTO" resultType="com.bryce.yang.test.dto.TestEntity">
        SELECT *
            FROM test
    </select>
</mapper>
```

### 적용 후
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<mapper namespace="TestXml">
    <select id="selectTest" parameterType="TestSearchDTO" resultType="TestEntity">
        SELECT *
            FROM test
    </select>
</mapper>
```
