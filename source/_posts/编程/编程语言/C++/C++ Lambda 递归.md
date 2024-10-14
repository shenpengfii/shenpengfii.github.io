[](https://artificial-mind.net/blog/2020/09/12/recursive-lambdas#:~:text=Recursive%20Lambdas%20in%20C%2B%2B%20auto%20fib%20%3D%20%5B%5D,%3D%20fib%287%29%3B%20If%20only%20it%20were%20that%20simple)

```c++
#include <functional>
// Definition
std::function<int(int)> fib = [&](int n) {
    return n <= 1 ? n : fib(n - 1) + fib(n - 2);
};

// Practice
int main() {
    fib(7);
}
```

```c++
// Definition
auto fib = [](auto&& fib, int n) {
    return n <= 1 ? n : fib(n - 1) + fib(n - 2);
}

// Practice
int main() {
    fib(7, fib);
}
```
