# ModSQL Query Language Specification

## Intro

ModSQL uses an SQL variant called **mSQL** (short for "modular").
**mSQL** does not specify a schema when creating a table. You can insert into nonexistent columns and it will create them for you.
The supported types are defined in the [`TDX Specification`](https://github.com/modsql/specification/blob/master/TDX/README.md), which also explains how the data types are implemented.

## Grammar 
```text
KEYWORD
    : DatatypeKwd
        : string
        | character
        | int
        | short
        | long
        | bigint
        | decimal
        | bigdecimal
        | hex (hexadecimal)
        | scn (scientific number)
        | dynamic
        | bool
    | QueryKwd
       : select
       | insert
       | update
       | delete
       | count
       | cursor
       | match
       | link
       | right
       | left
       | ensure
       | index
       | for
       | where
       | with
       | return
       | set
       | on
       | move
       | next
       | table
       | load
       | db
       | create
       | from
       | into
       | drop

QUERY
    : QueryKind
        : Simple
        | Expressive
        | Predicated
        | Compound
        | Functional

STATEMENT
    : QUERY
    | …
    ;

OPERATOR
    : Unary
        : …
    | Binary
        : Logical
            : &&
            | ||
        | Arithmetic
            : +
            | -
            | *
            | /
            | %
        | Relationship
            : <
            | <=
            | >
            | >=
        | Equality
            : ==
            | !=
        | Location
            : >|
            | =>
        | = (assignment)
    | XQuery
        : or
        | in
        | is 
        | and
    | …

LITERAL
    : "string literal"
    | 'c' (character literal)
    | true (boolean literal)
    | false (boolean literal)
    | 0x (hexadecimal literal)
    | BASEeEXP (scientific number literal)
    | null (null literal; value only)
    | dynamic { body } (dynamic literal) 
    | Array Literal
        : { body } (inferred array; T is type of first element)
        | T { body } (explicit array; T defined before array body)
    | ...

SEPARATOR
    : " " (whitespace)
    | \t
    | \n
    | // single line comment
    | /* multi line comment */
    | (
    | )
    | [
    | ]
    | {
    | }
    
EXPRESSION 
    : 
```

## Examples

#### Create and load database
```sql
create db MyDemoDatabase;
load MyDemoDatabase;
```

#### Create table
```sql
create table Users;
```
*Create with Id property*
```sql
create table Users with id("IdPropertyName");
```
*Specific string encoding*
```sql
create table Users with encoding("unicode") and id("Id"); 
```

#### Insert into table without ID
```sql
insert [Username = "MyUsername", Password = "Password", CreationDate = utcnow()] into Users;
```
*Insert multiple rows in a single statement*
```sql
insert [Username = "MyUsername", Password = "Password", CreationDate = utcnow()], [Username = "username2"] into Users;
```

#### Insert into table with ID
*If id is not defined, it will get added automatically*
```sql
insert [Id = newid(), Username = "MyUsername"] into Users;
```
*New ID will get generated automatically*
```sql
insert [Username="MyUsername", RandomNumber=10] into Users;
```

*Inserting inferred string array*
```sql
insert [UserId = id<gi> 075b817f-5979-48f5-8672-31120fd44502, Permissions = {
    "Read",
    "Write"
}] into Users;
```

*Inserting explicit string array*
```sql
insert [UserId = id<gi> 075b817f-5979-48f5-8672-31120fd44502, Permissions = string {
    "Read",
    "Write"
}] into Users;
```
*Inserting dynamic array*
```sql
insert [UserId = id<gi> 075b817f-5979-48f5-8672-31120fd44502, Permissions = dynamic {
    string "Write",
    int 5
}] into Users;
```
*Insert with return (x represents the modified row)*
```sql
insert [UserId=newid(), Username="Username"] into Users return x.UserId;
```


#### Query
*select where value==property*
```sql
select * from Users where Username == "MyUsername";
```
*select where less than 10*
```sql
select * from Users where RandomNumber < 10;
```
*select where property in array*
```sql
select Username, Email from Users where Username => (string {
    "Mike",
    "Matt",
    "Sarah",
    "Eliah"
});
```
*select where value in array property*
```sql
select Username, FullName from Users where "Jake" => FriendsArray;
```


*Xselect - select Username where value is Username 1 or Username 2*
```sql
select * from Users where Username is ("Username 1' or 'Username 2");
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
select * from Users where Username is ("Username 1" or (length(_) > 5));
```



#### Update
*simple update*
```sql
update Users set Field1="new value" where Field == "value";
```
*update multiple tables*
```sql
update Users, Profiles set Users.Username="New Username", Profiles.Username="New Username" where Users.Id == @idParam on Users.Id == Profiles.UserId;
```

#### Count
```sql
count Users where NumericField > 5;
```

#### Delete
```sql
delete from Users where Username == "Username";
```

#### LINK/JOIN
*Full outer join*
```sql
link Table1, Table2 where Table1.Id == @idParam on Table1.OtherId == Table2.OtherId;
```
*Inner join*
```sql
match link Table1, Table2 where Table1.Id == @idParam on Table1.OtherId == Table2.OtherId;
```
*left join*
```sql
left link Table1, Table2 where Table1.Id == @idParam on Table1.OtherId == Table2.OtherId;
```
*right join*
```sql
right link Table1, Table2 where Table1.Id == @idParam on Table1.OtherId == Table2.OtherId;
```

#### CURSORS

Innactive cursors should be dropped after 5 minutes of inactivity by default

*create cursor query*
```sql
cursor CursorName select * from Table1 where BoolProperty == true;
```
*move through dataset*
```sql
cursor CursorName move next 100;
```
*drop cursor*
```sql
drop cursor CursorName;
```

#### INDEXES
```sql
ensure index for Table1 on Property1, Property2; 
```
