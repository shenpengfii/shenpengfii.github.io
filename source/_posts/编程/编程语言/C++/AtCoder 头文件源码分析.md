## internal_queue.cpp - `atcoder::internal::simple_queue`

Implemented in this file is a naive queue class template `atcoder::internal::simple_queue`, which is similar to `std::queue` but more efficient. It saves the program's executing time a lot by removing many unnecessary queue operations.

该文件中实现了一个简单的队列类模板 `atcoder::internal::simple_queue`。它与 `std::queue` 类似，但在内部实现上更加高效，并且剔除了许多不常用的队列操作，这极大缩小了程序的运行时间。

`simple_queue` internally uses a `std::vector` to store the elements of the queue, and uses an index variable `pos` to support the deleting operation, avoiding the memory release process. However, even after the elements are deleted, they still exist in memory, so the memory waste of `simple_queue` is heavier than what is expected.

在内部实现中，`simple_queue` 使用了一个 `std::vector` 来存储队列中的元素，并使用了一个索引变量 `pos` 用于支持队列元素的删除，免去了内存释放的过程。但由于删除后的元素依然存在于内存中，`simple_queue` 的内存浪费会比想象中的要更严重一些。

Hence, you may try to use `simple_queue` ONLY if the data isn't too big. But if you need to frequently insert or remove plenty of elements, `std::queue` will be more suitable. 

因此，你只应该在数据总量较少的场景中尝试使用 `simple_queue`，而在需要频繁插入与删除大量元素的场景下，`std::queue` 会更加适合。

## maxflow.cpp - `atcoder::mf_graph`

Implemented in this file is a class template `atcoder::mf_graph` for solving the maximum flow problem through the Dinic algorithm.

该文件中实现了一个最大流算法类模板 `atcoder::mf_graph`。它是 Dinic 算法的 C++ 封装。


