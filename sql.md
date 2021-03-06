# SQL
## General architecture
Table data are stored in so called pages. Pages store multiple records, so whenever we query the data from storage - the first thing that is fetched is the page. And only after the data that confirms to the query is looked up within that page. Single page can store multiple records and depends on the settings of the underlying storage settings.

Whenever we query a record, server always fetch whole data with all the columns. Because that is the nature of the SQL data - singe records stored in a page contains whole record data. Its lenght is equals to the sum of column data type. That is why better to always choose correct data type with correct length to decrease the IO operations between server and application.

## ACID - SQL transaction property
* Atomicity - the transaction is atomic. The result of the operation shows the data either successfully written or failed.
* Consistency - any read operation after successfull write or delete returns latest value of the record
* Isolation - determines the visibility level to users. The lower the lavel the latest data (or durtiest) data the users will see. And conversely, the higher the level the farther from the other transaction changes.
* Durable - the successfully committed transaction survives permanently after crash.

### Isolation
#### Read phenomenas
It has following read phenomenas:
* Dirty reads - the data changes by other transaction whether committed or not is visible in other transactions
* Non-repeatable reads - during the transaction the same read query can return different values.
* Phantom reads - during the transaction the query returns differnt set of results, because of other transactions adding/removing the rows from db.
* Lost updates - during the transaction the queried data might be outdated because of the updates in other transaction, thus overriding the latest value.

#### Isolation levels
1. Read committed (mostly default option) - the transaction always reads committed data.
2. Read uncommitted - the transaction can see other transactions' uncommitted data.
3. Repeatable read - during the transaction the record values keep consistent no matther how many queries made.
4. Serializable - the strongest level. Parallelly run transaction act the same way as if they run sequentially. At the same time it can degredate the overall performance because of the locks and latencies it produces.

## Partitioning vs Sharding
Partitioning - Dividing large table into seperate tables within one instance of the SQL service.<br>
Sharding - the same as partitioning, but in different service instances.<br>
By partitioning not only the table is data is divided, but also table indexes, thus improving the performance.<br>
