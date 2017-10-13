---
title: Java语言算法设计基础
---

[TOC]

# 什么是程序？
有一个著名的公式说:

>==__程序=数据结构+算法__==

## 什么是数据结构？

- 简单的数据结构就是众所周知的整型、实型这样的数据表示，
- 复杂的数据结构可以是由简单的数据结构组合而成，如数组。

## 什么是算法？

算法就是用什么样的==方法==处理给定的数据从而得到所需的结果。

__例如__：

圆面积的计算公式为：
$$area = \pi \times r^2$$

其计算过程如下所示：

~~~java
// 程序:一个计算圆面积的程序
public class ComputeArea {
	public static void main(String[] args) {
		int r = 10;

		double area=3.14*r*r;

		System.out.println(area);
	}
}
~~~

---

# 算法设计基础


## 求解素数 

### 什么是素数

- 素数是这样的整数，它除了能表示为它自己和1的乘积以外，不能表示为任何其它两个整数的乘积。
- 例如：$2$，$3$，$5$，$7$，$11$等。

### 求解素数算法 
验证一个正整数$n$($n>3$)是否为素数，一个最直观的方法是看在$2$～$n/2$中能否找到一个整数$m$能将$n$整除。

- 若$m$存在，则$n$不是素数；
- 若找不到$m$，则$n$为素数。

这是一个循环验证算法，用下列表达式控制：

>初值：m=2；<br>
>循环条件：m<=n/2；<br>
>修正：m++；

注：它的作用是穷举$2$～$n/2$中的各$m$值。
循环体是判断$n$是否可以被$m$整除。

程序：求解100以内的全部素数

~~~java
public class PrimeApp {
	public static void main(String[] args) {
		int m, n;// 变量n为要判断的数字
		System.out.println("100以内的素数有：");
		A: for (n = 2; n <= 100; n++) {
			for (m = 2; m < n / 2; m++) {
				if (n % m == 0)
					continue A;
             // 如果能被整除则变量n肯定不是素数，跳出内层循环
			}

			System.out.print(n + " " + "\t");//输出素数
		}
	}
}
~~~


## 查找

### 什么是查找

查找指在一个数据元素集合中查找关键字或等于某个给定关键字数据元素的过程，是对数据进行操作或处理时经常使用的操作。

在计算机应用系统中使用查找的地方有很多，比如高考学生成绩管理系统中的高考学生成绩表中查找考生号码等于某个号码的考生成绩，或是从一批图书中找出某一本图书等。

### 顺序查找

查找最简单的方法是“顺序查找法”，即一个一个数据查找，看是否是所需的数据。

例如:查找从命令行输入的参数是否在数组中。

~~~java
public class SequenceSearchApp {
	public static void main(String[] args) {
		String[] arr = { "张三", "李四", "王五", "赵六"};
		String temp = args[0];
		
		for (int i = 0; i < arr.length; i++) {
			if (temp.equals(arr[i])) {
		       System.out.println(temp + "在arr集合中");
		       return;
		    }
		}

		System.out.println(temp + "不在arr集合中");
	}
}
~~~

顺序查找效率分析

顺利查找方法实现起来很简单，但是效率低，例如要从$1000$个数据中要查找某一个所需的数据，而该数据恰恰是最后一个，则需要取数据和比较数据$1000$次，对$n$个数据的取数和比较的平均次数为$n/2$次。

### 折半查找

- 折半查找法是一种效率较高的查找方法；
- 用折半查找法的前提是：==数据已按一定的规则（升序或降序）排列好。==
- 其基本思路是：先检索居中的一个数据，看它是否为所需的数据，如果不是，则判断要找的数据是在居中数的哪一边，下次就在这个范围内查找。


假如有一组数为：

$8$，$13$，$21$，$28$，$35$，$41$，$52$，$63$，$71$，$76$，$81$，$95$，$101$，$150$，$164$

要找$21$这个数。
为了直观，把这$15$个数画成如图所示的倒立的树形。

![](fig/SearchExample.png)

“树”的根在上面，“树枝”和“树叶”在下面，每一个数据称了一个“结点”。树根结点是15个数中居中的数（$63$），它有两个“子结点”，左边的比它小，右边的比它大，以下逐级都是按这个规律。

可按以下步骤查找：

- 先将要找的数$21$与数列中“树根结点”比较，由于$21<63$，因此，$21$显然在$63$左面的范围内，而且是第$1$个数到第$7$个数之间的一个数（由于$21<63$，所以已知道不可能是第$8$个数）。
- 再找$1$～$7$个数中居中位置的一个数（第$4$个数），即$28$，将$21$与$28$相比，由于$21<28$，因此，所找的数必然在第$1$～$3$个数范围内。
- 再找第$1$～$3$个数的居中位置的一个数$13$（即第$2$个数）,将$21$与$13$相比，由于$21>13$，因此，$21$必然在13的右面。
- 由于$21$下面不再分支，因此下一次就直接找到$21$。
- 以上一共查找了$4$次（即树的深度或树的层次），就找到所需的数。

~~~java
public class BinaraySearchApp {
	public static void main(String[] args) {
		int mid, low, high;
		int[] arr = { 8, 13, 21, 28, 35, 41, 52, 63, 71, 76, 81, 95, 101, 150,164 };
		
		low = 0; // 下界
		high = arr.length - 1; // 上界
		int x = Integer.parseInt(args[0]); // x为要查找的元素

		do {
			mid = (low + high) / 2;
			
			if (x == arr[mid]) {		  	
		  		System.out.println(x + "查找成功，在" + mid + "号位置");
				return;
			} 
			else if (x < arr[mid]) // 如果x小于中间值则在左半段查找
				high = mid - 1;
			else if (x > arr[mid]) // 如果x大于中间值则在右半段查找
				low = mid + 1;
		} while (low <= high);
		
		System.out.println(x + "查找失败");
	}
} 
~~~

如果有n个数，采用折半查找，在其中找任何一个数所需查找次数不超过 $[\log_2 n] + 1$

## 排序

### 什么是排序？

- 所谓排序是指将一组无序的数据元素调整为一个从小到大或者从大到小排列的有序序列。
- 排序的目的是为了便于查找或为了适应某些查找算法的需要。

排序的算法很多，如果依据各种排序算法的基本处理思想及基本的执行过程来进行分类，大致可分为插入排序法、交换排序法、选择排序法、归并排序法及基数排序法。 


### 插入排序法

直接插入排序法的排序原则是：<br>
将一组无序的数字排列成一排，左端第一个数字为已经完成排序的数字，其他数字为未排序的数字。<br>
然后从左到右依次将未排序的数字插入到已排序的数字中。

直接插入排序具体算法描述如下：

- 1）从第一个元素开始，该元素可以认为已经被排序； 
- 2）取出下一个元素，在已经排序的元素序列中从后向前扫描 ；
- 3）如果该元素（已排序）大于新元素，将该元素移到下一位置；
- 4）重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
- 5）将新元素插入到该位置中；
- 6）重复步骤2；


直接插入排序示例

$49$  $38$  $65$  $97$  $76$  $13$  $27$  $49$

| | |
|:---|:---|
|1|$49$|
|2|$38$ $49$|
|3|$38$ $49$ $65$|
|4|$38$ $49$ $65$ $97$|
|5|$38$ $49$ $65$ $76$ $97$|
|6|$13$ $38$ $49$ $65$ $76$ $97$|
|7|$13$ $27$ $38$ $49$ $65$ $76$ $97$|
|8|$13$ $27$ $38$ $49$ $49$ $65$ $76$ $97$|


程序: 插入排序 

~~~java
public class InsertSortApp {
	public static void main(String[] args) {
		int[] arr = { 49, 38, 65, 97, 76, 13, 27 };
		int i, j, temp;
		int len = arr.length;
		
		for (i = 1; i < len; i++) // 进行len-1趟插入
		{        
			// temp记录本趟要进行插入的元素值
			temp = arr[i];
			j = i - 1;
	       
	       //与已排序的数逐一比较，大于temp时，该数移后
			while ((j >= 0) && (arr[j] > temp)) {
				arr[j + 1] = arr[j];
				j--;
			}
			
			arr[j+1] = temp;	
		}
		
		System.out.println("插入排序的结果是");
		for(i=0;i<len;i++) {
			System.out.print(arr[i]+"  ");
		}
	}
} 

~~~


### 冒泡排序法

冒泡排序是交换排序中一种简单的排序方法。<br>
它的基本处理思想是通过对相邻两个数据的比较及其交换来达到排序的目的。


首先将第一个数据与第二个数据进行比较，如果是逆序（即arr[j]>arr[j+1）则交换之，然后比较第二个数据和第三个数据，依此类推，直到第n-1个数据和第n个数据进行过比较为止。<br>
上述过程称为冒泡排序的第一趟处理，第一趟处理的结果使得最大的数据被安置在最后一个元素的位置上。<br>
然后进行第二趟处理，即对前n-1个数据进行上述的处理，第二趟处理的结果是使次大的数据被安置在倒数第二个数据位置上。

一般地，冒泡排序的第i趟是从arr[0]到arr[n-i]依次比较相邻两个元素的关键字，并在逆序时交换相邻的数据，其结果是将这n-i+1个数据中最大的交换到n-i的位置上。<br>
整个排序过程需要进行n-1趟处理，第n-1趟处理使倒数第二的数据安置在第二个数据位置上，这时整个排序过程就已完成。

示例

![](fig/sortsample.png)

程序: 冒泡排序 

~~~java
public class BubbleSortApp {
  public static void main(String[] args) {
	int[] arr = { 51, 38, 49, 27, 62, 5, 16 };
	int i, j, temp, len;
	len = arr.length;
	for (i = len - 1; i > 0; i--) {
		for (j = 0; j < i; j++) {
			if (arr[j] > arr[j + 1]) {
				// 进行数据交换
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
	}

	System.out.println("冒泡排序的结果是");
	for (i = 0; i < len; i++) {
		System.out.print(arr[i] + "  ");
	}
}
} 

~~~


### 选择排序法

选择排序的基本思想是每一趟（假设是第$i$趟）处理都是从$n-i+1$个数据中选择一个数据最小（或最大）的作为有序序列中的第$i$个数据。

其中最简单的一种称为直接选择排序。

直接选择排序的第一趟处理是从数据序列所有$n$个数据中选择一个最小的数据作为有序序列中的第$1$个元素并将它定位在第一号存储位置，第二趟处理从数据序列的$n-1$个数据中选择一个第二小的元素作为有序序列中的第$2$个元素并将它定位在第二号存储位置，依此类推，当第$n-1$趟处理从数据序列的剩下的$2$个元素中选择一个较小的元素作为有序序列中的最后第$2$个元素并将它定位在倒数第二号存储位置，至此，整个的排序处理过程就已完成。

![](fig/sortsample2.png)

~~~java
public class SelectSortApp {
	public static void main(String[] args) {
		int[] arr = { 51, 38, 49, 27, 62, 5, 16 };
		int i,j,min,temp,len;
		
		len=arr.length;
		
		for(i=0;i<len-1;i++) { //进行len-1趟选择
			min=i; 
			
			for(j=i+1;j<len;j++) {  
				//选择从i到n-1号位置中最小的值的下标
				
				if(arr[min]>arr[j])	{
					min=j;
				}
			} //如果最小值不在i号位置，则交换min与i号位置的值
			
			if(min!=i) {			
				temp=arr[i];
				arr[i]=arr[min];
				arr[min]=temp;
			}  
		}      
		
		System.out.println("选择排序的结果是");
		
		for(i=0;i<len;i++){
			System.out.print(arr[i]+"  ");
		}
	}
}
~~~




## 递归

### 什么是递归

在数学及程序设计方法学中为递归下的定义是：
==若一个对象部分地包含它自己或用它自己给自己定义，则称这个对象是递归的。==<br>
对于一个方法来说，==若直接或间接地调用它自己，则该方法为递归方法==。

递归是一种算法设计方法，也是计算机程序设计问题中可利用的最有效的解题方法之一。<br>
在程序设计中，递归的解决方案的使用相当普遍，递归的解决方法比非递归的解决方法更精妙、更简洁。
有些问题看起来很复杂，但是使用递归的方法来解决却非常简单。 

求解费波纳契数列—递推法

其基本思想是不断地由已知值推出新值，直至求得解。

~~~java
int fib(int n) {
	int i, x, y, z;
	
	x = 1;	
	y = 1;	
	i = 3;
	
	do {
		z = x + y;
		x = y;
		y = z;
		i++;
	} while (i <= n);
	
	return z;
}
~~~

其基本思想是不断把问题分解成规模较小的同类问题，直至问题足够小能直接求得解为止。 

~~~java
int fib(int n) {
		if (n == 1 || n == 2)
			return 1;
		else
			return fib(n - 1) + fib(n - 2);
	}
~~~

从上述程序设计实例可以看出，递归算法适应于这样一类问题：在对求解的过程中得到的是和原问题性质相同的子问题，这一类问题自然可以用一个递归方法进行描述。

设计递归算法时，通常可以先写出问题求解的递归定义。递归定义由基本项和归纳项两部分组成。基本项描述递归过程的终结状态。所谓终结状态，是指不需要继续递归而可直接求解的状态。归纳项描述了如何实现从当前状态到终结状态的转化。

递归过程的特点是结构清晰、程序简练易读、正确性容易证明，因此是程序设计的有力工具。



# Java 数据结构

Java工具包提供了强大的数据结构。在Java中的数据结构主要包括以下几种接口和类：

- 枚举（Enumeration）
- 位集合（BitSet）
- 向量（Vector）
- 栈（Stack）
- 字典（Dictionary）
- 哈希表（Hashtable）
- 属性（Properties）

<!---
以上这些类是传统遗留的，在Java2中引入了一种新的框架-集合框架(Collection)，我们后面再讨论。
--->


## 枚举(Enumeration)
枚举（Enumeration）接口虽然它本身不属于数据结构,但它在其他数据结构的范畴里应用很广。 枚举（The Enumeration）接口定义了一种从数据结构中取回连续元素的方式。
例如，枚举定义了一个叫nextElement 的方法，该方法用来得到一个包含多元素的数据结构的下一个元素。

Enumeration接口中定义了一些方法，通过这些方法可以枚举（一次获得一个）对象集合中的元素。
这种传统接口已被迭代器取代，虽然Enumeration 还未被遗弃，但在现代代码中已经被很少使用了。尽管如此，它还是使用在诸如Vector和Properties这些传统类所定义的方法中，除此之外，还用在一些API类，并且在应用程序中也广泛被使用。 

下表总结了一些Enumeration声明的方法：

|序号|	方法|描述|
|---|:---|:---|
|1|	boolean hasMoreElements( )| 测试此枚举是否包含更多的元素。
|2|	Object nextElement( )|如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。|

**实例**

~~~java
import java.util.Vector;
import java.util.Enumeration;
 
public class EnumerationTester {
 
   public static void main(String args[]) {
      Enumeration<String> days;
      Vector<String> dayNames = new Vector<String>();
      dayNames.add("Sunday");
      dayNames.add("Monday");
      dayNames.add("Tuesday");
      dayNames.add("Wednesday");
      dayNames.add("Thursday");
      dayNames.add("Friday");
      dayNames.add("Saturday");
      days = dayNames.elements();
      while (days.hasMoreElements()){
         System.out.println(days.nextElement()); 
      }
   }
}
~~~

以上实例编译运行结果如下：
>Sunday
>Monday
>Tuesday
>Wednesday
>Thursday
>Friday
>Saturday

## 位集合（BitSet）
位集合类实现了一组可以单独设置和清除的位或标志。
该类在处理一组布尔值的时候非常有用，你只需要给每个值赋值一"位"，然后对位进行适当的设置或清除，就可以对布尔值进行操作了。

### Java Bitset类
- 一个Bitset类创建一种特殊类型的数组来保存位值。
- BitSet中数组大小会随需要增加。
- 和位向量（vector of bits）比较类似。

### BitSet定义了两个构造方法。

- 第一个构造方法创建一个默认的对象：

	BitSet()

- 第二个方法允许用户指定初始大小。所有位初始化为0。

	BitSet(int size)

### BitSet中实现了Cloneable接口中定义的方法如下表所列：

|序号|方法|描述|
|---|:---|:---|
|1|void and(BitSet set)|对此目标位 set 和参数位 set 执行逻辑与操作。|
|2|void andNot(BitSet set)|清除此 BitSet 中所有的位，其相应的位在指定的 BitSet 中已设置。|
|3|	int cardinality( )|返回此 BitSet 中设置为 true 的位数。|
|4|	void clear( )|将此 BitSet 中的所有位设置为 false。|
|5|	void clear(int index)|将索引指定处的位设置为 false。|
|6|	void clear(int startIndex, int endIndex)|将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为 false。|
|7|	Object clone( )|复制此 BitSet，生成一个与之相等的新 BitSet。|
|8|	boolean equals(Object bitSet)|将此对象与指定的对象进行比较。|
|9|	void flip(int index)|将指定索引处的位设置为其当前值的补码。|
|10|void flip(int startIndex, int endIndex)|将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的每个位设置为其当前值的补码。|
|11|boolean get(int index)|返回指定索引处的位值。|
|12	|BitSet get(int startIndex, int endIndex)|返回一个新的 BitSet，它由此 BitSet 中从 fromIndex（包括）到 toIndex（不包括）范围内的位组成。|
|13|int hashCode( )|返回此位 set 的哈希码值。|
|14|boolean intersects(BitSet bitSet)|如果指定的 BitSet 中有设置为 true 的位，并且在此 BitSet 中也将其设置为 true，则返回 ture。|
|15|boolean isEmpty( )|如果此 BitSet 中没有包含任何设置为 true 的位，则返回 ture。|
|16|int length( )|返回此 BitSet 的"逻辑大小"：BitSet 中最高设置位的索引加 1。|
|17|int nextClearBit(int startIndex)|返回第一个设置为 false 的位的索引，这发生在指定的起始索引或之后的索引上。|
|18|int nextSetBit(int startIndex)|返回第一个设置为 true 的位的索引，这发生在指定的起始索引或之后的索引上。|
|19|void or(BitSet bitSet)|对此位 set 和位 set 参数执行逻辑或操作。|
|20|void set(int index)|将指定索引处的位设置为 true。|
|21|void set(int index, boolean v)| 将指定索引处的位设置为指定的值。|
|22|void set(int startIndex, int endIndex)|将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为 true。|
|23|void set(int startIndex, int endIndex, boolean v)|将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为指定的值。|
|24|int size( )|返回此 BitSet 表示位值时实际使用空间的位数。|
|25|String toString( )|返回此位 set 的字符串表示形式。|
|26|void xor(BitSet bitSet)|对此位 set 和位 set 参数执行逻辑异或操作。|


**实例**

~~~java
import java.util.BitSet;
 
public class BitSetDemo {
 
  public static void main(String args[]) {
     BitSet bits1 = new BitSet(16);
     BitSet bits2 = new BitSet(16);
      
     // set some bits
     for(int i=0; i<16; i++) {
        if((i%2) == 0) bits1.set(i);
        if((i%5) != 0) bits2.set(i);
     }
     System.out.println("Initial pattern in bits1: ");
     System.out.println(bits1);
     System.out.println("\nInitial pattern in bits2: ");
     System.out.println(bits2);
 
     // AND bits
     bits2.and(bits1);
     System.out.println("\nbits2 AND bits1: ");
     System.out.println(bits2);
 
     // OR bits
     bits2.or(bits1);
     System.out.println("\nbits2 OR bits1: ");
     System.out.println(bits2);
 
     // XOR bits
     bits2.xor(bits1);
     System.out.println("\nbits2 XOR bits1: ");
     System.out.println(bits2);
  }
}
~~~

以上实例编译运行结果如下：

>Initial pattern in bits1:
>{0, 2, 4, 6, 8, 10, 12, 14}

>Initial pattern in bits2:
>{1, 2, 3, 4, 6, 7, 8, 9, 11, 12, 13, 14}

>bits2 AND bits1:
>{2, 4, 6, 8, 12, 14}

>bits2 OR bits1:
>{0, 2, 4, 6, 8, 10, 12, 14}

>bits2 XOR bits1:
>{}



## 向量（Vector）
向量（Vector）类和传统数组非常相似，但是Vector的大小能根据需要动态的变化。
和数组一样，Vector对象的元素也能通过索引访问。
使用Vector类最主要的好处就是在创建对象的时候不必给对象指定大小，它的大小会根据需要动态的变化。

### Java Vector 类
- Vector类实现了一个动态数组。和ArrayList和相似，但是两者是不同的：
- Vector是同步访问的。
- Vector包含了许多传统的方法，这些方法不属于集合框架。
- Vector主要用在事先不知道数组的大小，或者只是需要一个可以改变大小的数组的情况。

### Vector类支持4种构造方法。
- 第一种构造方法创建一个默认的向量，默认大小为10：
	Vector()

- 第二种构造方法创建指定大小的向量。
	Vector(int size)

- 第三种构造方法创建指定大小的向量，并且增量用incr指定. 增量表示向量每次增加的元素数目。
	Vector(int size,int incr)

- 第四种构造方法创建一个包含集合c元素的向量：
	Vector(Collection c)

除了从父类继承的方法外Vector还定义了以下方法：

|序号|方法|描述|
|:---|:---|:---|
|1|void add(int index, Object element)|在此向量的指定位置插入指定的元素。|
|2|boolean add(Object o)|将指定元素添加到此向量的末尾。|
|3|boolean addAll(Collection c)|将指定 Collection 中的所有元素添加到此向量的末尾，按照指定 collection 的迭代器所返回的顺序添加这些元素。|
|4|boolean addAll(int index, Collection c)|在指定位置将指定 Collection 中的所有元素插入到此向量中。|
|5|void addElement(Object obj)|将指定的组件添加到此向量的末尾，将其大小增加 1。|
|6|int capacity()|返回此向量的当前容量。|
|7|void clear()|从此向量中移除所有元素。|
|8|Object clone()|返回向量的一个副本。|
|9|boolean contains(Object elem)|如果此向量包含指定的元素，则返回 true。|
|10|boolean containsAll(Collection c)|如果此向量包含指定 Collection 中的所有元素，则返回 true。|
|11|void copyInto(Object[] anArray)|将此向量的组件复制到指定的数组中。|
|12|Object elementAt(int index)|返回指定索引处的组件。|
|13|Enumeration elements()|返回此向量的组件的枚举。|
|14|void ensureCapacity(int minCapacity)|增加此向量的容量（如有必要），以确保其至少能够保存最小容量参数指定的组件数。|
|15|boolean equals(Object o)|比较指定对象与此向量的相等性。|
|16|Object firstElement()|返回此向量的第一个组件（位于索引 0) 处的项）。|
|17|Object get(int index)|返回向量中指定位置的元素。|
|18|int hashCode()|返回此向量的哈希码值。|
|19|int indexOf(Object elem)|返回此向量中第一次出现的指定元素的索引，如果此向量不包含该元素，则返回 -1。|
|20|int indexOf(Object elem, int index)|返回此向量中第一次出现的指定元素的索引，从 index 处正向搜索，如果未找到该元素，则返回 -1。|
|21|void insertElementAt(Object obj, int index)|将指定对象作为此向量中的组件插入到指定的 index 处。|
|22|boolean isEmpty()|测试此向量是否不包含组件。|
|23|Object lastElement()|返回此向量的最后一个组件。|
|24|int lastIndexOf(Object elem)|返回此向量中最后一次出现的指定元素的索引；如果此向量不包含该元素，则返回 -1。|
|25|int lastIndexOf(Object elem, int index)|返回此向量中最后一次出现的指定元素的索引，从 index 处逆向搜索，如果未找到该元素，则返回 -1。|
|26|Object remove(int index) |移除此向量中指定位置的元素。|
|27|boolean remove(Object o) |移除此向量中指定元素的第一个匹配项，如果向量不包含该元素，则元素保持不变。|
|28|boolean removeAll(Collection c) |从此向量中移除包含在指定 Collection 中的所有元素。|
|29|void removeAllElements()|从此向量中移除全部组件，并将其大小设置为零。|
|30|boolean removeElement(Object obj)|从此向量中移除变量的第一个（索引最小的）匹配项。|
|31|void removeElementAt(int index)|删除指定索引处的组件。|
|32|protected void removeRange(int fromIndex, int toIndex)|从此 List 中移除其索引位于 fromIndex（包括）与 toIndex（不包括）之间的所有元素。|
|33|boolean retainAll(Collection c)|在此向量中仅保留包含在指定 Collection 中的元素。|
|34|Object set(int index, Object element)|用指定的元素替换此向量中指定位置处的元素。|
|35|void setElementAt(Object obj, int index)|将此向量指定 index 处的组件设置为指定的对象。|
|36|void setSize(int newSize) |设置此向量的大小。|
|37|int size() |返回此向量中的组件数。|
|38|List subList(int fromIndex, int toIndex)|返回此 List 的部分视图，元素范围为从 fromIndex（包括）到 toIndex（不包括）。|
|39|Object[] toArray()|返回一个数组，包含此向量中以恰当顺序存放的所有元素。|
|40|Object[] toArray(Object[] a)|返回一个数组，包含此向量中以恰当顺序存放的所有元素；返回数组的运行时类型为指定数组的类型。|
|41|String toString()|返回此向量的字符串表示形式，其中包含每个元素的 String 表示形式。|
|42|void trimToSize()|对此向量的容量进行微调，使其等于向量的当前大小。|

**实例**

~~~java
import java.util.*;
public class VectorDemo {

   public static void main(String args[]) {
      // initial size is 3, increment is 2
      Vector v = new Vector(3, 2);
      System.out.println("Initial size: " + v.size());
      System.out.println("Initial capacity: " +
      v.capacity());
      v.addElement(new Integer(1));
      v.addElement(new Integer(2));
      v.addElement(new Integer(3));
      v.addElement(new Integer(4));
      System.out.println("Capacity after four additions: " +
          v.capacity());

      v.addElement(new Double(5.45));
      System.out.println("Current capacity: " +
      v.capacity());
      v.addElement(new Double(6.08));
      v.addElement(new Integer(7));
      System.out.println("Current capacity: " +
      v.capacity());
      v.addElement(new Float(9.4));
      v.addElement(new Integer(10));
      System.out.println("Current capacity: " +
      v.capacity());
      v.addElement(new Integer(11));
      v.addElement(new Integer(12));
      System.out.println("First element: " +
         (Integer)v.firstElement());
      System.out.println("Last element: " +
         (Integer)v.lastElement());
      if(v.contains(new Integer(3)))
         System.out.println("Vector contains 3.");
      // enumerate the elements in the vector.
      Enumeration vEnum = v.elements();
      System.out.println("\nElements in vector:");
      while(vEnum.hasMoreElements())
         System.out.print(vEnum.nextElement() + " ");
      System.out.println();
   }
}
~~~

以上实例编译运行结果如下：

>Initial size: 0
>Initial capacity: 3
>Capacity after four additions: 5
>Current capacity: 5
>Current capacity: 7
>Current capacity: 9
>First element: 1
>Last element: 12
>Vector contains 3.
>Elements in vector:
>1 2 3 4 5.45 6.08 7 9.4 10 11 12



##栈（Stack）

栈（Stack）实现了一个后进先出（LIFO）的数据结构。
你可以把栈理解为对象的垂直分布的栈，当你添加一个新元素时，就将新元素放在其他元素的顶部。
当你从栈中取元素的时候，就从栈顶取一个元素。换句话说，最后进栈的元素最先被取出。

栈是Vector的一个子类，它实现了一个标准的后进先出的栈。
堆栈只定义了默认构造函数，用来创建一个空栈。 堆栈除了包括由Vector定义的所有方法，也定义了自己的一些方法。
Stack()
除了由Vector定义的所有方法，自己也定义了一些方法：

|序号|方法|描述|
|:---|:---|:---|
|1|	boolean empty()| 测试堆栈是否为空。|
|2|	Object peek()|查看堆栈顶部的对象，但不从堆栈中移除它。|
|3|	Object pop()|移除堆栈顶部的对象，并作为此函数的值返回该对象。|
|4|	Object push(Object element)|把项压入堆栈顶部。|
|5|	int search(Object element)|返回对象在堆栈中的位置，以 1 为基数。|


**实例**

~~~java
import java.util.*;
 
public class StackDemo {
 
    static void showpush(Stack<Integer> st, int a) {
        st.push(new Integer(a));
        System.out.println("push(" + a + ")");
        System.out.println("stack: " + st);
    }
 
    static void showpop(Stack<Integer> st) {
        System.out.print("pop -> ");
        Integer a = (Integer) st.pop();
        System.out.println(a);
        System.out.println("stack: " + st);
    }
 
    public static void main(String args[]) {
        Stack<Integer> st = new Stack<Integer>();
        System.out.println("stack: " + st);
        showpush(st, 42);
        showpush(st, 66);
        showpush(st, 99);
        showpop(st);
        showpop(st);
        showpop(st);
        try {
            showpop(st);
        } catch (EmptyStackException e) {
            System.out.println("empty stack");
        }
    }
}
~~~

以上实例编译运行结果如下：
>stack: [ ]
>push(42)
>stack: [42]
>push(66)
>stack: [42, 66]
>push(99)
>stack: [42, 66, 99]
>pop -> 99
>stack: [42, 66]
>pop -> 66
>stack: [42]
>pop -> 42
>stack: [ ]
>pop -> empty stack



## 字典（Dictionary）
字典（Dictionary） 类是一个抽象类，它定义了键映射到值的数据结构。
当你想要通过特定的键而不是整数索引来访问数据的时候，这时候应该使用Dictionary。
由于Dictionary类是抽象类，所以它只提供了键映射到值的数据结构，而没有提供特定的实现。

Dictionary 类是一个抽象类，用来存储键/值对，作用和Map类相似。
给出键和值，你就可以将值存储在Dictionary对象中。一旦该值被存储，就可以通过它的键来获取它。所以和Map一样， Dictionary 也可以作为一个键/值对列表。

Dictionary定义的抽象方法如下表所示：

|序号|方法|描述|
|:---|:---|:---|
|1|Enumeration elements( )|返回此 dictionary 中值的枚举。|
|2|Object get(Object key)|返回此 dictionary 中该键所映射到的值。|
|3|boolean isEmpty( )|测试此 dictionary 是否不存在从键到值的映射。|
|4|Enumeration keys( )|返回此 dictionary 中的键的枚举。|
|5|Object put(Object key, Object value)|将指定 key 映射到此 dictionary 中指定 value。|
|6|Object remove(Object key)|从此 dictionary 中移除 key （及其相应的 value）。|
|7|int size( )|返回此 dictionary 中条目（不同键）的数量。|

Dictionary类已经过时了。在实际开发中，你可以实现Map接口来获取键/值的存储功能。


## 哈希表（Hashtable）

Hashtable类提供了一种在用户定义键结构的基础上来组织数据的手段。
例如，在地址列表的哈希表中，你可以根据邮政编码作为键来存储和排序数据，而不是通过人名。
哈希表键的具体含义完全取决于哈希表的使用情景和它包含的数据。

- Hashtable是原始的java.util的一部分， 是一个Dictionary具体的实现。
然而，Java 2 重构的Hashtable实现了Map接口，因此，Hashtable现在集成到了集合框架中。它和HashMap类很相似，但是它支持同步。
- 像HashMap一样，Hashtable在哈希表中存储键/值对。当使用一个哈希表，要指定用作键的对象，以及要链接到该键的值。然后，该键经过哈希处理，所得到的散列码被用作存储在该表中值的索引。

Hashtable定义了四个构造方法。
- 第一个是默认构造方法：
	Hashtable()

- 第二个构造函数创建指定大小的哈希表：
	Hashtable(int size)

- 第三个构造方法创建了一个指定大小的哈希表，并且通过fillRatio指定填充比例。填充比例必须介于0.0和1.0之间，它决定了哈希表在重新调整大小之前的充满程度：
	Hashtable(int size,float fillRatio)

- 第四个构造方法创建了一个以M中元素为初始化元素的哈希表。哈希表的容量被设置为M的两倍。
	Hashtable(Map m)

Hashtable中除了从Map接口中定义的方法外，还定义了以下方法：

|序号|方法|描述|
|:---|:---|:---|
|1|void clear( )|将此哈希表清空，使其不包含任何键。|
|2|Object clone( )|创建此哈希表的浅表副本。|
|3|boolean contains(Object value)|测试此映射表中是否存在与指定值关联的键。|
|4|boolean containsKey(Object key)|测试指定对象是否为此哈希表中的键。|
|5|boolean containsValue(Object value)|如果此 Hashtable 将一个或多个键映射到此值，则返回 true。|
|6|Enumeration elements( )|返回此哈希表中的值的枚举。|
|7|Object get(Object key)|返回指定键所映射到的值，如果此映射不包含此键的映射，则返回 null. 更确切地讲，如果此映射包含满足 (key.equals(k)) 的从键 k 到值 v 的映射，则此方法返回 v；否则，返回 null。|
|8|boolean isEmpty( )|测试此哈希表是否没有键映射到值。|
|9|Enumeration keys( )| 返回此哈希表中的键的枚举。|
|10|Object put(Object key, Object value)|将指定 key 映射到此哈希表中的指定 value。|
|11|void rehash( )|增加此哈希表的容量并在内部对其进行重组，以便更有效地容纳和访问其元素。|
|12|Object remove(Object key)|从哈希表中移除该键及其相应的值。|
|13|int size( )| 返回此哈希表中的键的数量。|
|14|String toString( )|返回此 Hashtable 对象的字符串表示形式，其形式为 ASCII 字符 ", " （逗号加空格）分隔开的、括在括号中的一组条目。|

**实例**

~~~java
import java.util.*;

public class HashTableDemo {

   public static void main(String args[]) {
      // Create a hash map
      Hashtable balance = new Hashtable();
      Enumeration names;
      String str;
      double bal;

      balance.put("Zara", new Double(3434.34));
      balance.put("Mahnaz", new Double(123.22));
      balance.put("Ayan", new Double(1378.00));
      balance.put("Daisy", new Double(99.22));
      balance.put("Qadir", new Double(-19.08));

      // Show all balances in hash table.
      names = balance.keys();
      while(names.hasMoreElements()) {
         str = (String) names.nextElement();
         System.out.println(str + ": " +
         balance.get(str));
      }
      System.out.println();
      // Deposit 1,000 into Zara's account
      bal = ((Double)balance.get("Zara")).doubleValue();
      balance.put("Zara", new Double(bal+1000));
      System.out.println("Zara's new balance: " +
      balance.get("Zara"));
   }
}
~~~

以上实例编译运行结果如下：
>Qadir: -19.08
>Zara: 3434.34
>Mahnaz: 123.22
>Daisy: 99.22
>Ayan: 1378.0
>Zara's new balance: 4434.34


## 属性（Properties）
Properties 继承于 Hashtable.Properties 类表示了一个持久的属性集.属性列表中每个键及其对应值都是一个字符串。
Properties 类被许多Java类使用。例如，在获取环境变量时它就作为System.getProperties()方法的返回值。

- Properties 继承于 Hashtable.表示一个持久的属性集.属性列表中每个键及其对应值都是一个字符串。

- Properties 类被许多Java类使用。

	例如，在获取环境变量时它就作为System.getProperties()方法的返回值。

- Properties 定义如下实例变量.这个变量持有一个Properties对象相关的默认属性列表。

	Properties defaults;

Properties类定义了两个构造方法. 

- 第一个构造方法没有默认值。

	Properties()

- 第二个构造方法使用propDefault 作为默认值。两种情况下，属性列表都为空：

	Properties(Properties propDefault)

除了从Hashtable中所定义的方法，Properties定义了以下方法：

|序号|方法|描述|
|:---|:---|:---|
|1|String getProperty(String key)|用指定的键在此属性列表中搜索属性。|
|2|String getProperty(String key, String defaultProperty)|用指定的键在属性列表中搜索属性。|
|3|void list(PrintStream streamOut)|将属性列表输出到指定的输出流。|
|4|void list(PrintWriter streamOut)|将属性列表输出到指定的输出流。|
|5|void load(InputStream streamIn) throws IOException|从输入流中读取属性列表（键和元素对）。|
|6|Enumeration propertyNames( )|按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。|
|7|Object setProperty(String key, String value)|调用 Hashtable 的方法 put。|
|8|void store(OutputStream streamOut, String description)| 以适合使用  load(InputStream)方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。|

**实例**

~~~java
import java.util.*;

public class PropDemo {

   public static void main(String args[]) {
      Properties capitals = new Properties();
      Set states;
      String str;
      
      capitals.put("Illinois", "Springfield");
      capitals.put("Missouri", "Jefferson City");
      capitals.put("Washington", "Olympia");
      capitals.put("California", "Sacramento");
      capitals.put("Indiana", "Indianapolis");

      // Show all states and capitals in hashtable.
      states = capitals.keySet(); // get set-view of keys
      Iterator itr = states.iterator();
      while(itr.hasNext()) {
         str = (String) itr.next();
         System.out.println("The capital of " +
            str + " is " + capitals.getProperty(str) + ".");
      }
      System.out.println();

      // look for state not in list -- specify default
      str = capitals.getProperty("Florida", "Not Found");
      System.out.println("The capital of Florida is "
          + str + ".");
   }
}
~~~

以上实例编译运行结果如下：
>The capital of Missouri is Jefferson City.
>The capital of Illinois is Springfield.
>The capital of Indiana is Indianapolis.
>The capital of California is Sacramento.
>The capital of Washington is Olympia.
>The capital of Florida is Not Found.



---

本文档 Github ：
https://github.com/bushehui/Java_tutorial


<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>

