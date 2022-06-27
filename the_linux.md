# linux 多线程
>网址：[多线程详解](https://blog.csdn.net/w903414/article/details/110005612)  
> #线程创建
>>* int pthread_create(pthread_t *thread, const pthread_attr_t *attr,void *(*start_routine) (void *), void *arg);
>
> ##查看自己线程ID  
>>* pthread_self();
>
> ##判断两个线程ID是否对应着同一个线程
>>* int pthread_equal(pthread_t t1, pthread_t t2);
>
> ##获取、调整线程栈的大小：
>>* int pthread_attr_setstacksize(pthread_attr_t *attr,size_t stacksize);
>>* int pthread_attr_getstacksize(pthread_attr_t *attr,size_t *stacksize);
>
> ##线程结束
>> ### 自己结束自己
>>>* void pthread_exit(void *retval);
>>
>> ### 可结束其他线程
>>>* int pthread_cancel(pthread_t thread);
>
> ## 线程等待  
>>* int pthread_join(pthread_t thread, void **retval);
>>>* 有些线程不想让别的线程等待，可以使用线程分离。
>
> ## 线程分离
>>* int pthread_detach(pthread_t thread); 
>>>* ** pthread_detach **应该在 ** pthread_join ** 被调用前，调用，否则，该线程分离无效。
>>
>>* 如果线程创建时使用默认创建，则还可以使用如下办法使线程分离
```
pthread_attr_t  attr;
pthread_attr_init(&attr);
pthread_attr_setdetachstate(&attr,  PTHREAD_CREATE_DETACHED);
pthread_create(&pthreadid,  &attr,  myprocess,  &arg);
```
>
> ## 互斥量的初始化  
>>* 静态分配：`pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;`
>>* 动态分配：`int pthread_mutex_init(pthread_mutex_t *restrict mutex,const pthread_mutexattr_t *restrict attr);`
>
> ## 互斥量的销毁
>>* int pthread_mutex_destroy(pthread_mutex_t *mutex);
>
>* ## 互斥量的加锁  
>>* int pthread_mutex_lock(pthread_mutex_t *mutex);
>>* int pthread_mutex_trylock(pthread_mutex_t *mutex);
>>* int pthread_mutex_timedlock(pthread_mutex_t *restrict mutex, const struct timespec *restrict abs_timeout);
>
> ## 互斥量的解锁
>>* int pthread_mutex_unlock(pthread_mutex_t *mutex);
>
> ## 互斥锁的类型
>>* PTHREAD_MUTEX_NORMAL： 最普通的一种互斥锁。 它不具备死锁检测功能，如线程对自己锁定的互斥量再次加锁， 则会发生死锁。
>>* PTHREAD_MUTEX_RECURSIVE_NP： 支持递归的一种互斥锁， 该互斥量的内部维护有互斥锁的所有者和一个锁计数器。 当线程第一次取到互斥锁时， 会将锁计数器置1， 后续同一个线程再次执行加锁操作时， 会递增该锁计数器的值。 解锁则递减该锁计数器的值， 直到降至0， 才会真正释放该互斥量， 此时其他线程才能获取到该互斥量。 解锁时， 如果互斥量的所有者不是调用解锁的线程， 则会返回EPERM。
>>* PTHREAD_MUTEX_ERRORCHECK_NP： 支持死锁检测的互斥锁。 互斥量的内部会记录互斥锁的当前所有者的线程ID（调度域的线程ID） 。 如果互斥量的持有线程再次调用加锁操作， 则会返回EDEADLK。 解锁时， 如果发现调用解锁操作的线程并不是互斥锁的持有者， 则会返回EPERM。
>>* 自旋锁，自旋锁采用了和互斥量完全不同的策略， 自旋锁加锁失败， 并不会让出CPU， 而是不停地尝试加锁， 直到成功为止。 这种机制在临界区非常小且对临界区的争夺并不激烈的场景下， 效果非常好。自旋锁的效果好， 但是副作用也大， 如果使用不当， 自旋锁的持有者迟迟无法释放锁， 那么， 自旋接近于死循环， 会消耗大量的CPU资源， 造成CPU使用率飙高。 因此， 使用自旋锁时， 一定要确保临界区尽可能地小， 不要有系统调用， 不要调用sleep。 使用strcpy/memcpy等函数也需要谨慎判断操作内存的大小， 以及是否会引起缺页中断。
>>* PTHREAD_MUTEX_ADAPTIVE_NP：自适应锁，首先与自旋锁一样， 持续尝试获取， 但过了一定时间仍然不能申请到锁， 就放弃尝试， 让出CPU并等待。 PTHREAD_MUTEX_ADAPTIVE_NP类型的互斥量， 采用的就是这种机制。
>
> ## 读写锁 
>>* 初始化
>>>*  int pthread_rwlock_init(pthread_rwlock_t *restrict rwlock,
              const pthread_rwlockattr_t *restrict attr);
>
>>* 销毁
>>>*  int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);             
>
>>* 读-加锁
>>>*  int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock); //阻塞类型的读加锁接口    
>>>* int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock); //非阻塞类型的读加锁接口
>
>>* 写-加锁
>>>*  int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);// 非阻塞写 
>>>*  int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);//阻塞写         
>
>>* 解锁
>>>*  int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);
>
>>* 读写锁的类型
>>>*  PTHREAD_RWLOCK_PREFER_READER_NP, //读者优先 
>>>*  PTHREAD_RWLOCK_PREFER_WRITER_NP, //很唬人， 但是也是读者优先
>>>*  PTHREAD_RWLOCK_PREFER_WRITER_NONRECURSIVE_NP, //写者优先
>>>*  PTHREAD_RWLOCK_DEFAULT_NP = PTHREAD_RWLOCK_PREFER_READER_NP



