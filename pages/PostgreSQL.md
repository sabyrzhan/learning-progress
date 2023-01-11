- ## [[PostgreSQL How-To]]
	- **Restore with PostgreSQL**
		- ```
		  psql -U <username> -d <dbname> -1 -f <filename>.sql
		  or
		  pg_restore -U <username> -d <dbname> -1 <filename>.dump
		  ```