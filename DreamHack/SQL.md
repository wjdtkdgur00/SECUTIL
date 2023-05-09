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
# Blind SQL Injection
인증 우회 이외에도 데이터베이스의 데이터를 알아낼 수 있습니다. 이때 사용할 수 있는 공격 기법으로는 **Blind Injection** 이 있습니다. 해당 공격 기법은 스무고개 게임과 유사한 방식으로 데이터를 알아낼 수 있습니다. 스무고개는 질문자와 답변자가 있고, 질문자가 답변을 하면 답변자가 예/아니오로 대답을 해 질문자가 답을 유추하는 게임입니다.

예를 들어 답변자가 7이라는 숫자를 칠판에 적어둔 뒤, 질문을 받습니다. 질문자는 해당 숫자가 6인지 묻습니다. 답변자는 6보다 큰 숫자라고 대답을 합니다. 이제 질문자는 해당 숫자가 8 인지 묻자, 답변자는 8보다 작은 숫자라고 대답을 합니다. 이때 질문자는 정답이 7이라는 것을 알 수 있습니다. 공격자는 이러한 방법으로 데이터베이스의 내용을 알아낼 수 있습니다. 

다음은 계정 정보를 탈취한다고 가정했을 때의 예시입니다.

* Question #1. google 계정의 비밀번호 첫 번째 글자는 'x'인가요? 
    * Answer. 아닙니다.

* Question #2. google 계정의 비밀번호 첫 번째 글자는 'p'인가요?
    * Answer. 맞습니다(첫 번째 글자 = ```p```)

* Question #3. google 계정의 비밀번호 두 번째 글자는 'y'인가요?
    * Answer. 아닙니다.

* Question #4. google 계정의 비밀번호 두 번째 글자는 'a'인가요?
    * Answer. 맞습니다. (두 번째 글자 = ```a```)

위와 같은 형태로 DBMS가 답변 가능한 형태로 질문하면서 google 계정의 비밀번호인 ```password```를 알아낼 수 있습니다. 이처럼 질의 결과를 이용자가 화면에서 직접 확인하지 못할 때 참/거짓 반환 결과로 데이터를 획득하는 공격 기법을 Blind SQL Injection 기법이라고 합니다.
# **Blind SQL Injection** 예시
**Figure4** 는 Blind SQL Injection 공격시에 사용할 수 있는 쿼리입니다.
```sql
# 첫 번째 글자 구하기 (아스키 114 = 'r', 115 = 's')
SELECT * FROM user_table WHERE uid='admin' and ascii(substr(upw,1,1))=114-- ' and upw=''; # False
SELECT * FROM user_table WHERE uid='admin' and ascii(substr(upw,1,1))=115-- ' and upw=''; # True
# 두 번째 글자 구하기 (아스키 115 = 's', 116 = 't')
SELECT * FROM user_table WHERE uid='admin' and ascii(substr(upw,2,1))=115-- ' and upw=''; # False
SELECT * FROM user_table WHERE uid='admin' and ascii(substr(upw,2,1))=116-- ' and upw=''; # True 
```
쿼리를 살펴보면, 세 개의 조건이 있는 것을 확인할 수 있습니다. 조건을 살펴보기 전에 ```ascii```와 ```substr``` 함수에 대해서 알아보겠습니다.

### **ascii**
<U>**전달된 문자를 아스키 형태로 반환하는 함수**</U>입니다. 예를 들어, ```ascii('a')```를 실행하면 'a'문자의 아스키 값인 97을 반환합니다.

### **substr**
해당 함수에 전달되는 인자와 예시는 다음과 같습니다. 해당 함수는 문자열에서 지정한 위치부터 길이까지의 값을 가져옵니다.
```sql
substr(string, position, length)
substr('ABCD', 1, 1) = 'A'
substr('ABCD', 2, 2) = 'BC'
```

공격 쿼리문의 두 번째 조건을 살펴보면, ```upw``` 의 첫 번째 값을 아스키 형태로 변환한 값이 114('r') 또는 115('s')인지 질의합니다. 질의 결과는 로그인 성공 여부로 참/거짓을 판단할 수 있습니다. 만약 로그인이 실패할 경우 첫 번째 문자가 'r'이 아님을 의미합니다. 이처럼 쿼리문의 반환 결과를 통해 admin 계정의
비밀번호를 획득할 수 있습니다.

# **Blind SQL Injection 공격 스크립트**
Blind SQL Injection은 한 바이트씩 비교하여 공격하는 방식이기 때문에 다른 공격에 비해 많은 시간을 들여야합니다. 이러한 문제를 해결하기 위해서는 공격을 자동화하는 스크립트를 작성하는 방법이 있습니다. 공격 스크립트를 작성하기에 앞서 유용한 라이브러리를 알아보겠습니다. 파이썬은 HTTP 통신을 위한 다양한 모듈이 존재하는데, 대표적으로 **requests** 모듈이 있습니다. 해당 모듈은 다양한 메소드를 사용해 HTTP 요청을 보낼 수 있으며 응답 또한 확인할 수 있습니다.

**Figure5**는 requests 모듈을 통해 HTTP의 GET 메소드 통신을 하는 예제 코드입니다.
```python
import requests

url = 'https://dreamhack.io/'

headers = {
    'Content-Type': 'application/x-www-form-urlencoded',
    'User-Agent': 'DREAMHACK_REQUEST'
}

params = {
    'test': 1,
}

for i in range(1, 5):
    c = requests.get(url + str(i), headers=headers, params=params)

print(c.request.url)
    print(c.text)
```
```requests.get```은 GET 메소드를 사용해 HTTP 요청을 보내는 함수로, URL과 Header, Parameter와 함께 요청을 전송할 수 있습니다.

**Figure6**은 HTTP의 POST 메소드 통신을 하는 예제 코드입니다.
```python
import requests

url = 'https://dreamhack.io/'

headers = {
    'Content-Type': 'application/x-www-form-urlencoded',
    'User-Agent': 'DREAMHACK_REQUEST'
}

data = {
    'test': 1,
}

for i in range(1, 5):
    c = requests.post(url + str(i), headers=headers, data=data)
    print(c.text)
```
```requests.post```는 POST 메소드를 사용해 HTTP 요청을 보내는 함수로 URL과 Header, Body와 함께 요청을 전송할 수 있습니다.

GET, POST 메소드 이외에도 다양한 메소드를 사용해 요청을 전송할 수 있습니다.