第一次课 时间复杂度及对数器

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
* 归并排序: 时间复杂度: O(N * logN) 额外空间复杂度: O(N)
* 快速排序: 
* 堆排序:   

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


## 6、小和问题和逆序对问题
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



### 逆序对问题
	同理解决
			
	代码:
				
