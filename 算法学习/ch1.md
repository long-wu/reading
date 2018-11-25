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
* 插入排序: 时间复杂度: O(N^2) 额外空间复杂度: O(1)
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


### 对数器使用


