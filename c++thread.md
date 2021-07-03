# c++并发、线程相关

## 基础知识点
- 线程与进程
- 如何创建线程：thread传参、lambda表达式
- join、detach、yield、get_id、sleep_for、sleep_until
- 一次调用，用于初始化任务：call_once、once_flag、c++线程单例模式
## 互斥体与锁
- mutex、time_mutex、recursive_mutex、recursive_timed_mutex
	- lock
	- lock_guard
	- unlock
- 超时，可重入（可递归）锁，共享锁
- unique_lock取代lock_guard:更灵活（多出来很多用法），效率差一点。
unique_lock myUniLock(myMutex);
- unique_lock的第二个参数
 - adopt_lock
 - try_to_lock
 - defer_lock(lock()/unlock()/try_lock()/release())  
- 死锁示例程序
## atomic/async
一般atomic原子操作，针对++，–，+=，-=，&=，|=，^=是支持的，其他操作不一定支持。2.1 std::async参数详述，async 用来创建一个异步任务

延迟调用参数 std::launch::deferred【延迟调用】，std::launch::async【强制创建一个线程】

std::async()我们一般不叫创建线程（他能够创建线程），我们一般叫它创建一个异步任务。

std::async和std::thread最明显的不同，就是 async 有时候并不创建新线程。

①如果用std::launch::deferred 来调用async？

延迟到调用 get() 或者 wait() 时执行，如果不调用就不会执行

②如果用std::launch::async来调用async？

强制这个异步任务在新线程上执行，这意味着，系统必须要创建出新线程来运行入口函数。

③如果同时用 std::launch::async | std::launch::deferred

这里这个或者关系意味着async的行为可能是 std::launch::async 创建新线程立即执行， 也可能是 std::launch::deferred 没有创建新线程并且延迟到调用get()执行，由系统根据实际情况来决定采取哪种方案

④不带额外参数 std::async(mythread)，只给async 一个入口函数名，此时的系统给的默认值是 std::launch::async | std::launch::deferred 和 ③ 一样，有系统自行决定异步还是同步运行。

2.2 std::async和std::thread()区别：

std::thread()如果系统资源紧张可能出现创建线程失败的情况，如果创建线程失败那么程序就可能崩溃，而且不容易拿到函数返回值（不是拿不到）
std::async()创建异步任务。可能创建线程也可能不创建线程，并且容易拿到线程入口函数的返回值；

由于系统资源限制：
①如果用std::thread创建的线程太多，则可能创建失败，系统报告异常，崩溃。

②如果用std::async，一般就不会报异常，因为如果系统资源紧张，无法创建新线程的时候，async不加额外参数的调用方式就不会创建新线程。而是在后续调用get()请求结果时执行在这个调用get()的线程上。

如果你强制async一定要创建新线程就要使用 std::launch::async 标记。承受的代价是，系统资源紧张时可能崩溃。

③根据经验，一个程序中线程数量 不宜超过100~200 。

2.3 async不确定性问题的解决
不加额外参数的async调用时让系统自行决定，是否创建新线程。

std::future result = std::async(mythread);
问题焦点在于这个写法，任务到底有没有被推迟执行。
