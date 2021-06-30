# 单例模式
保证一个类仅有一个实例，并提供一个该实例的全局访问点。

在软件系统中，经常有这样一些特殊的类，必须保证他们在系统中只存在一个实例，才能确保它们的逻辑正确性、以及良好的效率。

所以得考虑如何绕过常规的构造器（不允许使用者new出一个对象），提供一种机制来保证一个类只有一个实例。

应用场景：

- Windows的Task Manager（任务管理器）就是很典型的单例模式，你不能同时打开两个任务管理器。Windows的回收站也是同理。
- 应用程序的日志应用，一般都可以用单例模式实现，只能有一个实例去操作文件。
- 读取配置文件，读取的配置项是公有的，一个地方读取了所有地方都能用，没有必要所有的地方都能读取一遍配置。
- 数据库连接池，多线程的线程池。

-[x]线程不安全版本
```c++
//线程不安全单例模式：构造私有、拷贝构造私有、成员指针
class Singleton{
public:
    static Singleton* getInstance(){
        if(m_instance==nullptr){
            m_instance=new Singleton;
        }
        return m_instance;
    }
private:
    Singleton();
    Singleton(const Singleton& other);
    static Singleton* m_instance;
};
Singleton* Singleton::m_instance= nullptr;
```

-[x]加入模板类
```c++
//加入模板类
template<typename T>
class singleton{
public:
    static T& getInstance(){
        if(!value_){
            value_=new T();
        }
        return *value_;
    }
private:
    singleton();
    ~singleton();
    static T* value_;
};
template<typename T>
T* singleton<T>::value_= nullptr;
```
