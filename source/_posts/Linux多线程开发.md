---
title: Linux多线程开发
categories:
  - Linux
tags:
  - 多线程开发
abbrlink: 610d7d3d
date: 2023-05-05 16:34:57
---

## 线程

### 线程概述

- 与进程（process）类似，线程（thread）是允许应用程序并发执行多个任务的一种机制。一个进程可以包含多个线程。同一个程序中的所有线程均会独立执行相同程序，且共享同一份全局内存区域，其中包括初始化数据段、未初始化数据段，以及堆内存段。（传统意义上的UNIX进程只是多线程程序的一个特例，该进程只包含一个线程）
- 进程是CPU分配资源的最小单位，线城市操作系统调度执行的最小单位
- 线程是轻量级的进程（LWP：Light Weight Process），在Linux环境下线程的本质仍是进程
- 查看指定进程的LWP号: ps -Lf pid

### 线程和进程的区别

- 进程间的信息难以共享。由于出去只读代码段外，父子进程并未共享内存，因此必须采用一些进程间通信方式，在进程间进行信息共享
- 调用fork()来创建进程的代价相对较高，即便利用写时复制技术，仍然需要复制注入内存页表和文件描述符表之类的多种进程属性，这意味着fork()调用在时间上的开销依然不菲
- 线程之间能够方便、快速地共享信息。只需将数据复制到共享（全局或堆）变量中即可
- 创建线程比创建进程通常要快10倍甚至更多。线程间是共享虚拟地址空间的，无需采用写时复制来复制内存，也无需复制页表

### 线程和进程虚拟地址空间

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230504153052.png)

### 线程之间共享和非共享资源

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230504153152.png)

### 线程操作

1. 获取线程ID
    
    `pthread_t pthread_self(void);`
    
    - 功能：获取当前的线程的线程ID
2. 比较线程
    
    `int pthread_equal(pthread_t t1, pthread_t t2);`
    
    - 功能：比较两个线程ID是否相等
3. 线程创建
    
    `int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);`
    
    - 功能：创建一个子线程
    - 参数：
        - `thread`：传出参数，线程创建成功后，子线程的线程ID被写入该变量中
        - `attr`：需要设置线程的属性，一般是默认，NULL
        - `start_routine`：函数指针，子线程需要处理的逻辑代码
        - `arg`：给第三个参数使用，传参
    - 返回值：
        - 成功 返回0
        - 失败 返回错误号。这个错误号和之前的errno不太一样。获取错误号的信息：`char *strerror(int errnum);`
    
    函数示例
    
    ```cpp
    void* callback(void *arg){
        printf("child thread... arg=%d\n", *(int *)arg);
        return NULL;
    }
    
    int main(){
    
        // 创建一个子线程
        pthread_t tid;
        int num = 10;
        int ret = pthread_create(&tid, NULL, callback, (void *) &num);
    
        if(ret != 0){
            char *errstr = strerror(ret);
            printf("%s\n", errstr);
        }
    
        sleep(3);
    
        for(int i = 0; i < 5; i++){
            printf("%d\n", i);
        }
    
        return 0;
    }
    ```
    
4. 终止线程
    
    `void pthread_exit(void *retval);`
    
    - 功能：终止一个线程，在哪个线程调用，就表示终止哪个线程
    - 参数
        - `retval`：需要传递一个指针，作为返回值，可以在`pthread_join()`中获得
    
    函数示例：
    
    ```cpp
    void * callback(void *arg){
        printf("child thread %ld\n", pthread_self());
        return NULL;
    }
    
    int main(){
    
        // 创建一个子线程
        pthread_t tid;
    
        int ret = pthread_create(&tid, NULL, callback, NULL);
    
        if(ret != 0){
            char *strerr = strerror(ret);
            printf("%s\n", strerr);
            // exit(0);
        }
    
        // 主线程
    
        printf("tid: %ld, parent thread %ld\n", tid, pthread_self());
    
        for(int i = 0; i < 4; i++){
            printf("%d\n", i);
        }
        
        // 让主线程退出
        pthread_exit(NULL);
        // 当主线程退出时，不会影响其它线程
    		printf("main exit\n");//不会打印
    
        return 0;
    }
    ```
    

5. 连接线程

`int pthread_join(pthread_t thread, void **retval);`

- 功能：和一个已经终止的线程连接（为什么要连接？是为了释放结束线程的资源）。回收子线程的资源
    - 特点：
        - 这个函数是阻塞函数，调用一次只能回收一个子线程
        - 一般在主线程中使用
- 参数
    - thread：需要回收子线程的ID
    - retval：二级指针，接收子线程退出时的返回值
        - 为什么要用二级指针
            
            在 **`pthread_join`** 函数中，第二个参数需要传递一个指向线程返回值的指针。由于线程的返回值类型是 **`void *`**，因此需要传递一个指针来存储线程的返回值。
            
            但是，线程返回值的指针需要在调用 **`pthread_join`** 函数之前分配内存空间。如果将指针作为参数传递给 **`pthread_join`** 函数，则无法在函数内部为其分配内存空间。
            
            为了解决这个问题，**`pthread_join`** 函数使用了一个指向指针的指针，也就是二级指针。在调用 **`pthread_join`** 函数之前，需要在调用线程的代码中为返回值指针分配内存空间，并将其地址赋值给二级指针。这样，在调用 **`pthread_join`** 函数时，可以传递指向二级指针的指针作为第二个参数，并在函数内部通过该指针来操作返回值指针。
            
            因此，**`pthread_join`** 函数的第二个参数是一个二级指针，以便在函数内部对返回值指针进行操作，而不需要在函数外部为其分配内存空间。
            
- 返回值
    - 成功 返回0
    - 失败 返回错误号，用strerror接收错误信息

函数示例

```cpp
int value = 10;

void * callback(void *arg){
    printf("child thread %ld\n", pthread_self());
    sleep(3);

    // return NULL;

    // int value = 10;
    pthread_exit((void*)&value);
}

int main(){

    // 创建一个子线程
    pthread_t tid;

    int ret = pthread_create(&tid, NULL, callback, NULL);

    if(ret != 0){
        char *strerr = strerror(ret);
        printf("%s\n", strerr);
        // exit(0);
    }

    // 主线程

    printf("tid: %ld, parent thread %ld\n", tid, pthread_self());

    for(int i = 0; i < 4; i++){
        printf("%d\n", i);
    }

    // 主线程调用pthread_join()回收子线程的资源
    int *thread_value;
    ret = pthread_join(tid, (void **)&thread_value);

    if(ret != 0){
        char *strerr = strerror(ret);
        printf("%s\n", strerr);
        // exit(0);
    }
    printf("exit data: %d\n", *thread_value);
    printf("回收子线程资源成功!\n");
    
    // 让主线程退出
    pthread_exit(NULL);
    // 当主线程退出时，不会影响其它线程

    return 0;
}
```

1. 线程分离
    
    `int pthread_detach(pthread_t thread);`
    
    - 功能：分离一个线程。被分离的线程在终止的时候会自动释放资源，返回系统
        1. 不能多次分离，会产生不可预料的行为
        2. 不能去连接已经分离的线程，如果连接，则会报错
    - 参数
        - thread：需要分离的线程ID
    - 返回值：
        - 成功 返回0
        - 失败 返回错误ID
    
    函数示例：
    
    ```cpp
    void *callback(){
    
        printf("child thread...\n");
    
        return NULL;
    }
    
    int main(){
    
        pthread_t tid;
        int ret = pthread_create(&tid, NULL, callback, NULL);
    
        if(ret != 0){
            char *strerr = strerror(ret);
            printf("%s\n", strerr);
        }
    
        // 输出主线程和子线程的id
        printf("child thread : %ld, main thread : %ld\n", tid, pthread_self());
    
        // 设置子线程分离后，对分离的子线程进行连接 pthread_join();
        ret = pthread_detach(tid);
    
        if(ret != 0){
            char *strerr = strerror(ret);
            printf("%s\n", strerr);
        }
    
        ret = pthread_join(tid, NULL);
        if(ret != 0){
            char *strerr = strerror(ret);
            printf("%s\n", strerr);
        }
    
        pthread_exit(NULL);
    
        return 0;
    }
    ```
    
2. 线程取消
    
    `int pthread_cancel(pthread_t thread);`
    
    - 功能：取消线程（让线程终止）
        - 取消某个线程，可以终止某个线程的运行，但是并不是马上终止，而是当子线程执行到一个取消点，线程才会终止调用
        - 取消点：系统规定好的一些系统调用，我们可以粗略的理解为从用户区到内核区的切换，这个位置称之为取消点
    - 参数
        - thread：需要取消的线程号
    - 返回值
        - 成功 返回0
        - 失败 返回错误编号
        

## 线程属性

1. 初始化线程属性变量
    
    `int pthread_attr_init(pthread_attr_t *attr);`
    
2. 释放线程属性资源
    
    `int pthread_attr_destroy(pthread_attr_t *attr);`
    
3. 获取线程分离的状态属性
    
    `int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);`
    
4. 设置线程分离的状态属性
    
    `int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);`
    

函数示例

```cpp
void *callback(){

    printf("child thread...\n");

    return NULL;
}

int main(){

    pthread_t tid;

    // 创建一个线程属性的变量
    pthread_attr_t attr;

    // 初始化属性变量
    pthread_attr_init(&attr);

    // 设置属性
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);

    // 获取线程的栈的大小

    size_t size;
    pthread_attr_getstacksize(&attr, &size);

    printf("stacksize is %ld\n", size);

    // 创建一个子线程
    int ret = pthread_create(&tid, NULL, callback, NULL);

    if(ret != 0){
        char *strerr = strerror(ret);
        printf("%s\n", strerr);
    }

    // 输出主线程和子线程的id
    printf("child thread : %ld, main thread : %ld\n", tid, pthread_self());

    // 释放线程属性资源
    pthread_attr_destroy(&attr);

    pthread_exit(NULL);

    return 0;
}
```

## 线程同步

### 线程同步

- 线程的主要优势在于，能够通过全局变量来共享信息。不过，这种便携的共享是有代价的：必须确保多个线程不会同时修改同一变量，或者某一线程不会读取正在由其它线程修改的变量
- 临界区是指访问某一共享资源的代码片段，并且这段代码的执行应为原子操作，也就是同时访问同意共享资源的其他线程不应中断该片段的执行
- 线程同步：即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作，而其他线程则处于等待状态。

### 互斥量

- 为避免线程更新共享变量时出现问题，可以使用互斥量（mutex时mutual exclusion的缩写）来确保同时仅有一个线程可以访问某项共享资源。可以使用互斥量来保证对任意共享资源的原子访问
- 互斥量有两种状态：已锁定（locked）和未锁定（unlocked）。任何时候，至多只有一个线程可以锁定该互斥量。试图对已经锁定的某一互斥量再次加锁，将可能阻塞线程或者报错失败，具体取决于加锁时使用的方法。
- 一旦线程锁定互斥量，随机成为该互斥量的所有者，只有所有者才能给互斥量解锁。一般情况下，对每一共享资源（可能有多个相关变量组成）会使用不同的互斥量，每一线程在访问统一资源时将采用如下协议：
    - 针对共享资源锁定互斥量
    - 访问共享资源
    - 对互斥量解锁
- 如果多个线程试图执行这一块代码（一个临界区），事实上只有一个线程能够持有该互斥量（其他线程将遭到阻塞），即同时只有一个线程能够进入这段代码区域，如下图所示

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230504225849.png)

### 互斥量函数介绍

1. 初始化互斥量
    
    `int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);`
    
    - 参数：
        - mutex：需要初始化的互斥量变量
        - attr：互斥量的相关属性，一般NULL，默认
    - restrict：修饰符，被修饰的指针，不能由另外的一个指针进行操作
2. 释放互斥量的资源
    
    `int pthread_mutex_destroy(pthread_mutex_t *mutex);`
    
3. 加锁
    
    `int pthread_mutex_lock(pthread_mutex_t *mutex);`
    
    阻塞的，如果有一个线程已经枷锁了，那么其他线程只能阻塞等待
    
4. 尝试加锁
    
    `int pthread_mutex_trylock(pthread_mutex_t *mutex);`
    
    如果加锁失败，不会阻塞，会直接返回
    
5. 解锁
    
    `int pthread_mutex_unlock(pthread_mutex_t *mutex);`
    

函数示例

```cpp
// 创建一个互斥量

pthread_mutex_t mutex;

int tickets = 1000;

void *sellticket(void *args){
    // 卖票
    while(1){
        // 加锁
        pthread_mutex_lock(&mutex);
        if(tickets > 0){
            // usleep(500);
            printf("%ld 正在卖第 %d 门票\n", pthread_self(), 1001 - tickets);
            tickets--;
					   // 解锁
            pthread_mutex_unlock(&mutex);
        }else{
					   // 解锁
            pthread_mutex_unlock(&mutex);
            break;
        }
    }
    return NULL;
}

int main(){

    //  初始化互斥量
    pthread_mutex_init(&mutex, NULL);

    // 创建三个子线程
    pthread_t tid1, tid2, tid3;

    pthread_create(&tid1, NULL, sellticket, NULL);
    pthread_create(&tid2, NULL, sellticket, NULL);
    pthread_create(&tid3, NULL, sellticket, NULL);

    // 回收子线程的资源
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    pthread_join(tid3, NULL);

    // 设置线程分离
    // pthread_detach(tid1);
    // pthread_detach(tid2);
    // pthread_detach(tid3);

    pthread_exit(NULL); // 退出主线程

    // 释放互斥量资源
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

### 死锁

- 有时，一个线程需要同时访问两个或更多不同的共享资源，而每个资源又都由不同的互斥量管理。当超过一个线程加锁同一组互斥量时，就可能发生死锁。
- 两个或两个以上的进程在执行过程中，因争夺共享资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁
- 死锁的几种场景：
    - 忘记释放锁
    - 重复加锁
    - 多线程多锁，抢占锁资源
    
    ![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230505131235.png)
    

### 读写锁

- 当有一个线程已经持有互斥锁时，互斥锁将所有试图进入临界区的线程都阻塞住。但是考虑一种情形，当前持有互斥锁的线程只是要读访问共享资源，而同时有其它几个线程也想读取这个共享资源，但是由于互斥锁的排它性，所有其它线程都无法获取锁，也就无法读访问共享资源了，但是实际上多个线程同时读访问共享资源并不会导致问题。
- 在对数据的读写操作中，更多的是读操作，写操作比较少，例如对数据库数据的读写应用。为了满足当前能够允许多个读出，但只允许一个写入的需求，线程提供了读写锁来实现
- 读写锁的特点：
    - 如果有其他线程读数据，则允许其他线程执行读操作，但不允许写操作
    - 如果有其他线程写数据，则其他线程都不要允许读、写操作
    - 写是独占的，写的优先级更高
    

读写锁函数

1. 初始化读写锁
    
    `int pthread_rwlock_init(pthread_rwlock_t *restrict rwlock, const pthread_rwlockattr_t *restrict attr);`
    
2. 释放锁资源
    
    `int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);`
    
3. 上读锁
    
    `int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);`
    
4. 尝试上读锁
    
    `int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock)`
    
5. 上写锁
    
    `int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);`
    
6. 尝试上写锁
    
    `int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);`
    
7. 解锁
    
    `int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);`
    

函数示例

```cpp
int num = 1;
// pthread_mutex_t mutex;
pthread_rwlock_t rwlock;

void *writeNum(void *args){

    while(1){
        pthread_rwlock_wrlock(&rwlock);
        num++;
        printf("++write, tid : %ld, num : %d\n", pthread_self(), num);
        pthread_rwlock_unlock(&rwlock);
        usleep(100);
    }

    return NULL;
}

void *readNum(void *args){

    while(1){
        // num++;
        pthread_rwlock_rdlock(&rwlock);
        printf("===read, tid : %ld, num : %d\n", pthread_self(), num);
        pthread_rwlock_unlock(&rwlock);
        usleep(100);
    }

    return NULL;
}

int main(){

    // 创建3个写线程，5个读线程

    pthread_rwlock_init(&rwlock, NULL);

    pthread_t wtids[3], rtids[5];

    for(int i = 0; i < 3; i++){
        pthread_create(&wtids[i], NULL, writeNum, NULL);
    }

    // 设置线程分离
    for(int i = 0; i < 3; i++){
        pthread_detach(wtids[i]);
    }

    for(int i = 0; i < 5; i++){
        pthread_create(&rtids[i], NULL, readNum, NULL);
    }

    for(int i = 0; i < 5; i++){
        pthread_detach(rtids[i]);
    }

    pthread_exit(NULL);

    pthread_rwlock_destroy(&rwlock);

    return 0;
}
```

### 条件变量

1. 初始化条件变量
    
    `int pthread_cond_init(pthread_cond_t *restrict cond, const pthread_condattr_t *restrict attr);`
    
2. 释放条件变量
    
    `int pthread_cond_destroy(pthread_cond_t *cond);`
    
3. 等待条件响应
    
    `int pthread_cond_wait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex);`
    
4. 设置等待时间
    
    `int pthread_cond_timedwait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex, const struct timespec *restrict abstime);`
    
5. 唤醒个别等待线程
    
    `int pthread_cond_signal(pthread_cond_t *cond);`
    
6. 唤醒所有等待线程
    
    `int pthread_cond_broadcast(pthread_cond_t *cond);`
    

函数示例

```cpp
struct node{
    int num;
    struct node *next;
};

// 头节点
struct node *head = NULL;

// 创建一个互斥量

pthread_mutex_t mutex;

// 创建条件变量
pthread_cond_t cond;

void * producer(void *args){

    // 不断的创建新的节点，添加到链表中
    while(1){

        pthread_mutex_lock(&mutex);
        struct node *newNode = (struct node*)malloc(sizeof(struct node));
        newNode->next = head;
        head=newNode;
        newNode->num = (int)(rand()) * 1000;
        printf("add node, num : %d, tid : %ld\n", newNode->num, pthread_self());

        // 只要生产一个，就通知消费者消费
        pthread_cond_signal(&cond);

        pthread_mutex_unlock(&mutex);
        usleep(100);
    }

    return NULL;
}

void *customer(void *args){

    while(1){
        // 保存头节点的指针
        pthread_mutex_lock(&mutex);
        struct node *tmp = head;

        // 判断是否有数据
        if(head == NULL){
            // 没有数据 需要等待
            pthread_mutex_unlock(&mutex);
            pthread_cond_wait(&cond, &mutex);
        }else{
            head = head->next;
            printf("delete node, num : %d, tid : %ld\n", tmp->num, pthread_self());
            free(tmp);
            pthread_mutex_unlock(&mutex);
            usleep(100);
        }

        
    }
    return NULL;
}

int main(){

    // 创建5个生产者线程，5个消费者线程
    
    pthread_t ptids[5], ctids[5];

    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    for(int i = 0; i < 5; i++){
        pthread_create(&ptids[i], NULL, producer, NULL);
        pthread_detach(ptids[i]);
    }

    for(int i = 0; i < 5; i++){
        pthread_create(&ctids[i], NULL, customer, NULL);
        pthread_detach(ctids[i]);
    }

    // while(1){
    //     sleep(10);
    // }

    while(1){
        sleep(10);
    }

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    pthread_exit(NULL);

    return 0;
}
```

### 信号量

1. 初始化
    
    `int sem_init(sem_t *sem, int pshared, unsigned int value);`
    
2. 释放资源
    
    `int sem_destroy(sem_t *sem);`
    
3. 对信号量进行加锁，调用一次对信号量的值-1，如果值为0，则阻塞
    
    `int sem_wait(sem_t *sem);`
    
4. 尝试-1
    
    `int sem_trywait(sem_t *sem);`
    
5. 设置等待时间（存疑）
    
    `int sem_timedwait(sem_t *sem, const struct timespec *abs_timeout);`
    
6. 对信号量解锁，调用一次对信号量的值+1
    
    `int sem_post(sem_t *sem);`
    
7. 获取信号量的值
    
    `int sem_getvalue(sem_t *sem, int *sval);`
    

函数示例：

```cpp
struct node{
    int num;
    struct node *next;
};

// 头节点
struct node *head = NULL;

// 创建一个互斥量

pthread_mutex_t mutex;
// 创建两个信号量
sem_t psem, csem;

void * producer(void *args){
    // 不断的创建新的节点，添加到链表中
    while(1){
        sem_wait(&psem);
        pthread_mutex_lock(&mutex);
        struct node *newNode = (struct node*)malloc(sizeof(struct node));
        newNode->next = head;
        head=newNode;
        newNode->num = (int)(rand()) * 1000;
        printf("add node, num : %d, tid : %ld\n", newNode->num, pthread_self());
        pthread_mutex_unlock(&mutex);
        sem_post(&csem);
        usleep(100);
    }

    return NULL;
}

void *customer(void *args){

    while(1){
        // 保存头节点的指针
        sem_wait(&csem);
        pthread_mutex_lock(&mutex);
        struct node *tmp = head;

        head = head->next;
        printf("delete node, num : %d, tid : %ld\n", tmp->num, pthread_self());
        free(tmp);
        pthread_mutex_unlock(&mutex);
        sem_post(&psem);
        usleep(100);
        
    }
    return NULL;
}

int main(){

    // 创建5个生产者线程，5个消费者线程
    
    pthread_t ptids[5], ctids[5];

    pthread_mutex_init(&mutex, NULL);

    sem_init(&psem, 0, 8);
    sem_init(&csem, 0, 0);

    for(int i = 0; i < 5; i++){
        pthread_create(&ptids[i], NULL, producer, NULL);
        pthread_detach(ptids[i]);
    }

    for(int i = 0; i < 5; i++){
        pthread_create(&ctids[i], NULL, customer, NULL);
        pthread_detach(ctids[i]);
    }

    // while(1){
    //     sleep(10);
    // }

    while(1){
        sleep(10);
    }

    pthread_mutex_destroy(&mutex);

    pthread_exit(NULL);

    return 0;
}
```