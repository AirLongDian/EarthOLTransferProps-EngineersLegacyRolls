# C++多线程（std::thread）

# C++多线程
## 一、 概念
###### 1. 进程
可简单理解为exe的一次运行
###### 2. 线程
一个进程可以有多个线程（但有且只有一个主线程）
## 二、 并发实现

###### 1. 多进程
解决进程间通讯
###### 2. 多线程
一个主线程多个子线程实现并发，进程资源在线程间共享
## 三、 C++线程创建
### 3.1 基本函数创建
#### 1. 包含头文件
```C++
#include <thread>
```
#### 2. 创建线程
创建thread对象
```C++
thread th1(func);
```
* 需要后续处理：否则主函数结束会抛异常
#### 3. 后续处理
1. 阻塞主线程
```C++
th1.join();
```
主线程等待子线程执行完毕

2. 分离子线程
```C++
th1.detach();
```
将子线程与主线程分离，子线程与主线程不再关联

* 每个线程只能做一次后续处理，不管是`join`还是`detach`.

3. 判断一个进程是否可以做后续处理
```C++
th1.joinable();
```
### 3.2 类和对象创建
重载括号运算符制作函数类，将函数类的对象作为线程参数
### 3.3 Lambda表达式创建
将lambda表达式作为参数
### 3.4 带参创建
传引用参数需要`ref()`包装
### 3.5 带智能指针
用`move()`但是之后原智能指针销毁
### 3.6 类成员函数创建
```C++
thread th1(成员函数指针，成员函数所属的对象，参数（可能需要包装）)
```
## 四、异步
#### 4.1 包含头文件
```C++
#include <future>
```
#### 4.2 创建未来返回值对象
```C++
future<type> fu1;
```
#### 4.3 异步运行
```C++
fu1=async(func/lambda)
```
* 此时程序分出一个线程执行异步程序，并继续向后执行
#### 4.4 取回异步运行结果

```C++
fu1.get()
```
* 此时异步程序若仍未执行完，将阻塞程序至异步程序执行完并取回值
#### 4.5 等待异步程序执行完毕但不需要返回结果
```C++
fu1.wait()
```
#### 4.6 仅等待一段时间（超时检测）
```C++
fu1.wait_for(chrono时间单位（）)
```
* 如超时未执行完毕则返回`future_status::timeout`,按时执行完毕则返回`future_status::ready`
#### 4.7 不异步仅延迟至get时运行
将`async`第一个参数设为`std::launch::deferred`
#### 4.8 手动管理线程
不使用async而是使用promise
```C++
std::promise<type> pr1=promise(func/lambda)
```
在传入的可执行体中设置未来值`pr1.set_value()`，然后在外面`get_future`获取未来返回值对象，并进一步get获取未来值
## 五、锁
###### 1. 导入头文件
```C++
#include <mutex>
```
###### 2. 创建锁对象
```C++
mutex mtx1
timed_mutex tmtx1 //可以等待一会的锁
```
###### 3. 基本操作
```C++
mtx1.lock()  //加锁
mtx1.lock()  //解锁
```
* 一个锁锁住就不能再锁，后面试图加锁的就会被阻塞至前一线程解锁
###### 4. 自动解锁
```C++
lock_guard gl1(mtx1)
```
* 创建即加锁，离开作用域自动解锁

```C++
unique_lock<std::mutex> ul1(mtx1) //创建即加锁
unique_lock<std::mutex> ul1(mtx1,defer_lock)  //创建时不加锁
unique_lock<std::mutex> ul1(mtx1,try_to_lock)  //创建时尝试加锁但不阻塞
ul1.try_lock() //尝试加锁
ul1.try_lock_for() //尝试再一段时间内加锁（timed_mutex）
ul1.try_lock_until() //尝试在某个时间点之前加锁(timed_mutex)
ul1.owns_lock() //锁是不是自己的（try_to_lock结果）
ul1.lock() //手动加锁
ul1.unlock（） //手动解锁
```
* 锁所有权转移要用`move()`
###### 5. 读写分离锁
```C++
shared_mutex sml1 //创建锁
sml1.lock() //锁写
sml1.unlock() //解锁写
sml1.lock_shared() //读锁
sml1.unlock_shared() //解锁读
shared_lock sl1(shared_mutexd对象) //自动解锁（类似unique_lock）
```
* 读锁可以锁多次，多个线程一起读，但读的时候不能写（计数）

* 多个对象建议每个对象一个锁

###### 6. 死锁

1. 一个线程不要同时持有多个锁
2. 线程的上锁顺序一致
3. 使用`lock(mtx1,mtx2,...)`对多个锁上锁可以自动处理上锁顺序确保不产生死锁
4. 使用`scoped_lock sl1(mtx1,mtx2,...)`对多个锁上锁可以确保不产生死锁并自动解锁
5. 单个线程死锁可以用`recursice_mutex`代替`mutex`,每次加锁会加一个计数，计数0解锁，但是会有性能损失

## 六、条件变量
#### 1. 导入头文件
```C++
#include <condition_variable>
```
#### 2. 创建条件变量对象
```C++
condition_variable cv1
```
#### 2. 等待条件
```C++
cv1.wait(锁，可以提供执行体如返回true才唤醒) //多个wait被唤醒时锁确保只有一个线程被运行，等待状态中锁会被暂时解锁

```
#### 4. 发送唤醒信号（另一线程）
```C++
cv1.notify_one() //唤醒一个wait的线程
cv1.notify_all() //唤醒所有wait的线程
```
* `condition_variable`只支持`unique_lock`，其他锁可以用`condition_variable_any`
* 也有`wait_for()`和`wait_until()`,会返回布尔值作为等待结果
## 七、原子操作
```C++
atomic<type> name
```
放弃分解和乱序优化，确保对该变量的操作是一次性的（不会有其他线程在插入分解后的执行序列）
```C++
+=
-=
*=
&=
|=
```
