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
SQL은 DBMS에 데이터를 질의하는 언어입니다. 웹 서비스는 이용자의 입력을 SQL 구문에 포함해 요청하는 경우가 있습니다. 예를 들면,