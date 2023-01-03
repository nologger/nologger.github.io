---
layout: post
title: "JdbcTemplate Documentation"
date: 2023-01-03
category: "Spring"
---

{% raw %}
# JdbcTemplate Documentation

> [https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#jdbc](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#jdbc)


* * *

**index**

1. JDBC 데이터베이스 액세스를 위한 접근 방식 선택
2. 패키지 계층
3. JDBC 코어 클래스를 사용하여 기본 JDBC 처리 및 오류 처리 제어

* * *

  

JDBC를 사용하는 새로운 프로젝트를 구축할 때마다, 문서와 실 사용자들의 레퍼런스를 참고하게 된다. 이 기회에 Spring 공식 문서를 통해 정리해 놓고자 한다.

  

## 1\. JDBC 데이터베이스 액세스를 위한 접근 방식 선택

JDBC 데이터베이스 액세스의 기반을 형성하기 위해 여러 접근 방식을 선택할 수 있다. `JdbcTemplate` 의 세 가지 특징 외에도 새로운 `SimpleJdbcInsert` 와 `SimpleJdbcCall`  접근 방식은 데이터베이스 메타데이터를 최적화하고 RDBMS 개체 스타일은 JDO 쿼리 디자인과 유사한 개체 지향 접근 방식을 취한다. 이러한 접근 방식들을 사용하면, 다른 접근 방식의 기능을 포함하도록 혼합하고 매치시킬 수 있다. 모든 접근 방식은 JDBC 2.0 호환 드라이버가 필요하며, 일부 고급 기능에는 JDBC 3.0 드라이버가 필요하다.

  

- `JdbcTemplate` 은 고전적이고 가장 인기 있는 Spring JDBC 접근 방식이다. 가장 기본이되는 접근 방식부터 다른 모든 접근 방식들 또한 내부적으로 `JdbcTemplate` 를 사용한다.
- `NamedParameterJdbcTemplate` 은 `JdbcTemplate` 을 래핑하여 기존 JDBC `?` 자리표시자 접근 방식을 대체한다. 이 접근 방식은 SQL문에 다양한 매개변수가 존재할 경우에 사용하기 쉬우며 문서화가 잘 이루어져 있다.
- `SimpleJdbcInsert` 와 `SimpleJdbcCall` 은 구성에 필요한 양을 제한하기 위해 데이터베이스 메타데이터를 최적화한다. 이 접근 방식은 코딩을 단순화하므로 테이블 또는 프로시저의 이름만 제공하고 열 이름과 일치하는 매개변수 맵을 제공하면 된다. 이는 데이터베이스가 적절한 메타데이터를 제공하는 경우에만 동작한다. 만약 데이터베이스가 이러한 데타데이터를 제공하지 않으면, 매개변수의 명시적을 구성을 제공해야 한다.
    
- `MappingSqlQuery` , `SqlUpdate`  그리고 `StoredProcedure` 를 포함하는 RDBMS 개체는 데이터 접근 계층을 초기화하는 동안 재사용이 가능하고 스레드로부터 안전한 개체를 생성해야 한다. 이 접근 방식은 쿼리 문자열을 정의하고, 쿼리를 컴파일하는 JDO 쿼리를 모델로 한다. 이렇게 하면 `execute(...)` , `update(...)` , `findObject(...)`  메서드를 다양한 매개 변수 값으로 여러 번 호출할 수 있다.
    

  

## 2\. 패키지 계층

Spring Framework의 JDBC 추상화 프레임워크는 다음과 같은 네 가지 패키지로 구성된다.

  

- `core` : `org.springframeowrk.jdbc.core` 패키지에는 `JdbcTemplate`  클래스와 다양한 콜백 인터페이스 및 관련 클래스가 포함되어 있다. `org.springframework.jdbc.core.simple` 이라는 하위 패키지에는 `SimpleJdbcTemplate` 및 `SimpleJdbcCall` 클래스가 포함되어 있다. `org.springframework.jdbc.core.namedparam` 에는 `NamedParameterJdbcTemplate`  클래스 및 관련 지원 클래스가 포함되어 있다. 
- `datasource`  : `org.springframework.jdbc.datasource`  패키지에는 쉬운 `DataSource`  액세스를 위한 다양한 유틸리티 클래스와 Jakarta EE 컨테이너 외부에서 수정되지 않은 JDBC 코드를 테스트하고 실행하는 데 사용할 수 있는 다양한 간단한 `DataSource`  구현이 포함되어 있다. `org.springframework.jdbc.datasource.embedded`  라는 이름의 하위 패키지는 HSQL, H2 그리고 Derby와 같은 Java 데이터베이스 엔진을 사용하여 임베디드 데이터베이스 생성을 지원한다.
- `object` : `org.springframework.jdbc.object`  패키지에는 RDBMS 쿼리, 업데이트 및 저장 프로시저를 스레드 안전하고 재사용 가능한 개체로 나타내는 클래스가 포함되어 있다. 이 접근 방식은 JDO에 의해 모델링되지만 쿼리에 의해 반환된 개체는 자연스럽게 데이터베이스에서 연결이 끊어진다. 이 상위 수준의 JDBC 추상화는 `org.springframework.jdbc.core`  패키지의 하위 수준 추상화에 따라 다르다.
- `support`  : `org.springframework.jdbc.support` 패키지는 `SQLException` 번역 기능과 일부 유틸리티 클래스를 제공한다. JDBC 처리 중에 발생한 예외는 `org.springframework.dao`  패키지에 정의된 예외로 반환된다. 이는 Spring JDBC 추상화 계층을 사용하는 코드가 JDBC 또는 RDBMS 관련 오류 처리를 구현할 필요가 없음을 의미한다. 변환된 모든 예외는 선택 해제되어 다른 에외가 호출자에게 전파되도록 하면서 복구할 수 있는 예외를 포착할 수 있는 옵션을 제공한다.

  

## 3\. JDBC 코어 클래스를 사용하여 기본 JDBC 처리 및 오류 처리 제어

### 3.1 `JdbcTemplate`  사용

`JdbcTemplate` 은 JDBC 코어 패키지의 중심 클래스이다. 연결을 닫는 것을 잊는 것과 같은 일반적인 오류를 방지하는 데 도움이 되는 리소스 생성 및 해제를 처리한다. core JDBC 워크플로의 기본작업(생성 및 실행)을 수행하고 애플리케이션 코드는 SQL을 제공하고 결과를 추출한다.

  

- SQL 쿼리 실행
- 업데이트 문 및 저장 프로시저 호출
- `ResultSet`  인스턴스에 대한 반복 및 반환된 매개변수 값 추출
- JDBC 예외를 포착하고 `org.springframework.dao` 에 정의된 일반적이고 더 많은 정보를 포함하는 예외 계층 구조로 변환

  

코드에 `JdbcTemplate` 을 사용하는 경우 콜백 인터페이스를 구현하여 명확하게 정의된 계약을 제공하기만 하면 된다. `JdbcTemplate`  클래스에서 제공하는 연결이 주어지면 `PreparedStatementCreator`  콜백 인터페이스는 SQL 및 필요한 매개변수를 제공하는 준비된 명령문을 작성한다. 호출 가능한 명령문을 생성하는 `CallableStatementCreator`  인터페이스의 경우에도 마찬가지이다. `RowCallbackHandler`  인터페이스는 `ResultSet` 의 각 행에서 값을 추출한다.

  

`DataSource`  참조로 직접 인스턴스화를 통해 DAO 구현 내에서 `JdbcTemplate` 을 사용하거나 Spring IoC 컨테이너에서 구성하고 DAO에 빈 참조로 제공할 수 있다.

  

이 클래스에서 발행한 모든 SQL은 템플릿 인스턴스의 정규화된 클래스 이름에 해당하는 범주 아래의 `DEBUG`  수준에서 기록된다(일반적으로 `JdbcTemplate` 이지만 `JdbcTemplate`  클래스의 사용자 지정 하위 클래스를 사용하는 경우 다를 수 있음).  

  

#### Querying (SELECT)

```
int rowCount = this.jdbcTemplate.queryForObject("select count(*) from t_actor", Integer.class);

int countOfActorNamedJoe = this.jdbcTemplate.queryForObject("select count(*) from t_actor where first_name = ?", Integer.class, "Joe");

String lastName = this.jdbcTemplate.queryForObject("select last_name from t_actor where id = ?", String.class, 1212L);

Actor actor = this.jdbcTemplate.queryForObject (
 "select first_name, last_name from t_actor where id = ?",
 (resultSet, rowNum) -> {
  Actor actor = new Actor();
  actor.setFirstName(resultSet.getString("first_name"));
  actor.setLastName(resultSet.getString("last_name"));
  return actor;
 },
 1212L
);
 
List<Actor> actors = this.jdbcTemplate.query(
 "select fist_name, last_name from t_actor",
 (resultSet, rowNum) -> {
  Actor actor = new Actor();
  actor.setFirstName(resultSet.getString("first_name"));
  actor.setLastName(resultSet.getString("last_name"));
  return actor;
 }
);
```

  

```
private final RowMapper<Actor> actorRowMapper = (resultSet, rowNum) -> {
  Actor actor = new Actor();
  actor.setFirstName(resultSet.getString("first_name"));
  actor.setLastName(resultSet.getString("last_name"));
  return actor;
};
 
public List<Actor> findAllActors() {
 return this.jdbcTemplate.query("select first_name, last_name from t_actor", actorRowMapper);
} 
```

  

#### Updating (INSERT, UPDATE and DELETE)

```
this.jdbcTemplate.update(
 "insert into t_actor (first_name, last_name) values (?, ?)",
 "Leonor",
 "Watling"
);
 
this.jdbcTemplate.update(
 "update t_actor set last_name = ? where id = ?",
 "Bango",
 5276L
);
 
this.jdbcTemplate.update(
 "delete from t_actor where id = ?",
 Long.valueOf(actorId)
);
```

  

#### Other Operations

```
this.jdbcTemplate.execute("create table mytable (id integer, name varchar(1000)");
 
this.jdbcTempate.update("call SUPPORT.REFRESH_ACTORS_SUMMARY(?)", Long.valueOf(unionId));
```

  

#### JdbcTempalte Best Practices

```
@Repository 
public class JdbcDao implements dao {
 private JdbcTemplate jdbcTemplate;

 @Autowired
 public void setDataSource(DataSource dataSource) {
  this.jdbcTemplate = new JdbcTemplate(dataSource);
 }
}
```
{% endraw %}
