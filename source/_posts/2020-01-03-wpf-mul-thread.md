---
title: WPF multiple thread
date: 2020-01-03
tags:
- WPF
- ASP 
---
https://www.quartz-scheduler.net/
> Quartz.NET is a full-featured, open source job scheduling system that can be used from smallest apps to large scale enterprise systems.

Quartz.NET

https://www.cnblogs.com/jys509/p/4628926.html）。


##### WPF multiple thread

```
class Program   
{   
    private static int newTask(int ms)   
    {   
        Console.WriteLine("任务开始");   
        Thread.Sleep(ms);   
        Random random = new Random();   
        int n = random.Next(10000);   
        Console.WriteLine("任务完成");   
        return n;   
    }   
  
    private delegate int NewTaskDelegate(int ms);   
    static void Main(string[] args)   
    {   
        NewTaskDelegate task = newTask;   
        IAsyncResult asyncResult = task.BeginInvoke(2000, null, null); // EndInvoke方法将被阻塞2秒   

        //Do Something else  
        int result = task.EndInvoke(asyncResult);   
        Console.WriteLine(result);  
    }  
}
```
这里的`BeginInvoke`就是异步调用，运行后立即返回，不会引起当前线程的阻塞。但是在本例程中，因为`newTask`中要`Sleep 2`秒中，如果Do Something else的时间没有2秒的话，`EndInvoke`还是会引起当前线程的阻塞，因为它要等待`newTask`执行完毕。
那能不能不调用`EndInvoke`，让它自己结束呢？不太好。因为一来BeginInvoke和EndInvoke必须成对调用。即使不需要返回值，但EndInvoke还是必须调用，否则可能会造成内存泄漏，因为它是利用了线程池资源。二来往往要调用EndInvoke来获得函数的==返回值==。

