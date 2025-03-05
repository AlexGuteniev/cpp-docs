---
description: "Learn more about: Compiler Error C2526"
title: "Compiler Error C2526"
ms.date: "03/08/2024"
f1_keywords: ["C2526"]
helpviewer_keywords: ["C2526"]
---
# Compiler Error C2526

'identifier1' : C linkage function cannot return C++ class 'identifier2'

A function defined with C linkage cannot return a user-defined type.

The following sample generates C2526:

```cpp
// C2526.cpp
// compile with: /c
template <typename T>
class A {};

extern "C" A<int> func()   // C2526
{
    return A<int>();
}
```
