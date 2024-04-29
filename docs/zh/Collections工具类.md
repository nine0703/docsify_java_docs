## Collections工具类

### addAll方法（添加元素）

```java
Collections.addAll(list, 33, 44, 55, 66);	// 传入列表（实现List接口），参数（可变参数）
```

### shuffle方法（打乱顺序）

```java
Collections.sort(list, 排序方法);		// 方法可被列表list.sort代替
排序方法：
1.new compare接口
Collections.sort(list, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return 0;
            }
        });

2.lamda表达式
Collections.sort(list, (o1, o2) -> Integer.compare(o1,o2));

3.方法引用
Collections.sort(list, Integer :: compare);
```

