+++
title = "Managing threads in C/C++ applications with pool-day"
date = "2025-11-15"
+++

### Brief context

Sometimes deal with threads is a tedious task, especially when we have to manage the entire lifecycle (creation, execution, and termination) of multiple threads while ensuring thread safety and optimal resource utilization. For C/C++ applications, a traditional approach often involves using low-level threading libraries like `pthread`, which can be cumbersome and error-prone, leading developers to introduce bugs related to concurrency, synchronization and resource wasting into its applications. To address these challenges, higher-level abstractions like the well known threading pool approach can be used to provide a better way to manage this kind of resources. In this scenario, `pool-day` comes in to play.

### pool-day

[pool-day](https://github.com/andrelcmoreira/pool-day/) library provides a portable, simple and efficient way to handle threads, allowing applications to easily create, manage and execute tasks in parallel. Currently in version `0.1.1`, with `pool-day` you can create a pool of worker threads that can execute tasks concurrently, improving the performance and responsiveness of your applications.

> **NOTE**: The library was tested primarily on `Linux` based systems (that is the main target of the library for now), but due to its POSIX compliance, it should work on other operating systems as well. Reports of issues and contributions are welcome via email or GitHub repository.

### Build and Installation

The library relies on `cmake` tool to be built and installed, as follows below:

```bash
$ cmake -S . -B build
$ cmake --build build
$ sudo cmake --install build
```

Additional flags can be supplied as parameter to the build command as well, like `BUILD_SAMPLES` and `ENABLE_LOGGING`, to respectively build the library's sample applications and enable library logging support (specially useful for debugging purposes).

### Examples

A minimal example of how to use the library in a C application is shown below:

```c
#include <assert.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#include <pool_day.h>

void *task_cb(void *param) {
  char *str = (char *)param;
  char *ret = (char *)calloc(10, sizeof(char));

  for (int i = 0; i < 10; i++) {
    printf("%s %d\n", str, i);
    usleep(1000000);
  }

  strcat(ret, "success!");

  return ret;
}

int main(void) {
  pool_day_t pool;
  task_t *task;

  pool = create_pool(1);
  if (!pool) {
    // handle error
    exit(EXIT_FAILURE);
  }

  task = create_task(task_cb, (void *)"hello, world!");

  assert(enqueue_task(pool, task) == POOL_DAY_SUCCESS);
  char *ret = (char *)wait_task_finish(pool, task);

  destroy_pool(&pool);

  printf("result = %s\n", ret);
  free(ret);

  exit(EXIT_SUCCESS);
}
```

Please refer to the [samples](https://github.com/andrelcmoreira/pool-day/tree/develop/samples) folder for more examples.

### Roadmap

The library is still in its early stages of development and there are some features planned for future releases, including:

- Guarantee the support for other operating systems;
- Implementation of Python bindings;
- More comprehensive documentation and examples.

### Conclusion

The `pool-day` library provides a simple and efficient way to manage threads in C/C++ applications, by using the threading pool approach. By abstracting away the complexities of thread management, it allows developers to focus on writing concurrent code without worrying about low-level threading details. `pool-day` can be a valuable tool for any C/C++ developer looking to improve the performance and responsiveness of their applications through multithreading. For more details, please refer to the [documentation](https://github.com/andrelcmoreira/pool-day/releases/download/0.1.1/pool-day-doc-0.1.1.tar.gz).
