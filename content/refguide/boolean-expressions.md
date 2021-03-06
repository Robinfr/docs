---
title: "Boolean Expressions"
parent: "expressions"
tags: ["studio pro"]
---

## 1 Introduction

Boolean expressions can be used to perform logical operations such as checking if two conditions hold.

## 2 and

Combines two Boolean expressions and only returns True if both of the expressions evaluate to True.

{{% alert type="info" %}}

```java
(6 > 4) and (3 < 5)
```

evaluates to True because both of the expressions are True.

```java
('hello' = 'hallo') and (3 < 5)
```

evaluates to False, because only the second expression is True.

{{% /alert %}}

## 3 or

Combines two Boolean expressions, and returns True if at least one of the expressions evaluates to True.

{{% alert type="info" %}}

Given a domain entity instance with name "$product" that has an integer attribute "price" with value "3" and another integer attribute "recommendedPrice" with value "2", the following expression:

```java
($product/price < $product/recommendedPrice : 2) or ($product/price > 0)
```

will return True because at least one of the expressions evaluates to True (the second one, to be precise). Note that the expression would still return True if both statements had been True.

The following example returns False, because both expressions evaluate to False:

```java
('hello' = 'nothello') or ('byebye' = 'stillnotbyebye')
```

{{% /alert %}}

## 4 not

The function 'not' negates the specified Boolean expression.

### 4.1 Input

An expression of type Boolean.

### 4.2 Output

Returns the negation of the specified expression. If the expression evaluates to True, it returns False; otherwise it returns True.

{{% alert type="info" %}}

```java
not('hello' = 'hallo')

```

returns:

```java
true

```

and

```java
not(true)

```

returns:

```java
false

```

{{% /alert %}}
