
The String[] whereArgs contains the arguments to be appended to the whereClause.
For example, you want to make a delete query:
    `delete from InfoTable where name = "ABC" and id = "23"`
then the query should be:
`delete("InfoTable", "name = ? AND id = ?" , new String[] {"ABC", "23"});`

