+++
title = "pool-day: version 1.0.0 is out"
date = "2026-4-5"
+++

Yesterday, version [1.0.0](https://github.com/andrelcmoreira/pool-day/releases/tag/1.0.0) of `pool-day` was released. For those who don't know what I'm talking about, I already wrote a more comprehensive [post](https://andrelcmoreira.me/blog/pool-day/) about the project. Almost 18 months after its [first release](https://github.com/andrelcmoreira/pool-day/releases/tag/0.1.1), `pool-day 1.0.0` includes a number of significant improvements, ranging from memory safety bug fixes to a more stable and flexible API. More details about these changes are listed below:

**Library**

- API improvements (**BREAKING CHANGES ALERT**):
    - Making `task_t` an opaque pointer;
    - Refactoring the `task` concept, splitting it into **sync** and **async** tasks, resulting two different functions for task creation:
        - `create_sync_task`: Similar to the old `create_task` function;
        - `create_async_task`: Used to create an async task, allowing the usage of callbacks to mark the task beginning and end, as well as retrieve the task's result. It also provides the `auto_release` parameter to enable/disable automatic task release after its execution.
    - Splitting the `wait_task_finish` function into:
        - `get_task_result`: Blocks the current thread until the task is finished and returns its result;
        - `wait_task_finish`: Only blocks the current thread until the task is finished.
- Fixing memory issues.

**Samples**

- Adding a http-server sample to demonstrate a real-life use case for the library;
- Refactoring the existing samples to follow the new API changes and provide a better demonstration of the library’s sync and async use cases.

**CI**

- Adding Clang build to `ci` and `release` workflows to improve the source code portability checking;
- Improving the `ci` workflow by adding a static analysis job (using cppcheck, btw) to it.

I think that's it. Please refer to the [documentation](https://github.com/andrelcmoreira/pool-day/releases/download/1.0.0/pool-day-doc-1.0.0.tar.gz) for more information about the library's usage, and feel free to reach me out by e-mail or open an issue/make a contribution through the [GitHub repo](https://github.com/andrelcmoreira/pool-day/).
