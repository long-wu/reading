第二次课 基础数据结构
=======================


## 1、队列和栈相关
### 数组实现队列和栈
	// 数组实现栈:
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
			
	// 数组实现队列:		
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
	请用队列实现栈结构以及用栈结构实现队列结构:
		
	public static class TwoStacksQueue{
		// 用栈实现队列
		private Stack<Integer> stackPush;		// 入队栈
		private Stack<Integer> stackPop;		// 出队栈
		
		public TwoStacksQueue(){
			stackPush = new Stack<Integer>();
			stackPop = new Stack<Integer>();
		}
		
		public void push(int pushInt){
			// 入队: 把数据压入入队栈(入队只把数据放进入队栈)
			stackPush.push(pushInt);
		}
		
		public void dao(){
			// 倒数据的方法 -> 入队栈倒到出队栈
			if(!stackPop.isEmpty()){
				// 出队栈不为空时不能倒数据
				return;
			} 
			while(!stackPush.isEmpty()){
				// 只要到数据就要依次倒完
				stackPop.push(stackPush.pop());
			}
		}
		
		public int pop(){
			// 出队: 取出出队栈栈顶元素(如果出队栈为空入队栈有数据就把入队栈数据压入出队栈)
			if(stackPop.empty() && stackPush.empty()){
				throw new RuntimeException("Queue is empty!");
			} 
			dao();
			
			return stackPop.pop();
		}
		
		public int peek(){
			// 获得顶部元素: 原理和上面的pop一样 只不过最后用的是栈的peek
			if(stackPop.empty() && stackPush.empty()){
				throw new RuntimeException("Queue is empty!");
			} 
			dao();
			
			return stackPop.peek();
		}
		
	}
		
	public static class TwoQueuesStack{
		// 用队列实现栈
		private Queue<Integer> data;
		private Queue<Integer> help;
		
		public TwoQueuesStack(){
			data = new LinkedList<Integer>();
			help = new LinkedList<Integer>();
		}
		
		public void push(int pushInt){
			// 入栈: 把数据放入data队列中(数只进data队列)
			data.add(pushInt);
		}
		
		public int pop(){
			// 出栈: 若data队列不为空把data队列中全部-1个数据出队放到help队列中 
			// 然后data队列中剩下的那个元素就是栈的顶部元素  poll弹出即可 然后交换data队列和help队列
			if(data.isEmpty()){
				throw new RuntimeException("Stack is empty!");
			}
			while(data.size() != 1){
				// 把data栈的东西全部拿出依次放入help栈
				help.add(data.poll());
			}
			int res = data.poll();
			swap();
			
			return res;
		}
		
		public int peek(){
			// 获得顶部元素: 若data队列不为空把data队列中全部-1个数据出队放到help队列中 
			// 然后data队列中剩下的那个元素就是栈的顶部元素  用peek取出即可 然后交换data队列和help队列
			if (data.isEmpty()) {
				throw new RuntimeException("Stack is empty!");
			}
			while (data.size() != 1) {
				// 把data栈的东西全部拿出依次放入help栈
				help.add(data.poll());
			}
			int res = data.peek();
			swap();			
			
			return res;
		}
		
		private void swap(){
			Queue<Integer> tmp = help;
			help = data;
			data = tmp;
		}
		
	}
	
### 猫狗队列问题
	代码结构:
		public class Pet{
	        private String type;
	        public Pet(String type){
	            this.type = type;          
	        }
	        public String getPetType(){
	            return this.type;
	        }
	    }
	    public class Dog extends Pet {
	        public Dog(){
	            super("dog");
	        }
	    }
	    public class Cat extends Pet{
	        public Cat(){
	            super("cat");
	        }
	    }
				
	实现一种猫狗队列的结构，要求如下： 			
		用户可以调用add方法将cat类或者dog类的实例放入队列中；
		用户可以调用pollAll方法，将队列中所有的实例按照队列的先后顺序依次弹出；
		用户可以调用pollDog方法，将队列中dog类的实例按照队列的先后顺序依次弹出；
		用户可以调用pollCat方法，将队列中cat类的实例按照队列的先后顺序依次弹出；
		用户可以调用isEmpty方法，检查队列中是否还有dog和cat的实例；
		用户可以调用isDogEmpty方法，检查队列中是否还有do的实例；
		用户可以调用isCatEmpty方法，检查队列中是否还有cat的实例。
			
	代码:
		public static class Pet {
			private String type;
				
			public Pet(String type) {
				this.type = type;
			}
			
			public String getPetType() {
				return this.type;
			}
		}
			
		public static class Dog extends Pet {
			public Dog() {
				super("dog");
			}
		}
			
		public static class Cat extends Pet {
			public Cat() {
				super("cat");
			}
		}
			
		public static class PetEnterQueue {
			// 队列中的猫或狗
			private Pet pet;
			private long count;
			
			public PetEnterQueue(Pet pet, long count) {
				this.pet = pet;
				this.count = count;
			}
				
			public Pet getPet() {
				return this.pet;
			}
			
			public long getCount() {
				return this.count;
			}	
		}
			
		public static class dogCatQueue {
			private Queue<PetEnterQueue> dogQ;
			private Queue<PetEnterQueue> catQ;
			private long count;
			
			public dogCatQueue() {
				this.dogQ = new LinkedList<DogCatQueue.PetEnterQueue>();
				this.catQ = new LinkedList<DogCatQueue.PetEnterQueue>();
				this.count = 0;
			}
				
			public void add(Pet pet) {
				// 向队列中添加Cat和Dog的实例
				if (pet.getPetType().equals("dog")) {
					this.dogQ.add(new PetEnterQueue(pet, this.count++));
				} else if (pet.getPetType().equals("cat")) {
					this.catQ.add(new PetEnterQueue(pet, this.count++));
				} else {
					throw new RuntimeException("err, not dog or cat");
				}
			}
			
			public Pet pollAll() {
				// 猫狗队列中所有的实例按照队列的先后顺序依次弹出
				if (!this.dogQ.isEmpty() && !this.catQ.isEmpty()) {
					// count小的是先添加进队列的
					if (this.dogQ.peek().getCount() < this.catQ.peek().getCount()) {
						return this.dogQ.poll().getPet();
					} else {
						return this.catQ.poll().getPet();
					}
				} else if (!this.dogQ.isEmpty()) {
					return this.dogQ.poll().getPet();
				} else if (!this.catQ.isEmpty()) {
					return this.catQ.poll().getPet();
				} else {
					// 猫狗队列都为空
					throw new RuntimeException("err, queue is empty!");
				}
			}
			
			public Pet pollDog() {
				// 将队列中dog类的实例按照队列的先后顺序依次弹出
				if(this.isDogEmpty()){
					throw new RuntimeException("err, dog queue is empty");
				} else{
					return this.dogQ.poll().getPet();
				}
			}
			
			public Pet pollCat() {
				// 将队列中cat类的实例按照队列的先后顺序依次弹出
				if(this.isCatEmpty()){
					throw new RuntimeException("err, cat queue is empty");
				} else{
					return this.catQ.poll().getPet();
				}	
			}
			
			public boolean isEmpty() {
				// 检查队列中是否还有dog和cat的实例
				return this.dogQ.isEmpty() && this.catQ.isEmpty();
			}
			
			public boolean isDogEmpty() {
				// 检查队列中是否还有dog的实例
				return this.dogQ.isEmpty();
			}
			
			public boolean isCatEmpty() {
				// 检查队列中是否还有cat的实例
				return this.catQ.isEmpty();
			}
			
		}
			
		public static void main(String[] args) {
			dogCatQueue test = new dogCatQueue();
			
			Pet dog1 = new Dog();
			Pet cat1 = new Cat();
			Pet dog2 = new Dog();
			Pet cat2 = new Cat();
			Pet dog3 = new Dog();
			Pet cat3 = new Cat();
				
			test.add(dog1);
			test.add(cat1);
			test.add(dog2);
			test.add(cat2);
			test.add(dog3);
			test.add(cat3);
			
			test.add(dog1);
			test.add(cat1);
			test.add(dog2);
			test.add(cat2);
			test.add(dog3);
			test.add(cat3);
			
			test.add(dog1);
			test.add(cat1);
			test.add(dog2);
			test.add(cat2);
			test.add(dog3);
			test.add(cat3);
				
			while (!test.isDogEmpty()) {
				// 弹出所有dog实例  并输出
				System.out.println(test.pollDog().getPetType());
			}
			
			while (!test.isEmpty()) {
				// 弹出所有cat实例  并输出
				System.out.println(test.pollAll().getPetType());
			}
			
			System.out.println(test.isEmpty());
		}	


## 2、矩阵相关
### 转圈打印矩阵
	给定一个整形矩阵matrix,请按照转圈的方式打印它。
			
	例如：		
		1    2    3    4			
		5    6    7    8		
		9    10   11   12		
		13   14   15   16
			
	打印结果为： 1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
	要求额外空间复杂度为O(1)
			
	代码:
		public class RotateOutputMatrix {
			private int M;
			private int N;
			private int[][] arr;
			private final int startValue = 1;
				
			public RotateOutputMatrix(int M, int N) {
				this.M = M;
				this.N = N;
				int value = startValue;
				arr = new int[M][N];
				for (int i = 0; i < M; i++) {
					for (int j = 0; j < N; j++) {
						this.arr[i][j] = value++;
					}
				}
			}
			
			public void printEdge(int a, int b, int c, int d) {
				// 输出一层
				// (a, b) -> 左上角 (c, d) -> 右下角
				if (b == d) {
					// 两个点在一列上
					for (int i = a; i <= c; i++) {
						System.out.print(arr[i][b] + " ");
					}
				} else if (a == c) {
					// 两个点在一行上
					for (int j = b; j <= d; j++) {
						System.out.print(arr[a][j] + " ");
					}
				} else {
					// 两个点不在一行也不在一列上 -> 两个点在对角线上
					int curA = a;
					int curB = b;
					while (curB < d) {
						System.out.print(arr[curA][curB] + " ");
						curB++;
					}
					while (curA < c) {
						System.out.print(arr[curA][curB] + " ");
						curA++;
					}
					while (curB > b) {
						System.out.print(arr[curA][curB] + " ");
						curB--;
					}
					while (curA > a) {
						System.out.print(arr[curA][curB] + " ");
						curA--;
					}
				}
			}
				
			public void printAllEdge() {
				// 输出所有层
				int a = 0;
				int b = 0;
				int c = this.M - 1;
				int d = this.N - 1;
				while (a <= c && b <= d) {
					printEdge(a++, b++, c--, d--);
				}
			}
				
			public static void main(String[] args) {
				RotateOutputMatrix test = new RotateOutputMatrix(4, 4);
				test.printAllEdge();
			}
		}
			
	
### 旋转正方形矩阵
	给定一个整型正方形矩阵matrix,请把该矩阵调整成 顺时针旋转90度的样子
	要求额外空间复杂度为O(1)
		
	代码:
		public class RotateSquare90 {
			private int M;
			private int[][] arr;
			private final int startValue = 1;
					
			public RotateSquare90(int M) {
				this.M = M;
				int value = startValue;
				arr = new int[M][M];
				for (int i = 0; i < M; i++) {
					for (int j = 0; j < M; j++) {
						this.arr[i][j] = value++;
					}
				}
			}
			
			public void rotateEdge(int a, int b, int c, int d) {
				// 旋转一层
				int times = c - a;
				int tmp = 0;
				for (int i = 0; i < times; i++) {
					tmp = arr[a][b + i];
					arr[a][b + i] = arr[c - i][b];
					arr[c - i][b] = arr[c][d - i];
					arr[c][d - i] = arr[a + i][d];
					arr[a + i][d] = tmp;
				}
			}
			
			public void rotateAllEdge() {
				// 旋转所有层
				int i = 0;
				int j = 0;
				int m = this.M - 1;
				int n = this.M - 1;
				while(i<=m&&j<=n){
					rotateEdge(i++, j++, m--, n--);
				}
			}
				
			// for test
			public void printMatrix() {
				int[][] m = this.arr;
				for (int i = 0; i < m.length; i++) {
					for (int j = 0; j < m[0].length; j++) {
						System.out.print(m[i][j] + " ");
					}
					System.out.println();
				}
			}
				
			public static void main(String[] args) {
				RotateSquare90 test = new RotateSquare90(4);
				test.printMatrix();
				test.rotateAllEdge();
				test.printMatrix();
			}
					
		}				

### 之字形打印矩阵
	 给定一个矩阵matrix， 按照“之” 字形的方式打印这个矩阵， 例如： 1 2 3 4 5 6 7 8 9 10 11 12
	“之” 字形打印的结果为： 1， 2， 5， 9， 6， 3， 4， 7， 10， 11，8， 12
	要求额外空间复杂度为O(1)。
				
	代码:
		public class ZigZagPrintMatrix {
			private int M;
			private int N;
			private int[][] arr;
			private final int startValue = 1;
			
			public ZigZagPrintMatrix(int M, int N) {
				this.M = M;
				this.N = N;
				int value = startValue;
				arr = new int[M][N];
				for (int i = 0; i < M; i++) {
					for (int j = 0; j < N; j++) {
						this.arr[i][j] = value++;
					}
				}
			}
						
			public void printLevel(int a, int b, int c, int d, boolean f) {
				// 打印一条斜线 f为true表示从右上到左下打印 
				if (f) {
					// (a, b) -> (c, d)
					while (a != c + 1) {
						System.out.print(arr[a++][b--] + " ");
					}
				} else {
					// (c, d) -> (a, b)
					while (d != b + 1) {
						System.out.print(arr[c--][d++] + " ");
					}
				}
			}
				
			public void printMatrixZigZag() {
				// 打印所有斜线
				int a = 0;
				int b = 0;
				int c = 0;
				int d = 0;
				int endR = M - 1;
				int endC = N - 1;
				boolean fromUp = false;
				while (a != endR + 1) {
					printLevel(a, b, c, d, fromUp);
					a = b == endC ? a + 1 : a;
					b = b == endC ? b : b + 1;
					d = c == endR ? d + 1 : d;
					c = c == endR ? c : c + 1;
					fromUp = !fromUp;
				}
				System.out.println();
			}
				
			// test
			public void printMartix() {
				for (int i = 0; i < arr.length; i++) {
					for (int j = 0; j < arr[0].length; j++) {
						System.out.print(arr[i][j] + " ");
					}
					System.out.println();
				}
			}
				
			public static void main(String[] args) {
				ZigZagPrintMatrix test = new ZigZagPrintMatrix(3, 4);
				test.printMartix();
				test.printMatrixZigZag();
			}
		}
			 

### 在行和列都排序好的矩阵中找数
	给定一个N×M的整型矩阵matrix和一个正数K，matrix的每一行每一列都是排好序的。实现一个函数，判断K是否在matrix中
	例如： 
	　　0,　1,　2,　5 
	　　2,　3,　4,　7 
	　　4,　4,　4,　8 
	　　5,　7,　7,　9 
	如果K为7，返回True；如果K为6，返回False。 
	要求时间复杂度O(N+M)，空间复杂度O(1)
				
	思路: 从右上角开始对比 如果当前数比K大那么当前数往左走 如果当前数比K小那么当前数往下走
		  直到找到数K或者数走到左下角为止 时间复杂度为O(N+M)
		
		O(N+M)对应的最坏情况(找不到数或数就在左下角的位置):
		    1 3 5 6
		    2 5 7 9 
		    4 6 8 10   
			k = 4		



## 3、链表相关


## 4、二分拓展