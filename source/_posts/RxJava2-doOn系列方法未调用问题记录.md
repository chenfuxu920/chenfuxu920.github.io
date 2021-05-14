---
title: RxJava2-doOn系列方法未调用问题记录
date: 2018.01.04 14:50:52
tags: [Android, RxJava]
categories: Android
toc: true
---
现象：
>在map，flatmap中手动抛出异常，Subscriber的onError()方法被调用，但是doOnError/doOnTerminate等操作符未被调用，仅有doOnCancel被调用。

尝试解决过程：
>修改一下，将doOn系列方法置于线程切换方法之前，此时，doOnTerminate操作符生效，但是doOnError仍然无效。
>再次修改，将map，flatmap操作符置于doOn系列方法之前，此时不管doOn系列方法是否在线程切换方法之前，doOn系列方法均正常生效。另外有一点，未抛出异常时，不管方法顺序如何，doOn系列方法均正常生效

结论：
>doOn系列方法应该在最接近订阅方法的地方添加，不要在doOn系列方法之后再执行变换等数据变化操作，否则会导致doOn系列方法无法被正确调用。