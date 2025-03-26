---
layout: post
title: CSP-J 题目复习 2020-16 [字符映射]
date: 2023-07-15 22:11
tags: [csp, dev]
author: you7n
---
2020年CSP普及组第一轮第16题。
```cpp
#include <cstdlib>
#include <iostream>
using namespace std;

char encoder[26] = {'C','S','P',0};
char decoder[26];

string st;

int main()  {
  int k = 0;
  for (int i = 0; i < 26; ++i)
    if (encoder[i] != 0) ++k;
  for (char x ='A'; x <= 'Z'; ++x) {
    bool flag = true;
    for (int i = 0; i < 26; ++i)
      if (encoder[i] ==x) {
        flag = false;
        break;
      }
      if (flag) {
        encoder[k]= x;
        ++k;
      }
  }
  for (int i = 0; i < 26; ++i)
     decoder[encoder[i]- 'A'] = i + 'A';
  cin >> st;
  for (int i = 0; i < st.length( ); ++i)
    st[i] = decoder[st[i] -'A'];
  cout << st;
  return 0;
}
```
![img-1](https://github.com/user-attachments/assets/65496b34-d500-47f1-b7eb-9116eb212d1b)

这还是一道字符映射题，做法和上一题差不多。

先来看L5。
![img-2](https://github.com/user-attachments/assets/b4c8be9d-b0e2-4887-b4b7-792f3e460b9b)

encoder和decoder数组都总共有26个index，不难看出是encoder中的字母映射到decoder中的字母上。在这里，需要注意这本质是字符间的映射，脱离整数看更容易理解。结合后文你会明白为什么。

直接进到main函数。

![img-3](https://github.com/user-attachments/assets/72b279d6-6ce2-4c55-ab18-ce4e3dada916)

这是一段初始化。这里k是一个counter，当遇到被映射过的字符就自增1。因为前文定义encoder为{'C' , 'S' , 'P' , 0}，这里被映射过的字符就是CSP三个，故这里k最后值为3。

下面这一坨是这段代码的重点。

![img-4](https://github.com/user-attachments/assets/47868d16-1d95-4372-a6f4-a898260108c9)

L15定义了一个boolean值flag，结合后面的if判断可以看出flag为true的时候代表这位字符没有被映射。这里k是没有被映射的最小数。因为encoder数组是从上到下挨个映射的。这坨代码想表达的就是，如果这位encoder字符和decoder有映射关系，就break;。没有，那就找一个映上去。
![img-gif](https://mmbiz.qpic.cn/mmbiz_gif/2picj4SWJxAAw8vVDXRlr9WcCQD8InibF6nhKU1zVLVSkj3Quw46aHqCWUk19YedbZZr9zUm6KxbXDFtCyuH4nVA/640?wx_fmt=gif&tp=wxpic&wxfrom=5&wx_lazy=1)
![img-6](https://github.com/user-attachments/assets/a11ef2a0-99aa-4286-9550-be5ca33bc64f)

L27是将encoder的所映射的decoder在逆映射回去，不做过多解释。
![img-7](https://github.com/user-attachments/assets/b62fb741-693e-4e1d-94b6-b11bfabf4105)

这里L29，L30扫描整个字符并逐个映射。

至此，程序已经大概看明白了，没有什么难点。

![img-8](https://github.com/user-attachments/assets/4e468536-bfad-439f-ae9b-34a8a1cfdfac)

第一题正确。输入的字符串index总共只有26个，使用了别于26个大写字母的字符可能会导致index大于26，造成数组越界。

![img-9](https://github.com/user-attachments/assets/10789040-758c-4b4a-908a-026f115f7191)

第二题错误。有可能出现输入的字符串和输出字符串一样的情况。当k>'S'的时候，encode数组就和decode数组一一对应了，正如动图所示。

![img-10](https://github.com/user-attachments/assets/a7d86e76-9bc7-4aa0-b85a-200894718d08)

第三题正确。很明显，L12这里用于判断已映射的字符个数，总共只有下表123三个，所以26改成4都没问题。

![img-11](https://github.com/user-attachments/assets/61514826-f228-4b60-8113-8b3852e49f06)

错误。很显然，L26这里是将decoder逆映射回去，如果改成16就映射不完整了。

![img-12](https://github.com/user-attachments/assets/f71ae0f3-6f46-4ea4-bcde-b1c8dd96ff6a)

第五题。这题其实可以根据上面的动图来看，输出ABC在decoder数组中找到与之相对应的decoder即可。输出ABC其实就是下标的[1][2][3]，这也就是为什么说脱离整数看字符本身更容易理解。[1][2][3]是最开始就定义了的C，S和P，所以输入为CSP。

第六题。这题答案并不是ABC。这题也是根据动图来看。同第五题，C对应着P。画图可得，S对应R，P对应N。所以输入应该是PRN。
第五题和第六题是改过的题目。原题是这样的
 
最后答案还是差不多的。5题中CSP既有S又有P，选A。6题中PRN既有P又有R，选D。
以上，本题不是很难。

