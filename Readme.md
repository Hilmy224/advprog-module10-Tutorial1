## Explanation 1.2. Understanding how it works.

![alt text](image.png)

+ `Hilmy's Computer: hey hey` - This is immediately printed from main after spawning the task but before the executor starts processing tasks.
+ During task execution, the executor begins working through its list of tasks. 
+ One of these tasks prints `Hilmy's Computer: howdy!` when it starts executing. Additionally, there's a task waiting for a TimerFuture to finish. Inside its loop, the executor checks tasks by polling their futures to determine if they're complete. Initially, the status of the timer future is Pending because it's set to complete after a 2-second delay. 
+ Meanwhile, a separate thread created by TimerFuture::new(Duration::new(2, 0)) is sleeping. After 2 seconds, this thread wakes up and marks the task as complete by setting shared_state.completed = true and waking up the task. When the executor polls the task again, it finds that it's now Ready, indicating it's finished. With no more tasks left to do, the executor completes its work. This process illustrates how tasks can wait for events to finish asynchronously.
+ Hilmy's Computer: done! - This is printed by the spawned task when the TimerFuture completes after 2 second