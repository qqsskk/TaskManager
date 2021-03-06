![alt text](https://raw.githubusercontent.com/Tastyep/TaskManager/master/assets/task_manager_logo.png)

[![Build Status](https://travis-ci.org/Tastyep/TaskManager.svg?branch=master)](https://travis-ci.org/Tastyep/TaskManager)

TaskManager is an asynchronous task management library using the features of C++14.

### Requirements
A compiler supporting the C++14 features.

### Features
###### Components
* A ThreadPool, manages workers (threads).
* A Task manager for running tasks asynchronously.
* A Scheduler for scheduling tasks asynchronously.

###### Interface
* The library exposes a module with free functions for creating managers and schedulers.

### Documentation
Online documentation can be found [here](https://tastyep.github.io/TaskManager/API/html/index.html).

The documentation is generated using [Doxygen](https://www.stack.nl/~dimitri/doxygen/) which means it can also be found in the source code.

Note: Files located in the detail/ directories are left undocumented as they are not exposed and only used internally.

### Basic Usage
###### Manager

Note: The following examples use chrono literals.

```C++
// Create the thread pool with the initial number of threads (2 here).
Task::Module::init(2);

// Create a task manager with one worker.
auto manager = Task::Module::makeManager(1);

// Add a new task and get its future.
auto future = manager.push([] { return 42; });

// Get the result from the future and print it.
std::cout << future.get() << std::endl; // Prints 42

// Not necessary here, but the stop method ensures that all launched tasks have been executed.
manager.stop().get();
```

In the above example if we were to push more tasks, only one at a time would be executed as the manager has only one worker assigned.

###### Scheduler
```C++
// Create the thread pool with the initial number of threads (2 here).
Task::Module::init(2);

// Create a scheduler with one worker.
auto scheduler = Task::Module::makeScheduler(1);

// Declare the variable n.
size_t n = 0;

// Add new tasks and get the future.
auto future = scheduler.scheduleIn("Task1", 2s, [&n] { n++; });
scheduler.scheduleIn("Task2", 1s, [&n] { n = 41 });

// Get the future and print the updated value.
future.get()
std::cout << n << std::endl; // Prints 42

// Not necessary here, but the stop method ensures that all the scheduled tasks have been executed.
scheduler.stop().get();
```

The same note applies for the scheduler regarding the number of associated workers.
Also an identifier ("TaskX") is provided so that periodic tasks could be removed.
