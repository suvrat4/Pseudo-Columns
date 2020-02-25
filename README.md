# Pseudo Columns

* A Pseudo column behave likes a table column, but is not actually stoed in the table. You can select from the pseudo columns, but you cannot
insert,update or delete their values.

 ## The available pseudo columns are:
 
 * ROWID
 * ROWNUM
 * LEVEL
 * CURRVAL
 * NEXTVAL
 
 ### ROWNUM Pseudo colums:
 
 * For ecah row retured by a query, the ROWNUM pseudo column returns a number indicating the oredr in which Oracle selects the row from a table or set of joined rows.
 * The first row selected has a rownum of 1, the second has 2, and so on.
 * You can use ROWNUM to limit the number of rows returned by a query.
 * If an ORDER BY cluase follows ROWNUM in the same query, then the rows will be reordered by the ORDER BY clause. The rseults can vary depending on the ways the rows are accessed.
 * For example, if the ORDER BY caluse causes Oracle to use an index to access the data, then Oracle may retrive the rows in a different order than without the index. Therefore, the following statement will not have the same effect as the preceding example.
 * ROWNUM can also be used in conditions but only two operators; < and <= make sense with ROWNUM.
 
 ### Advantages of ROWNUM:
* Using rownum for top-n queries.
* Using rownum with range bound queries.

###### Examples:

SELECT * FROM emp WHERE ROWNUM < 11 ORDER BY ename;

* If you embed the ORDER BY clause in a sub query and place the ROWNUM condition in the top-level query, then you can force the ROWNUM condition to be applied after the ordering of the rows.
* For example, the following query returns the employees with the 10 smallest employee numbers.

#### NOTE:

* ROWNUM values greater than a positive integer are always false. For example, the below query returns no rows.

SELECT * FROM emp WHERE ROWNUM > 1;
no rows selected

* You can also use ROWNUM to assign unique values to each row of a table.

UPDATE emp SET empno=ROWNUM WHERE empno=7902;

###### Examples of ROWNUM:

SQL> SELECT rownum,empno,ename,deptno FROM emp;

    ROWNUM      EMPNO ENAME          DEPTNO
---------- ---------- ---------- ----------
         1       7369 SMITH              20
         2       7499 ALLEN              30
         3       7521 WARD               30
         4       7566 JONES              20
         5       7654 MARTIN             30
         6       7698 BLAKE              30
         7       7782 CLARK              10
         8       7788 SCOTT              20
         9       7839 KING               10
        10       7844 TURNER             30
        11       7876 ADAMS              20
        12       7900 JAMES              30
        13       7902 FORD               20
        14       7934 MILLER             10

14 rows selected.

SQL> SELECT rownum,empno,ename,deptno FROM emp WHERE deptno = 10;

    ROWNUM      EMPNO ENAME          DEPTNO
---------- ---------- ---------- ----------
         1       7782 CLARK              10
         2       7839 KING               10
         3       7934 MILLER             10

SQL> SELECT ename FROM emp WHERE rownum=1;

ENAME
----------
SMITH

### Querying for top 'N' records:

* We can ask for the Nth largest or smallest of a coloumn.
* Never use rownum and order by caluse together as oracle first fetches the rows according to rownum and then sort's the found rows.
* From oracle 8i, order by clause can be used in inline views.

###### Examples:

SQL> SELECT ename,sal,rownum FROM (SELECT rownum,ename,sal FROM emp ORDER BY sal desc) WHERE rownum<=5;

ENAME             SAL     ROWNUM
---------- ---------- ----------
KING             5000          1
SCOTT            3000          2
FORD             3000          3
JONES            2975          4
BLAKE            2850          5

### ROWID Pseudo column:

* For each row in the database, the ROWID pseudocolumn returns the address of the row.
* Oracle Database rowid values contain information necessary to locate a row.
1. The object number of the object.
2. The data block in the data file in which the row resides.
3. The positon of the row in the data block(first row is 0).
4. The datafile in which the row resides(first file is 1).

* Usually, a rowid value uniquely identifies a row in the database. However, rows in different tables taht are stored together in the same cluster can have the same rowid.

### ROWID values have several important uses:

* They are fastest way to access a single row.
* They can show you how the rows in a table are stored.
* They are unique identifiers for rows ina table.
* You should not use ROWID as the primary key of a table. If you delete and reinsert a row with the import and export utilities, for example, then its rowid may change. If you delete a row, then oracle may reassign its rowid to a new row inserted later.
* You can use the ROWID pseudo column in the SELECT and WHERE clause of a query, these pseudo column values are not actually stored in the database.
* You cannot insert,update or delet a value of the ROWID pseudo column.

### Advantage of rowid:
* A rowid is a pseudo column(like version_xid), that uniquely identifies a row within a table, but not within a database.
* It is possible for to rows of two different tables stored in the same cluster to have the same rowid.
Example This statement selects the adddress of all orws that contain data for employees in departement 20.

SQL> SELECT ROWID,ename FROM emp WHERE deptno=20;

ROWID              ENAME
------------------ ----------
AAAR3sAAEAAAACXAAA SMITH
AAAR3sAAEAAAACXAAD JONES
AAAR3sAAEAAAACXAAH SCOTT
AAAR3sAAEAAAACXAAK ADAMS
AAAR3sAAEAAAACXAAM FORD

SQL> SELECT ename,sal,job FROM emp WHERE rowid = 'AAAR3sAAEAAAACXAAA';

ENAME             SAL JOB
---------- ---------- ---------
SMITH             800 CLERK

SQL> SELECT ename,sal,job FROM emp WHERE rowid < 'AAAR3sAAEAAAACXAAA';

no rows selected

SQL> SELECT max(rowid) FROM emp;

MAX(ROWID)
------------------
AAAR3sAAEAAAACXAAN

SQL> SELECT min(rowid) FROM emp;

MIN(ROWID)
------------------
AAAR3sAAEAAAACXAAA

SQL> SELECT * FROM emp p WHERE rowid<(SELECT max(rowid) from emp s WHERE p.ename=s.ename);

no rows selected

##### NOTE:
* Duplicate ename's is there that only display the above statement.

SQL> DELETE FROM emp p WHERE rowid<(SELECT max(rowid) from emp s WHERE p.ename=s.ename);

### Frequently Asked Questions:

* How many columns can table have?
The number of columns in a table can range from 1 to 1000.

* Deleting Duplicate rows in the table?
We can delet the duplicate rows in the table by using ROWID.

* What is ROWID? In a session before accesing next value?
ROWID is a pseudo column attached to each row of a table. It is 18 caharcter long, blockno, rownumber are the components of RWOID.

* Display the record between two range? using rownum

SQL> SELECT rownum,empno,ename FROM emp WHERE rowid in
  2  (
  3  SELECT rowid FROM emp WHERE rownum<=&upto
  4  MINUS
  5  SELECT rowid from emp WHERE rownum<&start
  6  );
Enter value for upto: 10
old   3: SELECT rowid FROM emp WHERE rownum<=&upto
new   3: SELECT rowid FROM emp WHERE rownum<=10
Enter value for start: 5
old   5: SELECT rowid from emp WHERE rownum<&start
new   5: SELECT rowid from emp WHERE rownum<5

    ROWNUM      EMPNO ENAME
---------- ---------- ----------
         1       7654 MARTIN
         2       7698 BLAKE
         3       7782 CLARK
         4       7788 SCOTT
         5       7839 KING
         6       7844 TURNER

6 rows selected.

* Display Odd/Even number of records?

===========ODD RECORDS================
SQL> SELECT empno,ename FROM emp WHERE (rowid,1) in ( SELECT rowid, mod(rownum,2) FROM emp);

     EMPNO ENAME
---------- ----------
      7369 SMITH
      7521 WARD
      7654 MARTIN
      7782 CLARK
      7839 KING
      7876 ADAMS
      7902 FORD

7 rows selected.

===========EVEN RECORDS===============

SQL> SELECT empno,ename FROM emp WHERE (rowid,1) in ( SELECT rowid, mod(rownum,2) FROM emp);

     EMPNO ENAME
---------- ----------
      7369 SMITH
      7521 WARD
      7654 MARTIN
      7782 CLARK
      7839 KING
      7876 ADAMS
      7902 FORD

7 rows selected.

* How many rows will following SQL return?

SELECT * FROM emp where rownum = 10;

Answer: No rows.







