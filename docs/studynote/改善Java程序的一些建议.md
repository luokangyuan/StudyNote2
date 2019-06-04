<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">改善Java程序的一些建议</h1>

> The reasonable man adapts himself to the world: the unreasonable one persists in trying to adapt the world to himself.

### 1.不要在变量或者常量中出现易混淆的字母

```java
 long i = 1l;
```

**说明：**上述就是数字`1`和小写字母`l`进行的混淆，在IDEA开发中，IDE会自动提示使用大写字母`L`。

### 2.不能让常量变为变量

```java
 public static final int FILE_SIZE = new Random().nextInt();
```

**说明：**常量的值在运行期是不能改变的。

### 3.三元操作符的类型必须一致

```java
 int i = 80;
String s = String.valueOf(i < 100 ? 80 : 100);
String s1 = String.valueOf(i < 100 ? 80 : 100.0);
boolean equals = s.equals(s1);
// 两者是否相等： false,字符串s的值：80,字符串s1的值：80.0
log.debug("两者是否相等： {},字符串s的值：{},字符串s1的值：{}", equals, s, s1);
```

**说明：**在开发中，使用三目运算来简化`if-else`操作，如果三目运算符的类型不一致，返回的结果也不一致。

### 4.避免有变长参数的方法重载

```java
public class Client {

    private static final Logger log = LoggerFactory.getLogger(Client.class);

    public static void main(String[] args) {
        Client client = new Client();
        client.calPrice(5000, 80);
    }

    /**
     * calPrice 简单折扣计算
     *
     * @param price    价格
     * @param discount 折扣
     * @description
     * @author luokangyuan
     * @date 2019-4-2 14:58
     * @version 1.0.0
     */
    private void calPrice(int price, int discount) {
        float knockdownPrice = price * discount / 100.0F;
        log.info("简单的折扣后的价格：{}", knockdownPrice);
    }

    /**
     * calPrice
     *
     * @param price     价格
     * @param discounts 折扣
     * @description 复杂折扣计算
     * @author luokangyuan
     * @date 2019-4-2 15:08
     * @version 1.0.0
     */
    private void calPrice(int price, int... discounts) {
        float knockdownPrice = price;
        for (int discount : discounts) {
            knockdownPrice = knockdownPrice * discount / 100;
        }
        log.info("复杂折扣后的价格：{}", knockdownPrice);
    }
}
```

**说明：**方法重载就是**方法名相同，参数类型或者参数数量不同**，在上述例子中`Java`编辑器会根据方法签名找到合适的合适的方法，上述测试调用的就是简单的折扣计算，而非复杂折扣计算。

### 5.别让`null`值和空值威胁变长方法

```java
public void countSum(String type, Integer... is){}

public void countSum(String type, String... strs){}

public static void main(String[] args) {
    ClientReload clientReload = new ClientReload();
    clientReload.countSum("China", 0);
    clientReload.countSum("China", "People");
    // 编译报错
    clientReload.countSum("China");
    // 编译报错
    clientReload.countSum("China", null);
}
```

**说明：**同样是含有变长参数的重载方法，外部调用的使用使用`NULL`或者空值都会出现编译不通过的错误，这是应为`NULL`和空值在上述重载的方法中都满足参数条件，所以编译器不知道调什么方法，在重载方法的设计中违反了`KISS`原则，同时在外部调用的时候，外部调用这隐藏了实参的类型，如将调用代码做如下修改，就不存在编译报错了。

```java
String strs = null;
clientReload.countSum("China", strs);
```

### 6.变长方法重写也要循规蹈矩

```java
public class ClientOverride {
    public static void main(String[] args) {
        // 向上转型
        Base base = new Sub();
        base.countNum(100, 50);
        // 不转型，编译报错
        Sub sub = new Sub();
        sub.countNum(100, 50);
    }
}

class Base {
    void countNum(int price, int... discounts) {
        System.out.println("这是父类的方法");
    }
}

class Sub extends Base {
    @Override
    void countNum(int price, int[] discounts) {
        System.out.println("这是子类的方法");
    }
}
```

**说明：**可变参数编译后字节码形参也是一个数组类型，因此可以说子类是复写了父类的方法，只不过重写的时候并没有循规蹈矩的写，通过父类调用子类的方法，遵循的是父类的参数列表，调用子类重写的方法时候，参数列表不是`int`类型的数组就会出现编译报错。

**方法重写的原则：**

* 重写方法不能缩小访问权限。
* 参数列表必须与被重写方法一样。
* 返回类型必须被重写方法一样，或者是其子类。
* 重写的方法不能抛出异常，或者不能抛出比被重写方法还要的大的异常。

### 7.警惕自增的陷阱

```java
public static void main(String[] args) {
    int count = 0;
    for (int i = 0; i < 100; i++) {
        int i1 = count++;
        count = i1;
        System.out.println("每次count++的值：" + i1);
    }
    System.out.println(count);
}
```

**说明：**结果是`0`，而不是我们`100`，这是`count++`是一个表达式，返回的是自加之前`count`的值。

### 8.少用静态导入

```java
import static java.lang.Math.PI;

public double calCircleArea(double r) {
    return Math.PI * r * r;
}

public double calBallArea (double r) {
    return 4 * PI * r * r;
}
```

**说明：**静态导入可以减少代码的量，但不易于阅读，可读性差。

### 9.break不可忘记

```java
public static void main(String[] args) {
    String s = toChineseNumber(2);
    log.info("转换结果：{}", s);
}

private static String toChineseNumber(int n) {
    String chineseNumber = "";
    switch (n) {
        case 0 : chineseNumber = "零";
        case 1 : chineseNumber = "壹";
        case 2 : chineseNumber = "贰";
        case 3 : chineseNumber = "叁";
        case 4 : chineseNumber = "肆";
        case 5 : chineseNumber = "伍";
        case 6 : chineseNumber = "陆";
        case 7 : chineseNumber = "柒";
        case 8 : chineseNumber = "捌";
        case 9 : chineseNumber = "玖";
        case 10 : chineseNumber = "拾";

    }
    return chineseNumber;
}
```

**说明：**在`switch`中`break`一定不能少。

### 10.避免instanceof和预想的结果不一致

```java
public static void main(String[] args) {
    // String对象是否是Object的实例 - true,"String"是要给字符串，字符串继承了Object，当然是Object的实例。
    boolean b1 = "String" instanceof Object;
    // String对象是否是String类的实例 - true，一个类的对象当然是一个类的实例。
    boolean b2 = new String() instanceof String;
    // Object对象是否是String的实例，编译报错，Object是父类。
    boolean b3 = new Object() instanceof String;
    // 拆箱类型是否是装箱类型的实例，编译报错，“A”是一个Char型，是一个基本类型，instanceof只能用于对象判断。
    boolean b4 = "A" instanceof Character;
    // 空对象是否是String的实例 - false，instanceof的规则，左边为null，无论右边什么类型，都返回false。
    boolean b5 = null instanceof String;
    // 类型转换后的空对象是否是String的实例 - false，null可以说是没有类型，类型转换后还是null。
    boolean b6 = (String) null instanceof String;
    // Date对象是否是String的实例，编译报错，Date类和String类没有继承关系
    boolean b7 = new Date() instanceof String;
}
```

### 11.使用偶判断，不使用奇判断

```java
String s = n % 2 == 1 ? "奇数" : "偶数";
String s1 = n % 2 == 0 ? "偶数" : "奇数";
```

**说明：**通常使用第二种偶数判断，使用第一种的话。`-1`也会被判断为偶数。

### 12.用整数型处理货币

```java
// 0.40000000000000036
System.out.println(10.00 - 9.60);
```

**说明：**Java中的浮点数是不准确的，在处理货币上使用浮点数就会存在问题，因此使用`BigDecimal`，类来进行计算，或者使用整数，将需要计算的数放大100倍，计算后在缩小。

### 13.不要让类型悄悄转换

```java
/** 光速*/
private static final int LIGHT_SPEED = 30 * 10000 * 1000;

public static void main(String[] args) {
    long dis = LIGHT_SPEED * 60 * 8;
    // -2028888064
    System.out.println(dis);
}
```

**说明：**`LIGHT_SPEED * 60 * 8`计算后是`int`类型，可能存在越界问题，虽然，我们在代码中写了转换为`Long`型，但是，在Java中是先运算后在进行类型转换的，也就是` LIGHT_SPEED * 60 * 8`计算后是`int`型，超出了长度，从头开始，所以为负值，修改为显示的定义类型。如下：

```java
/** 光速*/
private static final long LIGHT_SPEED = 30L * 10000 * 1000;

public static void main(String[] args) {
    long dis = LIGHT_SPEED * 60 * 8;
    System.out.println(dis);
}
```

### 14.不要在接口中存在实现代码

**说明：**虽然接口中可以存在实现代码，但是不遵循规范，接口是抽象的方法。

### 15.不要复写父类的静态方法

```java
public class ClientInterface {
    public static void main(String[] args) {
        Base1 base1 = new Sub1();
        // 我是子类非静态方法
        base1.doAnyThing();
        // 我是父类静态方法
        base1.doSomeThing();
    }

}

class Base1 {
    public static void doSomeThing() {
        System.out.println("我是父类静态方法");
    }

    public void doAnyThing() {
        System.out.println("我是父类非静态方法");
    }
}

class Sub1 extends Base1 {

    public static void doSomeThing() {
        System.out.println("我是子类静态方法");
    }

    @Override
    public void doAnyThing() {
        System.out.println("我是子类非静态方法");
    }
}
```

### 16.使用String直接赋值

```java
public static void main(String[] args) {
    String str1 = "China";
    String str2 = "China";
    String str3 = new String("China");
    String str4 = str3.intern();

    System.out.println(str1 == str2); // true

    System.out.println(str1 == str3); // false

    System.out.println(str1 == str4); // true
}
```

**说明：**建议使用`String str1 = "China";`这中方式对字符串赋值，而不是通过`new String("China");`这种方式，在Java中会给定义的常量存放在一个常量池中，如果池中存在就不会在重复定义，所以`str1 == str2`返回`true`。`new`出的是一个对象，不会去检查字符串常量池是否存在，所以`str1 == str3`是不同的引用，返回`false`。经过`intern()`处理后，返回`true`，是因为`intern()`会去对象常量池中检查是否存在字面上相同得引用对象。

### 17.性能考虑，首选数组

```java
private static int getSumForArray(int[] array) {
    int sum = 0;
    for (int i = 0; i < array.length; i++) {
        sum += array[i];
    }
    return sum;
}

private static int getSumForList(List<Integer> list) {
    return list.stream().mapToInt(Integer::intValue).sum();
}
```

### 18.asList产生的list不可修改

```java
private static void arraysTest() {
    String[] week = {"Mon", "Tue", "Wed", "Thu"};
    List<String> strings = Arrays.asList(week);
    strings.add("Fri");
}
```

**说明：**运行报错，asList产生的list不可修改