

# DB에 대한 기초정보

## DB를 사용하는 이유

- 파일저장소를 사용하는 경우의 단점
    - 개발자가 구현해야할 것이 너무 많아짐
    - 자료의 안정성이 보장 안됨 (파일을 임의로 수정하다가 실수할 수 있음)

## DBMS (Database management system)

- DB를 관리하는 메니지먼트 시스템 (ex. Oracle, MySQL, MS Access 등)
- 여러 계정(HR, TEST, SYSTEM 등)이 있으며, 각 계정에 접속해서 DB관리 가능
- DB의 table은 binary로 저장되므로, 다른 편집기에선 열람 자체가 불가능
- 제약조건을 설정하여 중복값, 잘못된 값 등을 방지할 수 있음 (자료의 안정성)
- sqlplus, sqldeveloper 같은 프로그램에서 SQL구문을 DB로 송신한 후, DB에서 다시 결과를 수신받는 구조 → 네트워크 비용을 최소화할 필요가 있음

### ORACLE

- RDBMS(relational database management system, 관계형 데이터베이스)의 한 종류
- 세계적으로 가장 많이 사용하는 RDBMS
- 수업용 버전 : oracle express edition 11G 버전
- obdbcX.jar → Java와 DB 사이의 연결을 담당하는 라이브러리
- (경로 : oraclexe\app\oracle\product\11.2.0\server\jdbc\lib)

### SQL (Structured query language) 이란?

- DBMS의 종류와 독립적임 (표준이 있음)
- query를 사용하여 데이터를 다룸
- sqlplus : oracle 설치시 자동 설치되는 클라이언트용 프로그램

## Database Modeling

- 정보시스템 구축의 절차
    - **분석 (實使用者의 입장에서 요구사항을 분석)** → 설계 → 구현

# SQL문법

## SQLPLUS 기본명령어

conn (hr)/(hr) → (hr) 계정에 연결 (conn ID/PW)

alter user (hr) account unlock; → (hr) 계정의 lock 풀기

alter user (hr) identified by hr; → (hr) 계정의 PW를 (hr)로 설정

SELECT * FROM TAB; → 해당 계정에 있는 table 전체 검색

desc table_name; → 해당 table에 대한 세부정보 보기

## Query

- SELECT 구문
    - SELECT ... FROM ... [JOIN ON] ... [WHERE] ... [GROUP BY] ... [HAVING] ... [ORDER BY] ...
    - 처리순서 : FROM → JOIN ON → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
    - FROM ~ HAING : 행 찾기 (selection) / SELECT ~ ORDER BY : 출력 or 조회할 열 찾기 (projection)
- table의 이름에 별칭을 주기
    - SELECT employee_id, department_name FROM employees e JOIN departments d ON (e.department_id = d.DEPARTMENT_ID);
- column의 이름에 별칭을 주기 (헤딩 바꾸기)
    - SELECT employee_id “사원 번호”, first_name “이름”, last_name “성”, salary “Sal” FROM employees;
- 복수의 column을 결합하기 : ‘||’
    - SELECT employee_id, first_name, last_name, first_name || ‘-’ || last_name FROM employees;

### 1. 자료형 (data type)

1. 문자형
    1. VARCHAR2, NVARCHAR2 : 길이 가변적
    2. CHAR, NCHAR : 길이 고정적
2. 숫자형 : NUMBER (P : 정수길이, S : 소수점이하길이)
3. 날짜형
    1. DATE (년, 월, 일, 시, 분, 초까지 저장)
    2. TIMESTAMP (DATE와 같으나 밀리초까지 저장)

### 2. 연산자 (operator)

- 산술연산자
    - +, -, *, / (*나머지는 MOD()함수를 사용하여 계산)
    
    ```sql
    SELECT employee_id, salary, salary + salary * NVL(commission_pct, 0) “실급여” FROM employees;
    //* NVL() 함수 : null 값이 있을 경우 다른 값으로 설정하여 연산에 참여시킬 수 있게 하는 함수
    ```
    
- 비교연산자
    - >, <. >=, <=, =, <>(같지 않다)
    
    ```sql
    SELECT employee_id, salary FROM employees WHERE salary >= 3000;
    ```
    
- 논리연산자
    - AND, OR, NOT
    
    ```sql
    SELECT employee_id, department_id, last_name, salary FROM employees WHERE department_id = 30 OR department_id = 70 OR department_id = 80;
    ```
    
- BETWEEN 연산자 (AND와 유사)
    
    ```sql
    SELECT employee_id, department_id, last_name, salary FROM employees WHERE salary BETWEEN 10000 AND 15000;
    ```
    
- IN 연산자 (OR과 유사)
    
    ```sql
    SELECT employee_id, department_id, last_name, salary FROM employees WHERE department_id IN (30, 70, 80);
    ```
    
- LIKE 연산자
    - 퍼포먼스가 나쁘므로, 가능한 쓰지 말것을 추천
    - % : ex. 김% → 김, 김밥, 김가네, 김밥천국 을 출력
    - _ : ex. 김_ → 김, 김밥 을 출력
    
    ```sql
    SELECT employee_id, last_name FROM employees WHERE last_name LIKE '%e%'; //이름에 e가 포함된 모든 사원들의 사번, 이름 출력 
    ```
    
- NULL 관련 연산자
    - NULL은 아무 값도 아니라는 뜻이므로, 다른 연산자와 사용하면 연산 자체가 안됨 → NULL과 관련된 연산자를 사용해야함
    
    ```sql
    //commission_pct가 NULL이 아닌 사원들만 출력하기 (IS NOT NULL)
    SELECT employee_id, commission_pct FROM employees WHERE commission_pct IS NOT NULL;
    
    //commission_pct가 NULL인 사원들만 출력하기 (IS NULL)
    SELECT employee_id, commission_pct FROM employees WHERE commission_pct IS NULL;
    ```
    
- 집합 연산자
    - 합집합 : UNION (중복부분 미출력), UNION ALL (중복부분도 출력)
    - 교집합 : INTERSECT
    - 차집합 : MINUS

```sql
--이전 직무 정보
SELECT *
FROM job_history
ORDER BY employee_id, start_date;

--현재 직무 정보
SELECT employee_id, job_id
FROM employees
ORDER BY employee_id;

//UNION, UNIONALL 연산자 (합집합)
--현재&이전 직무 정보 (두 table의 column 수, 자료형이 같아야함, null은 괜찮음)
SELECT employee_id, job_id, start_date, end_date
FROM job_history
UNION ALL -- 중복부분을 제외하려면 UNION 사용
SELECT employee_id, job_id, null, null
FROM employees ORDER BY employee_id, start_date;

//INTERSECT 연산자 (교집합)
--이전직무를 현재직무로 다시 담당하는 사원 (두 table의 column 수, 이름이 같아야함)
SELECT employee_id, job_id
FROM job_history
INTERSECT
SELECT employee_id, job_id
FROM employees;

//MINUS 연산자 (차집합)
--이전직무를 현재직무로 다시 담당하는 사원 (두 table의 column 수, 이름이 같아야함)
SELECT employee_id, job_id
FROM job_history
MINUS
SELECT employee_id, job_id
FROM employees;

```

### 3. 함수 (function)

- 단일 함수
    - 1개 행씩 계산하여 각각의 행으로 결과를 리턴
    - 조건문
    
    ```sql
    //DECODE() 함수 - JAVA의 if~else 구문과 유사
    -- commission_pct가 null이면 '수당없음', 나머지는 commission_pct를 출력 
    SELECT DECODE(commission_pct, null, '수당없음', TO_CHAR(commission_pct)) FROM employees; 
    -- commission_pct가 null이면 '수당없음', 0.1이면 'A', 나머지는 commission_pct를 출력 
    SELECT DECODE(commission_pct, null, '수당없음', 0.1, 'A', TO_CHAR(commission_pct)) FROM employees;
    
    // CASE절 : 범위 비교에 유리
    SELECT commission_pct, CASE WHEN commission_pct IS NULL THEN '수당없음'
    														WHEN commission_pct >= 0.4 THEN 'D'
    														WHEN commission_pct >= 0.3 THEN 'C'
    														WHEN commission_pct >= 0.2 THEN 'B'
    														WHEN commission_pct >= 0.1 THEN 'A'
    											 END
    FROM employees;
    ```
    
    - 숫자형 함수
    
    ```sql
    //MOD() 함수
    SELECT 3/2, MOD(3, 2) FROM dual; // 3을 2로 나눈 나머지값 1이 출력. *dual : 테스트용 테이블
    //ROUND() 함수
    SELECT 3/2, ROUND(3/2), ROUND(1.4567, 2) FROM dual; // 3/2의 계산결과(1.5)를 반올림한 2 & 1.4567을 소수점이하 2번째자리까지 반올림한 1.46이 출력 
    //TRUNC() 함수
    SELECT 3/2, TRUNC(3/2), TRUNC(1.2345, 2) FROM dual; // 3/2의 계산결과(1.5)를 버림한 1 & 1.2345를 소수점이하 2번째자리까지 버림한 1.23이 출력
    ```
    
    - 문자형 함수
    
    ```sql
    //REPLACE() 함수
    SELECT REPLACE('이것이 자바다', '자바', '오라클') FROM dual; // '자바'를 '오라클로 바꿈
    //TRANSLATE() 함수
    SELECT TRANSLATE('이것이 자바다', '자바', '오라클') FROM dual; // '자'를 '오' 로, '바'를 '라'로 바꿈
    //INSTR() 함수
    SELECT INSTR('가나다가나다', '나') FROM dual; // 2 (가-1, 나-2, 다-3 ...)
    //SUBSTR() 함수
    SELECT SUBSTR('가나다라마바', 2, 3) FROM dual; // 나다라 ('나' 부터 3글자)
    //LPAD() 함수
    SELECT LPAD('abc', 10, '@') FROM dual; // 결과 : @@@@@@@abc
    //TRIM() 함수
    SELECT TRIM(BOTH 'ㅋ' FROM 'ㅋㅋㅋㅋㅋㅋ김가네ㅋㅋㅋㅋㅋㅋ') FROM dual; // 문자열 양옆의 'ㅋ' 제거
    ```
    
    - 일자, 시간 함수 (한국판으로 설치해서 연월일로 해석)
    
    ```sql
    // 일자 +- 숫자 = 일자 ex. TO_DATE('2022-5-18') + 10 = 2022.05.28
    // 일자 - 일자 = 숫자 ex. TO_DATE('2022/5/20') - TO_DATE('2022/5/18') = 2
    
    //SYSDATE 함수
    SELECT SYSDATE FROM dual; // 년월일 값 반환
    SELECT TO_CHAR(SYSDATE, 'yyyy-mm-dd DAY am HH:MI:SS') FROM dual; // 년월일 요일 & 시(12시간패턴)분초 값 반환
    SELECT TO_CHAR(SYSDATE, 'yyyy-mm-dd DAY HH24:MI:SS') FROM dual; // 년월일 요일 & 시(24시간패턴)분초 값 반환
    
    //TO_DATE() 함수
    SELECT TO_DATE('18/5/2022', 'dd/mm/yyyy') FROM dual; // 오라클이 일, 월, 년 으로 인식하도록 설정
    
    //LAST_DAY() 함수
    SELECT LAST_DAY(TO_DATE('2022/02/01')) FROM dual; // 해당 달의 마지막 일자 반환 (2022/2/28)
    
    //NEXT_DAY() 함수
    SELECT NEXT_DAY(TO_DATE('2022/12/25'), '일요일') FROM dual; // 2022년 12월 25일 기준으로 돌아오는 일요일이 언제인지 반환
    
    //MONTH_BETWEEN() 함수
    SELECT MONTHS_BETWEEN(TO_DATE('22/02/18'), TO_DATE('22/05/18')), MONTHS_BETWEEN(TO_DATE('22/05/18'), TO_DATE('22/02/18')) FROM dual;
    // 달과 달 사이의 개월 수 반환
    
    //ADD_MONTH() 함수
    SELECT ADD_MONTHS(SYSDATE, 1), ADD_MONTHS(SYSDATE, -1) FROM dual; // 각각 한달전, 한달후 반환
    ```
    
    - NULL과 관련된 함수 : NVL(NULL값을 연산하기 위해 숫자를 셋팅)
    
    ```sql
    //NVL() 함수
    SELECT employee_id "사번", NVL(commission_pct, 0) "수당률", salary + salary * NVL(commission_pct, 0) "실급여" 
    FROM employees ORDER BY 실급여 ASC; //commission_pct가 NULL일 경우 0으로 간주하여 계산함
    
    //NVL2() 함수 : 첫번째 인자값이 null이 아닌경우 두번째 인자값을 반환하며, NULL인 경우 세번째 인자값을 반환
    SELECT NVL2(commission_pct, '수당있음', '수당없음') FROM employees; // commission_pct가 NUll이면 '수당없음', NULL이 아니면 '수당있음' 출력
    
    //NULLIF() 함수 : 첫번째와 두번째 인자값이 같으면 NULL을 반환, 같지 않으면 첫번째 인자값이 반환
    SELECT NULLIF(10, 10) FROM dual; // NULL
    SELECT NULLIF(10, 20) FROM dual; // 10
    
    ```
    
    - 형변환 함수
    
    ```sql
    // 형변환 가능 여부
    -- 숫자형 <--O--> 문자형 문자->숫자 : TO_NUMBER(), 숫자->문자 : TO_CHAR()
    -- 날짜형 <--O--> 문자형 문자->날짜 : TO_DATE(), 날짜->문자 : TO_CHAR()
    -- 숫자형 <--X--> 날짜형 
    
    // 문자형  ---> 숫자형 : TO_NUMBER() 
    SELECT TO_NUMBER('1,234.5', '9,999.9') FROM dual; --숫자 1234.5
    SELECT TO_NUMBER('1,234.5', '9,999.999') FROM dual; --숫자 1234.5
    SELECT TO_NUMBER('1,234.5', '9,999') FROM dual;--X 자리수 모자람
    SELECT TO_NUMBER('1,234,567.8', '9,999.9') FROM dual;--X 자리수 모자람
    
    // 숫자형  ---> 문자형 : TO_CHAR() '0':자리수고정, '9':자리수고정안함
    SELECT TO_CHAR(1234.5, '99,999.9') FROM dual; -- 문자 '1,234.5'
    SELECT TO_CHAR(123.4, '99,999.990') FROM dual; -- 문자 '123.400'
    SELECT TO_CHAR(123.45, '0,000.990') FROM dual; --문자 '0,123.450'
    SELECT TO_CHAR(1234.5, 'L99,999.9') FROM dual; -- 문자 '￦1,234.5' ('L' : 통화기호)
    
    //자동 형변환 (예시) - 왠만하면 자료형을 명확하게 명시하는게 좋음
    SELECT * FROM employees WHERE department_id = '050';  // 문자형 '050'은 숫자형 50으로 자동 형변환됨
    SELECT * FROM employees WHERE hire_date = '04/07/18'; // 문자형 '04/07/18'은 날자형으로 자동 형변환됨
    ```
    
- 집계함수
    - 복수의 행을 계산하지만, 결과는 1개 행으로만 리턴됨
    - GROUP BY절에서 사용한 column만 집계함수와 함께 SELECT절에서 사용가능
    
    ```sql
    //SUM(), AVG()
    SELECT SUM(salary), AVG(salary) FROM employees; // employees table의 모든 직원의 salary의 합, 평균(null이 있는 행은 제외하고 계산)이 출력 
    //COUNT()
    SELECT COUNT(commission_pct), COUNT(*) FROM employees; // null이 없는 행의 수, (null을 포함한) 전체 행의 수 출력
    //MIN(), MAX()
    SELECT MIN(commission_pct), MAX(commission_pct) FROM employees;
    
    //GROUP BY와 집계함수
    SELECT COUNT(*), department_id FROM employees GROUP BY department_id; //가능
    SELECT COUNT(*) ,employee_id, department_id FROM employees GROUP BY department_id; //불가능
    ```
    

### 4. 정렬 (sort)

```sql
//오름차순 정렬
SELECT employee_id, salary FROM employees ORDER BY salary ASC; // salary를 기준으로 오름차순 정렬
SELECT employee_id "사번", salary "급여", NVL(commission_pct, 0) "수당률", salary + (salary * NVL(commission_pct, 0)) "실급여" 
FROM employees ORDER BY 4 ASC; // 4번째 index (실급여)를 기준으로 오름차순 정렬, *4 대신 실급여(별칭)를 입력해도 같은 결과가 출력됨 

//내림차순 정렬
SELECT employee_id, salary FROM employees ORDER BY salary DESC; // salary를 기준으로 내림차순 정렬

//여러개의 조건을 가진 자료의 정렬
SELECT employee_id "사번", hire_date "입사일", '$'||salary "급여", TRUNC(SYSDATE - hire_date, 1)||'일' "근무일수"
FROM employees
ORDER BY 근무일수 DESC, salary ASC; // 근무일수를 내림차순으로 정렬, 단 근무일수가 같으면 급여가 적은 순으로 정렬

//ROWNUM : 각 행을 insert할 때 발급되는 index (실제 column이 아님), 초기값 = 1
SELECT ROWNUM, employee_id FROM employees WHERE ROWNUM <= 5 ORDER BY employee_id;

//WHRE에서 ROWNUM을 2 이상부터 시작할 경우, 출력되는 행이 없는 이유
SELECT ROWNUM, employee_id, salary FROM employees WHERE ROWNUM BETWEEN 2 AND 10 ORDER BY salary desc;
//ROWNUM은 초기값이 1이고, 1씩 증가함. 그러므로 ROWNUM이 1인 행이 없게됨. -> ROWNUM이 2,3,4... 인 행도 있을 수 없음
```

### 5. Group 처리

```sql
SELECT COUNT(*), department_id FROM employees GROUP BY department_id; //department_id에 따라 사원들을 grouping
	
SELECT COUNT(*) "인원수", AVG(salary) "평균", MAX(salary) "최대급여", MIN(salary) "최소급여" 
FROM employees GROUP BY department_id HAVING AVG(salary) >= 10000; //salary가 10000 이상인 department_id들만 grouping

SELECT department_id, job_id, AVG(salary) FROM employees 
GROUP BY department_id, job_id ORDER BY department_id DESC; //department_id-job_id로 grouping함

SELECT department_id, job_id, SUM(salary) FROM employees 
GROUP BY ROLLUP(department_id, job_id); //ROLLUP() 함수 : department_id 別 subtotal도 같이 출력

SELECT department_id, job_id, SUM(salary) FROM employees 
GROUP BY CUBE(department_id, job_id); //CUBE() 함수 : 계산 가능한 모든 조합 別 subtotal도 같이 출력
```

### 6. JOIN 구문

- table 여러개의 자료를 동시에 조회할 때 사용 가능
- JOIN의 종류
    - 조건에 따른 분류
        - EQUI JOIN : = 로 비교하는 JOIN
        - NON EQUI JOIN : = 이외의 연산자로 비교하는 JOIN
    - 형태에 따른 분류
        - INNER JOIN : 조건에 맞는 행만 검색 (ex. 사원의 사번, 이름, 급여, 부서번호 를 출력)
            - NATURAL JOIN
            - JOIN USING
            - JOIN ON
        - OUTER JOIN : 조건에 맞지 않은 행도 같이 검색 (ex. 사원의 사번, 이름, 급여, 부서번호를 출력, 부서번호가 없는 사원도 포함)

```sql
//INNER JOIN
--2개 table을 JOIN하기 : 사원의 사번, 직무번호, 직무이름 출력
//1. NATURAL JOIN 구문
SELECT employee_id, job_id, job_title FROM employees NATURAL JOIN jobs;
//2. JOIN .. USING 구문
SELECT employee_id, job_id, job_title FROM employees JOIN jobs USING (job_id); --employees, job table에서 중복되는 job_id column을 ()로 묶음
//3. JOIN .. ON 구문
SELECT employee_id, jobs.job_id, job_title FROM employees JOIN jobs ON (employees.job_id = jobs.job_id);
SELECT employee_id, j.job_id, job_title FROM employees e JOIN jobs j ON (e.job_id = j.job_id); --table에 별칭 주기로 글자 수 줄임

-- *column 이름이 같다고 항상 같은 정보를 담고 있는 것이 아님 -> NATURAL JOIN, JOIN USING 보다 JOIN ON이 더 안전한 방법

--3개 table을 JOIN하기 : 사원의 사번, 이름, 부서번호, 부서명, 직무번호, 직무이름 출력
SELECT e.employee_id, e.first_name, e.department_id, d.department_name, j.job_id, j.job_title
FROM employees e JOIN departments d ON (e.department_id = d.department_id) JOIN jobs j ON (e.job_id = j.job_id);

--self JOIN (같은 table에 저장되어있는 column을 JOIN함) : 사원의 사번, 이름, 관리자번호, 관리자이름 출력
SELECT e.employee_id "사번", e.first_name "사원의 이름", m.employee_id "관리자번호", m.first_name "관리자이름"
FROM employees e JOIN employees m ON (e.manager_id = m.employee_id);

//OUTER JOIN
SELECT employee_id, first_name, salary, e.department_id
FROM employees e LEFT OUTER JOIN departments d ON (e.department_id = d.department_id); 
-- 기준이 되는 column이 왼쪽에 있으면 LEFT OUTER JOIN, 오른쪽에 있으면 RIGHT OUTER JOIN, 양쪽 모두를 기준으로 하려면 FULL OUTER JOIN
```

### 7. Sub Query

- query 문 안의 query
- 위치에 따른 명칭
    - SELECT 절에 오는 sub query : Scalar query
    - FROM 절에 오는 sub query : Inline view
    - WHERE 절에 오는 sub query : Sub query
    
    ```sql
    //Scalar query (SELECT절에 오는 sub query)
    //사원의 사번, 부서번호, 부서명을 출력
    SELECT employee_id, department_id, 
    	(SELECT department_name FROM departments WHERE departments.department_id = employees.department_id)
    FROM employees;
    -- main query의 FROM절, SELECT절의 employee_id, department_id, sub query 順으로 연산
    
    //Inline view (FROM절에 오는 sub query)
    //급여가 적은 순으로 5명의 사번, 급여를 출력
    SELECT employee_id, salary
    FROM (SELECT * 
          FROM employees 
    			ORDER BY salary)
    WHERE ROWNUM <= 5;
    -- 급여를 오름차순으로 정렬시킨 후(sub query), 5명의 사원을 출력(main query)
    
    //급여가 적은 순으로, 11번째부터 20번째 사원의 사번, 급여를 출력
    SELECT *
    FROM (SELECT rownum r, employee_id, salary
        FROM (SELECT * 
            FROM employees ORDER BY salary)
        )
    WHERE r BETWEEN 11 AND 20;
    ```
    
- sub query가 반환하는 行 수에 따른 구분
    - 단일행 sub query : main query와 비교할때 일반비교연산자(=,<>,<,>,<=,>=) 사용
    - 다중행 sub query : main query와 비교할때 ANY, ALL, IN, EXIST 연산자 사용
        - IN : 같은 값을 찾음
        - >ANY, <ALL : 최소값을 찾음
        - <ANY, >ALL : 최대값을 찾음
        - EXIST :  sub query의 값이 있을 경우 반환
    
    ```sql
    //단일행 sub query : 사원들중 최대급여를 받는 사원의 사번, 급여를 출력
    SELECT employee_id, salary FROM employees WHERE salary = (SELECT MAX(salary) FROM employees);
    
    //다중행 sub query : 부서별 최대급여를 받는 사원의 부서번호와 사번, 급여를 출력
    SELECT department_id, employee_id, salary FROM employees WHERE (department_id, salary) IN  
    (SELECT department_id, MAX(salary) FROM employees GROUP BY department_id);                                       
    -- main query의 department_id, salary, sub query의 department_id, MAX(salary)를 쌍맞춤(pairwise)해야 부서 당 1명씩만 출력됨
    
    //*참고 : 상호연관 sub query : main query에서 사용한 부분을 sub query에서 재사용하는 방법
    --성이 'Davies'인 사원과 같은 부서에 근무하면서 부서평균급여보다
    --많은 급여를 받는 사원의 사번, 성, 이름, 급여를 출력
    SELECT department_id "부서ID", employee_id "사번", last_name "성", first_name "이름", salary "급여"
    FROM employees e
    WHERE department_id = (SELECT department_id 
                           FROM employees 
                           WHERE last_name = 'Davies')
    AND salary >  (SELECT AVG(salary)
                   FROM employees 
                   WHERE department_id = e.department_id) -- main query 에서 이미 Davies가 있는 department_id를 걸러냈음
    ORDER BY salary;
    ```
    

### 8. 계층형 쿼리

```sql
SELECT LEVEL, board_post_no, board_parent_no 
FROM board
START WITH board_parent_no = 0
CONNECT BY PRIOR board_post_no = board_parent_no
ORDER SIBLINGS BY board_post_no DESC;

--result
1	3	0
2	7	3
1	1	0
2	4	1
3	6	4
2	2	1
3	5	2
```

## DDL (Data Definition Language)

- 객체생성(CREATE), 객체의 구조 변경(ALTER), 객체 제거에 사용(DROP)
- 무결성 제약조건(integrity contraint)을 설정할 수 있음
    - NOT NULL - 해당 column에 NULL 값을 저장할 수 없음 (=보조식별자)
    - UNIQUE - 해당 column에 중복된 값을 저장할 수 없음 (=보조식별자)
    - PRIMARY KEY - 해당 column에 NULL값 또는 중복된 값을 저장할 수 없음 (=주식별자)
    - CHECK - 해당 column에 입력가능/불가능한 값 지정 가능 (ex. 20000 이상의 숫자 입력 불가, 첫글자가 무조건 a로 시작해야함)
    - FOREIGN KEY - 다른 table에 있는 column의 값만 참조할 수 있음 (ex. employees table의 job_id는 jobs table의 job_id의 값만 참조 가능), 참조하는 쪽을 child entity, 참조되는 쪽을 parent entity라 부름
        - 참조에 대한 더 자세한 설명은 [https://inpa.tistory.com/entry/DB-📚-데이터-모델링-1N-관계-📈-ERD-다이어그램#ERD_(Entity_Relationship_Diagram)](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81-1N-%EA%B4%80%EA%B3%84-%F0%9F%93%88-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8#ERD_(Entity_Relationship_Diagram)) 참고
- 제약조건의 작성방법
    - 칼럼레벨 제약조건
    - 테이블레벨 제약조건

```sql
//객체 생성 (table 객체 기준)
CREATE TABLE product(
    product_no VARCHAR2(5), 
    product_name VARCHAR2(30), 
    product_price NUMBER(6),
    product_info VARCHAR2(100),
    );

//객체의 구조 변경
ALTER TABLE product
ADD product_mfd DATE -- append column
ADD CONSTRAINT product_no_pk PRIMARY KEY(product_no) -- 제약조건 추가 (NOT NULL 제약조건은 MODIFY에서 추가 가능)
RENAME COLUMN product_mfd TO product_mfdate -- column 이름 바꾸기
MODIFY product_mfdate VARCHAR2(10) NOT NULL -- data type or 자리수, 제약조건 변경
DROP COLUMN product_nfdate; -- column 지우기
DROP CONSTRAINT product_no_pk; -- 제약조건 삭제 (제약조건 이름으로 삭제함)
--*제약조건의 이름은 user_constraints 테이블에서 조회 가능

//객체 제거
DROP TABLE product;
DROP TABLE parent CASCADE CONSTRAINT; -- 참조관계가 있는 다른 테이블이 있으면 관계를 끊고 삭제함.
-- 제약조건이 있는 테이블 강제 삭제
DROP TABLE 테이블 이름 CASCADE CONSTRAINTS PURGE;

//객체의 정보 확인
DESC product;

//제약조건 설정 (column level 제약조건 설정)
//1. NOT NULL (일반적으로 제약조건 이름을 설정하지 않음)
CREATE TABLE product(
    product_no VARCHAR2(5) NOT NULL, -- product_no 는 NULL값이 되어서는 안됨
    product_name VARCHAR2(30), 
    product_price NUMBER(6),
    product_info VARCHAR2(100)
    );
//2. UNIQUE
CREATE TABLE product(
    product_no VARCHAR2(5) CONSTRAINT product_no_uk UNIQUE -- product_no 에 중복된 값을 넣어서는 안됨 (product_no_uk 오류메시지 발생)
    );
//3. PRIMARY KEY
CREATE TABLE product(
    product_no VARCHAR2(5) CONSTRAINT product_no_pk PRIMARY KEY -- product_no 에 NULL or 중복된 값을 넣어서는 안됨 (product_no_pk 오류메시지 발생)
    );
//4. CHECK
CREATE TABLE product(
    product_price NUMBER(6) CONSTRAINT product_price_ck CHECK(product_price >= 1000) -- product_price는 1000 이상이어야함 (product_no_ck 오류메시지 발생)
    );
//5. FOREIGN KEY
CREATE TABLE parent (
    pk_column VARCHAR2(2) PRIMARY KEY,
    general_column NUMBER
    );
CREATE TABLE child (
    pk_column NUMBER PRIMARY KEY,
    fk_column VARCHAR2(2) CONSTRAINT child_tbl_fk REFERENCES parent(pk_column) -- parent table의 pk_column 참조
    );
INSERT INTO parent (pk_column, general_column) VALUES ('fi', 1);
INSERT INTO parent (pk_column, general_column) VALUES ('se', 2);
INSERT INTO child (pk_column, fk_column) VALUES (2, 'se');
INSERT INTO child (pk_column, fk_column) VALUES (4, 'fo'); -- parent의 pk_column에 'fo'가 없어서 삽입불가 (*참고 : NULL은 삽입가능)

// 테이블레벨 제약조건 * NOT NULL 제약조건은 테이블레벨에서 설정할 수 없음
CREATE TABLE product(
    product_no VARCHAR2(5),
		product_name VARCHAR2(30),
		product_price NUMBER(10),
		product_mfd DATE
		CONSTRAINT product_no_pk PRIMARY KEY(product_no),
		CONSTRAINT product_name_uk UNIQUE(product_name),
    CONSTRAINT product_price_ck CHECK(product_price >= 1000),
		CONSTRAINT product_mfd_fk FOREIGN KEY(product_mfd) REFERENCES other_table(other_column)
    );
```

## DML (Data Manipulation Language)

- 데이터 추가, 수정, 삭제 可能
- DML에 sub query도 사용가능

```sql
//데이터 추가
INSERT INTO product(product_no, product_name, product_price) VALUES ('B0001', '아메리카노', 2000);
INSERT INTO product VALUES ('B0002', '카페라뗴', 2000, null, '');

//데이터 수정
UPDATE customer SET name = '권민호', password = '1234' WHERE id='id1'; -- id가 'id11'인 행의 이름, 패스워드를 변경
UPDATE customer SET status = 1; -- status column의  모든 행의 값을 1로 설정
UPDATE product SET product_price = product_price * 1.1; -- product_price column의 모든 행의 값을 1.1배로 늘림

//데이터 삭제
DELETE FROM customer WHERE id = 'id1'; -- id가 'id1'인 행 삭제

```

## DCL (Data Control Language)

- 권한의 설정, 제거

## TCL (Transaction Control Language)

- transaction의 완료, 복구

# Database 객체

- 종류 : table, view, index, sequence, synonym, procedure, package, function(함수) etc…

## View

- 가상 테이블
- 기본적으로는 데이터를 볼 수만 있고, 데이터의 입력, 수정, 삭제 등은 어려움 (보안상의 이점 有)
    - WITH READ ONLY 구문을 추가하면 데이터의 입력, 수정, 삭제 등이 완전히 불가능해짐
    - WITH CHECK OPTION 구문을 추가하면 제한적으로 데이터의 입력, 수정, 삭제 등이 가능해짐
- 길고 복잡한 SQL구문을 view로 저장하여 여러 곳에서 재사용 가능 (네트워크 프로그래밍을 할 때 네트워크 비용을 줄일 수 있음)
- view에는 SELECT 구문이 저장되어있음 (실제 데이터가 저장되는 것이 아님)
- view의 구조를 변경하려면 CREATE OR REPLACE VIEW view_name 구문을 사용 (ALTER VIEW 불가능)

```sql
--길고 복잡한 SQL구문을 view로 저장하기 예시
CREATE VIEW a_view
AS 
SELECT d.department_id, d.department_name, j.job_id, j.job_title, 
           count(employee_id) "employee_cnt"
FROM employees e JOIN departments d ON (e.department_id=d.department_id)
                           JOIN jobs j ON (e.job_id = j.job_id)
WHERE salary >= 3000
GROUP BY d.department_id, d.department_name, j.job_id, j.job_title
HAVING count(employee_id)>=2
ORDER BY count(employee_id);
SELECT * FROM a_view; -- a_view를 검색하기

--view의 내용 변경
CREATE OR REPLACE VIEW a_view
AS 
SELECT d.department_id, j.job_id, 
           count(employee_id) "employee_cnt"
FROM employees e JOIN departments d ON (e.department_id=d.department_id)
                           JOIN jobs j ON (e.job_id = j.job_id)
WHERE salary >= 3000
GROUP BY d.department_id, d.department_name, j.job_id, j.job_title
HAVING count(employee_id)>=2
ORDER BY count(employee_id);

-- DML 처리 불가능하게 설정 (WITH READ ONLY)
CREATE OR REPLACE VIEW view_read_only
AS SELECT employee_id FROM employees
WITH READ ONLY; 

-- 조건을 만족하는 DML의 처리가 가능한 뷰
CREATE OR REPLACE VIEW view_condition
AS SELECT a, b FROM b_tble WHERE b=10;
WITH CHECK OPTION; -- WHERE절 에서의 조건을 만족하면 DML 처리가 가능해짐
INSERT INTO view_condition(a, b) VALUES (1, 10); -- b의 값이 10 -> 조건 만족 & 삽입 가능
INSERT INTO view_condition(a, b) VALUES (2, 11); -- b의 값이 10이 아님 -> 조건 불만족 & 삽입 불가능

--view 삭제
DROP VIEW a_view;
```

## Index

- 생성지침
    - 만들어야 하는 경우
        - 열에 광범위한 값이 포함된 경우
        - 열에 많은 null 값이 포함된 경우
        - 하나 이상의 열이 WHERE 절이나 JOIN 조건에서 함께 자주 사용되는 경우
        - 테이블이 크고 대부분의 query가 테이블에서 2~4% 미만의 행을 검색할 것으로 예상되는 경우
    - 만들지 말아야하는 경우
        - column이 query에서 조건으로 자주 사용되지 않는 경우
        - 테이블이 작거나 대부분의 query가 테이블에서 2~4% 이상의 행을 검색할 것으로 예상되는 경우
        - 테이블이 자주 갱신되는 경우
        - 인덱스화된 column이 표현식의 일부로 참조되는 경우

## Sequence

- 일련번호 값을 발급해주는 객체
- sub query 에서는 사용 불가
- ALTER를 이용하여 수정할 수 있으나, 권장하지 않음 (삭제 후 재생성 추천)

```sql
-- sequence 만들기 예시
CREATE SEQUENCE order_seq; -- 1부터 1씩 증가 (default)
CREATE SEQUENCE a_seq -- 4부터 시작하여 2씩 증가, 최대값 60에 도달하면 최소값인 3으로 돌아가서 계속 일련번호 발급
START WITH 4 -- 4부터 시작
INCREMENT BY 2 -- 2씩 증가
MAXVALUE 60 -- 최대값은 60
CYCLE -- 최대값에 도달하면 MINVALUE로 돌아감 (CYCLE 설정을 안하면 최대값에 도달했을때 squence에 값 추가 안됨)
MINVALUE 3; -- 최소값은 3

--sequence의 일련변호 값을 얻기
INSERT INTO order_info(order_no, order_id, order_date) 
                VALUES(order_seq.NEXTVAL, 'id1', SYSDATE); -- NEXTVAL : 일련번호 값을 갖는 sequence 객체에서만 쓰이는 pseudo column

--현재 번호 조회
INSERT INTO order_line(order_no, order_product_no, order_quantity)
                VALUES(order_seq.CURRVAL, 'C0001', 7); -- CURRVAL : 일련번호 값을 갖는 sequence 객체에서만 쓰이는 pseudo column 

-- sequence 삭제
DROP SEQUENCE a_seq;
```

## Procedure

- PL/SQL을 수행하는 객체

```sql
//프로시져 예시
// 더하기 연산 수행
CREATE OR REPLACE PROCEDURE a_proc( -- 프로시져 생성
    num1 number, 
    num2 number)
IS num3 number := 0;
BEGIN
  num3 := num1 + num2;
  DBMS_OUTPUT.PUT_LINE(num1 || ' + ' || num2 || ' = ' || num3);
END;
-- 보기 -> DBMS출력 클릭 후 창 하단에 DBMS출력이 뜨면 '+'를 클릭하고 질의작성기와 연결해야함
EXEC a_proc(202, 203); -- 프로시져 실행 (202 + 203 = 405 출력)

// 조건문 (IF)
CREATE OR REPLACE PROCEDURE b_proc( -- 프로시져 생성
    num number)
IS
BEGIN
  IF MOD(num, 2) = 1 THEN 
    DBMS_OUTPUT.PUT_LINE('odd number');
  ELSE
    DBMS_OUTPUT.PUT_LINE('even number');   
  END IF;
  IF num > 10 THEN
    DBMS_OUTPUT.PUT_LINE('num > 10');  
  ELSIF num > 5 THEN
     DBMS_OUTPUT.PUT_LINE('5 < num <= 10');  
  ELSE
      DBMS_OUTPUT.PUT_LINE('num <= 5'); 
  END IF;
END;
EXEC b_proc(202); -- 프로시져 실행 (even number / num > 10 출력)

// 반복문 (FOR LOOP)
CREATE OR REPLACE PROCEDURE c_proc
IS
BEGIN
  FOR i IN 1..10 LOOP
    DBMS_OUTPUT.PUT_LINE(i);
  END LOOP;
END;
EXEC c_proc(); -- 프로시져 실행 (1 ~ 10까지의 숫자가 출력)

// 테이블 조회
CREATE OR REPLACE PROCEDURE d_proc(v_department_id employees.department_id%TYPE) -- v_department_id의 data type은 employees의 department_id의 것과 같음
IS v_sum NUMBER;
   v_department_name departments.department_name%TYPE;
BEGIN
    SELECT SUM(salary) INTO v_sum -- v_sum = SUM(salary)
    FROM employees
    WHERE department_id = v_department_id;
    DBMS_OUTPUT.PUT_LINE(v_department_id || ' 부서의 급여합은 ' || v_sum);

    SELECT department_name INTO v_department_name
    FROM departments 
    WHERE department_id = v_department_id;
    DBMS_OUTPUT.PUT_LINE(v_department_id || ' 부서의 이름은 ' || v_department_name);
END;
EXEC d_proc(50);

// 테이블 조회 2 - CURSOR 사용
CREATE OR REPLACE PROCEDURE e_proc(v_salary employees.salary%TYPE)
IS v_salary2 employees.salary%TYPE;
   CURSOR cursor1 IS  -- cursor1 이라는 이름의 cursor 생성
        SELECT *
        FROM employees
        WHERE salary > v_salary;
BEGIN
    FOR e IN cursor1 LOOP
        DBMS_OUTPUT.PUT_LINE('사번 : ' || e.employee_id || ', 급여 : ' ||e.salary);
    END LOOP;
END;
EXEC e_proc(5000);
```

## Function

- Procedure 처럼 PL/SQL에서 사용
- RETURN 값을 가진다는 것이 Procedure와의 차이

```sql
//Function 예시
--덧셈 결과를 table의 row로 출력
CREATE OR REPLACE FUNCTION a_func(num1 number, num2 number)
RETURN number --return할 data type을 설정
IS -- 필요한 지역변수선언
    num3 number;
BEGIN
    num3 := num1 + num2;
    RETURN num3;
END;
/
SELECT a_func(1,2) FROM dual;

--페이지의 시작행을 table에 출력
CREATE OR REPLACE FUNCTION start_row(current_page number, cnt_per_page number) -- 현재페이지 번호 & 한 페이지 당 보여줄 row 수
RETURN number --return할 data type을 설정
IS -- 필요한 지역변수선언
    start_row number;
BEGIN
    start_row := (current_page - 1) * cnt_per_page + 1;
    RETURN start_row;
END;
/
SELECT start_row(1, 10), -- 1 : 1페이지의 시작 행 (페이지당 10개 행 존재)
       start_row(2, 10), -- 11 : 2페이지의 시작 행 (페이지당 10개 행 존재)
       start_row(2, 4), -- 5 : 2페이지의 시작 행 (페이지당 4개 행 존재)
       start_row(3, 4), -- 9 : 3페이지의 시작 행 (페이지당 4개 행 존재)
       start_row(10, 5) -- 46 : 10페이지의 시작 행 (페이지당 5개 행 존재)      
FROM dual;

--페이지의 마지막행을 table에 출력
CREATE OR REPLACE FUNCTION end_row(current_page number, cnt_per_page number) -- 현재페이지 번호 & 한 페이지 당 보여줄 row 수
RETURN number --return할 data type을 설정
IS -- 필요한 지역변수선언
    end_row number;
BEGIN
    end_row := (current_page * cnt_per_page); 
    RETURN end_row;
END;
/
SELECT end_row(1, 10), -- 10 : 1페이지의 마지막 행 (페이지당 10개 행 존재)
       end_row(2, 10), -- 20 : 2페이지의 마지막 행 (페이지당 10개 행 존재)
       end_row(2, 4), -- 8 : 2페이지의 마지막 행 (페이지당 4개 행 존재)
       end_row(3, 4), -- 12 : 3페이지의 마지막 행 (페이지당 4개 행 존재)
       end_row(10, 5) -- 50 : 10페이지의 마지막 행 (페이지당 5개 행 존재)      
FROM dual;
```

## Trigger

- PL/SQL에서 사용되는 객체
- 한 table에 DML (INSERT, UPDATE or DELETE) 실행시 자동으로 다른 table에 Trigger의 내용이 실행되도록 한다.

```sql
--Trigger 예시

--point 테이블 생성
CREATE TABLE point(
    point_id VARCHAR2(5) PRIMARY KEY,
    point_score NUMBER(3),
    CONSTRAINT point_id_fk FOREIGN KEY(point_id) REFERENCES customer(id)
);

-- point_trigger 트리거 생성 (customer 테이블에 행이 추가될 때마다 point테이블에 작업 실행)
CREATE OR REPLACE TRIGGER point_trigger
AFTER INSERT ON customer -- customer table에 행을 INSERT 한 직후 
FOR EACH ROW -- 각각의 行別로 실행
BEGIN
    INSERT INTO point(point_id, point_score) VALUES (:NEW.id , 1); -- :NEW의 뜻 - 방금 추가된 customer table의 id column의 행     
END;

INSERT INTO customer VALUES ('id2', 'p1', 'n1', 'a1', 1); -- customer table에 행 추가 (point table에도 트리거가 실행될 것임)
SELECT * FROM point;

-- point1_trig 트리거 생성 (order_info 테이블에 행이 추가될 때마다 point테이블에 작업 실행)
CREATE OR REPLACE TRIGGER point1_trig
AFTER INSERT ON order_info -- order_info table에 행을 INSERT 한 직후
FOR EACH ROW -- 각각의 行別로 실행
BEGIN
    UPDATE point SET point_score = point_score + 1 WHERE point_id = :NEW.order_id; -- :NEW의 뜻 - 방금 추가된 order_info table의 order_id column의 행 
END;

INSERT INTO order_info(order_no, order_id, order_date) VALUES (5, 'id2', SYSDATE); -- order_info table에 행 추가 (point table에도 트리거가 실행될 것임)

--point_delete_trigger 생성 (customer table에서 행이 삭제되기 직전, point table의 해당 고객의 행도 자동 삭제)
CREATE OR REPLACE TRIGGER point_delete_trigger
BEFORE DELETE ON customer -- customer table에 행을 DELETE 하기 직전
FOR EACH ROW -- 각각의 行別로 실행
BEGIN
    DELETE FROM point WHERE point_id = :OLD.id; -- :OLD의 뜻 - 삭제되기 직전의 customer table의 행    
END;

DELETE FROM customer WHERE id = 'id2'; -- customer table의 행 삭제 (point table에도 트리거가 실행될 것임)
```

# PL/SQL

- PL/SQL은 SQL, 변수선언, 조건처리, 반복처리가 가능 (SQL에서는 불가능)

//흐름
INSERT/UPDATE/DELETE ----송신--- > DBMS

                                   프로시져 / 함수 / 트리거
                                   a_proc   a_func   a_trig
           exec a_proc ---송신--->  INSERT/UPDATE/DELETE --- 
           SELECT a_func() ---송신--->
           FROM;

# 트랜잭션 (Transaction)

- 업무의 실행단위 / 논리의 명령단위를 뜻함
    - 업무의 실행단위 예시 : 계좌 이체 / 명령어 실행단위 예시 : 송금자의 계좌 차감 & 수금자의 계좌 증가 등 계좌 이체의 세부과정
- locking
    - 한 트랜잭션이 진행되는 동안 다른 작업이 끼어들지 못하도록 lock을 거는것 (트랜잭션 과정을 끝내려면 COMMIT / ROLLBACK을 해야함)
- 교착상태 (Dead lock)
    - 두개 이상의 트랜잭션이 특정 자원(table or row)의 Lock을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않음 → 교착상태라 부름
- 특성 ([ACID](https://ko.wikipedia.org/wiki/ACID))
    - Atomicity(원자성)
    - Consistency(일관성)
    - Isolation(독립성)
    - Durability(지속성)

```sql
// 트랜잭션 예시
-- account table 생성
  CREATE TABLE account (
    no CHAR(3) PRIMARY KEY, 
    balance NUMBER(10)
  );
-- 계좌 이체 성공 - COMMIT (작업한 내용을 적용시키기)
  INSERT INTO account(no, balance) VALUES ('101', 1000); 
  INSERT INTO account(no, balance) VALUES ('102', 1000);
  COMMIT; 

--계좌 이체 실패 - ROLLBACK (진행 중인 작업을 취소하고 초기상태로 되돌림)
  UPDATE account SET balance = balance - 10000 WHERE no = '101';
  UPDATE account SET balance = balance + 10000 WHERE no = '102';
  ROLLBACK;
  
  --DML(INSERT. UPDATE. DELETE)실행시 트랜잭션 자동시작, 트랜잭션 자동완료안됨
  --트랜잭션완료 명령어 : COMMIT. ROLLBACK.
  
  --DDL(CREATE, ALTER, DROP)실행시 트랜잭션 자동시작, 트랜잭션 자동완료됨
  --하지만 DML처럼 COMMIT. ROLLBACK.을 할 수 없어 실제 데이터에 반영은 안됨.
```

# Entity Relationship Diagram (ERD)

- 개념적 모델링을 수행 (실제로 데이터베이스를 만들지는 않지만 어떻게 만들어야하는지 스케치를 하는 작업)
- exERD : ERD를 그릴 수 있는 프로그램 (이클립스에 연결하여 사용 가능)
    - FORWARD ENGINEERING : ERD를 sql로 변환
    - REVERSE ENGINEERING : sql을 ERD로 변환
- 관계선 긋기

![식별자, 비식별자관계](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1969355d-d689-412b-8c9a-79a657a92007/Untitled.png)

식별자, 비식별자관계

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9555ee1f-61a6-4c55-9f0f-7525285d2ae2/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5791e58a-ff6c-4fe3-acf9-d7e7824b106f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f055292-1703-4f64-8b1a-a3e88ccf4977/Untitled.png)

## 정규화 (normalization)

- [https://en.wikipedia.org/wiki/Database_normalization#Satisfying_1NF](https://en.wikipedia.org/wiki/Database_normalization#Satisfying_1NF)

# JDBC프로그래밍 - JAVA와 DB의 연동

- oracle database와 java와의 연결&해제를 담당하는 클래스

```java
public class MyConnection {
	static { // JDBC 클래스 로드는 한번만 하면 더 이상 할 필요가 없으므로 MyConnection 클래스가 로드될 때 자동호출되도록 static 블럭에 넣음
		// 1. JDBC 드라이버 설치
		// -> ojdbc8.jar 다운로드 & build path로 연결
		// 2. JDBC 드라이버 클래스 로드
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver"); // Oracle driver 연결
			System.out.println("Driver와 연결하였습니다");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}		
	}
	
	//db에 connect하기
	public static Connection getConnection() throws SQLException {
		// 3. DB 연결
		Connection con = null;
		String url = "jdbc:oracle:thin:@localhost:1521:xe";
		String user = "hr";
		String password = "hr";
		con = DriverManager.getConnection(url, user, password);
		System.out.println(url + "에 연결하였습니다");
		return con;
	}
	
	//db와의 연결 해제
	public static void close(ResultSet rs, Statement stat, Connection con) {
		// 4.. DB 와의 연결 해제
		if (rs != null) {
			try {
				rs.close(); // ResultSet 닫기
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		if (stat != null) {
			try {
				stat.close(); // Statement 닫기
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		if (con != null) {
			try {
				con.close(); // Connection 닫기
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
	
	//db와의 연결 해제
	public static void close(Statement stat, Connection con) {
		// 4. DB 와의 연결 해제
		close(null, stat, con);
	}
}
```

- 자바에서 DDL, DML구문 사용하기

```java
public class JDBCMyConnectiontest {
	public static void search() { // DB에서 자료 검색하기
		Connection con = null;
		Statement stat = null;
		ResultSet rs = null; // ResultSet의 뜻 : 쿼리 실행 시 출력되는 행,열
		try {
			con = MyConnection.getConnection();
			// SQL문 송신 (SELECT, INSERT/UPDATE/DELETE, CREATE/ALTER/DROP)
			stat = con.createStatement();
			// (executeQuery(), executeUpdate())
			String selectSQL = "SELECT employee_id, first_name, salary, hire_date FROM employees"; // 송신할때는 sql구문 끝에 ;를
																									// 붙이면 안됨
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd hh:mm:ss");
			// 결과 수신 (행(ResultSet), 처리건수(int), 0 )
			rs = stat.executeQuery(selectSQL); // DB에서 검색
			// ex) ResultSet rs = stat.executeQuery("SELECT ~~~"); // 행이 반환됨
			// int rowcnt = stat.executeQuery("INSERT ~~~"); // 처리건수가 반환됨
			// int rowcnt = stat.executeQuery("CREATE ~~~"); // 무조건 0이 반환됨

			// 결과 출력
			while (rs.next()) {
				int employee_id = rs.getInt("employee_id"); // employee_id column의 행 출력, getInt(1)도 가능
				String first_name = rs.getString("first_name"); // first_name column의 행 출력, getString(2)도 가능
				int salary = rs.getInt("salary"); // salary column의 행 출력, getInt(3)도 가능
				java.sql.Date hire_date = rs.getDate("hire_date"); // salary column의 행 출력, getDate(4)도 가능
				System.out.println(employee_id + ", " + first_name + ", " + salary + ", " + sdf.format(hire_date));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			// DB 와의 연결 해제
			MyConnection.close(rs, stat, con);
		}
	}
	
	public static void add() { // DB에 자료 입력하기 (INSERT)
		Connection con = null;
		Statement stat = null;
		try {
			con = MyConnection.getConnection();
			// SQL문 송신 (SELECT, INSERT/UPDATE/DELETE, CREATE/ALTER/DROP)
			stat = con.createStatement();
			// (executeQuery(), executeUpdate())
			String insertSQL = "INSERT INTO customer(id, password, name, address, status) \r\n"
					+ "VALUES ('id7', 'p8', 'n8', 'a8', 1)"; // 송신할때는 sql구문 끝에 ;를 붙이면 안됨
			// 결과 수신 (행(ResultSet), 처리건수(int), 0 )
			int rowcnt = stat.executeUpdate(insertSQL); // 행의 갯수
			System.out.println(rowcnt + "건이 추가 되었습니다.");
			// 결과 출력
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			// DB 와의 연결 해제
			MyConnection.close(stat, con);
		}
	}
}
```

- 트랜잭션

```java
public class TransactionTest {

	public static void main(String[] args) {
		Connection con = null;
		PreparedStatement pstat = null; 
		try {
			con = MyConnection.getConnection();
			con.setAutoCommit(false); // autocommit이 불가능하도록 설정
			String insertInfoSQL = 
					"INSERT INTO order_info(order_no, order_id, order_date)"
					+" VALUES     (order_seq.NEXTVAL,     ?,       SYSDATE)";
			String insertLineSQL = 
					"INSERT INTO order_line(order_no, order_product_no, order_quantity)"
					+" VALUES     (order_seq.CURRVAL,     ?,           ?)";
			pstat = con.prepareStatement(insertInfoSQL);
			pstat.setString(1, "id1");
			pstat.executeUpdate();
			System.out.println("1번째 구문 insert");
			pstat = con.prepareStatement(insertLineSQL);
			pstat.setString(1, "G0001");
			pstat.setInt(2, 10);
			pstat.executeUpdate();
			System.out.println("2번째 구문 insert");			
			pstat.setString(1, "G0001");
			pstat.setInt(2, 10);
			pstat.executeUpdate();	
			System.out.println("3번째 구문 insert");
			con.commit(); // commit하기(transaction완료)
			System.out.println("commit complete");
		} catch (SQLException e) {
			if(con != null) {
				try {
					con.rollback();// insert 중에 문제 발생시 rollback하기 (transaction 취소)
					System.out.println("rollback complete");
	 			} catch (SQLException e1) {
				}
			}
			e.printStackTrace();
		} finally {
			MyConnection.close(pstat, con);
		}
	}
}
```

- batch (일괄처리)

```java
public class BatchTest {
	public static void main(String[] args) {
		Connection con = null;
		PreparedStatement pstat = null;
		
		try {
			String insertSQL = "INSERT INTO a_tble (a) VALUES (?)";
			con = MyConnection.getConnection();
			pstat = con.prepareStatement(insertSQL);
			
//			for (int i = 101; i <= 200; i++) {
//				pstat.setInt(1,  i);
//				pstat.executeLargeUpdate();
//			}
//			pstat.clearBatch();
			for (int i = 101; i <= 200; i++) {
				pstat.setInt(1,  i);
				pstat.addBatch(); // 작업중인 데이터들을 buffer에 쌓아두기
			}
			System.out.println("일괄처리 완료직전");
			pstat.executeBatch();
			System.out.println("일괄처리 완료");
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			MyConnection.close(pstat, con);
		}
	}
}
```

- 분산트랜잭션 : [https://docs.microsoft.com/ko-kr/sql/connect/jdbc/understanding-xa-transactions?view=sql-server-ver16](https://docs.microsoft.com/ko-kr/sql/connect/jdbc/understanding-xa-transactions?view=sql-server-ver16)
