---
title: hello world
date: 2024-05-16
tags:
  - WorkFlow
summary: 测试第一篇博客上传
image:
---
## Initialization
### 阿迪斯

> direct initialization

```C++
int a = 12.0;
int b(12.0);
```

- 使用=或()
    
- 存在的问题：如果出现narrow conversion不会报错，这是潜在的bug
    
- narrow conversion：就是size大的类型转换到size小的类型，比如上面的double转到int
    

> uniform initialization (C++**11**)

```C++
int a{12};
vector<int> arr{1, 2, 3};
```

- 使用花括号{}
- 会检查narrow conversion
- 对**任何类型**都能使用uniform initialization
- asd

- asd 
- asd 十大
- 阿松大
- 阿松大


> structured binding (C++**17**)

```C++
std::tuple<int, int, int> func() {
  return {1, 2, 3}; // uniform initialization
}

auto [a, b, c] = func();  // structured binding
```

```C++
// arr  -- std::vector<std::pair<int, int>>
for (auto [a, b] : arr) {
  a++;
  b++;
}
```

- 支持同时给几个值赋值

使用**uniform initialization + structured binding**就非常好用了

## Reference
### 阿松大
#### 阿松大
### 123

> 一个由&和structured binding导致的常见bug

```C++
void shift(std::vector<std::pair<int, int>> &nums) {
  for (auto [a, b] : nums) {  // auto vs. auto&
    a++;
    b++;
  }
  nums.push_back({4, 4});
}

int main() {
  std::vector<std::pair<int, int>> nums{{1, 1}, {2, 2}, {3, 3}};
  shift(nums);

  for (auto [a, b] : nums) {
    std::cout << a << ' ' << b << std::endl;
  }

  return 0;
}
```

- 如果单纯是auto，则不改变pair里的值，输出11，22，33，44
    
- 如果是auto&，输出22，33，44，44
    

> l-value vs. r-value


```C++
void func(int& num) {
  std::cout << num << std::endl;
}

int a = 0;
func(a);  // √
func(2);  // x
```

- 不能将r-value传递给reference，因为r-value是暂时的，没有address；换句话说，**只能引用l-value**

> const和reference

```C++
  std::vector<int> vec{1, 2, 3};
  const std::vector<int> const_vec{1, 2, 3};
  std::vector<int> &vec_ref{vec};
  const std::vector<int> const_ref{vec};
  std::vector<int> &const_vec_ref{const_ref};  // compiler error，将const绑定到非const引用

  vec.push_back(4);
  const_vec.push_back(4);   // compiler error
  vec_ref.push_back(4);
  const_ref.push_back(4);   // compiler error
```

- **可以将非const类型绑定到const引用，反之则不行**


## 测试markdown
dsfsdf sdfs <mark>asd</mark> asdj akh kjsd kjkj

$asd: 1+2^2 = 5$

$$
1 + \frac{1}{2} = 1.5
$$

`int a = 10`

## 测试图片
![](Pasted%20image%2020240516205354.png)

- [ ] 阿松大
	- [x] 阿松大

asd 

- asd 
	- asd 
- asdh jk
	- asd 
- asd
- asd 
- asdj

> asd asd j

{{callout note}}
asd 

大图片渲染测试
![](Pasted%20image%2020240516211905.png)