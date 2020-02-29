# Java 期末考试
## 基本数据类型
```
byte = 0; // 占1个字节
char = '/u0000'; // 占2个字节
short = 0; // 占2个字节
int = 0; // 占4个字节
float = 0; // 占4个字节
double = 0.0f;// 占8个字节
long = 0; // 占8个字节
boolean = false; //  视编译环境而定

```
## File 类的使用，概念 ，内容

```
File file = new File("C:\\verstion.txt);
try{
    System.out.printIn(file.createNewFile())
}catch(IOException e){
    e.printStackTrace();
}
	System.out.println("单级文件夹创建成功了吗？"+file.mkdir());
	System.out.println("多级文件夹创建成功了吗？"+file.mkdirs());
	File dest = new File("F:\\c.txt");
	System.out.println("重命名成功了吗？"+file.renameTo(dest));

// 删除文件
File file = new File("F:\\delete.txt")
        System.out.println("删除成功了吗？"+file.delete());
        file.deleteOnExit();

//判断方法
File file = new File("F:\\a.txt");
        System.out.println("文件或者文件夹存在吗？"+file.exists());
        System.out.println("是一个文件吗？"+file.isFile());
        System.out.println("是一个文件夹吗？"+file.isDirectory());
        System.out.println("是隐藏文件吗？"+file.isHidden());
        System.out.println("此路径是绝对路径名？"+file.isAbsolute());

```
## 静态方法
非静态方法既可以访问静态数据成员 又可以访问非静态数据成员，而静态方法只能访问静态数据成员；

## 说明 Set 接口 和 Map 接口的主要区别。
1. Set 无序 继承 与 Collection 接口 ，不允许重复值，适合用于去重
   1. HashSet：为快速查找设计的Set。存入HashSet的对象必须定义hashCode()。 
   2. TreeSet：保存次序的Set, 底层为树结构。使用它可以从Set中提取有序的序列。 
   3. LinkedHashSet：具有HashSet的查询速度，且内部使用链表维护元素的顺序(插入的次序)。于是在使用迭代器遍历Set时，结果会按元素插入的次序显示。

2. Map 存储键值对映射


非静态方法既可以访问静态方法又可以访问非静态方法，而静态方法只能访问静态数据方法。

**原因**：因为静态方法和静态数据成员会随着类的定义而被分配和装载入内存中，而非静态方法和非静态数据成员只有在类的对象创建时在对象的内存中才有这个方法的代码段。

## 关键字 this,super,final,static

## 访问控制符 public， protected ，default（默认修饰符） private。

# 编程题
## 1.java网络编程代码
1. 服务器端。


2. 客户端

## 找素数
请编写程序，从键盘输入两个整数m，n，找出等于或大于m的前n个素数。

### 输入格式:
第一个整数为m，第二个整数为n；中间使用空格隔开。例如：  

第一个整数为m，第二个整数为n；中间使用空格隔开。例如：  

```
103 3
```
输出格式:

从小到大输出找到的等于或大于m的n个素数，每个一行。例如：

```
103

107

109
```

输入样例:
` 9223372036854775839 2`
输出样例:
```
9223372036854775907
9223372036854775931
```
## 大数整除
请编写程序，从键盘输入一个整数n，找出大于long.MAX_VALUE且能被n整除的前3个数字。
```
  public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int i = scanner.nextInt();


        BigInteger bigInteger = new BigInteger(String.valueOf(Long.MAX_VALUE));

        int count = 0;
        while (count < 3) {
            if (bigInteger.mod(BigInteger.valueOf(i)).intValue() == 0) {
                count++;
                System.out.println(bigInteger);
            }
            bigInteger = bigInteger.add(BigInteger.ONE);
        }

    }

```
## 两个巨大素数（质数）的乘积
得到两个巨大素数（质数）的乘积是简单的事，但想从该乘积分解出这两个巨大素数却是国际数学界公认的质因数分解难题。这种单向的数学关系，是不对称加密RSA算法的基本原理。 本题给出两个大素数（128bit位）的乘积和其中一个素数，请你编程求出另一个素数。
```
   public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        BigInteger a = scanner.nextBigInteger();
        BigInteger b = scanner.nextBigInteger();
        System.out.println(a.divide(b));
        
    }
```
> 有关数学的题目多注意自带函数。

## 求阶乘(大整数)

## 求解给定字符串的前缀
求解给定字符串的前缀。
```
public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        while (in.hasNextInt()) {
            BigInteger t = new BigInteger("1");
            int n = in.nextInt();
            for (int i = 1; i <= n; i++) {
                BigInteger c = new BigInteger(String.valueOf(i));
                t = t.multiply(c);
            }
            System.out.println(t);
        }
        in.close();
    }
```