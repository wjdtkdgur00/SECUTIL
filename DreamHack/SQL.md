# 데이터베이스 관리 시스템
웹 서비스는 데이터베이스에 정보를 저장하고, 이를 관리하기 위해 **DataBase Management System(DBMS)** 을 사용합니다. DBMS는 데이터베이스에 새로운 정보를 기록하거나, 기록된 내용을 수정, 삭제하는 역할을 합니다. DBMS는 다수의 사람이 동시에 데이터베이스에 접근할 수 있고, 웹 서비스의 검색 기능과 같이 복잡한 요구사항을 만족하는 데이터를 조회할 수 있다는 특징이 있습니다.

DBMS는 크게 관계형과 비관계형을 기준으로 분류하며, 다양한 종류의 DBMS가 존재합니다. 각 종류별 대표적인 DBMS는 우측 표에 정리되어 있습니다.

두 DBMS의 가장 큰 차이로 관계형은 행과 열의 집합인 테이블 형식으로 데이터를 저장하고, 비관계형은 테이블 형식이 아닌 키-값 (Key-Value) 형태로 값을 저장합니다.
```
Relational(관계형): MySQL, MariaDB, PostgreSQL, SQLite

Non-Relational(비관계형): MongoDB, CouchDB, Redis
```
# Relational DBMS
**Relational DataBase Management System(RDBMS, 관계형 RDBMS)** 는 1970년에 Codds가 12가지 규칙을 정의하여 생성한 데이터베이스 모델입니다.

RDBMS는 행 (Row)과 열(Column)의 집합으로 구성된 **테이블의 묶음 형식**으로 데이터를 관리하고, 테이블 형식의 데이터를 조작할 수 있는 관계 연산자를 제공합니다. Codds는 12가지 규칙을 정의했지만 실제로 구현된 RDBMS들은 12가지 규칙을 모두 따르지는 않았고, 최소한의 조건으로 앞의 두 조건을 만족하는 DBMS를 RDBMS라고 부르게 되었습니다. RDBMS에서 관계 연산자는 **Structured Query Language(SQL)** 라는 쿼리 언어를 사용하고, 쿼리를 통해 테이블 형식의 데이터를 조작합니다.
# SQL
**Structured Query Language (SQL)** 는 RDBMS의 데이터를 정의하고 질의, 수정 등을 하기 위해 고안된 언어입니다. SQL은 구조화된 형태를 가지는 언어로 웹 어플리케이션이 DBMS와 상호작용할 때 사용됩니다. SQL은 사용 목적과 행위에 따라 다양한 구조가 존재하며 대표적으로 아래와 같이 구분됩니다.
```
DDL
(Data Definition Language)

데이터를 정의하기 위한 언어입니다. 데이터를 저장하기 위한 스키마, 데이터베이스의 생성/수정/삭제 등의 행위를 수행합니다.

DML
(Data Manipulation Language)

데이터를 조작하기 위한 언어입니다. 실제 데이터베이스 내에 존재하는 데이터에 대해 조회/저장/수정/삭제 등의 행위를 수행합니다.

DCL
(Data Control Language)

데이터베이스의 접근 권한 등의 설정을 하기 위한 언어입니다. 데이터베이스 내에 이용자의 권한을 부여하기 위한 GRANT와 권한을 박탈하는 REVOKE가 대표적입니다.
```
# SQL Injection
SQL은 DBMS에 데이터를 질의하는 언어입니다. 웹 서비스는 이용자의 입력을 SQL 구문에 포함해 요청하는 경우가 있습니다. 예를 들면, 로그인 시에 ID/PW를 포함하거나, 게시글의 제목과 내용을 SQL 구문에 포함합니다. **Figure2** 는 로그인 할 때 애플리케이션이 DBMS에 질의하는 예시 쿼리입니다.
```SQL
/*
아래 쿼리 질의는 다음과 같은 의미를 가지고 있습니다.
- SELECT: 조회 명령어
- *: 테이블의 모든 컬럼 조회
- FROM accounts: accounts 테이블 에서 데이터를 조회할 것이라고 지정
- WHERE user_id='dreamhack' and user_pw='password': user_id 컬럼이 dreamhack이고, user_pw 컬럼이 password인 데이터로 범위 지정
즉, 이를 해석하면 DBMS에 저장된 accounts 테이블에서 이용자의 아이디가 dreamhack이고, 비밀번호가 password인 데이터를 조회
*/
SELECT * FROM accounts WHERE user_id='dreamhack' and user_pw='password'
```
쿼리문을 살펴보면, 이용자가 입력한 "dreamhack"과 "password" 문자열을 SQL 구문에 포함하는 것을 확인할 수 있습니다. 이렇게 이용자가 SQL 구문에 임의 문자열을 삽입하는 행위를 **SQL Injection**이라고 합니다. SQL Injection이 발생하면 조작된 쿼리로 인증을 우회하거나, 데이터베이스의 정보를 유출할 수 있습니다.

**Figure3**은 SQL Injection으로 조작한 쿼리문의 예시입니다. 
```SQL
/*
아래 쿼리 질의는 다음과 같은 의미를 가지고 있습니다.
- SELECT: 조회 명령어
- *: 테이블의 모든 컬럼 조회
- FROM accounts: accounts 테이블 에서 데이터를 조회할 것이라고 지정
- WHERE user_id='admin': user_id 컬럼이 admin인 데이터로 범위 지정
즉, 이를 해석하면 DBMS에 저장된 accounts 테이블에서 이용자의 아이디가 admin인 데이터를 조회
*/
SELECT * FROM accounts WHERE user_id='admin'
```
쿼리를 살펴보면, ```user_pw``` 조건문이 사라진 것을 확인할 수 있습니다. 조작한 쿼리를 통해 질의하면 DBMS는 ID가 admin인 계정의 비밀번호를 비교하지 않고 해당 계정의 정보를 반환하기 때문에 이용자는 admin 계정으로 로그인할 수 있습니다.