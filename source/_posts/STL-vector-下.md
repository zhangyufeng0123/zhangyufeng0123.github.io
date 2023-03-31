---
title: STL-vector 下
categories:
  - C++
  - C++提高
abbrlink: b27c37e9
date: 2023-03-31 13:36:49
tags:
---

## vector基本概念

**功能**：

vector数据结构和**数组非常相似**，也成为**单端数组**

**vector与普通数组的区别**：

不同之处在于数组是静态空间，而vector可以**动态扩展**

**动态扩展**：

并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间

## vector构造函数

**功能描述**：

创建vector容器

函数原型：

- `vector<T> v;`//采用模板实现类实现，默认构造函数
- `vector(v.begin(), v.end());`//将v[begin(), end())区间中的元素拷贝给本身。
- `vector(n, elem);`//构造函数将n个elem拷贝给本身。
- `vector(const vector &vec);`//拷贝构造函数。

**示例：**

```cpp
#include <vector>

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01(){
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int> v2(v1.begin(), v1.end());
	printVector(v2);

	vector<int> v3(10, 100);
	printVector(v3);
	
	vector<int> v4(v3);
	printVector(v4);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131647.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131647.png)

- 总结
    
    vector的多种构造方式没有可比性，灵活使用即可
    

## vector赋值操作

**功能描述：**

给vector容器进行赋值

函数原型：

- `vector &operator=(const vector &vec);`//重载等号操作符
- `assign(beg, end);`//将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n, elem);`//将n个elem拷贝赋值给本身。

**示例：**

```cpp
#include <vector>

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//赋值操作
void test01(){
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	v2 = v1;
	printVector(v2);

	vector<int>v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int>v4;
	v4.assign(10, 100);
	printVector(v4);
}

int main() {
	test01();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131810.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131810.png)

- 总结
    
    vector赋值方式比较简单，使用operator=，或者assign都可以
    

## vector容量和大小

**功能描述**：

对vector容量的容器和大小操作

函数原型：

- `empty();`//判断容器是否为空
- `capacity();`//容器的容量
- `size();`//返回容器中元素的个数
- `resize(int num);`//重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
- `resize(int num, elem);`//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。//如果容器变短，则末尾超出容器长度的元素被删除

**示例**：

```cpp
#include <vector>

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01(){
	vector<int> v1;
	for (int i = 0; i < 10; i++)	{
		v1.push_back(i);
	}
	printVector(v1);
	if (v1.empty())	{
		cout << "v1为空" << endl;
	}	else	{
		cout << "v1不为空" << endl;
		cout << "v1的容量 = " << v1.capacity() << endl;
		cout << "v1的大小 = " << v1.size() << endl;
	}

	//resize 重新指定大小 ，若指定的更大，默认用0填充新位置，可以利用重载版本替换默认填充
	v1.resize(15,10);
	printVector(v1);

	//resize 重新指定大小 ，若指定的更小，超出部分元素被删除
	v1.resize(5);
	printVector(v1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131911.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331131911.png)

- 总结
    - 判断是否为空 ——— empty
    - 返回元素个数 ——— size
    - 返回容器容量 ——— capacity
    - 重新指定大小 ——— resize

## vector插入和删除

**功能描述：**

对vector容器进行插入、删除操作

**函数原型：**

- `push_back(elem);`//尾部插入元素elem
- `pop_back();`//删除最后一个元素
- `insert(cosnt_iterator pos, elem);`//迭代器指向位置pos插入元素elem
- `insert(const_iterator pos, int count, elem);`//迭代器指向位置pos插入count个元素elem
- `erase(const_iterator pos);`//删除迭代器指向的元素
- `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
- `clear();`//删除容器中所有元素

**示例：**

```cpp
#include <vector>

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01(){
	vector<int> v1;
	//尾插
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);
	v1.push_back(50);
	printVector(v1);
	//尾删
	v1.pop_back();
	printVector(v1);
	//插入
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 2, 1000);
	printVector(v1);

	//删除
	v1.erase(v1.begin());
	printVector(v1);

	//清空
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133324.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133324.png)

- 总结
    - 尾插 ——— push_back()
    - 尾删 ——— pop_back()
    - 插入 ——— insert(位置迭代器)
    - 删除 ——— erase(位置迭代器)
    - 清空 ——— clear
    

## vector数据存储

**功能描述：**

对vector中的数据的存取操作

函数原型：

- `at(int idx);`//返回索引idx所指的数据
- `operator[];`//返回索引idx所指的数据
- `front();`//返回容器中第一个数据元素
- `back();`//返回容器中最后一个数据元素

**示例**：

```cpp
#include <vector>

void test01(){
	vector<int>v1;
	for (int i = 0; i < 10; i++)	{
		v1.push_back(i);
	}

	for (int i = 0; i < v1.size(); i++)	{
		cout << v1[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < v1.size(); i++)	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	cout << "v1的第一个元素为： " << v1.front() << endl;
	cout << "v1的最后一个元素为： " << v1.back() << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133417.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133417.png)

- 总结
    - 除了用迭代器获取vector容器元素，[]和at也可以
    - front返回容器第一个元素
    - back返回容器最后一个元素

## vector互换容器

**功能描述：**

实现两个容器内元素进行互换

**函数原型：**

- `swap(vec);`// 将vec与本身的元素互换

**示例：**

```cpp
#include <vector>

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01(){
	vector<int>v1;
	for (int i = 0; i < 10; i++)	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	for (int i = 10; i > 0; i--)	{
		v2.push_back(i);
	}
	printVector(v2);

	//互换容器
	cout << "互换后" << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

void test02(){
	vector<int> v;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
	}

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	//收缩内存
	vector<int>(v).swap(v); //匿名对象

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133519.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133519.png)

- 总结
    
    swap可以使两个容器互换，可以达到实用的收缩内存效果
    

## vector预留空间

**功能描述：**

减少vector在动态扩展容量时的扩展次数

**函数原型：**

- `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

**示例：**

```cpp
#include <vector>

void test01(){
	vector<int> v;

	//预留空间
	v.reserve(100000);

	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}

	cout << "num:" << num << endl;
}

int main() {

	test01();
    
	system("pause");

	return 0;
}
```

![https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133605.png](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230331133605.png)