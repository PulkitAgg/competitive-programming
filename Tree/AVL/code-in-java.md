```
public class AVLTree {
	
	private class Node {
		int val;
		int height;
		Node left;
		Node right;
		
		Node(int val) {
			this.val = val;
			this.height = 1;
		}
		
	}
	
	private Node root;

	public static void main(String[] args) {
		AVLTree tree = new AVLTree();
		tree.root = tree.insert(tree.root, 10);
        tree.root = tree.insert(tree.root, 20);
        tree.root = tree.insert(tree.root, 30);
        tree.root = tree.insert(tree.root, 40);
        tree.root = tree.insert(tree.root, 50);
        tree.root = tree.insert(tree.root, 25);
        tree.preOrderDisplay(tree.root);
	}
	
	private int height(Node node) {
		if(node == null) {
			return 0;
		}
		return node.height;
	}
	
	private Node leftRotate(Node x) {
		Node y = x.right;
		Node leftTree = y.left;
		
		// rotation
		y.left = x;
		x.right = leftTree;
		
		//update height
		x.height = Math.max(height(x.left), height(x.right)) + 1;
		y.height = Math.max(height(y.left), height(y.right)) + 1;
		
		// return new root
		return y;

	}
	
	
	private Node rightRotate(Node x) {
		Node y = x.left;
		Node rightTree = y.right;
		
		// rotation
		y.right = x;
		x.left = rightTree;
		
		// update height
		x.height = Math.max(height(x.left), height(x.right)) + 1;
		y.height = Math.max(height(y.left), height(y.right)) + 1;
		
		// new root
		return y;
	}
	
	private int getBalanceFactor(Node node) {
		return height(node.left) - height(node.right);
	}
	
	public void insert(int val) {
		this.root = insert(this.root, val);
	}
	
	private Node insert(Node node, int val) {
		if(node == null) {
			// node is not initialized
			return new Node(val);
		}
		
		if(val < node.val) {
			node.left = insert(node.left, val);
		} else if(val >= node.val) {
			node.right = insert(node.right, val);
		} 
		
		node.height = 1 + Math.max(height(node.left), height(node.right));
		
		int bf = getBalanceFactor(node);
		
		
		if(bf > 1 && val < node.left.val) {
			// Left Left Case
			return rightRotate(node);
		}
		
		if(bf > 1 && val > node.left.val) {
			// Left Right Case
			node.left = leftRotate(node.left);
			return rightRotate(node);
		}
		
		if(bf < -1 && val < node.right.val) {
			// Right Left Case
			node.right = rightRotate(node.right);
			return leftRotate(node);
		}
	
		if(bf < -1 && val > node.right.val) {
			// Right Right Case
			return leftRotate(node);
		}
		
		return node;
	}
	
	private void preOrderDisplay(Node node) {
		if(node != null) {
			System.out.print(node.val + " ");
			 preOrderDisplay(node.left);
			 preOrderDisplay(node.right);
		}
	}
	
	
}
```
