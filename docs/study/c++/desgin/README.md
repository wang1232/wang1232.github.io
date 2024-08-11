# 1、单例模式

Sinal

## 1.1、概念：

唯一的实例，保证**系统中的一个类只有一个实例对象**，而且该实例易于外界访问，在程序中**多次使用同一对象且作用相同时**，**仅创建一个可共享的对象**，让所有需要调用的地方都共享这一个单例对象，从而方便对示例个数的控制并节约系统资源。



## 1.2、核心思想：

**拿到其指针**。

* 私有化
	* 默认构造函数
	* 拷贝构造函数
	* 唯一实例指针
* 对外提供只读接口getInstance()，将指针返回。



## 1.3、程序代码：

主席单例模式：

```c++
class chairman{ //zhu
private:
    //将构造函数私有化，不可以创建多个对象
    chairman(){};
    chairman(const chairman&){}; //防止p3情况
private:
    //主席类对象虽然只能有一个，但是要被大家共享，主席是大家的主席
    //将主席的指针也要私有化，对外提供只读接口，防止对其操作赋值置空。
    static chairman * singleMan; //通过唯一的指针访问
public:
    static chairman *getInstance(){ //只读接口
        return singleMan;  //静态成员函数访问静态成员变量
    }
};

chairman * chairman::singleMan = new chairman; //类外初始化

void test(){
    //chairman * c1 = chairman::singleMan; //只能类名访问
    //chairman * c2 = chairman::singleMan; //这里即使再创建C2也是都指向chairman。
   	chairman *p =  chairman::getInstance();主席
    chairman *p2 = chairman::getInstance();  //p=p2
	//chairman *p3 = chairman(*p); //通过拷贝构造函数克隆主席，但是默认构造的是浅拷贝
}
```

打印机模式：

```c++
class Printer{
private:
    Printer(){};
    Printer(const Printer &p){};
	static Printer *pre;
    Static int count; //打印机使用次数
public:
    static Printer *getinstance(){
        return pre;
    }
    void print_txt(String s){
        count++;
        cout << s << endl;
	}
    Static int *getcount(){
        return count;
    }
}
Printer * Printer::pre = new Printer;
int Printer::count = 0;

void test(){
    Printer *p = Printer::getinstance();
    p->print_txt("打印入职证明");
    count << p->count << endl; //输出da
}
```



## 1.4、单例模式的类型

* **懒汉式**：在**真正需要使用对象时**采取创建单例类对象
	* 在程序使用前，先判断对象是否已经实例化
		* 若已经实例化直接返回该类对象
		* 否则先执行实例化操作
* **饿汉式**：在**类加载时**已经创建好该单例对象，等待程序使用

```c++
public class Singleton {
    //懒汉模式
    private static Singleton singleton;
    
    private Singleton(){}
    
    public static Singleton getInstance() {
        if (singleton == null) {
            singleton = new Singleton();
        }
        return singleton;
    }    
}

public class Singleton{
    //饿汉
    private static final Singleton singleton = new Singleton();
    
    private Singleton(){}
    
    public static Singleton getInstance() {
        return singleton;
    }
}
```





## 1.5、多线程下的单例模式

在多线程的懒汉模式下，如果两个线程同时对程序中的对象判空进行实例化，会引发**冲突**

**解决方法：**

* 加锁--->当线程A和B同时看到对象未实例化时，对类对象进行加锁，此时只有本线程能进行实例化并返回对象。

```c++
public class Singleton {
    
    private static Singleton singleton;
    
    private Singleton(){}
    
    public static Singleton getInstance() {
        if (singleton == null) {  // 线程A和线程B同时看到singleton = null，如果不为null，则直接返回singleton
            synchronized(Singleton.class) { // 线程A或线程B获得该锁进行初始化
                if (singleton == null) { // 其中一个线程进入该分支，另外一个线程则不会进入该分支
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }  
}
```





**总结：**

synchronized也可以保证有序性为什么还要用volatile呢？

我的理解是：单线程中为了提高效率会进行指令重排，而在多线程中这可能会带来意想不到的问题。volatile通过禁止指令重排保证了有序性从而解决了这个问题。而synchronized则是通过加锁同步来实现代码块串行执行以达到像单线程一样的效果，而单线程中指令重排的前提是不改变程序运行结果的，所以也保证了有序性。所以synchronized保证的是各个线程中使用同一把锁代码块内部代码相对的有序性。而单例模式中synchronized代码块外部的if (singleton == null)与内部singleton = new Singleton()两个语句结合来看就不具备有序性了。而volatile修饰的变量所在语句具有有序性，所以要用volatile。





# 2、享元模式（Flyweight）

## 2.1 概念：

资源的共享与复用，利用共享技术有效的支持大量系类，通过将对象存储到**对象池**中实现对象的重复利用，这样可以避免多次创建重复对象的开销，节约系统资源。

**享元池**：享元模式中存放数据的池

```c++
abstract class BikeFlyWeight{
	//内部状态
	protected Integer state = 0;
	//userName
	abstract void ride(String userName);
	adstract void back();
    
	public Integer getState(){  //接口返回state状态
		return state;
	}
};
```

