#Pseudo Columns

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
 
