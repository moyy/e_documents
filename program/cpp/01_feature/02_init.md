# 初始化

+ 成员 默认 初始化
+ 列表初始化
+ [C++ 20] 指定 成员 初始化
+ [C++ 17] if / switch 初始化

### 列表初始化

可以用 列表 初始化 一个自定义类

``` c++
struct C {
	C(int a) { }
	C(string a, int b) { }

	// 接受 std 的 列表进行初始化
	C(initializer_list<string> a) {
		// item 的 类型 不是 迭代器，而是 const T *
		for (auto item = a.begin(); item != a.end(); item++) {
			std::cout << *item << " ";
		}
		std::cout << std::endl;
	}
};
```

使用 类C:

``` c++
// 列表初始化：C++ 11
void initial_list_1() {
	int x1(8);
	int x2 = 5;

	C c1(1);
	C c2 = 2;
	C c3 = { 3 };
	C c4 = { "c4", 5 };
	C c5{ "a", "bb", "ccc" };
}
```

可以 列表初始化 标准库类型：

``` c++
void initial_list_2() {

	int a1[]{ 1, 2, 3, 4, 5 };
	int a2[] = { 1, 2, 3, 4, 5 };

	vector<int> v1{ 1, 2, 3, 4, 5 };
	vector<int> v2 = { 1, 2, 3, 4, 5 };

	// 内层 调用 std::pair 构造函数：(const T1 &x, const T2 &y)
	// 外层 调用 std::map(initializer_list<pair>, ...)
	map<string, int> m1{ {"apple", 4}, {"banana", 5} };
	map<string, int> m2 = { {"apple", 4}, {"banana", 5} };
}
```

### [C++ 20] 指定成员 初始化

``` c++
struct Point {
	// 成员 默认 初始化
	float x = 100.0;
	float y = 100.0;
	float z = 100.0;

	// static inline 声明时 初始化
	static float inline PI = 3.14159f;
};

void initial_list_3() {
	//  C++ 20 指定名称 初始化
	// 只能用于 聚合类型；顺序和成员声明一致
	// 聚合类型，就是 不含 成员方法 的 结构体 或 类，C -- POD
	Point p{ .y = 20.0, .z = 3.0 };

	std::cout << "Point(" << p.x << ", " << p.y << ", " << p.z << ")" << std::endl;
}
```

### [C++ 17] if 初始化

在 if 语句中 执行 初始化 变量

``` c++

void initial_if() {
	std::mutex mx;

	int shared_flag = 0;
	if (std::lock_guard<std::mutex> lock(mx); shared_flag == 0) {
		++shared_flag;
		std::cout << "shared_flag = " << shared_flag << std::endl;
	}
	else if (string s = "abcdefg"; shared_flag == 1) {
		shared_flag += 10;
		std::cout << "2 shared_flag = " << shared_flag << std::endl;
	}
	else {
		std::cout << "3 shared_flag = " << shared_flag << std::endl;
	}
}
```

### [C++ 17] switch 初始化

在 switch 语句 初始化

``` C++
void initial_switch() {
	std::mutex cv_m;
	std::condition_variable cv;

	switch (std::unique_lock<std::mutex> lk(cv_m); cv.wait_for(lk, 100ms)) {
	case std::cv_status::timeout:
		break;
	case std::cv_status::no_timeout:
		break;
	default:
		break;
	}
}
```