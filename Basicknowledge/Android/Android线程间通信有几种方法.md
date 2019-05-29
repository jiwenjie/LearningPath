**Android线程间通信有几种方法**

1. 通过 `Handler` 来通信

   ```
   val handler = @SuppressLint("HandlerLeak")
   object : Handler(){
       override fun handleMessage(msg: Message?) {
           Log.d("taonce","msg arg1: ${msg?.arg1}")
       }
   }
   thread {
       val msg: Message = handler.obtainMessage()
       msg.arg1 = 1
       handler.sendMessage(msg)
   }
   ```

2. 通过 `runOnUiThread()`

   ```
   thread {
       val text = "runOnUiThread"
       runOnUiThread {
           tv.text = text
       }
   }
   ```

3. 通过 `View.post()`

   ```
   thread {
       val text = "post"
       tv.post {
           tv.text = text
       }
   }
   ```

4. 通过 `AsyncTask`

   ```
   class MyAsyncTask(val name: String) : AsyncTask<String, Int, Any>() {
   
       // 执行任务之前的准备工作，比如将进度条设置为Visible，工作在主线程
       override fun onPreExecute() {
           Log.d("async", "onPreExecute")
       }
   
       // 在onPreExecute()执行完之后立即在后台线程中调用
       override fun doInBackground(vararg params: String?): Any? {
           Log.d("async", "$name execute")
           Thread.sleep(1000)
           publishProgress(1)
           return null
       }
   
       // 调用了publishProgress()之后，会在主线程中被调用，用于更新整体进度
       override fun onProgressUpdate(vararg values: Int?) {
           Log.d("async", "progress is: $values")
       }
   
       // 后台线程执行结束后，会把结果回调到这个方法中，并在主线程中被调用
       override fun onPostExecute(result: Any?) {
           Log.d("async", "onPostExecute")
       }
   }
   ```

   5. 使用EventBus、RxJava等框架