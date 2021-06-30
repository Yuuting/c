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

