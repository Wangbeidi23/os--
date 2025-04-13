# Git 使用与多线程编程实验报告

## 1. GitHub 仓库创建与管理

为了实现本地与虚拟机之间的协同开发，本实验首先创建了一个 GitHub 仓库，并将实验程序和记录上传至该仓库，便于在不同环境中同步和管理代码版本。

如图所示:

![image-20250413134146601](Git 使用与多线程编程实验报告.assets/image-20250413134146601-17445272135154.png)

```bash
git pull origin main
git add .
git commit -m "提交信息"
git push origin main
```


## 2. 多线程编程实验

### 2.1 基本多线程程序设计

首先编写一个简单的多线程程序，该程序使用 `pthread` 库创建一个线程并输出 "Ciallo world!"。以下是代码示例：

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

void *worker(void *arg)
{
    printf("Ciallo world!\n");
    return NULL;
}

int main(int argc, char *argv[])
{
    pthread_t tid;

    if (pthread_create(&tid, NULL, worker, NULL))
    {
        printf("can not create\n");
        exit(1);
    }

    printf("main waiting for thread\n");

    pthread_join(tid, NULL);

    exit(0);
}
```

如下图所示

![alt text](Git 使用与多线程编程实验报告.assets/11.png)

通过下面的指令编译

```bash
gcc cialloWorld.c -o cialloWorld -pthread
```

运行结果如下

```
main waiting for thread
Ciallo world!
```

如下图所示

![image-20250413141109497](Git 使用与多线程编程实验报告.assets/image-20250413141109497-17445272180305.png)


### 2.2 多线程性能测试

为了测试多线程在大规模计算中的性能提升，编写了一个多线程计算和单线程计算对比的程序。该程序计算从 1 到 `MAX_NUM`（100000000）的整数之和，并使用2个线程分配计算任务，从而提高计算效率。

编写代码如下

```c
#include <stdio.h>
#include <pthread.h>
#include <sys/time.h>  // 改用挂钟时间测量
#include <stdint.h>
#include <math.h>       // 用于添加复杂计算（可选）

#define MAX_NUM 100000000  // 增大计算量到1e8
#define THREADS 4          // 增加线程数

typedef struct {
    uint64_t start;
    uint64_t end;
    uint64_t result;
} ThreadArgs;

// 获取挂钟时间（精确到微秒）
double get_wall_time() {
    struct timeval time;
    gettimeofday(&time, NULL);
    return (double)time.tv_sec + (double)time.tv_usec / 1e6;
}

void *calculate_sum(void *arg) {
    ThreadArgs *args = (ThreadArgs *)arg;
    uint64_t sum = 0;
    for (uint64_t i = args->start; i <= args->end; i++) {
        sum += i;
        // 可选：添加复杂计算放大耗时差异
        // for (int j = 0; j < 100; j++) {
        //     sum += (uint64_t)sqrt((double)i);
        // }
    }
    args->result = sum;
    return NULL;
}

uint64_t single_thread_sum() {
    uint64_t sum = 0;
    for (uint64_t i = 1; i <= MAX_NUM; i++) {
        sum += i;
    }
    return sum;
}

uint64_t multi_thread_sum() {
    pthread_t threads[THREADS];
    ThreadArgs args[THREADS];
    uint64_t total_sum = 0;

    // 更均匀的任务分配
    uint64_t segment = MAX_NUM / THREADS;
    uint64_t remainder = MAX_NUM % THREADS;
    uint64_t current_start = 1;

    for (int i = 0; i < THREADS; i++) {
        args[i].start = current_start;
        args[i].end = current_start + segment - 1 + (i < remainder ? 1 : 0);
        args[i].result = 0;
        pthread_create(&threads[i], NULL, calculate_sum, &args[i]);
        current_start = args[i].end + 1;
    }

    for (int i = 0; i < THREADS; i++) {
        pthread_join(threads[i], NULL);
        total_sum += args[i].result;
    }

    return total_sum;
}

int main() {
    double start, end;
    uint64_t sum;

    // 单线程
    start = get_wall_time();
    sum = single_thread_sum();
    end = get_wall_time();
    printf("单线程结果: %lu, 耗时: %.5f 秒\n", sum, end - start);

    // 多线程
    start = get_wall_time();
    sum = multi_thread_sum();
    end = get_wall_time();
    printf("多线程结果: %lu, 耗时: %.5f 秒\n", sum, end - start);

    return 0;
}
```

运行结果如下图所示

![image-20250413144530420](Git 使用与多线程编程实验报告.assets/image-20250413144530420-17445267747662.png)

运行该程序后，可以观察到多线程计算的时间明显低于单线程计算，在多线程的支持下，程序的计算速度大约提高了一倍，证明了多线程在并行计算中的优势。