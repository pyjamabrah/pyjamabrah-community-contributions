---
# Change the Date below
date: "2025-02-19"

# Add the title of the Post here
title: "A shallow dive into do{..}while(0) loop in C"

# replace the "sample/cover.png" with path to a 16:9 image
# this image will be shown in SEO optimization and when
# you share the post on social media.
thumbnail: "/posts/do-while-magic/cover.png"

# Add your GitHub handle here
author: "sirilcodes"

# Add tags as suitable for the topic
tags:
  - "Macros"
  - "C"
  - "Linux Kernel"
  - "Programming"

# If it fits a certain category of topics, add that
categories:
  - "Programming"
  - "C"

# If this is a Series of posts, Add the name of the series
series:
  - ""

# Delete the line below when submitting for review
#draft: true

product: "c"
---

In this article, I want to talk about the `do{..}while(0)` construct in C. This construct is often used in C programming to define multi-statement macros. It is a common pattern used in the Linux kernel and other standard C projects. I will take an example of a macro that is not safe and show how `do{..}while(0)` construct can be used to make it safe. Especially in the conditional statements.


<!--more-->

# Overview
We read about loops in C, and we often come across `while`, `for`, `if else` which made sense to me. But there's one construct I could never make sense of. `do{..}while()`. Everything that can be achieved by `do{..}while()` can be achieved by using a `while()` loop. Why did Dennis Ritchie even create this? There has to be a reason, right? I think I found one of the reasons.
There is a scenario where `do{..}while()` shines. In fact, it's the best way to do that thing.

Before you continue to the actual problem statement, we will first see `#define MACROS` and then move to the problem. Trust me, this is an interesting use case.

## Macros in C
Macros are processed in the pre-processing stage of the C compilation process. They are used to define constants, functions, and other constructs that are expanded inline during compilation. Macros are defined using the `#define` directive, followed by the macro name and its replacement text.
They can also be used to replace code snippets, making the code more readable and maintainable. For example:

```c
#define BROKEN_MACRO(x) statement1(x);statement2(x);
```
The above macro `BROKEN_MACRO` takes a parameter `x` and expands to statements present after the space. This can be used to replace multiple lines of code with a single macro call.

```c
int main() {
    BROKEN_MACRO(x);  // Expands to  statement1(x); statement2(x);
    return 0;
}
```
Using it in code, it will expand to:
```c
int main() {
    statement1(x); statement2(x); // This is fine.
    return 0;
}
```

## The Problem
Here, `BROKEN_MACRO` is a simple macro that expands to a block of statements. However, this approach can lead to issues when used in certain contexts.

1. Multiple statements in a macro might not work correctly in all contexts
2. The macro might not behave as a single statement
3. Issues with trailing semicolons can cause unexpected behavior

Consider this same macro:
```c
#define BROKEN_MACRO(x) statement1(x); statement2(x);
```

Using it in a conditional statement, this can lead to problems:
```c
if (condition)
    BROKEN_MACRO(x);  // Expands to statement1(x); statement2(x);
else
    other_statement();
```

After preprocessing, this becomes:
```c
if (condition)
    statement1(x);
    statement2(x);  // This is now outside the if block!
else
    other_statement();  // Syntax error: else without if
```

## Another Approach
Let's try using curly braces. This is a common approach to solve the above problem.

```c
#define BROKEN_MACRO(x) {statement1(x); statement2(x);}
```
The code:
```c
if (condition)
    UNSAFE_MACRO(x);
else
    other_statement();
```
will expand to:
```c
if (condition)
    {statement1(x); statement2(x);}; // Dangling semicolon.
else                                // Syntax error: else without if
    other_statement();
```

You should be able to see the issue from the code above. The semicolon makes the `else` block to be outside the `if` block. This is not what we want. The dangling `else` creates a syntax error.

You can still argue that you will use something like this:
```c
if (condition)
    UNSAFE_MACRO(x)  // you need to remember not to add semicolon here.
else
    other_statement();
```
This will work, but it is not a good practice. Imagine you write a device driver, and every time the user wants to use this macro, they need to remember not to add a semicolon. Your manual should include this and developers need to follow your manual. This is not a good practice.

## The Solution: do{..}while(0)

Using `do-while(0)` solves these issues:
```c
#define BROKEN_MACRO(x) do { \
    statement1(x); \
    statement2(x); \
} while(0)
```

### Why It Works

1. **Single Statement**: The `do-while` construct makes the entire macro act as a single statement, even with multiple internal statements
2. **Semicolon Friendly**: It requires a trailing semicolon, maintaining consistent C syntax
3. **No Runtime Overhead**: The `while(0)` is optimized away by the compiler. Well, it depends on the compiler to optimize it away
4. **Block Scoping**: Variables declared inside the macro remain local to the macro. This prevents naming conflicts and unintended side effects

## Best Practices

1. Always use `do-while(0)` for multi-statement macros
2. Use backslashes for line continuation in macro definitions
3. Enclose all statement parameters in parentheses
4. Use block scope when declaring variables inside macros

## Example

Here's a complete example showing proper macro usage:

```c
#include <stdio.h>

#define SWAP(a, b) do { \
    int temp = (a); \
    (a) = (b); \
    (b) = temp; \
} while(0)

int main() {
    int x = 5, y = 10;
    if (x != y)
        SWAP(x,y);
    else
        printf("No swap required\n");
    printf("x = %d, y = %d\n", x, y);
    return 0;
}
```
After pre-processing, the above code would expand to:

```c
int main() {
    int x = 5, y = 10;
    if (x != y)
        do {
            int temp = (x);
            (x) = (y);
            (y) = temp;
        } while(0);
    else
        printf("No swap required\n");
    printf("x = %d, y = %d\n", x, y);
    return 0;
}
```
Output:
```bash
x = 10, y = 5
```

## Conclusion
Though the `do-while(0)` construct in macro might look strange at first, it offers a way to write safe macros in C. It is a common pattern used in the Linux kernel and other open-source C projects.
In fact, I found it in the Linux kernel source code, and thought I should show this to the world. What other clever macro techniques have you encountered? Let me know in the comments below.

