# 容器

## 线性

+ std::array
+ std::vector
+ std::forward_list, std::list
+ std::stack, std::queue

## 树状

+ 红黑树，有序 std::map / std::set / std::multimap / std::multiset，根据 < 运算符 判断
+ 哈希表，无序 std::unordered_[map, set, multimap, multiset]

## 元组

+ 序列对：std::pair<T1, T2>
+  元组: std::make_tuple0, std::get<index>, std::tie