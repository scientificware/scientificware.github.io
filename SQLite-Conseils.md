
The String[] whereArgs contains the arguments to be appended to the whereClause.

For example, you want to make a delete query:

`delete from myTable where name = "ABCD" and id = "234"`

then the query should be:

`delete("myTable", "name = ? AND id = ?" , new String[] {"ABCD", "234"});`

