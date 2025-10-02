#Apple #DailyCodingProblem #Medium 
# Problem

Implement a job scheduler which takes in a function `f` and an integer `n`, and calls `f` after `n` milliseconds.
# Solution

## JavaScript setTimeout Approach

```javascript
function scheduler(f, n) {
    setTimeout(f, n);
}

// Usage
scheduler(() => console.log("Hello"), 1000); // Prints "Hello" after 1 second
```

## C++ Thread Approach

```cpp
#include <thread>
#include <chrono>
#include <functional>

void scheduler(std::function<void()> f, int milliseconds) {
    std::thread([f, milliseconds]() {
        std::this_thread::sleep_for(std::chrono::milliseconds(milliseconds));
        f();
    }).detach();
}

// Usage
scheduler([]() { std::cout << "Hello" << std::endl; }, 1000);
```

## Python Threading Approach

```python
import threading
import time

def scheduler(f, n):
    def wrapper():
        time.sleep(n / 1000.0)  # Convert milliseconds to seconds
        f()
    
    thread = threading.Thread(target=wrapper)
    thread.daemon = True
    thread.start()

# Usage
scheduler(lambda: print("Hello"), 1000)
```

## Advanced: Priority Queue for Multiple Jobs

For scheduling multiple jobs at different times:

```cpp
#include <queue>
#include <functional>
#include <chrono>
#include <thread>

struct Job {
    std::function<void()> task;
    std::chrono::time_point<std::chrono::steady_clock> executeTime;
    
    bool operator<(const Job& other) const {
        return executeTime > other.executeTime; // Min heap
    }
};

class JobScheduler {
private:
    std::priority_queue<Job> jobs;
    
public:
    void schedule(std::function<void()> f, int delayMs) {
        auto executeTime = std::chrono::steady_clock::now() + 
                          std::chrono::milliseconds(delayMs);
        jobs.push({f, executeTime});
    }
    
    void run() {
        while (!jobs.empty()) {
            auto now = std::chrono::steady_clock::now();
            auto nextJob = jobs.top();
            
            if (nextJob.executeTime <= now) {
                nextJob.task();
                jobs.pop();
            } else {
                std::this_thread::sleep_for(nextJob.executeTime - now);
            }
        }
    }
};
```

# Considerations

- **Thread safety**: Use mutexes for concurrent access
- **Daemon threads**: Decide if scheduler should block main thread
- **Error handling**: Handle exceptions in scheduled functions
- **Cancellation**: Add ability to cancel scheduled jobs

# Related Topics

- [[Threading]]
- [[Concurrency]]
- [[Event Loop]]
