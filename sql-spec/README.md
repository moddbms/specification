# ModSQL Query Language Specification

## Intro

ModSQL uses an SQL variant called **mSQL** (short for "modular").
**mSQL** does not specify a schema when creating a table. You can insert into nonexistent columns and it will create them for you.
The supported types are defined in the [`TDX Specification`](https://github.com/modsql/specification/blob/master/TDX/README.md).

## Operators

**Normal Operators**
- == (equals)
- != (is not)
- < (less than)
- <= (less than or equal)
- \> (greater than)
- \>= (greater than or equal)
- \>| (value in array)
- && (and)
- || (or)
- => (in)

**XQuery Operators**
- is
- in
- or

## Examples

#### Create and load database
```sql
create db MyDemoDatabase;
load MyDemoDatabase;
```

#### Create table without ID
```sql
create table Users;
```

#### Create table with Guid ID
*Guid type ID*
```sql
create table Users with id<gi>('Id');
```

*Integer type ID*
```sql
create table Users with id<in>('Id');
```

#### Insert into table without ID
```sql
insert [Username = 'MyUsername', Password = 'Password', CreationDate = utcnow()] into Users;
```

#### Insert into table with ID
*If id is not defined, it will get added automatically*
```sql
insert [Id = new_guid(), Username = 'MyUsername'] into Users;
```

*New ID with type GUID verifying if already in table*
```sql
insert [Id=new_safe_id<gi>(), Username='MyUsername', RandomNumber=10] into Users;
```

*Inserting string array*
```sql
insert [UserId = '075b817f-5979-48f5-8672-31120fd44502', Permissions = st {
    'Read',
    'Write'
}] into Users
```
*Inserting dynamic array*
```sql
insert [UserId = id<gi> '075b817f-5979-48f5-8672-31120fd44502', Permissions = dy {
    st 'Write',
    in 5
}] into Users
```

*Insert with return (x represents the modified row)*
```sql
insert [UserId=new_guid(), Username='Username'] into Users return x.UserId
```


#### Query
*select where value==property*
```sql
select * from Users where Username == 'MyUsername';
```
*select where less than 10*
```sql
select * from Users where RandomNumber < 10;
```
*select where property in array*
```sql
select Username, Password where Username => (str {
    'Mike',
    'Matt',
    'Sarah',
    'Eliah'
});
```
*Xselect - select Username where value is Username 1 or Username 2*
```sql
select * from Users where Username is ('Username 1' or 'Username 2');
```
*XSelect where Amount is in between 5000 and 100000*
```sql
select from Wallets where Amount is (> 5000 and < 100000);
```
* '_' stands for 'this'*
```sql
select from Wallets where Amount is (5000 < _ < 100000);
```
*XQuery where length of property is > 5*
```sql
select * from Users where Username is ('Username 1' or (LENGTH(_) > 5));
```



#### Update
*simple update*
```sql
update Users set Field1='new value' where Field == 'value';
```
*update multiple tables*
```sql
update Users, Profiles set Users.Username='New Username', Profiles.Username='New Username' where Users.Id == @idParam on Users.Id == Profiles.UserId;
```

#### Count
```sql
count Users where NumericField > 5;
```

#### Delete
```sql
delete from Users where Username == 'Username';
```

#### LINK/JOIN
*Full outer join*
```sql
link Table1, Table2 where Table1.Id is @idParam on Table1.OtherId == Table2.OtherId;
```
*Inner join*
```sql
match link Table1, Table2 where Table1.Id is @idParam on Table1.OtherId == Table2.OtherId;
```
*left join*
```sql
left link Table1, Table2 where Table1.Id is @idParam on Table1.OtherId == Table2.OtherId;
```
*right join*
```sql
right link Table1, Table2 where Table1.Id is @idParam on Table1.OtherId == Table2.OtherId;
```
