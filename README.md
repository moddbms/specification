<h1 align="center">ModEDB Specification</h1>

Welcome to the **ModEDB Specification**! Continue reading to learn about the **mEQL** query language used by **ModEDB**, the data format, supported data types, and the low-level implementations of the data types.

## Table of Contents

- [```Query Language```](#query-language)
  - [```Grammar (Extended Backus-Naur Form)```](#grammar-extended-backus-naur-form)
  - [```Examples```](#examples)
- [```TDX Data Format```](#tdx-data-format)
  - [```Supported Datatypes```](#supported-datatypes)
  - [```Low Level Datatype Implementations```](#low-level-datatype-implementations)
    - [```String and Character```](#string-and-character)
  - [```Numeric Types```](#numeric-types)
  - [```DateTime Types```](#datetime-types)

## Query Language

**ModEDB** uses a query language named **mEQL**, or **Modular Expressive Query Language**. **mEQL** has a syntax different to that of traditional SQL, but the syntax is made to be nice to write, easy to read, and most importantly, nice to look at. It can be classified as:

- Statically and strongly typed
- Declarative
- Functional
- Modular

### Grammar (Extended Backus-Naur Form)

```ebnf
(* operators *)
prefix = "-/" | "--/" | "<0>" | "~" | "+" | "-" ;
postfix = "!" ;
unary = prefix | postfix ;
binary = "&&" | "||" | "+" | "-" | "*" | "/" 
          | "%" | "**" | "<" | "<=" | ">" | ">=" 
          | "==" | "!=" | "&" | "|" | "^" | ">>" 
          | "<<" | "=>" ;
assignment = "=" | "+=" | "-=" | "*=" | "/=" | "%=" 
              | "**=" | "&=" | "|=" | "^=" | ">>=" | "<<=" ;
xquery = "or" | "is" | "in" | "and" ;
op = unary | binary | assignment | xquery | "*" | "_" | "@" ;

(* punctuation *)
bracket = "(" | ")" | "[" | "]" | "{" | "}" 
       | "<" | ">" ;
sep = " " | "\t" | "\n" | "," | ";" ;

(* datatypes *) 
literal = '" ? all visible characters ? "'

(* query *)
query = clause [ ident | "*" ] [ "from" ] "(" ident "=" literal [ { "," ident = literal [ "," ] } ] ")" | ident [ { "," ident [ "," ] } ] "set" { ident "=" literal [ { "," ident = literal [ "," ] } ] | "into" ident | "from" ident | "move next" literal [ "->" predicate] [ "with" proc call [ { "," proc call [ "," ] } ] [ "return" ident ] ;

(* procedures *)
(* users can create their own procedures, along with calling builtin ones *)
new proc = "create" "proc" ident "(" proc args ")" proc body ;
proc args = { datatype keyword ident [ "," ] } ;
proc call = ident "(" proc args ")" ;
proc body = "{" { statement } "}" ;

(* transactions *)
new transaction = "create" "transaction" ident ;

(* cursors *)
new cursor = "cursor" ident query ; 
drop cursor = "drop" "cursor" ident ;

(* statements *)
variable = "var" ident "=" query ;
statement = query | variable | new proc | new transaction | new cursor | drop cursor ";" ;

(* main *)

clause = "insert" | "select" | "update" | "count" | "delete" | "link"
       | "match link" | "left link" | "right link" 
datatype keyword = "string" | "character" | "int" | "short" | "long" | "bigint" 
                 | "decimal" | "bigdecimal" | "hex" | "scn" | "dynamic" | "bool" 
                 | "guid" ;
keyword = clause | datatype keyword | "create" | "table" | "proc" | "transaction" 
        | "into" | "return" | "where" | "set" | "cursor" | "from" 
        | "var" | "index" | "on" | "with" | "commit" | "abort" 
        | "for" | "drop" | "move" | "next"
ident = IDENT ;
```

### Examples
