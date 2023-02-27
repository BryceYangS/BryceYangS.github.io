# 2023.02.27
## SQL 대량의 데이터 처리 시
- 만약 mybatis로 대량의 데이터를 조회하고자 할 때 default fetchSize를 확인하고 fetchSize를 조절해서 가져오자
- mybatis bulk insert
   ```xml
   <insert id="upsertBulk" parameterType="xxx">
      INSERT INTO user (id, name, address)
      <foreach collection="users" item="user" separator=" , ">
      VALUES (#{user.id}, #{user.name}, #{user.address})
      </foreach>
      ON DUPLICATE KEY UPDATE
         id = values(id),
         name = values(name),
         address = values(address)
   </insert>
   ```
   - ❗️ insert bulk 주의사항
      - 한 번에 너무 많은 insert를 하지 말 것 : INSERT - ON DUPLICAT KEY UPDATE는 삽입된 record에 대해 key 충돌 발생 경우 배타적 잠금 발생. => 테이블 전체 레코드에 잠금이 걸릴 수 있음. DB 복제 지연, 처리시간의 증가
      - VALUES 절 Multiple Value 전달 : 전체 삽입 과정이 하나의 트랜잭션으로 잡힘. => 트랜잭션 시간이 길어지고, 하나의 레코드에 문제가 발생할 경우 전체가 실패할 수 있음.
      - 따라서 !! 적절한 사이즈로 끊어서 insert를 해주자!