---
layout: post
title: DB2 SQL Tuning Tips
date: 2015-09-27 21:18:24
comments: true
categories: sql
---

List most critical tips from [DB2 SQL Tuning Tips for z/OS Developers](https://www.safaribooksonline.com/library/view/db2-sql-tuning/9780133038484/)

4. Stay Away from Distinct if Possbile
Details: `group by` or `in or exists subquery` if duplicates due to one-to-many relationship

12. Try Rewriting Range Predicates as Between Predicates
{% codeblock lang:sql %}
WHERE HIREDATE > :HV-DATE
WHERE HIREDATE BETWEEN :HV-DATE and '9999-12-31'
{% endcodeblock %}

19. Use Tools for Monitoring
Details: Some of the most common z/OS DB2 monitoring tools are OMEGAMON®, TMon, MainView, Apptune, Insight for DB2, and DB2PM. Some of the most common DB2 LUW monitoring tools are IBM’s Optim™ Query Tuner, Precise for DB2, Quest, DBI Software, Embarcadero’s Performance Analyst, and ITGAIN.

22. Avoid Discrepancies with Non-Column Expressions
{% codeblock lang:sql %}
WHERE EDLEVEL = 123.45 * 12
WHERE EDLEVEL = SMALLINT(ROUND(123.45*12,0))
{% endcodeblock %}

24. Ensure That Subquery Predicates Involving Min and Max Have the Possibility of Nulls Being Returned Handled

25. Always Code For Fetch Only or For Read Only with Cursor Processing When a Query Is Only Selecting Data

26. Avoid Selecting a Row from a Table to Help Decide Whether the Logic in the Code Should Execute an Update or an Insert

82. Use the DB2 V9 MERGE Statement

34. Keep Table and Index Files Healthy and Organized
Details: SYSIBM.SYSTABLESPACESTATS and SYSIBM.SYSINDEXSPACESTATS tables

42. Consider OLTP Front-End Processing
{% codeblock lang:sql %}
OPTIMIZE FOR 14 ROWS
{% endcodeblock %}

44. Take Advantage of Materialized Query Tables to Improve Response Time (Dynamic SQL Only)
{% codeblock lang:sql %}
CREATE TABLE DEPT_SUM_MQT_TABLE AS
(SELECT  WORKDEPT, AVG(SALARY) AS AVG_SAL,
AVG(BONUS) AS AVG_BONUS
AVG(COMM)  AS AVG_COMM,
SUM(SALARY) AS SUM_SAL,
SUM(BONUS) AS SUM_BONUS
SUM(COMM)  AS SUM_COMM
FROM EMP
GROUP BY  WORKDEPT)
DATA INITIALLY DEFERRED REFRESH DEFERRED
MAINTAINED BY SYSTEM
ENABLE QUERY OPTIMIZATION;
--original query
SELECT SUM(BONUS)
FROM EMP
WHERE  WORKDEPT = 'A00'
--rewrite by optimize
SELECT AVG_BONUS
FROM DEPT_SUM_MQT_TABLE
WHERE  WORKDEPT = 'A00'
{% endcodeblock %}

45. Insert with Select
{% codeblock lang:sql %}
SELECT EMPNO, LASTNAME, MIDINIT, FIRSTNME,
DEPTNO, SALARY, BONUS, COMM
FROM FINAL TABLE
(INSERT   (EMPNO, LASTNAME, MIDINIT, FIRSTNME, DEPTNO)
VALUES ('123456', 'SMITH', 'A', 'JOE', 'A00')
)
{% endcodeblock %}

52. Identify Times for Volatile Tables
Details: Using the VOLATILE keyword on the Create or Alter Table statement identifies tables whose data volumes fluctuate.
{% codeblock lang:sql %}
CREATE TABLE EMP_TEMP
 (EMPNO    CHAR(6)  Not Null,
   LASTNAME VARCHAR(15)  Not Null )
   VOLATILE
{% endcodeblock %}

53. Use the ON COMMIT DROP Enhancement
{% codeblock lang:sql %}
DECLARE GLOBAL TEMPORARY TABLE
SESSION.EMP_TEMP1  (COL1 CHAR (20)
COL2 CHAR (10)
COL3 SMALLINT )
ON COMMIT DROP TABLE

ON COMMIT DELETE ROWS
ON COMMIT PRESERVE ROWS
{% endcodeblock %}

54. Use Multiple Distincts
Details: This is an automatic performance gain that alleviates the need for separate calls to DB2.
{% codeblock lang:sql %}
SELECT SUM (DISTINCT SALARY),
     AVG(DISTINCT BONUS)
     FROM EMP;
{% endcodeblock %}

55. Take Advantage of Backward Index Scanning
Details: For each column that is in the Order By clause, the ordering that is specified in the index has to be exactly the opposite of the ordering that is specified in the Order By clause.

57. Set Your Clustering Index Correctly

67. Take Advantage of DB2 V8 Enhanced DISCARD Capabilities When It Comes to Mass Deletes

71. Consider Parallelism
https://www-01.ibm.com/support/knowledgecenter/SSEPEK_11.0.0/com.ibm.db2z11.doc.perf/src/tpc/db2z_enableparallelprocess.dita

73. Consider Direct Row Access Using ROWID Datatype (V8) or RID Function (V9)
Details: If the ROWID value is then used in the search condition of subsequent updates or deletes, DB2 may navigate directly to the row, bypassing any index processing or tablespace scans.
{% codeblock lang:sql %}
EMP_ID     ROWID    NOT NULL GENERATED ALWAYS
{% endcodeblock %}

75. Specify the Leading Index Columns in WHERE Clauses
Details: For a composite index, the following query would use the index and execute matching processing as long as the leading column of the index is specified in the WHERE clause.

77. Keep in Mind Index Only Processing Whenever Possible
Many times, developers code queries that need to execute one of the four column aggregate functions (Sum, Avg, Min, or Max). They can get some great results by including an extra column as part of an index. Take, for example, the following query and associated index, Index1 = (YEAR, PERIOD):
{% codeblock lang:sql %}
SELECT SUM(JRNL_AMT)
FROM LEDGER_TABLE
WHERE YEAR = 2008 AND PERIOD IN (1,2,3)
{% endcodeblock %}
DB2 is good at recognizing the JRNL_AMT column in the index, even though there is no specific predicate on it, and it will choose index only processing.

81. Take Advantage of DB2 V9 Optimistic Locking
{% codeblock lang:sql %}
CREATE TABLE EMP
 (EMPNO  CHAR(6)  NOT NULL,
   ....
   .....
   EMP_UPD  TIMESTAMP NOT NULL
   GENERATED BY DEFAULT
   FOR EACH ROW ON UPDATE
   AS ROW CHANGE TIMESTAMP)
)
{% endcodeblock %}

82. Use the DB2 V9 MERGE Statement
https://www-01.ibm.com/support/knowledgecenter/SSEPGG_9.7.0/com.ibm.db2.luw.sql.ref.doc/doc/r0010873.html

85. Code Boolean Term Predicates Whenever Possible
Details: A Boolean term predicate is a predicate that is connected by And logic, so when one predicate is evaluated as false for a row, it then makes the entire Where clause evaluate to false.
{% codeblock lang:sql %}
SELECT EMPNO, LASTNAME, SALARY
FROM EMP
WHERE (LASTNAME > 'SMITH')
OR (LASTNAME = 'SMITH' and FIRSTNME > 'BOB')

SELECT EMPNO, LASTNAME, SALARY
FROM EMP
WHERE  LASTNAME >= 'SMITH'
  AND (LASTNAME >  'SMITH'   OR
      (LASTNAME = 'SMITH' AND FIRSTNME > 'BOB'))
{% endcodeblock %}

87. Avoid Sorts with Order By
Details: DB2 may avoid sorts if there are leading columns in an index to match the Order By or if the query contains an equal predicate on the leading column(s) of an index.

89. Watch Out for Case Logic
{% codeblock lang:sql %}
SELECT EMPNO, LASTNAME, SALARY,
       CASE WHEN SALARY < ...
            WHEN SALARY > ...
       ELSE
       END
FROM EMP
{% endcodeblock %}
Details: If most of the rows have a Salary < 50000.00, then that piece of logic should be coded first.

91. Know Your Version of DB2
{% codeblock lang:sql %}
SELECT GETVARIABLE('SYSIBM.VERSION')
FROM SYSIBM.SYSDUMMY1
{% endcodeblock %}

96. If You Need True Uniqueness, Try the V8 Generate_Unique Function
{% codeblock lang:sql %}
CREATE TABLE ORDER_TBL
 (ORDER_ID        CHAR(13) FOR BIT DATA,
  ORDER_DATE      DATE,
  ORDER_LOC       CHAR(03) );

INSERT INTO ORDER_TBL
  VALUES(GENERATE_UNIQUE(), CURRENT DATE, 'NY');

SELECT TIMESTAMP(ORDER_ID), ORDER DATE, ....
FROM ORDER_TBL;
{% endcodeblock %}

100. Update and Delete with Select (V9)
Details: The words Old Table are SQL DB2 reserved words that need to be coded in order for this statement to work.
{% codeblock lang:sql %}
SELECT SALARY, BONUS
FROM OLD TABLE
 (UPDATE EMP
    SET SALARY = :HV1-SALARY,
    BONUS = :HV2-BONUS
    WHERE EMPNO = :HV-EMPNO)
SELECT LASTNAME, FIRSTNME, WORKDEPT
FROM OLD TABLE
  (DELETE FROM EMP
   WHERE EMPNO = :HV-EMPNO)
{% endcodeblock %}

104. Avoid Unnecessary Sorting
Details: Data sorting can be caused by Group By, Order By, Distinct, Union, Intersect, Except, and Join processing.


List most critical tips of store procedure related from [DB2 SQL Tuning Tips for z/OS Developers](https://www.safaribooksonline.com/library/view/db2-sql-tuning/9780133038484/)
