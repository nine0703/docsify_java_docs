# Java 动态代理详解

**什么是动态代理？**

动态代理是一种在运行时创建代理对象的机制，而不是在编译时确定。它允许你在运行时创建一个实现了一组特定接口的代理类，用来代理对这些接口的调用。

**为什么要使用动态代理？**

动态代理在许多场景下非常有用，特别是在实现 AOP（面向切面编程）和其他横切关注点时。它允许你在不修改原始类代码的情况下，增加额外的功能或行为。

### 快速开始

#### 实现接口

由于动态代理是基于接口实现的，所以我们需要有一个接口，下面就以Star接口为示例。

```java
package Anno;

public interface Star {
    void sing(String name);
    void dance();
}
```

这个接口中定义了两个属性

#### 使用Proxy类创建动态代理对象

```java
package Anno;

public class StarProxy {
    public static void main(String[] args) {
//      Object proxy = Proxy.newProxyInstance();		// 源语句，替换成	↓↓↓
        Star proxy = (Star) Proxy.newProxyInstance();	// <--- 强制类型转换语句
    }   // Method main end.
}   // Class end.
```

由于我们的  [动态代理对象]  需要实现对Star接口实现类的动态代理，为了避免出现idea的错误提示，我们提前对这个对象进行强制类型转换，免得后面写代码的时候好像如履薄冰。		`我这一生如履薄冰，你说我能走到对岸吗？	——庞青云《投名状》		`

图片.jpg（没有）

有了代理对象，我们要对代理对象进行参数的配置

```java
package Anno;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class StarProxy {
    public static void main(String[] args) {
        Star proxy = (Star) Proxy.newProxyInstance(
                StarProxy.class.getClassLoader(),	// 参数一，我们约定，使用代理对象所在类类名的getClassLoader
                new Class[] {Star.class},	// 参数二，传入接口类名
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        // 代理逻辑
                        method.invoke(wbq,args);	// 被代理的实际方法
                        // 代理逻辑
                        return null;
                    }
                }
        );
	}   // Method main end.
}   // Class end.
```

接下来，你需要使用反射，创建需要代理的对象，然后通过反射调用创建的对象，这样才能进行动态代理

假设我们有一个Star接口的实现类WangBoQiang，

现在，我们将使用 WangBoQiang类来创建代理对象。

```java
package Anno;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class StarProxy {
    public static void main(String[] args) {
        Star wbq = new WangBoQiang();		// 创建WangBoQiang对象
        Class<?> clazz = "";
        Star proxy = (Star) Proxy.newProxyInstance(
                StarProxy.class.getClassLoader(),	// 参数一，我们约定，使用代理对象所在类类名的getClassLoader
                new Class[] {Star.class},	// 参数二，传入
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        // 代理逻辑
                        method.invoke(wbq,args);	// 被代理的实际方法，在第一个参数传入WangBoQiang对象
                        // 代理逻辑
                        return null;
                    }
                }
        );
	}   // Method main end.
}   // Class end.
```

#### 实现动态代理

现在，我们已经实现了动态代理的创建，现在你可以使用动态代理对象，使用反射的方式调用创建的WangBoQiang对象，WangBoQiang对象所作的任何操作都会被invoke方法进行代理。

在main方法中运行：

```
proxy.sing("周杰伦");
```

你将看到输出：

```
// 先执行自定义操作：代理逻辑
WangBoQiang sing 周杰伦
// 执行自定义操作：代理逻辑
```

这证明了动态代理已成功地代理了对 `sing()` 方法的调用，并在调用前后添加了额外的行为。
