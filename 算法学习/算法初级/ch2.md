第二次课 基础数据结构
=======================


## 1、队列和栈相关
### 数组实现队列和栈
	// 数组实现栈
	public static class ArrayStack {
		private Integer[] arr;
		private Integer index;
		
		public ArrayStack(int initSize){
			// 栈的构造    initSize指定栈大小
			if(initSize<0){
				throw new IllegalArgumentException("The init index is less than 0");
			}
			arr = new Integer[initSize];
			index = 0;
		}
		
		public Integer peek(){
			// 返回栈顶
			if(index==0){
				return null;
			}
			return arr[index-1];
		}
		
		public void push(int obj){
			// 压数入栈
			if(index==arr.length){
				throw new ArrayIndexOutOfBoundsException("The stack is full!");
			}
			arr[index++] = obj;
		}
		
		public Integer pop(){
			// 弹出栈顶
			if(index==0){
				throw new ArrayIndexOutOfBoundsException("The stack is empty!");
			}
			return arr[--index];
		}
		
	}
			
	// 数组实现队列			
	public static class ArrayQueue{
		private Integer[] arr;
		private Integer size;
		private Integer start;
		private Integer end;
		
		public ArrayQueue(int initSize){
			if(initSize<0){
				throw new IllegalArgumentException("The init size is less than 0");
			}
			arr = new Integer[initSize];
			size = 0;
			start = 0;
			end = 0;
		}
		
		public Integer peek() {
			// 返回顶部元素
			if (size == 0) {
				return null;
			}
			return arr[start];
		}
		
		public void push(int obj) {
			// 向队列中加入元素
			if (size == arr.length) {
				throw new ArrayIndexOutOfBoundsException("The queue is full");
			}
			size++;
			arr[end] = obj;
			// end如果到最后一个位置就跳回到0 否则+1
			end = end == arr.length - 1 ? 0 : end + 1;
		}
			
		public Integer poll() {
			// 从队列出取出元素
			if (size == 0) {
				throw new ArrayIndexOutOfBoundsException("The queue is empty");
			}
			size--;
			int tmp = start;
			// start如果到最后一个位置就跳回到0 否则+1
			start = start == arr.length - 1 ? 0 : start + 1;
			return arr[tmp];
		}
		
	}

### 返回栈中最小元素
	实现一个特殊的栈  在实现栈的基本功能的基础上  再实现返回栈中最小元素的操作
	要求: pop push getMin 操作的时间复杂度都是O(1) 设计的栈类型可以使用现成的栈结构
			
	思路: 两个栈 一个data栈 一个min栈 data栈存数据 min栈存最小值 data栈就跟普通的栈一样压栈出栈
	min栈: 空时直接压栈 非空时且当前压入的数为最小值就把这个数压入栈中 非空且当前压入的数也不是最小值
	就把min栈的栈顶重复压入栈顶
		  	
	代码:
		public class GetMinStack {
			public static class MyStack1 {
				private Stack<Integer> stackData;
				private Stack<Integer> stackMin;
				
				public MyStack1() {
					this.stackData = new Stack<Integer>();
					this.stackMin = new Stack<Integer>();
				}
			
				public void push(int newNum) {
					if (this.stackMin.isEmpty()) {
						this.stackMin.push(newNum);
					} else if (newNum < this.getmin()) {
						this.stackMin.push(newNum);
					} else {
						int newMin = this.stackMin.peek();
						this.stackMin.push(newMin);
					}
					this.stackData.push(newNum);
				}
			
				public int pop() {
					if (this.stackData.isEmpty()) {
						throw new RuntimeException("Your stack is empty.");
					}
					this.stackMin.pop();
					return this.stackData.pop();
				}
				
				public int getmin() {
					if (this.stackMin.isEmpty()) {
						throw new RuntimeException("Your stack is empty.");
					}
					return this.stackMin.peek();
				}
			}
			
			public static void main(String[] args) {
				MyStack1 stack1 = new MyStack1();
				stack1.push(3);
				System.out.println(stack1.getmin());
				stack1.push(4);
				System.out.println(stack1.getmin());
				stack1.push(1);
				System.out.println(stack1.getmin());
				System.out.println(stack1.pop());
				System.out.println(stack1.getmin());
			}
			
		}

### 队列与栈
	请用队列实现栈结构以及用栈结构实现队列结构

## 2、矩阵相关


## 3、链表相关


## 4、二分拓展