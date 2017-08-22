关于守护线程 慎用 多用于信息提示
```java
thread1{
run() {
synchronized(obj) {
while(条件1不满足)obj.wait();
//下面写满足后的操作
}
}
}
```
//线程2中将条件1满足，然后唤醒线程1
```java
thread2{
run() {
synchronized(obj) {
//使条件1满足
obj.notifyAll();
}
}
} 
```
obj一定要是对同一个对象的引用