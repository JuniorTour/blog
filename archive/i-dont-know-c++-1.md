---
title: 我不知道的C++（一）
tags:
  - cpp
  - algorithm
categories:
  - cpp
date: 2018-04-04 11:25:00
---






```
0. 输入输出
     1. 保留两位小数
     2. 二维数组作为函数参数
1. <string>
   1. s.append(char); / s+char;
   2. substr(startPos, len);
   3. s.find(subStr);
   4. s.compare(anotherStr) == 0;
   5. char to string
   6. int to string
   7. int to char
   8. string + string = number
2. <set> C++ set操作
3. <algorithm>
   1. 升序排序
   2. 字符串字典序排序
   3. 倒序
      1. 倒序字符串
      2. 倒序数组
      3. 倒序向量
   4. unique()去重
   5. sort的回调函数
4. <stack>
5. <vector> and <sstream>
6. <map>
7. 原理
   1. 变量溢出
   2. 编译错误
8. 常用算法
   1. 是否是素数
   2. 递归
```



<!--more-->




### 0. 输入输出

0.  根据输入判断输出负号，可以提前先输出一个

```
// https://www.nowcoder.com/questionTerminal/ac61207721a34b74b06597fe6eb67c52

if(m<0){
   m=-m;
   cout<<"-";
 }
```



1. 单个数组未知长度-Single Array without length

``` c++
// 这个输入方式会把最后的结束符也输入，注意！！！
/*while (cin) {
  cin >> i;
  inputArr[i]++;
  n++;
}*/

int t;
while(cin >> t) {
	arr.push_back(t);
}


// 本地调试时，可以输入ctrl+d/Z表示结束输入。当cin遇到文件结束符（windows中为：ctrl +Z , Unix 中为：ctrl +D），或无效输入才能使cin状态无效。http://blog.csdn.net/u014182411/article/details/62053816
```



#### 1. 保留两位小数

```c++

#include <iomanip>

double sum = 123.666; 

cout.precision(4);			// 后续所有输出长度为4，注意！包括整数位 
cout << sum << endl;		// 123.7

cout<<setiosflags(ios::fixed)<<setprecision(2);
// 后续输出，保留两位小数 
cout << sum << endl;		// 123.67

std::cout << "scientific:\n" << std::scientific;
cout << sum << endl;		// 124e+002 
```



#### 2.  二维数组作为函数参数



```c++
void splitNum(int n, int (&digits)[4]) {
	
	int i = 3;
	while(i>=0) {
		if (n>0) {
			digits[i] =  n % 10;
			n /= 10;
		}
		i--;
	}
}

int eachDigits[n][4];

splitNum(arr[i], eachDigits[i]);
```









### 1.  `<string>`



#### 1. s.append(char); / s+char;

```c++
string s = "123";
	
s+="4";
	
cout << s;
```



#### 2. substr(startPos, len);



#### 3. s.find(subStr);



#### 4. s.compare(anotherStr) == 0;



#### 5. char to string

```c++
string target = "abc";
//	const char* name = typeid(target[0]).name();
cout << typeid(target[0]).name() << endl;	// c => char

string code = "";	
code += target[0];	// a
cout << code << endl;
```



#### 6. int to string

```c++
#include <sstream>

int sum = 1;

ostringstream ss;
ss << sum;
string sumChar = ss.str();

string sumStr.insert(0, sumChar);
```





#### 7. int to char

```c++
#include <sstream>

int number=33;

stringstream strs;
strs << number;
string temp_str = strs.str();	// 33

// c++03: string.c_str()
//char* char_type = (char*) temp_str.c_str();


/*Second*/

int t = 2;
char c = (char) t+48;

cout << c << endl; 	// 2
```



#### 8.  string + string = number



```c++
string a = "123", b = "456";

cout << ((a + b) < (b + a)) << endl;	// 1
```









### 2. `<set>` 

[std::set http://www.cplusplus.com/reference/set/set/](http://www.cplusplus.com/reference/set/set/)

set默认有一定顺序，内部的元素都是**唯一的**！
.insert(), .erase()等方法都会自动搜索set的内容进行操作。

```c++
#include <iostream>
#include <set>
#include <string>
using namespace std;

 
int main() {
    string str;
    set<string> datas;
    while (cin >> str) {
        datas.insert(str);
    }
    cout << datas.size() << endl;
  
  // 遍历set
  for (set<int>::iterator it=outcome.begin(); it!=outcome.end(); ++it) {
    cout << ' ' << *it;
  }
  
  /*链接：https://www.nowcoder.com/questionTerminal/635ff765d4af45b5bf8e3756ed415792
	来源：牛客网*/

/*有格式的输出，元素之间以空格分隔，最后没有空格。*/
  int size = st.size();
  set<int>::iterator it = st.begin();
  for (int i = 0; i < size - 1; ++i)
  {
    cout << *it << " ";
    ++it;
  }
  cout << *it << endl;
  
  return 0;
}
```



### 3.  `<algorithm>`



#### 1. 升序排序

```c++
// c++ 原生升序排序API

#include <algorithm>    // std::sort
/*std::sort封装了快速排序，不稳定（不保证相等元素的相对位置），原理和随机迭代器有关。
  还有一个std::stable_sort，是稳定的快速排序。*/

int testArr[3] = {3,2,1};

std::sort(testArr, testArr + 3);	// 传入的参数是：(数组头指针，数组尾指针)

cout << "testArr[0] = " << testArr[0] << endl;	// 1
cout << "testArr[1] = " << testArr[1] << endl;	// 2
cout << "testArr[2] = " << testArr[2] << endl;	// 3


// 示例
	int n;
	cin >> n;
	
	int* quesArr = new int(n);
	for (int i=0;i<n;i++) {
		cin >> quesArr[i];
	}
	
	sort(quesArr, quesArr+n);
```



#### 2. 字符串字典序排序

```c++
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main() {
	string strArr[3] = {"aa","aba","ab"};
	
	sort(strArr,strArr+3);
	
	cout << strArr[0] << endl;	// aa
	cout << strArr[1] << endl;	// ab
	cout << strArr[2] << endl;	// aba
	return 0;
} 

/*字典序排列规则注意用例：
1. aaa,a	no
2. abc,az	yes*/

/*string class 的原生字典序比较：
if (prevStr.compare(nextStr) > 0) {
	isABC = false;
}*/
```





#### 3. 倒序

http://www.cplusplus.com/reference/algorithm/reverse/

#### 倒序字符串

```c++
#include <algorithm>


string s;
cin << s;

reverse(s.begin(),s.end());

cout >> s;
```



#### 倒序数组

```c++
#include <algorithm>


int arr[4] = {1,2,3,4};
reverse(arr,arr+4);

cout << arr[0];	// 4
```



#### 倒序向量

```c++
#include <algorithm>    // std::reverse
#include <vector>       // std::vector


  std::vector<int> myvector;

  // set some values:
  for (int i=1; i<10; ++i) myvector.push_back(i);   // 1 2 3 4 5 6 7 8 9

  std::reverse(myvector.begin(),myvector.end());    // 9 8 7 6 5 4 3 2 1
```



#### 4. unique()去重

http://www.cplusplus.com/reference/algorithm/unique/?kw=unique

```c++
#include<algorithm>  

vector<int> v;

// push something

vector<int>::iterator it = unique (v.begin(), v.end() );  

v.erase (it, v.end() );//这里就是把后面藏起来的重复元素删除了  
```



#### 5. sort的回调函数

sort的第三个参数可以是一个`lambda`函数，即JavaScript中的匿名函数。

写法如下，**注意一对方括号**

```c++

int n;
cin >> n;

string input[n];

for (int i=0;i<n;i++) {
  cin >> input[i];
}
	
sort(input, input+n, [](string s1, string s2){
  return ((s1+s2) > (s2+s1));
});
	
for (int i=0;i<n;i++) {
  cout << input[i];
}

cout << endl;
```







###  4. `<stack> `



```c++
// c++ 原生栈操作api

#include <iostream>
#include <stack>

using namespace std;

int main() {
	int arr[] = {1,2,3,4};
	
// [Error] no matching function for call to 'std::stack<int>::stack(int [4])' 
//	stack<int> stackArr(arr);
	
	stack<int> stackArr;
	
	stackArr.push(199);
	
	if (!stackArr.empty()) {
		cout << stackArr.size() << endl;
      	// .pop()返回的是void，不能用来获取内容
		cout << stackArr.top() << endl;
	}
	
	stackArr.pop();
	
	return 0;
} 

```



### 5. `<vector>` and `<sstream>`

`vector`这个向量和数学中的向量不一样，这里的向量是一种能`自动增长长度`的数据结构。

``` c++
#include <iostream>
#include <vector>
#include <sstream>

vector<string> myVector;

for (int i=0;i<10;i++) {
  stringstream ss;
  ss << i;
  // myVector.push_back(string(i));	// 不能直接把int转成string 
  myVector.push_back(ss.str());
}

cout << myVector[0] << endl;
cout << myVector[4] << endl;
cout << "Size before pop_back()" << myVector.size() << endl;	// 10

myVector.pop_back();

cout << "Size AFTER pop_back()" << myVector.size() << endl;		// 9


cout << "\nTest Integer Vector:" << endl;
vector<int> v2;

for (int i=0;i<10;i++) {
  v2.push_back(i);
}


cout << v2[0] << endl;
cout << v2[4] << endl;

cout << v2[99] << endl;	// 一个随机的“垃圾数”，但程序不会崩溃。 
// vector对象发生了溢出后，仍然有一种保护机制来继续程序的运行。
	
```



``` c++

//  vector<int> res(10);
// (10)标识声明一个容量为10的向量，其值初始化为0. 
//  cout << res[9] << endl;		// 0
//  cout << res[10] << endl;	// 4613246851
//  cout << res[11] << endl;	// 6124654653
```



### 6. `<map>`

键值对



```c++

  map<string, double> map;
  
  
  int n = 3;
  for (int i=0;i<n;i++) {
  	double difficulty = i / 2.0;
  	string name;
  	name += i + '0';
  	cout << name << endl;
  	
    map.insert(pair<string, double>(name, difficulty));
  }
  
// Iteration
for (std::map<string,double>::iterator it=map.begin(); it!=map.end(); ++it) {
  std::cout << it->first << " => " << it->second << '\n';
}

/*
0
1
2
0 => 0
1 => 0.5
2 => 1*/
```





### 原理

#### 1.  变量溢出 

当输入中有较大的数（如10^9）时，就要注意int类型（32位，正负21亿）够不够用！通常两个几十万以上的int相乘时，就有可能出现变量溢出，导致结果成为负数。

```c++
int row = 87654321, col = 87654321;

int outcome = (row * col); 

cout << outcome;	// -836816543，负数，变量溢出

// 订正为足够长的long long类型：
long long row = 87654321, col = 87654321;

long long outcome = (row * col); 

cout << outcome;	// 7683279989971041，正确！
```



这时候可以考虑换用`long long` 类型（64位长，19位十进制数）。

另附基本类型长度：

> unsigned int  		0～4,294,967,295 （42亿）
> int  					-2147483648～2147483647 (21亿)
> unsigned long 		0～4294967295（42亿）
>
> long   				2147483648～2,147,483,647 (21亿)
> long long的最大值：	9223372036854775807 （19位）
> long long的最小值：	-9223372036854775808
> unsigned long long的最大值：	1844674407370955161
>
> __int64的最大值：9223372036854775807
> __int64的最小值：-9223372036854775808
> unsigned __int64的最大值：18446744073709551615



#### 2. 编译错误

- 牛客网C++编译器：`error: control may reach end of non-void function`

对非void类型的函数，需要仔细检查每一个流程是否都有return值。如果有些分支没有return就有这个错。

```c++
// 腾讯2017-编码
int generateCodeArr(vector<string> &codeArr, string code, int cutPos, string target) {
	char insertChar = 'a';
	for (int i=0;i<25;i++) {
		code = code.substr(0,cutPos);
		
		code += insertChar;
		codeArr.push_back(code);
		
		if (code == target) {
			int blockNum = (target[0] - 'a') * 16276;
			
//			cout << "codeArr.size() = " << codeArr.size() << endl;
			cout << blockNum+codeArr.size()-1 << endl;
			return 0;
		}
		
		if (cutPos < 3) {
			int innerRet = generateCodeArr(codeArr, code, cutPos + 1, target);
			if (innerRet == 0) {
				return innerRet;
			}
		}
		
		insertChar++;
	}	
  
  /*此处如果没有return值，就会报上述错误。*/
	return 1; 
}
```




## 常用算法



### 0. 是否是素数

```c++
bool isPrime(int x) {
	int y;
	if (x<=1) 
		return false;
	for(y=2;y<=sqrt(x);y++)
		if (x%y==0)
      		return false;
	 return true;
}
```



### 1. 递归

```c++
void generateCodeArr(vector<string> &codeArr, string code, int cutPos) {
	char insertChar = 'a';
	for (int i=0;i<25;i++) {
		code = code.substr(0,cutPos);
		
		code += insertChar;
		codeArr.push_back(code);
		if (cutPos < 3) {
//			generateCodeArr(codeArr, code, ++cutPos);
/*此处如果将++cutPos传入，会产生作用域“错乱”的问题，
外层的函数的cutPos依然会是内层的值，就像是传递了引用一样？？*/
			generateCodeArr(codeArr, code, cutPos + 1);
		}
		
		insertChar++;
	}	
}
```



