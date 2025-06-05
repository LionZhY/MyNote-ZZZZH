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