## shared_ptr: 共享所有权

[原文链接](https://www.boost.org/doc/libs/release/libs/smart_ptr/doc/html/smart_ptr.html)

### 描述

模版类 [shared_ptr]() 存储动态构造对象的指针，通常是由C++ new语句完成的。这个对象指针在最后一个持有指针所有权的shared_ptr被销毁或者重置(reset)的时候，指针被delete。

*例1: 使用 shared_ptr*
> shared_pt\<X\> p1( new X );<br>
> shared_ptr\<void\> p2( new int(5) );

shared_ptr 会使用构造时的原始类型从而正确的删除指针，不管模版类型是什么。在例1，p2被销毁或者重置(reset)的时候，会调用delete操作法删除int\*类型指针，即使模版类型是void，shared_ptr存储的也是void\*类型的指针。

shared_ptr 满足C++标准库拷贝构造、移动构造、拷贝赋值和移动赋值的需求，也可以用在C++标准库容器上。也提供了比较操作，所以shared_ptr也可以用在C++标准库关联容器上。

由于shared_ptr实现上采用了引用计数，循环引用将导致资源不能正确的回收。例如shared_ptr对象A，不管是直接或间接返回对象A，A的引用计数会变成2。析构对象A引用计数变成1，不会去释放资源。使用 [weak_ptr]() 可以避免循环引用。

shared_ptr类的模版参数T，是指向的对象的类型，其大部分成员函数不要求模版参数T。可以是不完整的类型或 void 类型。构造函数和reset需要显示地指出新的参数类型。

当 T* 可以隐式转换成 U* 时，shared_ptr\<T\>也可以隐式转换成shared_ptr\<U\>。尤其是shared_ptr\<T\>隐式转换成shared_ptr\<T const\>、shared_ptr\<U\>(类型U是类型T的可访问基类)、shared_ptr\<void\>。

shared_ptr 在C++11标准上被命名为 std::shared_ptr。

随着boost 1.53版本，shared_ptr 可以存储动态分配的数组，可以使用数组类型(T[]/T[N])作为模版参数。使用不定数量当数组T[]和一定数量的数组T[N]，几乎是没有区别的；后者能够使用operator[]对索引(index)执行范围检查。

*例2：使用数组类型的shared_ptr*
> shared_ptr<double[1024]> p1( new double[1024] );<br>
> shared_ptr<double[]> p2( new double[] );

### 最佳实践

一个简单的消除内存泄漏的可能的指导是总是使用命名的智能指针变量去存储new操作返回的指针对象。每个使用new的代码都应该是下面这种类型：

> shared_ptr<T> p(new Y);

当然可以使用另一种智能指针类型代替上面的shared_ptr，T和Y可以是相同的类型，h或传递参数给Y的构造函数也是可以的。

如果你遵守这个规则，很显然你不需要显示delete语句，try/catch结构也会很少。

避免使用不命名的shared_ptr临时变量去保存类型，要了解为什么这是危险的，请考虑这个例子：

*例3:使用shared_ptr的异常安全和不安全*

``` C++
void f(shared_ptr<int>, int);
int g();

void ok()
{
    shared_ptr<int> p(new int(2));
    f(p, g());
}

void bad()
{
    f(shared_ptr<int>(new int(2)), g());
}
```

函数 ok 是正确使用智能指针的方式，而 bad 使用 shared_ptr 临时变量的形式存在内存泄漏的可能。因为函数参数的计算顺序是未知的，可能是先计算 new int(2)，然后 g()，如果函数g抛出异常，就不会构造shared_ptr对象。关于这个问题的更多信息可以点击[Herb Sutter’s treatment](http://www.gotw.ca/gotw/056.htm)

上述异常安全的问题也可以使用工厂函数(make_shared/allocate_shared)，这两个函数定义在\<boost/smart_ptr/make_shared.hpp\>。这两个工厂函数还能保证效率上的提升。

### 简介

shared_ptr 定义在 \<boost/smart_ptr/shared_ptr.hpp\>

```
namespace boost {

  class bad_weak_ptr: public std::exception;

  template<class T> class weak_ptr;

  template<class T> class shared_ptr {
  public:

    typedef /*see below*/ element_type;

    constexpr shared_ptr() noexcept;
    constexpr shared_ptr(std::nullptr_t) noexcept;

    template<class Y> explicit shared_ptr(Y * p);
    template<class Y, class D> shared_ptr(Y * p, D d);
    template<class Y, class D, class A> shared_ptr(Y * p, D d, A a);
    template<class D> shared_ptr(std::nullptr_t p, D d);
    template<class D, class A> shared_ptr(std::nullptr_t p, D d, A a);

    ~shared_ptr() noexcept;

    shared_ptr(shared_ptr const & r) noexcept;
    template<class Y> shared_ptr(shared_ptr<Y> const & r) noexcept;

    shared_ptr(shared_ptr && r) noexcept;
    template<class Y> shared_ptr(shared_ptr<Y> && r) noexcept;

    template<class Y> shared_ptr(shared_ptr<Y> const & r, element_type * p) noexcept;

    template<class Y> shared_ptr(shared_ptr<Y> && r, element_type * p) noexcept;

    template<class Y> explicit shared_ptr(weak_ptr<Y> const & r);

    template<class Y> explicit shared_ptr(std::auto_ptr<Y> & r);
    template<class Y> shared_ptr(std::auto_ptr<Y> && r);

    template<class Y, class D> shared_ptr(std::unique_ptr<Y, D> && r);

    shared_ptr & operator=(shared_ptr const & r) noexcept;
    template<class Y> shared_ptr & operator=(shared_ptr<Y> const & r) noexcept;

    shared_ptr & operator=(shared_ptr const && r) noexcept;
    template<class Y> shared_ptr & operator=(shared_ptr<Y> const && r) noexcept;

    template<class Y> shared_ptr & operator=(std::auto_ptr<Y> & r);
    template<class Y> shared_ptr & operator=(std::auto_ptr<Y> && r);

    template<class Y, class D> shared_ptr & operator=(std::unique_ptr<Y, D> && r);

    shared_ptr & operator=(std::nullptr_t) noexcept;

    void reset() noexcept;

    template<class Y> void reset(Y * p);
    template<class Y, class D> void reset(Y * p, D d);
    template<class Y, class D, class A> void reset(Y * p, D d, A a);

    template<class Y> void reset(shared_ptr<Y> const & r, element_type * p) noexcept;
    template<class Y> void reset(shared_ptr<Y> && r, element_type * p) noexcept;

    T & operator*() const noexcept; // only valid when T is not an array type
    T * operator->() const noexcept; // only valid when T is not an array type

    // only valid when T is an array type
    element_type & operator[](std::ptrdiff_t i) const noexcept;

    element_type * get() const noexcept;

    bool unique() const noexcept;
    long use_count() const noexcept;

    explicit operator bool() const noexcept;

    void swap(shared_ptr & b) noexcept;

    template<class Y> bool owner_before(shared_ptr<Y> const & rhs) const noexcept;
    template<class Y> bool owner_before(weak_ptr<Y> const & rhs) const noexcept;
  };

  template<class T, class U>
    bool operator==(shared_ptr<T> const & a, shared_ptr<U> const & b) noexcept;

  template<class T, class U>
    bool operator!=(shared_ptr<T> const & a, shared_ptr<U> const & b) noexcept;

  template<class T, class U>
    bool operator<(shared_ptr<T> const & a, shared_ptr<U> const & b) noexcept;

  template<class T> bool operator==(shared_ptr<T> const & p, std::nullptr_t) noexcept;
  template<class T> bool operator==(std::nullptr_t, shared_ptr<T> const & p) noexcept;

  template<class T> bool operator!=(shared_ptr<T> const & p, std::nullptr_t) noexcept;
  template<class T> bool operator!=(std::nullptr_t, shared_ptr<T> const & p) noexcept;

  template<class T> void swap(shared_ptr<T> & a, shared_ptr<T> & b) noexcept;

  template<class T>
    typename shared_ptr<T>::element_type *
      get_pointer(shared_ptr<T> const & p) noexcept;

  template<class T, class U>
    shared_ptr<T> static_pointer_cast(shared_ptr<U> const & r) noexcept;

  template<class T, class U>
    shared_ptr<T> const_pointer_cast(shared_ptr<U> const & r) noexcept;

  template<class T, class U>
    shared_ptr<T> dynamic_pointer_cast(shared_ptr<U> const & r) noexcept;

  template<class T, class U>
    shared_ptr<T> reinterpret_pointer_cast(shared_ptr<U> const & r) noexcept;

  template<class E, class T, class Y>
    std::basic_ostream<E, T> &
      operator<< (std::basic_ostream<E, T> & os, shared_ptr<Y> const & p);

  template<class D, class T> D * get_deleter(shared_ptr<T> const & p) noexcept;

  template<class T> bool atomic_is_lock_free( shared_ptr<T> const * p ) noexcept;

  template<class T> shared_ptr<T> atomic_load( shared_ptr<T> const * p ) noexcept;
  template<class T>
    shared_ptr<T> atomic_load_explicit( shared_ptr<T> const * p, int ) noexcept;

  template<class T>
    void atomic_store( shared_ptr<T> * p, shared_ptr<T> r ) noexcept;
  template<class T>
    void atomic_store_explicit( shared_ptr<T> * p, shared_ptr<T> r, int ) noexcept;

  template<class T>
    shared_ptr<T> atomic_exchange( shared_ptr<T> * p, shared_ptr<T> r ) noexcept;
  template<class T>
    shared_ptr<T> atomic_exchange_explicit(
      shared_ptr<T> * p, shared_ptr<T> r, int ) noexcept;

  template<class T>
    bool atomic_compare_exchange(
      shared_ptr<T> * p, shared_ptr<T> * v, shared_ptr<T> w ) noexcept;
  template<class T>
    bool atomic_compare_exchange_explicit(
      shared_ptr<T> * p, shared_ptr<T> * v, shared_ptr<T> w, int, int ) noexcept;
}
```

### 成员简介

1. element_type
> typedef ... element_type

当T不是数组类型时，element_type是类型T，当T是数组类型(U[]/U[N])时，element_type是类型U。

2. default constructor
> constexpr shared_ptr() noexcept;

> constexpr shared_ptr(std::nullptr_t) noexcept;

构造一个空的shared_ptr，其满足条件
> use_count() == 0 && get() == 0

3. pointer constructor
> template\<class Y\> explicit shared_ptr(Y* p);

类型Y必须是完整类型。当T是数组类型，表达式delete[] p；当T不是数组类型，表达式delete p必须是符合语法规则、符合定义并不会抛出异常的。当类型T是U[N]，Y(\*)[N]可以转换成T\*；当类型T是U[]，Y(\*)[]可以转换成T\*；除此之外Y\*可以转换成T\*。

当类型T不是数组类型，构造一个拥有p所有权的shared_ptr。除此之外构造一个拥有数组p所有权的shared_ptr，默认的删除器是delete[] p。

use_count() == 1 && get() == p。如果类型T不是数组类型并且p可以明确的转换成enabled_shared_from_this\<V\>，p->shared_from_this()返回*this的副本。

内存不足时抛出std::bad_alloc或实现定义的异常。

异常安全，当T是数组类型，构造函数会调用delete[] p；当T不是数组类型，会调用delete p。

> note:<br>
p必须是0或者C++ new出来的指针类型。当p是0时，use_count()也是1，并且delete p也是不会报错的。<br>
这个构造函数会用模板记录实际传入的类型，析构函数会delete原始类型，即便T没有虚析构函数或void类型。

4. constructors taking a deleter

> template\<class Y, class D\> shared_ptr
(Y* p, D d);
> template\<class Y, class D, class A\> shared_ptr(Y* p, D d, A a);
> template\<class D\> shared_ptr(std::nullptr_t p, D d);
> template\<class D, class A\> shared_ptr(std::nullptr_t p, D d, A a);

类型D必须是可拷贝构造的。D的构造和析构不能抛出异常。表达式delete p必须是符合语法规则、符合定义并不会抛出异常的。类型A必须是符合C++标准的内存配置器。当类型T是U[N]，Y(\*)[N]可以转换成T\*；当类型T是U[]，Y(\*)[]可以转换成T\*；除此之外Y\*可以转换成T\*。

构造一个拥有指针p所有权和删除器d的shared_ptr对象。带内存配置器的构造函数使用配置器a分配内存。

use_count() == 1 && get() == p。如果类型T不是数组类型并且p可以明确的转换成enabled_shared_from_this\<V\>，p->shared_from_this()返回*this的副本。

内存不足时抛出std::bad_alloc或实现定义的异常。

异常抛出时，会调用d(p)。