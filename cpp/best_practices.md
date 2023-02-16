# Best practices
## Table of contents
* [Headers](#headers)

## Headers

### Headers order

Related header\
Local headers\
Sub-Project headers\
Third-Party headers\
Systems  headers

Example:
```cpp
// database.cpp

#include "database.h"
#include "database_tools.h"
#include "local_lib.h"
#include "gtest/gtest.h"
#include <iostream>
```

### Do not use using in header files

1. Inconsistency: Header files are often included in many different source files.
2. Increased compilation time.
3. Ambiguity: using instructions can introduce ambiguity

