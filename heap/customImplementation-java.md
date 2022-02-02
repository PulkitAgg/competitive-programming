```
public class CutstomMaxHeap {
	int heapSize = 0;
	int[] heap;
	int maxHeapSize;

	public static void main(String[] args) {
		CutstomMaxHeap heap = new CutstomMaxHeap(10000);
		heap.insert(5);
		heap.insert(10);
		heap.insert(4);
		heap.insert(3);
		heap.insert(17);
		heap.insert(11);
		heap.print();
		heap.extractMax();
		heap.print();
		System.out.println(heap.getMax());
		heap.changeValue(1, 6);
		heap.print();
		heap.changeValue(1, 2);
		heap.print();
		heap.changeValue(1, 15);
		heap.print();
	}
	
	CutstomMaxHeap(int size) {
		this.heapSize = 0;
		this.heap = new int[size];
		this.maxHeapSize = Integer.MAX_VALUE;
	}
	
	/**
	 * It will return maximum value from the heap
	 * @return maximum value
	 */
	public int getMax() {
		if(this.heapSize == 0) {
			return Integer.MIN_VALUE;
		}
		return this.heap[0];
	}

	/**
	 * It will return maximum value and also remove it from heap
	 * @return maximum value from heap
	 */
	public int extractMax() {
		if(this.heapSize == 0) {
			return Integer.MIN_VALUE;
		}
		int maxValue = this.heap[0];
		this.heap[0] = this.heap[this.heapSize - 1];
		this.heapSize--;
		this.downHeapify(0);
		return maxValue;
	}
	
	/**
	 * Maintain heap property for given index from top to bottom 
	 * @param i index on which we have to check and maintain the heap property
	 */
	public void downHeapify(int i) {
		int largest = i, left = this.left(i), right = this.right(i);
		if(left < this.heapSize && this.heap[left] > this.heap[largest]) {
			largest = left;
		}
		if(right < this.heapSize && this.heap[right] > this.heap[largest]) {
			largest = right;
		}
		if(largest != i) {
			int temp = this.heap[largest];
			this.heap[largest] = this.heap[i];
			this.heap[i] = temp;
			this.downHeapify(right);
		}
	}
  
  
  /**
	 * Maintain heap property for given index from top to bottom in recursive fashion
	 * @param pos index on which we have to check and maintain the heap property
	 */
	public void upHeapifyRecursively(int pos) {
		int parent = this.parent(pos);
		if(parent >= 0) {
			if(this.heap[parent] < this.heap[pos]) {
				int temp = this.heap[parent];
				this.heap[parent] = this.heap[pos];
				this.heap[pos] = temp;
				this.upHeapifyRecursively(parent);
			}
		}
	}
	
	/**
	 * Maintain heap property for given index from bottom to top 
	 * @param pos index on which we have to check and maintain the heap property
	 */
	public void upHeapify(int pos) {
		int temp = this.heap[pos];
		while(pos > 0 && temp > this.heap[this.parent(pos)]) {
			this.heap[pos] = this.heap[this.parent(pos)];
			pos = this.parent(pos);
		}
		this.heap[pos] = temp;
	}
	
	/**
	 * is given index leaf node
	 * @param i index
	 * @return boolean 
	 */
	public boolean isLeaf(int i) {
		return  i > (this.heapSize/2 -1);
	}
	
	/**
	 * Return parent index
	 * @param i index
	 * @return parent node index
	 */
	public int parent(int i) {
		return (int)Math.ceil((double)i/2) - 1;
	}
	
	/**
	 * Return left child index 
	 * @param i
	 * @return
	 */
	public int left(int i) {
		return 2*i + 1;
	}
	
	/**
	 * Return right child index value
	 * @param i
	 * @return
	 */
	public int right(int i) {
		return 2*i + 2;
	}

	/**
	 * Change  value at specific key
	 * @param i
	 * @param newVal
	 */
	public void changeValue(int i, int newVal) {
		boolean isNewValueIncrease = this.heap[i] < newVal;
		this.heap[i] = newVal;
		if(isNewValueIncrease) {
			// may pollute heap property for parent
			this.upHeapify(i);
//			this.upHeapifyRecursively(i);
		} else {
			// may pollute heap property for children
			this.downHeapify(i);
		}
	}

    /**
	 * Insert val to heap
	 * @param val
	 */
	public void insert(int val) {
		if(this.maxHeapSize == this.heapSize) {
			new Error("Heap is already fulled");
		}
		this.heap[this.heapSize++] = val;
		this.upHeapify(this.heapSize - 1);
//		this.upHeapifyRecursively(this.heapSize - 1);
	}
	
	/**
	 * Print Heap values
	 */
	public void print() {
		String heap = "";
		for(int i=0; i<this.heapSize; i++) {
			heap += this.heap[i] + " ";
		}
		System.out.println(heap);
	}
}
```
