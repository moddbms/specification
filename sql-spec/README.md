# ModSQL Query Language Specification

## Intro

ModSQL uses an SQL variant called mSQL (Short for "modular")
mSQL does not specify a schema when creating a table, you can insert into non-existant columns and it will create them for you
The supported types are defined in the <a>TDX specification</a>

## Operators

- is (equals)
- not (is not)
- lt (less than)
- lte (less than or equal)
- gt (greater than)
- gte (greater than or equal)
- in (value in array)

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
insert [UserId = '075b817f-5979-48f5-8672-31120fd44502', Permissions = st{
    'item 1',
    'item 2'
}] into Users
```
*Inserting dynamic array*
```sql
insert [UserId = '075b817f-5979-48f5-8672-31120fd44502', Permissions = st{
    'item 1',
    'item 2'
}] into Users
```


#### Query a row
*select where value==property*
`select * from Users where Username is 'MyUsername'`

*select where less than 10*
`select * from Users where RandomNumber lt 10`
