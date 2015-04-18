Mysql Teorik Notlar
===================

:Date: 04/12/2014 
:Author: Orkun Gunay
:tags: mysql,theory
:category: sql


**some of the important characteristics of MySQL**

* Fully multi-threaded using kernel threads
* Full operator and function support in the

.. code:: mysql

        SELECT CONCAT(first_name, " ", last_name) FROM tbl_name
        WHERE income/dependents > 10000 AND age > 30;       


*  16 indexes per table are allowed. Each index may consist of 1 to 15 columns
   or parts of columns. The maximum index length is 256 bytes (this may be
   changed when compiling MySQL ). An index may use a prefix of a ` CHAR ` or `
   VARCHAR ` field.


* All columns have default values. You can use ` INSERT ` to insert a subset of a table's columns; those columns that are not explicitly given values are set to their default values.

* All data are saved in ISO-8859-1 Latin1 format. All comparisons for normal string columns are case insensitive

**Tanimlar**

* qps: queries per second

**MySQL Stored Procedures**



