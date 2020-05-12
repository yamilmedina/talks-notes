# Structured Introduction to Kotlin Coroutines
- Link: https://www.recallact.com/presentation/structured-introduction-kotlin-coroutines

## Notes
- Async prgramming: Write code that waits withou blocking.
- Coroutines the promise is to write sync code and execute async.
- Options to Async:
    - Green Threads/Goroutines/Fibers.
    - Callback, event listeners.
    - Futures, promises, reactive.
    - Async/Await.
    - Suspending functions.

- Suspending, mark function as suspend, then you can await(), this will not block thread.
- You can write "normal" code, loops, highorder functions, etc.
- From kotlin to JVM get converted to Callback, using Continuation interface.
- You can wrap or integrate with other reactive libs.
    - Example with vanilla Java 8+: 
    ```suspend fun <T> CompletableFuture<T>.await(): T =
        suspendCoroutine { cont ->
            whenComplete { v, e ->
                if (e != null) cont.resumeWithExceotion(e)
                else cont.resume(v)
            }
        }

- Coroutines builders: 
    - All need a scope, launch, future, etc. 
    - `GlobalScope.launch {}`: returns immediately, in background thread.
    - `GlobalScope.launch(Dispatchers.Main) {}`: run in Main thread.
- Server blocking:
    - Spring webflux can make a blocking server suspend endpoints for instance.
    - You can use CompletableFuture and integrate with coroutines.
- Coroutines are like lightweight threads 
- Cancellation, use suspendCancellableCoroutine and invokeOnCancellation.
- Concurrency: Example 2 tasks.
    - Async/Futures: Classic approach use 2 Future, but you can leak in case of one fails.
    - Structured Concurrency: Delimit with `coroutineScope { }` and use async on calls.
    - So whe you await if one fails the other cancels, so no leak.
- Streams with coroutines:
    - Use a channel send, receive using builder `produce`. can leak if no one consume.
    - Kotlin flows, use builder `flow` (async sequence) and `emit` value from inside.
    - Then use `collect` to consume.
    - This not leak as it is a `cold` stream, doesn't run until collected.
