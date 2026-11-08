dates are in ISO format 'YYYY-MM-DD'
use single quotes for values bcz double quotes are for columns
```postgresql
drop table if exists people;
create table people(
	id serial not null primary key,
	first_name varchar(50),
	last_name varchar(50),
	dob date,
	job varchar(50)
);
INSERT INTO people(first_name, last_name, dob, job)
VALUES
    ('lance', 'smith', '1998-06-25', 'dev'),
    ('gregory', 'keneth', '1998-07-25', 'dev');

SELECT * FROM people where first_name like '%e%';
```
`TRUNCATE` to remove data from table

```postgresql
SELECT first_name, date_part('year', age(dob)) from people;

"first_name"	"date_part"
"lance"	         28
"gregory"	     28
```

`WHERE` for records, `HAVING` for groups of records
