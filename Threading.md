1. Sometimes you need to make two tasks run in the same thread:
	- ConfigureAwait(false) : You don't mind running the task on a different thread.
	- ConfigureAwait(true) : running task on same thread after releasing.
2. Every request for every endpoint is a thread on its own.
3. every endpoint in an API uses a thread form thread pool.
4. The "Main Thread" is the thread in which the app starts and ends.
5. Threading is concurrent and not parallel processing, Threading can work even on one core.
6. 