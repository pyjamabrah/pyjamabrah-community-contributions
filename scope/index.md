---
date: "2025-02-18"
title: 'Scopes of Identifiers in C'
thumbnail: "/posts/scope/thumbnail.png"
author: "streetdogg"

product: "embedded-engineering"

tags:
  - "C Language"

categories:
  - "basics"

series:
  - "C basics"
---

![](/posts/scope/thumbnail.png "various types of scopes.")

In the C programming language, the scope of an identifier determines where it can be accessed or referenced within a program. Understanding the different scopes in C is crucial for managing variable and function names effectively without causing conflicts.

<!--more-->

Let us start by understanding what an `identifier` is. It denotes -
> **An Object**

An identifier that represents a variable or constant in C. For example,

```c
int x;
```
where `x` is an object.

> **A Function**

An identifier that represents a function. For example,

```c
void printHello() {
  printf("Hello, World!");
}
```
where `printHello` is a function.

> **A Tag or Member of Structure, Union, or Enumeration**

An identifier representing the name of a **structure**, **union**, or **enumeration**. For example,

```c
struct Point {
  int x;
  int y;
};

struct Point p1;
```
where `Point` is a tag and `p1` is a member of the structure.

> **A Typedef Name**

An identifier that represents a typedef name. For example,

```c
typedef int Integer;
Integer num = 5;
```

where `Integer` is a typedef name.

> **A Label**

An identifier used as a label in statements like `goto labelName;`. For example,
```c
labelName:
  a = a + 1;
```
where `labelName` is a label.

> **Macro Parameter**

An identifier that represents a parameter in a macro definition. For example,
```c
#define MAX(a, b) ((a > b) ? a : b)
```
where `a` and `b` are macro parameters.

A member of enumeration is called `enumeration constant`. Macro parameters can be ignored as the macros are preprocessed and replaced with values as part of the text substitution.

The identifier is visible (i.e to say, it can be used) only within a region of program text called `scope`. Different entities designated by the same identifier either have different scopes, or are in the different name spaces.

## scope

There are four types of scopes:
1. function
1. file
1. block
1. function prototype

A `label name` is the only kind of identifier that has a function scope. It can be used in a `goto` statement anywhere in the function in which it appears. A label is declared implicitly by its syntactic appearance (followed by a `:` and a statement).

Every other type of identifier has a scope determined by the placement of its declaration.

### file scope

If the declarator or type specifier that declares the identifier appears outside of any block or list parameters, the identifier has `file` scope, which terminates at the end of the translation unit.

### block scope

If the declarator or type specifier that declares the identifier appears inside of a block or within the list parameter declarations in a function definition, the identifier has `block` scope. The scope terminates at the end of the associated block.

### function prototype scope

If the declarator or type specifier that declares the identifier appears within the list of parameter declarations in a function prototype, the identifier has `function prototype` scope. The scope terminates at the end of the function declarator.

## Inner and Outer Scope

If an identifier designates two different entities in the same name space, the scope might overlap. The scope of one entity (the `inner` scope) will end strictly before the scope of the other entity (the `outer` scope).

Within the inner scope, the identifier designates the entity declared in the inner scope. The entity declared in the outer scope is hidden (and not visible) within the inner scope.

## same scope

Two identifiers have the same scope if and only if their scopes terminate at the same point.

## structure, union and enumeration scope

Structure, union, and enumeration tags have scope that begins just after the appearance of the tag in a type specifier that declares the tag.

Each enumeration constant has scope that begins just after the appearance of its defining enumerator in an enumerator list. Any other identifier has scope that begins just after the completion of its declarator.

As a special case, a type name (which is not a declaration of an identifier) is considered to have a scope that begins just after the place within the type name where the omitted identifier would appear were it not omitted.
