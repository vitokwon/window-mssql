# window-mssql

## 데이터베이스의 이해와 관리 프로그램의 플요성
  - 데이터 분석의 핵심은 제대로 키워드를 골라내는 것
  - 데이터의 집합체는 데이터가 모여 있는 공간, 정보의 집합
    - 하나의 공간에 필요한 데이터를 중복되지 않게 모아두고 사용자 각자가 조회하도록 유도
    - 관리의 효율성과 속도 면에서 편리함.
  - 데이터, 데이터 관리 기능, 저장 방식 규칙에 대한 응용 프로그램의 필요성
    - 데이터베이스 관리 시스템(Database Management System, DBMS) 개발
    - 많은 동시 작업에 대한 중요도에 따라 다르게 작동
    - 조회 중인 데이터가 업데이트, 삭제가 된 경우 작동 시점
    - 작업 도중의 예기치 못한 상황 발생 시, 데이터 복구 기능
  - RDBMS, Relational DataBase Management System 
    - 관계형 데이터 베이스 (Oracle, MS SQL, DB2, Sybase)
    - SQL 언어를 기반으로 작동
  - SQL, Structured Query Language
    - 관계형 데이터베이스를 관리하기 위해 설계된 프로그래밍 언어
    - 자료 검색, 관리, 생성과 수정, 접근 조정 관리
    - 여러 RDBMS가 존재하지만, SQL을 표준으로 채택함.
  - MS SQL
    - Excel과의 연동이 추가 환경설정 없이 자동으로 가능
    - R이나 파이썬 같은 언어와의 연계가 SQL Server 2016년부터 내장, 자동화와 유지보수 편리
    - 질의 도구, 데이터베이스 관리 도구, XML이나 웹 응용 도구들의 사용 편리
    - 데이터베이스 관리 차원에서의 난이도가 낮아 초보자들에게 적합

## 설치
  - https://www.microsoft.com/ko-kr/sql-server/sql-server-downloads 또는 ms sql server 검색
  - 개발자용 다운로드 후 설치, 모든 설치옵션은 기본
  - 설치 완료 후, SSMS 설치 (버튼을 누르면 홈페이지로 연결, 다운로드 후 설치)

##  SSMS, Microsoft SQL Server Management studio
  - 최초 `connect` 접속
  - `database 탭` 에서 오른쪽 클릭 후 `attach - add` 로 필요한 파일 추가
  - `stepup_log.ldf` 파일은 없으므로 삭제
  -  추가된 데이터베이스 `study` 확인
  -  열(column)은 필드(field), 행(row)는 레코드(record)

## 쿼리 정리
  - 상단의 `New Query` 실행하여 입력창 생성
  - 쿼리 실행 단축키, Alt+X 또는 F5
``` sql
use study
go
select * from companyinfo

-- use study를 안 쓸 경우
-- select * from study.dbo.companyinfo
-- dbo : database owner의 약자
-- 대부분 99% dbo이므로 아래와 같이 입력하기도 함
-- select * from study..companyinfo

-- select 코드
-- select column1, columns2
-- from table_name

select name, city, IncInCtryCode from companyinfo

-- distinct 코드
-- 서로 다른 값만을 반환할 수 있음
-- 중복되는 행을 하나로 보여줌
-- select distinct column1, column2, from table_name

select distinct incInCtryCode from companyinfo

-- where 코드
-- 원하는 데이터만 가져오는 where 코드
-- select column1, column2 from table_name where condition

select * from companyinfo where IncInCtryCode='kor'
select * from companyinfo where Employees>=100000

-- 연산자 and, or, not
-- and : select column1, column2, from table_name where condition1 and condition2 
select *
from companyinfo
where incinctrycode='kor' and employees>5000
-- or : select column1, column2, from table_name where condition1 or condition2
select *
from companyinfo
where city='seoul' or city='busan'
-- not : select column1, column2, from table_name where not condition1
select *
from companyinfo
where not IncInCtryCode='usa'
-- not, and
select *
from companyinfo
where not IncInCtryCode='usa'
and not IncInCtryCode='jpn'

-- like 연산자 , 와일드카드 %, _
-- select column1, column2, from table_name where condition like pattern

-- a로 시작하는 이름 찾기
select * from companyinfo where name like 'a%'
-- _는 한 문자를 의미, a로 시작하는 4개의 문자로 이루어진 이름
select * from companyinfo where name like 'a____'

-- 와일드카드 사용 시, 다른 검색에 비해 시간이 오래 걸림 (성능저하)
-- 반드시 필요한 부분이 아니라면 뒤에서 사용
-- 'a%' a로 시작하는 고객명
-- '%a' a로 끝나는 고객명
-- '%or%' 이름에 or이 포함된 고객명
-- '_r%' 두번쨰글자가 r인 고객명
-- 'a_%_%' a로 시작하면서 최소 3글자 이상인 고객명
-- 'a%o' a로 시작해서 o로 끝나는 고객명

-- 데이터 정렬
-- 내림차순 desc, 오름차순 asc
-- select column1, column2,... from table_name order by column1, column2,... desc

select incinctrycode, employees, name
from companyinfo
order by IncInCtryCode

-- 조회값에서 NULL 삭제
select incinctrycode, employees, name
from companyinfo
where IncInCtryCode is not null
order by IncInCtryCode

-- 알파벳 순서 정렬, employees는 큰값에서 작은값으로 정렬
select incinctrycode, employees, name
from companyinfo
where IncInCtryCode is not null
order by IncInCtryCode, employees desc
```
