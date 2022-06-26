# ModDB Specification

## Table of Contents

- [Introduction](#introduction)
- [Overview of ModDB](#overview-of-moddb)
- [Implementation](#implementation)
  - [Caching](#caching)
  - [Indexing](#indexing)
- [Query Language](#query-language)
  - [Design](#design)
  - [Grammar](#grammar)
- [Data Format](#data-format)

## Introduction

Welcome to the ModDB specification. The specification will go over the core design decisions behind ModDB, implementation details, the query language, *mQL*, and the data format, *TDX*.

## Overview of ModDB

ModDB is designed to be a modern, flexible, extensible, fast, efficient, secure, scalable, reliable, intuitive, and safe database management system. Many of these design decisions are directly achievable due to the programming language in which ModDB was developed, *Rust*. *Rust* is a modern systems programming language, most notable for its speed and guarateed memory safety.

## Implementation


### Caching


### Indexing


## Query Language

ModDB uses a simple, modern, and powerful query language called *mQL* (short for Modular Query Language). *mQL* is designed to be familiar to normal programming languages like *Python*. It is important to note that *mQL* does not follow the *SQL* standard. It is a query language designed from scratch. 

### Design

*mQL* was developed with a few key design goals in mind:

- **Static and strong typing** - *mQL* was created with a static and strong type system to ensure that queries are executed in a predictable and safe manner. The type system is used to ensure that values of the correct type are inputted into the database. It is also used to determine the validity of the query.
<!-- - **Declarative** - *mQL* is a statement-based language. When the developer writes the code, they simply state what is desired and the rest will be taken care of by the *mQL* compiler and query planner. *mQL* does support imperative constructs like  -->
<!-- - **Functional** - todo -->


## Data Format

