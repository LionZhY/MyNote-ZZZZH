# C++项目目录结构





# std命名空间

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





# 大顶堆，小顶堆

> 大顶堆小顶堆就是一棵完全二叉树，其中父节点大于或小于左右子节点。
>
> 在 C++ 中，大顶堆和小顶堆可以通过 `priority_queue` 容器非常方便地实现。

**大顶堆**：在 C++ STL 中，默认的 `priority_queue` 就是 大顶堆（大的元素在top）

~~~C++
priority_queue<int> maxHeap; // 默认是大顶堆

// priority_queue<int, vector<int>, less<int>> maxHeap; // 默认就是less
~~~

**小顶堆**（小的元素在top），必须让**比较函数“反过来”**

~~~C++
priority_queue<int, vector<int>, greater<int>> minHeap; // 小顶堆
~~~

定义一个**最小堆**类型的 `priority_queue`，元素类型为 `int`，底层容器为 `vector<int>`，排序规则由一个叫 `greater<>` 的比较函数（可以直接用）决定。



> 比较函数：
>
> ~~~C++
> auto cmp = [](int a, int b) { return a > b; }; // 小的优先
> std::priority_queue<int, std::vector<int>, decltype(cmp)> minHeap;
> ~~~
>
> 在 C++ STL 的 `priority_queue` 中，**比较函数 `cmp(a, b)` 的语义是**：
>
> ​	“如果 `cmp(a, b)` 为 `true`，则认为 `a` 的优先级**低于** `b`，所以 `a` 会在 `b` 的**后面**。”
>
> 所以 `return a > b;` 表示 `a > b`时，返回true，大的数在后面，小的数在前（top）。
>
> 
>
> `decltype(expr)` 是 C++11 中的一个**类型推导工具**，表示“**expr 的类型**”。





**自定义结构体的堆**

比如按 pair 的第二个元素升序（小顶堆），需要写仿函数

~~~C++
// 小顶堆：仿函数改变 priority_queue 排序
    class mycomparison{
    public:
        bool operator() (const pair<int, int>& lhs, const pair<int, int>& rhs)
        {
            return lhs.second > rhs.second; // 次序，较小的在top
        }
    };

priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;
~~~



# C++20 `<ranges>` 

`std::ranges` 基本覆盖了 `<algorithm>` 里常用的函数，用法更简洁、安全，并且扩展了比较器 + 投影机制。

主要改进

1. **调用更简洁**：直接传容器，无需写 `.begin(), .end()`

   ```
   std::vector<int> v{3,1,4};
   auto it = std::ranges::max_element(v); // ✅ 更直观
   ```

2. **支持投影 (projection)**：直接指定比较属性

   ```
   struct Point { int x,y; };
   std::vector<Point> ps{{1,2},{3,1},{2,5}};
   auto p = *std::ranges::max_element(ps, {}, &Point::y); // 按 y 比较
   ```

3. **Concepts 约束**：参数类型检查更严格，编译期报错更清晰。



C++20 `std::ranges` 常用函数

| 类别         | 传统 `<algorithm>`                                           | C++20 `<ranges>`                                           | 用法区别与优势                                               |
| ------------ | ------------------------------------------------------------ | ---------------------------------------------------------- | ------------------------------------------------------------ |
| **最值**     | `max(a, b)` / `min(a, b)`                                    | `ranges::max(a, b)` `ranges::min(a, b)                     | `ranges::max()` 可直接作用于整个 range ⭐<br />如 `ranges::max(vec)` |
|              | `minmax(a, b)`                                               | `ranges::minmax(a, b)`                                     | 同上，可直接处理容器。                                       |
| **极值元素** | `max_element(vec.begin(), vec.end())`<br />`min_element(...)` | `ranges::max_element(vec)`<br />`ranges::min_element(...)` | 不需要 `.begin(), .end()`，更简洁、更安全。⭐                 |
| **查找**     | `find(vec.begin(), vec.end(), value)`                        | `ranges::find(vec, value)`                                 | 直接传容器，不必写迭代器。⭐                                  |
|              | `find_if(vec.begin(), vec.end(), pred)`                      | `ranges::find_if(vec, pred)`                               | 简化写法，更清晰。                                           |
| **计数**     | `count(vec.begin(), vec.end(), value)`                       | `ranges::count(vec, value)`                                | 免去 `.begin(), .end()`。⭐                                   |
|              | `count_if(vec.begin(), vec.end(), pred)`                     | `ranges::count_if(vec, pred)`                              | 更简洁。                                                     |
| **遍历**     | `for_each(vec.begin(), vec.end(), f)`                        | `ranges::for_each(vec, f)`                                 | 直接作用于容器。                                             |
| **排序**     | `sort(vec.begin(), vec.end())`                               | `ranges::sort(vec)`                                        | API 更直观，自动推断范围⭐                                    |
|              | `stable_sort(...)`                                           | `ranges::stable_sort(...)`                                 | 同理。                                                       |
|              | `is_sorted(vec.begin(), vec.end())`                          | `ranges::is_sorted(vec)`                                   | 写法更简洁。                                                 |
| **拷贝**     | `copy(src.begin(), src.end(), dest.begin())`                 | `ranges::copy(src, dest.begin())`                          | 源区间可直接传容器。                                         |
|              | `copy_if(...)`                                               | `ranges::copy_if(src, dest.begin(), pred)`                 | 同理，避免迭代器错误。                                       |
| **比较区间** | `equal(a.begin(), a.end(), b.begin())`                       | `ranges::equal(a, b)`                                      | 不必传 `end()`，减少错误。                                   |



# end
