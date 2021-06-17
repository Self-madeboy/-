# 动态数组

## 构造函数

**首先是对容量限制的构造函数**：

```
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}
```

这里没有数组size初始化，用不上，如果没有给定容量就使用默认实例

```
elementData
```

这是一个缓存数组，这个缓存数组的大小是数组的容量

```
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

<u>这个构造函数默认构造一个容量为10的list但是疑问在于哪里是去构造10容量了？</u>（TODO）

```
length 是数组的属性，查看里面有多少个元素
```

这里为什么要转换成class？（TODO）

```
// c.toArray might (incorrectly) not return Object[] (see 6260652)
if (elementData.getClass() != Object[].class)
    elementData = Arrays.copyOf(elementData, size, Object[].class);
```

**构造函数小结**：

```
ArrayList arrayList = new ArrayList(); //这种默认初始容量为10
ArrayList arrayList1 = new ArrayList(4);  //这种指定初始化容量
ArrayList<Integer> arrayList2 = new ArrayList<>(); //这种初始化一种具体的集合，会返回集合的迭代器（TODO）
```

**transient不参与序列化和反序列化**

Java的serialization提供了一种持久化对象实例的机制。当持久化对象时，可能有一个特殊的对象数据成员，我们不想用serialization机制来保存它。

为了在一个特定对象的一个域上关闭serialization，可以在这个域前加上关键字transient。

当一个对象被序列化的时候，transient型变量的值不包括在序列化的表示中，然而非transient型的变量是被包括进去的。

```
protected transient int modCount = 0;
```

这个字段表示这个列表结构化修改的次数，结构化修改指的是那些对列表大小进行的改变。否则在迭代过程中干扰列表可能会造成不正确的结果

**裁剪list**：个人认为是为了在对数组进行删除覆盖等减少size操作后，为了返回正确的修改结果，裁剪list

```
public void trimToSize() 
```

**数组扩容算法**：数组以前的容量+数组以前的容量>>1位

```
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

