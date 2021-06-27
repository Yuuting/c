# c++并发、线程相关

## 基础知识点
- 线程与进程
- 如何创建线程：thread传参、lambda表达式
- join、detach、yield、get_id、sleep_for、sleep_until
- 一次调用，用于初始化任务：call_once、once_flag
## 互斥体与锁
- mutex、time_mutex、recursive_mutex、recursive_timed_mutex
	- lock
	- try_lock
	- unlock
- 超时，可重入（可递归）锁，共享锁  
- 死锁示例程序

