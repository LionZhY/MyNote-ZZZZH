以下是常用 C++ 标准函数、容器与算法的 **命名空间、头文件和标准支持版本** 对照表



常用函数与算法

| 函数 / 算法            | 所属命名空间  | 头文件        | 支持标准版本 |
| ---------------------- | ------------- | ------------- | ------------ |
| `std::max(a, b)`       | `std`         | `<algorithm>` | C++98        |
| `std::min(a, b)`       | `std`         | `<algorithm>` | C++98        |
| `std::sort()`          | `std`         | `<algorithm>` | C++98        |
| `std::ranges::sort()`  | `std::ranges` | `<algorithm>` | C++20        |
| `std::swap()`          | `std`         | `<utility>`   | C++98        |
| `std::abs()`           | `std`         | `<cmath>`     | C++98        |
| `std::accumulate()`    | `std`         | `<numeric>`   | C++98        |
| `std::binary_search()` | `std`         | `<algorithm>` | C++98        |
| `std::clamp()`         | `std`         | `<algorithm>` | C++17        |



常用容器

| 容器名称             | 所属命名空间 | 头文件            | 支持标准版本 |
| -------------------- | ------------ | ----------------- | ------------ |
| `std::vector`        | `std`        | `<vector>`        | C++98        |
| `std::string`        | `std`        | `<string>`        | C++98        |
| `std::set`           | `std`        | `<set>`           | C++98        |
| `std::unordered_set` | `std`        | `<unordered_set>` | C++11        |
| `std::map`           | `std`        | `<map>`           | C++98        |
| `std::unordered_map` | `std`        | `<unordered_map>` | C++11        |
| `std::stack`         | `std`        | `<stack>`         | C++98        |
| `std::queue`         | `std`        | `<queue>`         | C++98        |



其他常用工具类函数

| 工具函数 / 类型   | 所属命名空间 | 头文件         | 支持标准版本 |
| ----------------- | ------------ | -------------- | ------------ |
| `std::pair`       | `std`        | `<utility>`    | C++98        |
| `std::make_pair`  | `std`        | `<utility>`    | C++98        |
| `std::tuple`      | `std`        | `<tuple>`      | C++11        |
| `std::make_tuple` | `std`        | `<tuple>`      | C++11        |
| `std::function`   | `std`        | `<functional>` | C++11        |
| `std::bind`       | `std`        | `<functional>` | C++11        |
| `std::move`       | `std`        | `<utility>`    | C++11        |





## 大顶堆，小顶堆

在 C++ STL 中，默认的 `priority_queue` 是 **大顶堆**（大的元素在前）

 如果你想要一个最小堆，必须让比较函数“反过来”，比如int类型

~~~C++
auto cmp = [](int a, int b) { return a > b; }; // 小的优先
std::priority_queue<int, std::vector<int>, decltype(cmp)> minHeap(cmp);
~~~

在 C++ STL 的 `priority_queue` 中，**比较函数 `cmp(a, b)` 的语义是**：

> “如果 `cmp(a, b)` 为 `true`，则认为 `a` 的优先级**低于** `b`，所以 `a` 会在 `b` 的**后面**。”

所以 `return a > b;` 表示 `a > b`时，返回true，大的数在后面，小的数在前。



**默认写法（最大堆）**

~~~C++
std::priority_queue<int> maxHeap;  // 最大堆，大的优先（使用 less<int>）
~~~



**小顶堆写法**

~~~C++
std::priority_queue<int, std::vector<int>, decltype(cmp)> minHeap(cmp);
~~~

定义一个**最小堆**类型的 `priority_queue`，元素类型为 `int`，底层容器为 `vector<int>`，排序规则由一个叫 `cmp` 的比较函数决定。

`decltype(expr)` 是 C++11 中的一个**类型推导工具**，表示“**expr 的类型**”。