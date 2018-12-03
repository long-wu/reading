第一次课 算法基础
====================


## 1、时间复杂度介绍
### 常数时间的操作
	一个操作如果跟数据量没有关系，每次都是固定时间内完成的操作就叫常数操作

### 时间复杂度
	时间复杂度为一个算法流程中常数操作数量的指标(一般是最差情况下)，常用O(读作big O)来表示
	具体来说在常数操作数量的表达式中，只要高阶项，不要低阶项，也不要高阶项的系数
	然后把剩下的部分记为f(N)，那么时间复杂度为O(f(N))

### 如何评价一个算法的好坏
	评价一个算法的好坏，先看时间复杂度的指标，
	然后再分析不同数据样本下的实际运行时间，也就是常数项时间


## 2、简单排序的复杂度
### 时间复杂度及空间复杂度
* 冒泡排序: 时间复杂度: 严格O(N^2) 额外空间复杂度: O(1)
* 选择排序: 时间复杂度: 严格O(N^2) 额外空间复杂度: O(1)
* 插入排序: 时间复杂度: O(N^2) 	 额外空间复杂度: O(1)
* 注: 插入排序的真正时间复杂度和数组状态有关，如果数组有序则时间复杂度为O(N)

### swap代码:
	// 后面都要用到的swap代码:
	public static void swap(int[] arr, int i, int j){
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

### 冒泡排序代码：
	public static void bubbleSort(int[] arr){
		if(arr==null || arr.length<2){
			return;
		}
		for(int end=arr.length-1; end>=0; end--){
			for(int i=0; i<end; i++){
				if(arr[i] > arr[i+1]){
					swap(arr, i, i+1);
				}
			}
		}
	}
		

### 选择排序代码:
	public static void selectionSort(int[] arr){
		if(arr==null || arr.length<2){
			return;
		}
		for(int i=0; i<arr.length-1; i++){
			int minIndex = i;
			for(int j=i+1; j<arr.length; j++){
				minIndex = arr[j] < arr[minIndex] ? j : minIndex;
			}
			swap(arr, i, minIndex);
		}
	}

### 插入排序代码:
	public static void insertionSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		for (int i = 1; i < arr.length; i++) {
			for (int j = i - 1; j >= 0 && arr[j] > arr[j + 1]; j--) {
				swap(arr, j, j+1);
			}
			
		}
	}


## 3、对数器的概念和使用
### 什么是对数器
	有一个你想要测试的方法a
	实现一个绝对正确但是复杂度不好的方法b(好实现能实现的方法)
	实现一个随机样本产生器
	实现将两种方法产生的结果对比的方法
	把方法a和方法b对比很多次来验证方法a是否正确
	如果有一个样本使得对比出错，打印样本分析是哪个方法出错
	当样本数量很多时比对测试依然正确，可以确定方法a已经正确
			
	另外当我们无法保证绝对正确的方法b正确时我们可以减少样本的数量，我们可以在
	对比验证不等时打印变量，然后看出问题所在

### 对数器使用
	以上面的冒泡排序为例:
		// for test
		public static void comparator(int[] arr) {
			// call java built-in sort
			Arrays.sort(arr);
		}
			
		// for test
		public static int[] generateRandomArray(int maxSize, int maxValue) {
			// generate a random array
			// (int) ((maxSize + 1) * Math.random()) -> [0, maxSize]
			int[] arr = new int[(int) ((maxSize + 1) * Math.random())];		
			for (int i = 0; i < arr.length; i++) {
				// value of array element -> [1 - maxValue, maxValue]
				arr[i] = (int) ((maxValue + 1) * Math.random()) - (int) (maxValue * Math.random());
			}
			return arr;
		}
			
		// for test
		public static int[] copyArray(int[] arr) {
			// copy a array to another array
			if (arr == null) {
				return null;
			}
			int[] res = new int[arr.length];
			for (int i = 0; i < arr.length; i++) {
				res[i] = arr[i];
			}
			return res;
		}
			
		// for test
		public static boolean isEqual(int[] arr1, int[] arr2) {
			// to judge two array is equal
			if ((arr1 == null && arr2 != null) || (arr1 != null && arr2 == null)) {
				return false;
			}
			if (arr1 == null && arr2 == null) {
				return true;
			}
			if (arr1.length != arr2.length) {
				return false;
			}
			for (int i = 0; i < arr1.length; i++) {
				if (arr1[i] != arr2[i]) {
					// 可以在此打印不一样的值
					// printArray(arr1); printArray(arr2);
					return false;
				}
			}
			return true;
		}
				
		// for test
		public static void printArray(int[] arr) {
			// print all elements of array 
			if (arr == null) {
				return;
			}
			for (int i = 0; i < arr.length; i++) {
				System.out.print(arr[i] + " ");
			}
			System.out.println();
		}
				
		// for test
		public static void main(String[] args) {
			// test main function
			int testTime = 500000;		// test times
			int maxSize = 100;			// max size of array
			int maxValue = 100;			// max value of array element
			boolean succeed = true;
			for (int i = 0; i < testTime; i++) {
				// generate a random array and copy the array
				int[] arr1 = generateRandomArray(maxSize, maxValue);
				int[] arr2 = copyArray(arr1);
				bubbleSort(arr1);
				comparator(arr2);
				if (!isEqual(arr1, arr2)) {
					succeed = false;
					break;
				}
			}
			// if no errors print Nice! else print Funcking fucked!
			System.out.println(succeed ? "Nice!" : "Fucking fucked!");
			
			// to generate a array and bubbleSort it
			int[] arr = generateRandomArray(maxSize, maxValue);
			printArray(arr);
			bubbleSort(arr);
			printArray(arr);
		}
			
	如果输出结果中输出了Fucking fucked!，说明有地方输出不一样，
	此时我们可以在isEqual函数中打印相关变量来查看


## 4、递归
### 递归实质
	递归实质就是系统将当前函数的所有信息压入栈中，然后调用子过程(函数)，
	子过程调用结束后系统从栈中取出栈顶函数相关信息还原现场继续执行函数
	故所有递归都可以改成非递归(自己来写压栈)

### 递归复杂度计算
	通式(master公式): T(N) = aT(N/b) + O(n^d)
		eg: a=2 b=2 d=0时 T(N) = 2T(N/2) + O(1)
	计算:
		log(b, a) > d => 复杂度为: O(N^log(b, a))
		log(b, a) = d => 复杂度为: O(N^d * logN)
		log(b, a) < d => 复杂度为: O(N^d)
	示例:
		T(N) = 2T(N/2) + O(1) => O(N) 			实际情况: 递归求数组最大值(递归之后只用比较)
		T(N) = 2T(N/2) + O(N) => O(N*logN)		实际情况: 递归之后还要遍历的
		T(N) = 2T(N/2) + O(N^2) => O(N^2) 		


## 5、复杂排序
* 归并排序: 		时间复杂度: O(N * logN) 额外空间复杂度: O(N)
* 随机快速排序: 	时间复杂度: O(N * logN) 额外空间复杂度: O(logN)
* 堆排序:    	时间复杂度: O(N * logN) 额外空间复杂度: O(1)

### 归并排序代码
	public static void mergeSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		sortProcess(arr, 0, arr.length - 1);
	}
			
	public static void sortProcess(int[] arr, int L, int R) {
		if (L == R) {
			return;
		}
		int mid = (L + R) / 2; // L和R中点的位置
		sortProcess(arr, L, mid);				// T(N/2)
		sortProcess(arr, mid + 1, R);			// T(N/2)
		merge(arr, L, mid, R);					// O(N)
		// T(N) = 2*T(N/2) + O(N) => 
	}
		
	public static void merge(int[] arr, int L, int mid, int R) {
		// merge: sort the array(L to R)
		int[] help = new int[R - L + 1];		// auxiliary array
		int i = 0;
		int p1 = L;
		int p2 = mid + 1;
		while (p1 <= mid && p2 <= R) {
			// if the value of arr[p1] < arr[p2] give value arr[p1] to help[i]
			// else give value arr[p2] to help[i]
			help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
		}
		// p1\p2 must be and only one arrive the boundary
		// give the remain elements of no-arrive boundary's one(p1 or p2) to help 
		while (p1 <= mid) {
			help[i++] = arr[p1++];
		}
		while (p2 <= R) {
			help[i++] = arr[p2++];
		}
		for (i = 0; i < help.length; i++) {
			// give elements of help to arr
			arr[L + i] = help[i];
		}
	}

### 快速排序代码(优化后的随机快速排序)
	public static void quickSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		quickSort(arr, 0, arr.length - 1);
	}
			
	public static void quickSort(int[] arr, int L, int R) {
		if (L < R) {
			// swap使最后一个数随机 减少极限情况的影响(eg: 1 2 3 4 5这样的数组)
			swap(arr, L + (int) (Math.random() * (R - L + 1)), R);
			// 将小于R的数移到左边 大于R的数移到右边 然后R移到中间 返回R的范围
			int[] p = partition(arr, L, R);		
			// 以中间区域划分	
			quickSort(arr, L, p[0] - 1);
			quickSort(arr, p[1] + 1, R);
		}
	}
			
	public static int[] partition(int[] arr, int L, int R) {
		// 默认以最后一个位置做划分
		int less = L - 1;
		int more = R;
		while (L < more) {
			// L表示当前位置
			if (arr[L] < arr[R]) {
				swap(arr, L++, ++less);
			} else if (arr[L] > arr[R]) {
				swap(arr, L, --more);
			} else {
				L++;
			}
		}
			
		swap(arr, more, R);			// 将最后一个数替换到右边界
		// 返回等于区域的左右边界
		return new int[] { less + 1, more };
	}

### 堆排序详解及代码
	堆排序实质上是利用了二叉树 
	实质上在工程中二叉树利用数组结构存储，抽象存储原理如下:
	二叉树可以转换成数组: 节点i的左右孩子分别是2*i+1和2*i+2 节点i的父节点是(i-1)/2
			
	大根堆和小根堆:
		大根堆: 树的任何一个子树的最大值是这颗子树的顶部
		小根堆: 树的任何一个子树的最小值是这颗子树的顶部
		
	堆排序过程:
		堆排序实际上就是利用堆结构的排序
		建立一个大根堆(复杂度为O(n)) -> 使用heapInsert插入数 
		然后把大根堆第一个数和最后一个数替换 拿出这个元素把堆的size-1 然后使用heapify对堆进行调整
			
	去掉堆顶的最大数或最小数: 
		把堆顶(第一个元素)和最后一个元素交换，拿出这个元素然后把堆的size-1，然后对堆顶进行hapify调整
		
	优先级队列结构其实就是堆结构			
		
	代码:
		public static void heapSort(int[] arr){
			if(arr==null||arr.length>2){
				return;
			}
			for(int i=0; i<arr.length; i++){
				heapInsert(arr, i);					// 初步建立大根堆 0到i
			}
			int heapSize = arr.length;
			swap(arr, 0, --heapSize);				// 第一个和最后一个交换 然后heapSize-1
			while(heapSize>0){
				heapify(arr, 0, --heapSize);
				swap(arr, 0, --heapSize);
			}
		}
			
		public static void heapInsert(int[] arr, int index){
			// 向大根堆中插入元素   并调整元素位置
			while(arr[index]>arr[(index-1)/2]){
				swap(arr, index, (index-1)/2);
				index = (index-1)/2;
			}
		}	
				
		public static void heapify(int[] arr, int index, int heapSize){
			// 拿出堆顶元素之后的调整堆
			int left = index*2+1;
			int right = left+1;
			while(left<heapSize){
				int largest = right<heapSize && arr[right] > arr[left] ? right : left;
				largest = arr[largest] > arr[index] ? largest : index;
				if(largest == index){
					break;
				}
				swap(arr, largest, index);
				index = largest;
				left = index * 2 + 1；
				right = left + 1;
			}
		}


## 6、排序算法总结
### 比较少用到的算法(了解即可)
* 桶排序:	时间复杂度O(N) 额外空间复杂度O(N)
* 计数排序:	时间复杂度O(N) 额外空间复杂度O(N)
* 基数排序:	时间复杂度O(N) 额外空间复杂度O(N)
* 上述三种都是非基于比较的排序，与被排序的样本数据实际情况很有关系，在实际中并不常用
* 另外以上三种都是稳定的排序

### 桶排序计数排序基数排序
	桶: 相当于一个容器，可以是数组中的某个位置也可以是双向链表也可以是一个队列也可以是一个堆
	把相应东西放入相应桶内 然后再从低位置的桶依次到高位置 依次把东西倒出来
		
	eg: 数组arr长度为60 数据值为0到60 使用桶排序： 生成一个数组temp长度为61 依次遍历arr得到arr[i]
		将temp对应的数组值++ temp[arr[i]] = temp[arr[i]] + 1 遍历完了之后遍历数组temp输出结果
		每个下标对应几个值就把下标值输出几遍
		实际上这个实例是计数排序 实质上是桶排序的一种实现
		
	基数排序将计数排序进行了改进，用10个桶进行排序 分别针对个位、十位、百位	

### 算法稳定性
* 追求算法稳定性的意义: 第一次排序和第二次排序在某些值相等的情况下保持原来的排序顺序
* 冒泡排序: 可以实现稳定(相等的情况下后一个继续往后走)
* 插入排序: 可以实现稳定(相等就不往前插入)
* 选择排序: 做不到稳定
* 归并排序: 可以实现稳定(merge相等的时候就先移动左边的)
* 快速排序: 做不到稳定(partition时做不到)
* 堆排序:   做不到稳定	

### 排序问题的补充说明
* 归并排序的额外空间复杂度可以变成O(1)，但是是非常难的、、、
* 快速排序也可以做到稳定性，但是非常难、、、

### 工程中的综合排序算法(例如Java内置的sort)
	基本思想: 
		数组长度很短(大概小于60)，用插入排序(插入排序常数极低)
		数组长度很长，会先判断里面装的是什么，如果都是基本类型就用快排(不在乎稳定性)，如果都是
		自己定义的类型就用归并排序(在乎稳定性)


## 7、比较器的概念和使用
### 什么是比较器
	在使用系统的内置sort时 如果针对的是普通类型 那么会根据普通类型的值来进行排序
	但是如果针对的不是普通类型而是自定义类型 这时候就会根据类型对应的对象在内存中的位置进行排序
	此时排序就没有任何意义了 但是我们可以写一个自定义的比较器传入给sort使用 让sort使用我们定义的比较器来进行排序

### 比较器的使用
	比较器返回负数表示第一个参数排序时应该放前面
	比较器返回正数表示第二个参数排序时应该放前面
	比较器返回0表示两个参数一样大
			
	代码:
		// 比较器
		import java.util.*;
			
		public class MyComparator {
			
			public static class Student {
				public String name;
				public int id;
				public int age;
				
				public Student(String name, int id, int age) {
					this.name = name;
					this.id = id;
					this.age = age;
				}
			}
				
			public static class IdAscendingComparator implements Comparator<Student> {
				// id升序排列
				@Override
				public int compare(Student o1, Student o2) {
					return o1.id - o2.id;
				}
			
			}
				
			public static class IdDescendingComparator implements Comparator<Student> {
				// id降序排列
				@Override
				public int compare(Student o1, Student o2) {
					return o2.id - o1.id;
				}
				
			}
			
			public static class AgeAscendingComparator implements Comparator<Student> {
				// age升序排序
				@Override
				public int compare(Student o1, Student o2) {
					return o1.age - o2.age;
				}
			
			}
			
			public static class AgeDescendingComparator implements Comparator<Student> {
				// age降序排序
				@Override
				public int compare(Student o1, Student o2) {
					return o2.age - o1.age;
				}
				
			}
				
			public static void printStudents(Student[] students) {
				for (Student student : students) {
					System.out.println("Name : " + student.name + ", Id : " + student.id + ", Age : " + student.age);
				}
				System.out.println("===========================");
			}
			
			public static void main(String[] args) {
				Student student1 = new Student("A", 1, 23);
				Student student2 = new Student("B", 2, 21);
				Student student3 = new Student("C", 3, 22);
			
				Student[] students = new Student[] { student3, student2, student1 };
				printStudents(students);
				
				// 下面的sort不加后面的参数会按照内存中的地址排序
				Arrays.sort(students, new IdAscendingComparator());
				printStudents(students);
			
				Arrays.sort(students, new IdDescendingComparator());
				printStudents(students);
				
				Arrays.sort(students, new AgeAscendingComparator());
				printStudents(students);
				
				Arrays.sort(students, new AgeDescendingComparator());
				printStudents(students);
				
				// 优先队列其实就是个堆
				Comparator comparator = new AgeAscendingComparator();
				Queue<Student> heap = new PriorityQueue<Student>(3, comparator);
				heap.add(student3);
				heap.add(student2);
				heap.add(student1);
				
				System.out.println("优先队列排序: ");
				while(!heap.isEmpty()){
					Student student = heap.poll();		// 弹出堆顶
					System.out.println("Name : " + student.name + ", Id : " + student.id + ", Age : " + student.age);
				}
				
			}
			
		}


## 8、实例问题
### 小和问题
	问题描述: 
	在一个数组中，每一个数左边比当前数小的数累加起来，叫做这个数组的小和。
	求一个数组的小和。
		例子：	
		[1,3,4,2,5]	
		1左边比1小的数，没有；	
		3左边比3小的数，1；	
		4左边比4小的数，1、3；	
		2左边比2小的数，1；	
		5左边比5小的数，1、3、4、2；	
		所以小和为1+1+3+1+1+3+4+2=16
			
	解决思路:
		使用归并排序中的merge将数组分割，然后合并的时候对比大小(如果小就计算)并使合并的整体有序
				
	代码:
		public static int smallSum(int[] arr) {
			if (arr == null || arr.length < 2) {
				return 0;
			}
			return mergeSort(arr, 0, arr.length - 1);
		}
			
		public static int mergeSort(int[] arr, int L, int R) {
			if (L == R) {
				return 0;
			}
			int mid = L + (R - L) / 2;			// 等同于int mid = (L + R) / 2;
			return mergeSort(arr, L, mid) + mergeSort(arr, mid + 1, R)
					+ merge(arr, L, mid, R);
		}
			
		public static int merge(int[] arr, int L, int mid, int R) {
			int[] help = new int[R - L + 1]; // auxiliary array
			int i = 0;
			int p1 = L;
			int p2 = mid + 1;
			int res = 0;
			while (p1 <= mid && p2 <= R) {
				res += arr[p1] < arr[p2] ? (R - p2 + 1) * arr[p1] : 0;
				help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
			}
			while (p1 <= mid) {
				help[i++] = arr[p1++];
			}
			while (p2 <= R) {
				help[i++] = arr[p2++];
			}
			for (i = 0; i < help.length; i++) {
				arr[L + i] = help[i];
			}
			return res;
		}	
		
		public static void main(String[] args) {
			int[] arr = { 1, 3, 4, 2, 5 };
			int[] arr2 = { 1 };
			int[] arr3 = { 3, 1 };
			System.out.println(smallSum(arr));
			System.out.println(smallSum(arr2));
			System.out.println(smallSum(arr3));
		}

### 逆序对问题
	原理:
		同上个问题一样的套路解决
			
	代码:
		public static void reverseSequence(int[] arr) {
			if (arr == null || arr.length < 2) {
				return;
			}
			mergeSort(arr, 0, arr.length - 1);
		}
			
		public static void mergeSort(int[] arr, int L, int R) {
			if (L == R) {
				return;
			}
			int mid = L + (R - L) / 2;
			mergeSort(arr, L, mid);
			mergeSort(arr, mid + 1, R);
			merge(arr, L, mid, R);
		}
				
		public static void merge(int[] arr, int L, int mid, int R) {
			int[] help = new int[R - L + 1];
			int i = 0;
			int p1 = L;
			int p2 = mid + 1;
			while (p1 <= mid && p2 <= R) {
				if (arr[p1] > arr[p2]) {
					System.out.println(arr[p1] + " " + arr[p2]);
				}
				help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
			}
			while (p1 <= mid) {
				help[i++] = arr[p1++];
			}
			while (p2 <= R) {
				help[i++] = arr[p2++];
			}
			for (i = 0; i < help.length; i++) {
				arr[L + i] = help[i];
			}
			
		}
			
		public static void main(String[] args) {
			int[] arr = { 3, 1, 2, 5, 3 };
			ReverseSequence.reverseSequence(arr);
		}		

### 荷兰国旗问题
	给定一个数组 arr，和一个数 num，请把小于 num 的数放在数组的左边，等于 num 的数放在数组的中间，
	大于 num 的数放在数组的右边。要求额外空间复杂度 O(1)，时间复杂度 O(N)
		
	代码:
		public static void simpleSort(int[] arr, int num){
			// 给定数组arr和数num 把小于等于num的放在数组左边 大于num的放在数组右边
			int x = -1;
			for(int i=0; i<arr.length; i++){
				if(arr[i] <= num){
					x += 1;
					swap(arr, i, x);
				}
			}
		}	
			
		public static int[] partition(int[] arr, int L, int R, int num){
			// 小于num的放数组左边，等于num的放数组中间，大于num的放在数组右边
			int less = L - 1;
			int more = R + 1;
			int cur = L;
			while(cur<more){
				if(arr[cur]<more){
					swap(arr, cur++, ++less);	
				} else if(arr[cur]>more){
					swap(arr, cur, --more);
				} else{
					cur++;
				}
			}
			
			// 返回等于区域的左右边界
			if(less + 1 > more - 1){
				// 区域不存在
				return new int[]{-1, -1};
			} else{
				// 区域存在
				return new int[]{less + 1, more -1};
			}
		}	
		
		// for test
		public static int[] generateArray() {
			int[] arr = new int[10];
			for (int i = 0; i < arr.length; i++) {
				arr[i] = (int) (Math.random() * 9);
			}
			return arr;
		}
			
		// for test
		public static void printArray(int[] arr) {
			if (arr == null) {
				return;
			}
			for (int i = 0; i < arr.length; i++) {
				System.out.print(arr[i] + " ");
			}
			System.out.println();
		}
				
		public static void main(String[] args) {
			// test simpleSort
			int[] test = generateArray();
			printArray(test);
			simpleSort(test, 5);
			printArray(test);
				
			// test partition
			int[] test2 = generateArray();
			printArray(test2);
			int[] res = partition(test2, 0, test2.length - 1, 5);
			printArray(test2);
			System.out.println(res[0]);
			System.out.println(res[1]);
				
		}	

### 补充问题
	给定一个数组 求如果排序之后相邻两数的最大差值 要求时间复杂度O(N)且要求不能用非基于比较的排序
	思路:
		数组有N个数 放N+1个桶 数组最小值和最大值分别放在第一个桶和第二个桶 
		然后剩下的桶分别放剩下的数 每个桶放的数的分布范围均匀(每个桶范围一样)
		那么最后一定有一个空桶 相邻两数最大差值肯定是两个相邻桶(忽略空桶)内的数产生的
		每个桶设置: boolean值表示是否放入了数 max值表示桶中最大值 min值表示桶中最小值
		最后遍历所有非空桶 把右边非空桶的min减去左边相邻非空桶(忽略空桶)的max得到值
		然后得到的值其中的最大值就是相邻两数的最大差值

