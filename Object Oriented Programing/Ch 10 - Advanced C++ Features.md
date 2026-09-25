# Advanced C++ Features

This chapter covers some important modern C++ features introduced in C++11 and later.

---

## 1. Move Semantics

Move semantics are used to **transfer resources instead of copying them**.

This is useful when objects contain expensive resources such as:

* Heap memory
* File handles
* Large data

### Lvalue and Rvalue

```cpp
int a = 5;        // lvalue
int& ref = a;     // lvalue reference

int&& rref = 42;  // rvalue reference
```

* `&` → lvalue reference
* `&&` → rvalue reference

### Move Constructor

A move constructor transfers resources from one object to another.

```cpp
Buffer(Buffer&& other) noexcept {
    data = other.data;
    other.data = nullptr;
}
```

Instead of copying the data, ownership is transferred.

### `std::move`

```cpp
vector<int> v1 = {1, 2, 3};
vector<int> v2 = std::move(v1);
```

`std::move()` does **not actually move anything by itself**.

It converts an object into an rvalue so that the move constructor or move assignment can be used.

### `std::forward`

`std::forward()` is mainly used in templates to preserve whether an argument was originally an lvalue or rvalue.

---

## 2. Rule of Five

When a class manages a resource, five special functions may be needed:

1. Destructor
2. Copy Constructor
3. Copy Assignment Operator
4. Move Constructor
5. Move Assignment Operator

### Rule of Zero

Prefer using RAII classes such as:

```cpp
std::vector
std::string
std::unique_ptr
std::shared_ptr
```

so that you don't have to manually manage these operations.

---

## 3. Type Traits

Type traits provide **information about types at compile time**.

Header:

```cpp
#include <type_traits>
```

Common type traits:

```cpp
std::is_integral<T>
std::is_floating_point<T>
std::is_class<T>
std::is_pointer<T>
std::is_reference<T>
std::is_same<T, U>
```

Example:

```cpp
static_assert(
    std::is_integral<int>::value,
    "T must be integral"
);
```

---

## 4. SFINAE

SFINAE means:

**Substitution Failure Is Not An Error**

It allows template functions to be conditionally enabled depending on the type.

C++17 provides a simpler alternative for many cases:

```cpp
if constexpr
```

Example:

```cpp
template<typename T>
auto half(T value) {

    if constexpr (std::is_integral<T>::value) {
        return value / 2;
    }
    else {
        return value / 2.0;
    }
}
```

---

## 5. `auto` and `decltype`

### `auto`

The compiler automatically determines the type.

```cpp
int x = 5;
auto y = x;
```

Here `y` is `int`.

### `decltype`

Gets the type of an expression.

```cpp
int x = 5;

decltype(x) y = x;
```

Here `y` is `int`.

Unlike `auto`, `decltype` can preserve references and `const`.

---

## 6. User-Defined Literals

User-defined literals allow us to create custom suffixes for literals.

Example:

```cpp
10_s
```

The suffix can be defined using:

```cpp
operator"" _s
```

Example:

```cpp
std::chrono::seconds operator"" _s(unsigned long long val) {
    return std::chrono::seconds(val);
}
```

Now:

```cpp
auto duration = 10_s;
```

means 10 seconds.

> User-defined literal suffixes should start with `_`.

---

# 7. Concurrency

Concurrency allows multiple tasks to execute at the same time.

---

## `std::thread`

Used to create a thread.

```cpp
void worker(int id) {
    cout << "Thread running";
}

std::thread t1(worker, 1);
```

### `join()`

```cpp
t1.join();
```

Waits for the thread to finish.

### `detach()`

Allows the thread to continue independently.

---

## `std::jthread`

`std::jthread` was introduced in C++20.

It automatically joins when it goes out of scope and supports cooperative interruption.

```cpp
std::jthread t(worker);
```

---

# 8. Mutex

A mutex is used to **protect shared data** from multiple threads accessing it at the same time.

```cpp
std::mutex mtx;
```

### `std::lock_guard`

```cpp
std::lock_guard<std::mutex> lock(mtx);
++counter;
```

It automatically locks and unlocks the mutex using RAII.

### `std::unique_lock`

`unique_lock` is more flexible than `lock_guard`.

It supports things such as:

* Deferred locking
* Unlocking
* Timed locking

---

# 9. `std::async` and `std::future`

`std::async` runs a function asynchronously.

```cpp
int compute(int x) {
    return x * x;
}

std::future<int> result =
    std::async(std::launch::async, compute, 5);

cout << result.get();
```

Output:

```text
25
```

`future` represents a result that will be available later.

---

# 10. `std::promise`

A `promise` is used to provide a value that can later be obtained through a `future`.

```cpp
std::promise<int> prom;
std::future<int> fut = prom.get_future();

prom.set_value(42);

cout << fut.get();
```

Output:

```text
42
```

Simple idea:

```text
promise
   ↓
sets value
   ↓
future
   ↓
gets value
```

---

# 11. `std::atomic`

Atomic variables allow operations to happen safely when accessed by multiple threads.

```cpp
std::atomic<int> counter = 0;

++counter;
```

Common operations include:

```cpp
load()
store()
exchange()
compare_exchange_weak()
compare_exchange_strong()
```

---

# Quick Revision

| Concept              | Main Idea                             |
| -------------------- | ------------------------------------- |
| Move Semantics       | Transfer resources instead of copying |
| `&&`                 | Rvalue reference                      |
| `std::move`          | Cast to rvalue                        |
| `std::forward`       | Preserve value category               |
| Rule of Five         | Five resource-management functions    |
| Rule of Zero         | Let RAII types manage resources       |
| Type Traits          | Compile-time type information         |
| SFINAE               | Conditional template selection        |
| `auto`               | Automatic type deduction              |
| `decltype`           | Get type of expression                |
| User-Defined Literal | Create custom literal suffix          |
| `std::thread`        | Create a thread                       |
| `join()`             | Wait for thread                       |
| `std::jthread`       | C++20 auto-joining thread             |
| `mutex`              | Protect shared data                   |
| `lock_guard`         | RAII-based locking                    |
| `async`              | Run task asynchronously               |
| `future`             | Receive future result                 |
| `promise`            | Provide result to a future            |
| `atomic`             | Safe atomic operations                |
