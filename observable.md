### 一、可观察对象
1、可观察对象是声明式的，也就是说，虽然定义了一个用于发布值的函数，但是在有消费者订阅它之前，这个函数并不会实际执行。
2、可观察对象的实例有subscriber()方法，调用后会返回一个Subscripition对象，该对象具有一个unsubscribe()方法。当调用该方法时，就会停止接收通知。
3、观察者要定义一些回调函数来处理可观察对象可能会发来的三种通知：
| 通知类型 | 说明 |
|---------|------|
| next | 必要。用来处理每个送达值。可能执行零次或多次 |
| error | 可选，用来处理错误通知。错误会中断这个可观察对象实例的执行过程 |
| complete | 可选，用来处理执行完毕通知。当执行完毕后，这些值就会传递给下一个处理器 |
如果不为某种通知类型提供处理器，这个观察者就会忽略相应类型的通知。

### 二、创建可观察对象(observable)
1、使用observable上定义的静态方法，of(...items)——返回一个observable实例，它用同步的方式把参数中提供的这些值发送出来。
```
const myObservable = of(1,2,3);
```
2、使用observable构造函数创建
```
function sequenceSubscriber(observer) {
    observer.next(1);
    observer.next(2);
    observer.next(3);
    observer.complete();

    return {
        unsubscriber() { }
    }
}

const myObservable = new Observable(sequenceSubscriber)

/////////////也可以把函数声明改为箭头函数//////////////////////
const myObservable = new Observable( () => {

})
```
3、使用observable上定义的静态方法，from(iterable)——把它的参数转换成一个observable实例。该方法通常用于把一个数组转换成一个可观察对象。
4、用内联方式定义
```
function fromEvent(target, eventName){
    return new Observable((observer) => {
        const handler = (e) => oberver.next(e);
        target.addEventLinstener(eventName, handler);
        return () => {
            target.removeEventListener(eventName, handler);
        }
    })
}
```

### 三、定义观察者
1、
```
const myObserver = {
    next: x => console.log('Observer got a next value: ' + x),
    error: err => console.log('Observer got an error: ' + err),
    complete: () => console.log('Observer got a complete notification')
};
myObserverble.subscriber(myObserver);
```
2、subscriber()方法还可以接收定义在同一行中的回调函数
```
//注意：subscribe方法中传入的是三个参数，而不是一个对象
myObservable.subscribe(
    x => console.log('Observer got a next value: ' + x),
    err => console.log('Observer got an error: ' + err),
    () => console.log('Observer got a complete notification')
)
```
3、
```
myObservable.subscribe({
    next(num) {console.log(num);},
    complete() {console.log('finish');}
})
```