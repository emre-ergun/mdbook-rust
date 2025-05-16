# 2.9 Concurrency

- Threads have their own stack memory but they share heap memory with other threads in the same process.
- Job of the scheduler is to make sure that each thread gets execution time and the way it does that is through preemptive scheduling. Each threads get a small amount of time to execute. Once that time is over, execution on that thread stops and another thread is scheduled in its place. This process of switching threads is called **context switching.**
- **Preemptive scheduling** means that the scheduler can stop executing a thread at any given point. The programmer has no control over this.
- **Cooperative scheduling** means that programmer can decide when to pause a task to allow other tasks to execute.
- Concurrency:
  - When different parts of your program **execute independently**.
  - Time Slicing:
    - Execution of these parts is interleaved on a single core.
  - Parallel Execution:
    - Execution of these parts happens at the same time using multiple cores.
- Concurrency Models:
  - OS Threads (in Rust)
    - A basic concurrency primitive provided by the operating system
  - Coroutines
    - execute functions concurrently. Hides low level details
  - The Actor Model
    - Divides concurrent computation into units called actors that could communicate with each other through message parsing.
  - Asynchronous Programming (in Rust)
    - execute functions concurrently. Exposes some of the low level details.
  - Event-driven Programming
    - With callbacks, which used to be popular concurrency model in JavaScript

| OS Threads                                    | Async Tasks                                  |
| --------------------------------------------- | -------------------------------------------- |
| Managed by the OS                             | Managed by language runtime or library       |
| Preemptive scheduling                         | Cooperative Scheduling                       |
| Higher performance overhead                   | Lower performance overhead                   |
| Ideal for small amount of CPU-bound workloads | Ideal for large amount of IO-bound workloads |
| Harder to reason about                        | Easier to reason about                       |

## Multi Producer Single Consumer (mpsc)

```rust
use std::{
    sync::mpsc,
    thread
};

let (tx, rx) = mpsc::channel();

for sentence in sentences {
    let tx = tx.clone();
    thread::spawn(move || {
        let sentence: String = sentence.chars().rev().collect();
        tx.send(sentence).unwrap();
    });
}

drop(tx);

for received in rx {
    println!("Got: {}", received);
}
```

## Mutexes

```rust
#[derive(Debug)]
struct Database {
    connections: Vec<i32>,
}

impl Database {
    fn new() -> Database {
        Database {
            connections: Vec::new(),
        }
    }

    fn connect(&mut self, id: i32) {
        self.connections.push(id);
    }
}
fn main() {
    let db = Mutex::new(Database::new());
    {
        let mut db_lock = db.lock().unwrap();
        db_lock.connect(1);
    }
}
```

## Mutexes with Arc (Atomic Reference Counter)

```rust
use std::{
    sync::{Arc, Mutex},
    thread
};

#[derive(Debug)]
struct Database {
    connections: Vec<i32>,
}

impl Database {
    fn new() -> Database {
        Database {
            connections: Vec::new(),
        }
    }

    fn connect(&mut self, id: i32) {
        self.connections.push(id);
    }
}

fn main() {
    let db = Arc::new(Mutex::new(Database::new()));

    let mut handles = vec![];

    for i in 0..10 {
        let db = db.clone();
        let handle = std::thread::spawn(move || {
            let mut db_lock = db.lock().unwrap();
            db_lock.connect(i);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    let db_lock = db.lock().unwrap();
    println!("{:?}", db_lock);
}
```

## async & await

- async function is simply a function which returns something that implements the Future trait.
- async / .await is special syntax which allows us to write functions, closures, and blocks that can pause execution and yield control back to the runtime allowing other code to make progress, and pick back up from where they left off.
- One advantage of the async / .await syntax is that it allows us to write asynchronous code, which looks like synchronous code.
- In Rust, Futures are lazy, meaning that they won’t do anything unless they are driven to completion by being polled. This allows Futures to be a zero cost abstraction.
- Futures could be driven to completion by either awaiting the future or giving it to an executor.
- .await keyword is only used if the function in which the keyword used is async function.
- Futures could be driven to completion in two ways, either calling await on the future or manually polling the future until it’s complete.

## Runtime or Executer

- A runtime is responsible polling the top level futures and running them till completion. It’s also responsible for running multiple futures in parallel.
- Rust standard library does not provide an async runtime. Most popular community built runtime is called as “tokio”.
