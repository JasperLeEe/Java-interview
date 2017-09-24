## ConcurrentHashMap

#### 初始化

通过位运算来初始化Segment大小，用ssize表示。

````java
int sshift = 0;
int ssize = 1;
while (ssize < concurrencyLevel) {
	++sshift;
  	ssize <<= 1;
}
````

因为是通过位运算，所以Segment的大小取值都是以2的N次方，没有指定的concurrencyLevel元素初始化，Segment的大小ssize默认为16。

每个Segment元素下的HashEntry的初始化也是通过位运算计算，用cap表示。

````java
int cap = 1;
while (cap < c)
  	cap <<= 1;
````

cap的初始值为1，所以HashEntry的最小容量为2。

#### put操作

对于ConcurrentHashMap的数据插入，要进行两次Hash去定位数组的存储位置。

````java
static class Segment<K,V> extends
ReentrantLock implement Serializable {}
````

当执行put操作时，会进行第一次key的hash来定位Segment的位置，如果该Segment还未初始化，即通过CAS操作进行赋值，然后进行第二次hash操作，找到相应的HashEntry位置，这里会利用继承过来的锁的特性，在数据插入指定的HashEntry位置时，会通过继承ReentrantLock的tryLock()方法尝试去获取锁，如果获取成功则插入，反之当前线程会以自旋的方式去继续调用tryLock()，超过指定次数则挂起，等待唤醒。

#### get操作

与HashMap类似，不过需要进行两次hash操作来定位。

#### 