# ModDB Specification

Welcome to the ModDB specification. Here, you will learn about a few things regarding ModDB:

- An overview of what ModDB is, the reason behind it, and design choices.
- mQL: the query language for ModDB
- Hawq: the database engine for ModDB

## Table of Contents

- [```Overview```](#overview)
- [```mQL (Modular Query Language)```](#mql-modular-query-language)
  - [```Grammar```](#grammar)
  - [```Examples```](#examples)
- [```Hawq```](#hawq)

## Overview

ModDB is a relational database management system, or RDBMS, along the lines of notable relational databases such as PostgreSQL and MySQL. ModDB was designed, and created, with a few core concepts in mind:

- Intutiveness
- Scalability
- Reliability
- Security
- Speed
- Efficiency

ModDB is able to achieve many of these design goals through the programming language that it was built on, Rust. If you are unfamiliar with Rust, it is a general-purpose programming language, which happens to be excellent for systems development. It guarantees a myraid of things that all modern systems and applications should be able to also guarantee. A few notable guarantees are: *memory safety*, *type safety*, *thread safety*, *security*, and *performance*. All of these guarantees do not come at the cost of speed or efficiency.

## mQL (Modular Query Language)

ModDB utilizes an *SQL-like* query language by the name of mQL, short for Modular Query Language. It is essentially our own take on SQL. mQL provides an attractive and intuitive syntax that allows you to interact with your database with incredible speed. mQL can be classified as:

- **Statically typed**: mQL is statically typed. At compile time, ALL types must be known wheter it be the types of procedure parameters, the types of columns in a table, or the types of columns in a result set. This type safety is achieved through type inference and explicit type annotations by the programmer.
- **Strongly-typed**: mQL is also strongly typed. For example, the types of procedure parameters must be defined by the programmer, as do the types of columns in a table.
- **Declarative**: Similar to SQL, mQL is a declarative, statement-based language. You, the programmer, will declare what you want to happen and the rest will be done for you. But, mQL still does have support for constructs like control flow with *if..elif..else* blocks.
- **Functional**: mQL is also functional through the concept of *procedures*, which are essentially like functions in other languages. Procedures are a way to organize your code in a way that makes it easier to understand and maintain.

### Grammar

mQL has a somewhat "complicated" grammar. The following is a list of the rules that make up the grammar, in EBNF notation:

```ebnf
(* tokens *)
identifier ::= IDENT ;
eof ::= EOF ;
keyword ::= "insert" | "select" | "update" | "count" | "delete" | "link"
        | "match" | "left" | "right" | "create" | "table" | "proc" 
        | "transaction" | "into" | "return" | "where" | "set" | "cursor" 
        | "from" | "var" | "index" | "on" | "with" | "commit" 
        | "abort" | "for" | "drop" | "move" | "next" | "execute" 
        | "string" | "character" | "int" | "short" | "long"  | "bigint" 
        | "decimal" | "bigdecimal" | "hex" | "scn" | "dynamic" | "bool" 
        | "guid" | "if" | "elif" | "else" ;
operator ::= "-/" | "--/" | "<0>" | "~" | "+" | "-"
          | "!" | "&&" | "||" | "+" | "-" | "*" 
          | "/" | "%" | "**" | "<" | "<=" | ">" 
          | ">=" | "==" | "!=" | "&" | "|" | "^" 
          | ">>" | "<<" | "=>" | "=" | "_" | "@" 
          | "or" | "is" | "in" | "and" ;
punctuation ::= "(" | ")" | "[" | "]" | "{" | "}" 
             | "<" | ">" | "," | ";" | "." ;
literal ::= STRING | CHARACTER | INT | SHORT | LONG | BIGINT 
         | DECIMAL | BIGDECIMAL | HEXADECIMAL | SCIENTIFICNUMBER | DYNAMIC | GUID 
         | "true" | "false" ;
token_kind ::= ident | eof | keyword | operator | punctuation | literal ;

(* rules *)
statement ::= expr | proc_def | body | variable ";" ;
variable ::= "var" ident "=" expr ;
expr ::=
body ::= "{" statement* "}" ;
proc_def ::= "create" "proc" identifier "("  ")" body ;
```

### Examples

*insert mQL examples here*

## Hawq

Hawq is the database engine for ModDB. It handles:

- Memory and disk management
- CRUD (Create, Read, Update, Delete) operations
- Indexing
- Concurrency
- Query optimization, execution, and planning
- Caching
- Transaction management and ACID (Atomicity, Consistency, Isolation, Durability) compliancy
