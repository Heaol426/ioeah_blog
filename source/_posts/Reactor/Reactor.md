---
title: Reactor
date: 2024-01-15 09:20:16
categories: ["笔记"]
tags: ["Reactor"]
cover: date
---

> 响应式编程是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。
<!-- more -->

# 响应式编程
在开发应⽤程序代码时，我们可以编写两种⻛格的代码，即命令式和响应式。

命令式（Imperative）的代码：它由⼀组任务组成，每次只运⾏⼀项任务，每项任务⼜都依赖于前⾯的任务。数据会按批次进⾏处理，在前⼀项任务还没有完成对当前数据批次的处理时，不能将这些数据递交给下⼀项处理任务。

响应式（Reactive）的代码：它定义了⼀组⽤来处理数据的任务，但是这些任务可以并⾏地执⾏。每项任务处理数据的⼀部分⼦集，并将结果交给处理流程中的下⼀项任务，同时继续处理数据的另⼀部分⼦集。

响应式编程基于reactor（Reactor 是一个运行在 Java8 之上的响应式框架）的思想，当你做一个带有一定延迟的才能够返回的io操作时，不会阻塞，而是立刻返回一个流，并且订阅这个流，当这个流上产生了返回数据，可以立刻得到通知并调用回调函数处理数据。

电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。

响应式传播核心特点之一：变化传播：一个单元格变化之后，会像多米诺骨牌一样，导致直接和间接引用它的其他单元格均发生相应变化。

**基于Java8实现观察者模式**

Observable类：此类表示可观察对象，或模型视图范例中的“数据”。

它可以被子类实现以表示应用程序想要观察的对象。

```JAVA
//想要观察的对象 ObserverDemo
public class ObserverDemo extends Observable {
    public static void main(String[] args) {
        ObserverDemo observerDemo = new ObserverDemo();
        //添加观察者
        observerDemo.addObserver((o,arg)->{
            System.out.println("数据发生变化A");
        });
        observerDemo.addObserver((o,arg)->{
            System.out.println("数据发生变化B");
        });
        observerDemo.setChanged();//将此Observable对象标记为已更改
        observerDemo.notifyObservers();//如果该对象发生了变化，则通知其所有观察者
    }
}


```
**创建一个Observable**
rxjava中，可以使用Observable.create() 该方法接收一个Obsubscribe对象
```JAVA
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
    @Override
    public void call(Subscriber<? super Integer> subscriber) {

    }
});
```

示例：
```JAVA
Observable<Integer> observable=Observable.create(new Observable.OnSubscribe<Integer>() {
    @Override
    public void call(Subscriber<? super Integer> subscriber) {
        for(int i=0;i<5;i++){
            subscriber.onNext(i);
        }
        subscriber.onCompleted();
    }
});
//Observable.subscribe(Observer)，Observer订阅了Observable
Subscription subscribe = observable.subscribe(new Observer<Integer>() {
    @Override
    public void onCompleted() {
        Log.e(TAG, "完成");
    }

    @Override
    public void onError(Throwable e) {
        Log.e(TAG, "异常");
    }

    @Override
    public void onNext(Integer integer) {
        Log.e(TAG, "接收Obsverable中发射的值：" + integer);
    }
});


输出：

接收Obsverable中发射的值：0
接收Obsverable中发射的值：1
接收Obsverable中发射的值：2
接收Obsverable中发射的值：3
接收Obsverable中发射的值：4


```