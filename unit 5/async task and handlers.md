## AsyncTask:

`AsyncTask` is a class provided by Android that allows you to perform background operations and update the UI thread with the results. It simplifies the process of handling asynchronous tasks in Android by providing convenient methods for executing code in the background and updating the UI. The use of `AsyncTask` is to perform background operations in Android applications, such as making network requests, database queries, or any other long-running tasks that should not block the main/UI thread. By executing these operations in the background, you ensure that the UI remains responsive and does not freeze.

The three generic types used in `AsyncTask` are `Params`, `Progress`, and `Result`, which represent the types of parameters passed to the task, the type of progress units published during the task's execution, and the result returned by the task.

## AsyncTask's generic types

The three types used by an asynchronous task are the following:

1. `Params`, the type of the parameters sent to the task upon execution.
2. `Progress`, the type of the progress units published during the background computation.
3. `Result`, the type of the result of the background computation.
   Not all types are always used by an asynchronous task. To mark a type as unused, simply use the type Void:

```java
 private class MyTask extends AsyncTask<Void, Void, Void> { ... }
```

## The 4 steps

When an asynchronous task is executed, the task goes through 4 steps:

1. `onPreExecute()`, invoked on the UI thread before the task is executed. This step is normally used to setup the task, for instance by showing a progress bar in the user interface.
2. `doInBackground(Params)`, invoked on the background thread immediately after onPreExecute() finishes executing. This step is used to perform background computation that can take a long time. The parameters of the asynchronous task are passed to this step. The result of the computation must be returned by this step and will be passed back to the last step. This step can also use publishProgress(Progress) to publish one or more units of progress. These values are published on the UI thread, in the onProgressUpdate(Progress) step.
3. `onProgressUpdate(Progress)`, invoked on the UI thread after a call to publishProgress(Progress). The timing of the execution is undefined. This method is used to display any form of progress in the user interface while the background computation is still executing. For instance, it can be used to animate a progress bar or show logs in a text field.
4. `onPostExecute(Result)`, invoked on the UI thread after the background computation finishes. The result of the background computation is passed to this step as a parameter.
   Here's an example of using `AsyncTask` to fetch data from a network API and update the UI with the results:

```java
public class NetworkTask extends AsyncTask<Void, Void, String> {

    @Override
    protected String doInBackground(Void... params) {
        // Perform network operation (e.g., API call) in the background
        // Return the result to onPostExecute
        return fetchDataFromAPI();
    }

    @Override
    protected void onPostExecute(String result) {
        // Update the UI with the fetched data
        textView.setText(result);
    }
}

// To execute the task:
new NetworkTask().execute();
```

In this example, `doInBackground()` method performs the network operation in the background, while `onPostExecute()` runs on the UI thread and updates the UI with the fetched data. The `execute()` method is used to start the task.

The `AsyncTask<Void, Void, String>` used in the `NetworkTask` class declaration represents the type parameters for the `AsyncTask` class. These type parameters define the input, progress, and result types for the task. Let's break down each type parameter:

1. `Void`: The first type parameter represents the input type for the task's `doInBackground` method. In this case, `Void` indicates that the task does not take any input parameters. The `doInBackground` method in the `NetworkTask` example does not require any input, so `Void` is used.

2. `Void`: The second type parameter represents the progress type for the task. It is used when you want to report the progress of the background operation to the UI thread. In this example, `Void` is used to indicate that no progress information will be reported. If you need to update the UI with progress updates, you can replace `Void` with a suitable progress type, such as `Integer` or `Float`.

3. `String`: The third type parameter represents the result type of the task's `doInBackground` method and the input type for the `onPostExecute` method. In this case, the `doInBackground` method fetches data from the API and returns it as a `String`. The `onPostExecute` method receives this result and uses it to update the UI.

The `AsyncTask` class provides a convenient way to handle the background operation and communicate with the UI thread. It contains several callback methods that allow you to execute code on the background thread (`doInBackground`), report progress (`onProgressUpdate`), and update the UI with the result (`onPostExecute`).

In the example you provided, `AsyncTask<Void, Void, String>` is used to create a background task that fetches data from an API and updates a `textView` with the fetched data. The `doInBackground` method performs the network operation, `onPostExecute` updates the UI, and `execute` is called to start the task.

Example that includes all three arguments of an `AsyncTask`:

```java
import android.os.AsyncTask;
import android.util.Log;

public class NetworkTask extends AsyncTask<String, Integer, String> {

    private static final String TAG = "NetworkTask";

    @Override
    protected String doInBackground(String... urls) {
        int totalProgress = 0;
        for (String url : urls) {
            // Perform network operation for each URL
            // ...
            // Simulating progress update
            totalProgress += 10;
            publishProgress(totalProgress);
        }
        return "All URLs processed";
    }

    @Override
    protected void onProgressUpdate(Integer... progressValues) {
        int progress = progressValues[0];
        Log.d(TAG, progress + "% completed");
    }

    @Override
    protected void onPostExecute(String result) {
        Log.d(TAG, "Task completed: " + result);
    }
}
```

In this example:

1. The `NetworkTask` class extends `AsyncTask<String, Integer, String>`, indicating that the first parameter (`String`) represents the input type, the second parameter (`Integer`) represents the progress type, and the third parameter (`String`) represents the result type.

2. The `doInBackground` method receives an array of URLs as the input (`String... urls`) and iterates over each URL. For demonstration purposes, it simulates progress by incrementing the `totalProgress` variable and publishing the progress using `publishProgress(totalProgress)`.

3. The `onProgressUpdate` method logs the progress value received (`progressValues[0]` in this case).

4. The `onPostExecute` method logs the completion of the task and the result.

To execute this `NetworkTask` and process multiple URLs, you can use the following code:

```java
String url1 = "https://www.example1.com/";
String url2 = "https://www.example2.com/";
String url3 = "https://www.example3.com/";

NetworkTask networkTask = new NetworkTask();
networkTask.execute(url1, url2, url3);
```

In this example, `url1`, `url2`, and `url3` represent the URLs you want to process. Create an instance of `NetworkTask` and call `execute` on it, passing the URLs as separate arguments. This will start the execution of the `NetworkTask`, triggering the appropriate methods (`doInBackground`, `onProgressUpdate`, `onPostExecute`) based on the execution flow.

As the `NetworkTask` processes each URL, it will publish progress updates through `publishProgress`, and the `onProgressUpdate` method will be invoked with the progress value. After all URLs are processed, the `onPostExecute` method will be called, logging the completion of the task and the result message.

## Handler:

`Handler` is a class that allows you to schedule and handle messages and runnable objects to be executed on a specific thread. It is often used for communication between background threads and the UI thread in Android. `Handler` allows you to post messages or runnables to the message queue of a thread and process them at a later time.

Here's an example of using `Handler` to update a UI element periodically:

```java
private Handler handler = new Handler();
private Runnable runnable = new Runnable() {
    @Override
    public void run() {
        // Update UI element here
        updateUI();
        // Schedule the next update after a delay
        handler.postDelayed(this, 1000); // 1 second delay
    }
};

// To start the periodic updates:
handler.post(runnable);

// To stop the periodic updates:
handler.removeCallbacks(runnable);
```

This code snippet demonstrates a common pattern used in Android development for performing periodic updates to the user interface (UI) elements.

1. `private Handler handler = new Handler();`: This line creates an instance of the `Handler` class, which is used to schedule and manage tasks on the main (UI) thread.

2. `private Runnable runnable = new Runnable() {...}`: This creates an anonymous inner class implementing the `Runnable` interface. The `Runnable` interface represents a unit of work that can be executed by a thread. In this case, the `run()` method will be executed periodically to update the UI elements.

3. `public void run() { ... }`: The `run()` method contains the code that will be executed periodically. In this example, the `updateUI()` method is called to update the UI elements. You would replace this method with your own code to update the UI based on your requirements.

4. `handler.postDelayed(this, 1000);`: This line schedules the next execution of the `run()` method after a 1-second delay. It uses the `postDelayed()` method of the `Handler` class to enqueue the `Runnable` for execution on the main (UI) thread.

5. `handler.post(runnable);`: This line starts the periodic updates by posting the `runnable` object to the `Handler` for execution. The `run()` method will be called immediately, and subsequent executions will be scheduled using `postDelayed()`.

6. `handler.removeCallbacks(runnable);`: This line stops the periodic updates by removing any pending executions of the `runnable` from the `Handler`'s queue. It ensures that the `run()` method will no longer be called periodically.

In summary, this code sets up a `Handler` to execute a `Runnable` periodically on the UI thread. The `run()` method updates the UI elements, and the updates occur every 1 second (1000 milliseconds) until `removeCallbacks()` is called to stop the periodic updates.


## ThreadPoolExecutor:

`ThreadPoolExecutor` is a class that provides an efficient way to manage and execute multiple threads in a pool. It is a more flexible alternative to using raw threads directly. `ThreadPoolExecutor` allows you to control the maximum number of threads, handle task scheduling, and manage thread lifecycle.

Here's an example of using `ThreadPoolExecutor` to execute multiple tasks concurrently:

```java
int corePoolSize = 2; //max 2 core
int maximumPoolSize = 4; //max 4 thread
long keepAliveTime = 60L; // 60 seconds
TimeUnit unit = TimeUnit.SECONDS;
BlockingQueue<Runnable> workQueue = new LinkedBlockingQueue<>();
ThreadFactory threadFactory = Executors.defaultThreadFactory();
RejectedExecutionHandler handler = new ThreadPoolExecutor.AbortPolicy();

ThreadPoolExecutor executor = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory, handler);

// Submit tasks to the executor
executor.submit(new Task1());
executor.submit(new Task2());
executor.submit(new Task3());

// Shutdown the executor when no more tasks need to be submitted
executor.shutdown();
```

This code snippet demonstrates the usage of `ThreadPoolExecutor` in Java for managing and executing multiple tasks concurrently.

1. `int corePoolSize = 2;`: This line defines the core pool size, which represents the number of threads that the executor will keep alive even if they are idle. In this example, the core pool size is set to 2.

2. `int maximumPoolSize = 4;`: This line specifies the maximum pool size, which is the maximum number of threads that can be created by the executor. If there are more tasks than the core pool size and the work queue is full, the executor can create additional threads up to the maximum pool size. In this example, the maximum pool size is set to 4.

3. `long keepAliveTime = 60L;`: This line sets the keep-alive time for idle threads in the pool. If a thread is idle for the specified duration, it may be terminated and removed from the pool until the pool size reduces to the core pool size. In this example, the keep-alive time is set to 60 seconds.

4. `TimeUnit unit = TimeUnit.SECONDS;`: This line specifies the time unit for the keep-alive time. In this case, it is set to seconds.

5. `BlockingQueue<Runnable> workQueue = new LinkedBlockingQueue<>();`: This line creates a `BlockingQueue` implementation called `LinkedBlockingQueue` to hold the tasks that are waiting to be executed. The `LinkedBlockingQueue` has an unlimited capacity, but other implementations with a fixed capacity can also be used.

6. `ThreadFactory threadFactory = Executors.defaultThreadFactory();`: This line creates a `ThreadFactory` that will be used by the executor to create new threads. In this case, the `defaultThreadFactory()` method from the `Executors` class is used to create a default thread factory.

7. `RejectedExecutionHandler handler = new ThreadPoolExecutor.AbortPolicy();`: This line sets the `RejectedExecutionHandler` that determines what happens when a task cannot be accepted by the executor due to resource limitations. In this example, the `AbortPolicy` is used, which throws a `RejectedExecutionException` when a task cannot be executed.

8. `ThreadPoolExecutor executor = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory, handler);`: This line creates an instance of `ThreadPoolExecutor` with the specified parameters. It represents the executor that will manage the execution of tasks.

9. `executor.submit(new Task1());`: This line submits a task (`Task1`) to the executor for execution. The `submit()` method returns a `Future` object representing the task's result and allows monitoring its status and obtaining the result if necessary.

10. `executor.shutdown();`: This line initiates the graceful shutdown of the executor. It allows the executor to finish executing the existing tasks but does not accept any new tasks. Once all tasks are completed, the executor terminates and releases its resources.

In summary, this code creates a `ThreadPoolExecutor` with a fixed number of core and maximum threads, a work queue for holding tasks, and specific policies for managing rejected tasks. It then submits tasks to the executor and shuts it down when no more tasks need to be submitted..
