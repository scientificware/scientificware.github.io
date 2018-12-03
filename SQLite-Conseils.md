Ce qui suit est un tutoriel personnel pour l'utilisation de SQLite sous Android ou Java.
----

![sqlite_logo](https://user-images.githubusercontent.com/19194678/49358751-1f188380-f6d4-11e8-899c-26b148a46cb8.png)

The String[] whereArgs contains the arguments to be appended to the whereClause.

For example, you want to make a delete query:

`delete from myTable where name = "ABCD" and id = "234"`

then the query should be:

`delete("myTable", "name = ? AND id = ?" , new String[] {"ABCD", "234"});`

