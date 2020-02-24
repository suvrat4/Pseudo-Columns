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

