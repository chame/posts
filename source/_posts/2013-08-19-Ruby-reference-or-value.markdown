---
layout: post
title: "Ruby:　reference or value"
date: 2013-08-19 14:18
comments: true
categories: [ruby, reference]
---

可能有不少人纠结于 ruby 到底是传值呢，还是传引用呢。我也很纠结啊！！

Thanks: https://www.ruby-forum.com/topic/41160

相信，ruby 确实为我们提供了很多便利，但是对于部分想深入操作代码的人来说，可能不点不友好。

```ruby
str = "a str"
copy = str
str = "a diff str"

p copy
p str

# => "a str"
# => "a diff str"
```

这或许是你想看到的结果，copy 不会因 str 的变化而变化，仍然保持当初由 str 得来的值。从对象的方法上说，这也是合情理的。copy 和 str 本不是同一 String 对象，当其一做出修改，根据对象的独立特性，另一个应该也不会变化，否则你也不会起不同的名称了。

但是有些人，会把c++中的指针套用到ruby上，因为他们想修改一个，相应修改另一个，他们才不管什么对象应该互相不影响的说法。

```c
string str="a str";
string *cp = &str;
str="a diff str"

cout << *cp << endl;
cout << str << endl;

# => "a diff str"
# => "a diff str"
```

在ruby中，如果有此想法的你，你不得不：

```ruby
str = "a str"
copy = str
str.replace "a diff str"

p copy
p str

# => "a diff str"
# => "a diff str"
```

但是，为什么用 replace 替代 = 这一操作符之后，就起作用了呢？难道原本它们就是指向同一地址？很可能是！

网友有说可能是 replace 在 ruby 中的 kenel/Object 进行了特殊操作

> this should be inherited from kernel/Object and then overriden
so a clear interface can be followed, my personal belief.

不过，我反而觉得是 ruby 对 = 操作符进行重写。

在上一代码中，ruby 为字符数组"a str"分配了内存，然后定义一个str相关的指针，指向这一字符数组的地址；然后在定义一个指针copy，其等同于str，指向也是字符数组的地址。

在最开始的例子中

```ruby
str = "a diff str"
```

由于 str 和 copy 在前两行的定义声明中是指向同一字符数组的。此行代码，则会对新的字符数组进行比较，由于不相同，ruby 重新分配内存来存储"a diff str"，之后str重新指向这一字符数组。

于是，从上说法来看，`=`例子的相应c++代码可能会是：

```c
string one_string = "a str";
string *str=&one_string, *copy;
copy = str;
string other_string = "a diff str";
if (other_string != one_string){
  str = &other_string;
}

cout << *copy << endl;
cout << *str << endl;

# => "a str"
# => "a diff str"
```

`replace`例子的相应c++代码可能会是：

```c
string one_string = "a str";
string *str=&one_string, *copy;
copy = str;
*str="a diff str";

cout << *cp << endl;
cout << *str << endl;

# => "a diff str"
# => "a diff str"
```

因为大多数的人肯定使用 `=` 操作符，而程序语言在设计时肯定会考虑内存的消耗。同样的工作与速率，内存消耗越少，当然也就越好。所以，重写`=`的可能性也就会更大些。Ruby 走的是简便的路线，你想操作指针，但它并不允许。在它的领域里，它希望你只看到对象。当你操作对象时，你已经在操作指针了。

当然，以上也只是我的猜测，我没有心思认真阅读 ruby 的源码。

---

今天看到个解释比较好的，用图的：

> http://stackoverflow.com/questions/1872110/is-ruby-pass-by-reference-or-by-value#answer-10974116 

![2013-08-22 09:19:31的屏幕截图.png](/images/photo/2013-08-22_09_19_31%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

![2013-08-22 09:19:50的屏幕截图.png](/images/photo/2013-08-22_09_19_50%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

![2013-08-22 09:20:12的屏幕截图.png](/images/photo/2013-08-22_09_20_12%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

![2013-08-22 09:20:28的屏幕截图.png](/images/photo/2013-08-22_09_20_28%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

![2013-08-22 09:20:46的屏幕截图.png](/images/photo/2013-08-22_09_20_46%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

明显，此网友的理解和我的是一样的。