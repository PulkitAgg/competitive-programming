# Topic wise Concepts
1. String [Link](#string)
2. Array [Link](#array)
3. Collection [Link](#collection)
4. Queue / Deque [Link](#queue--deque)
5. LinkedHashSet [Link](#linkedhashset)

## String
1. #### Reverse of String    
        new StringBuilder(str).reverse().toString();  
   
2. #### Length of String
        str.length();  
        
        
## Array
1. #### Length of Array  
        arr.length;  
      
2. #### Clone of Array  
        arr.clone();  
        
## Collection  
![Image](https://www.scientecheasy.com/wp-content/uploads/2018/09/java-collection-hierarchy.png)

1. #### Add object  
        add(Object);  
        
2. #### Check object exist  
        contains(Object o);  
   
3. #### Check is collection empty  
        isEmpty();  
     
4. #### Remove item  
        remove(Object o);  
        
5. #### Size  
        size();  

6. #### Conver to Array  
        toArray();  

## Queue / Deque
Use `ArrayDeque` (faster than `LinkedList`, O(1) at both ends). Does not allow `null`.

1. #### Create Queue  
        Queue<Integer> q = new ArrayDeque<>();  
        Deque<Integer> dq = new ArrayDeque<>();   // double-ended  

2. #### Add to end  
        q.offer(x);          // returns false if it fails  
        dq.offerLast(x);  
        dq.offerFirst(x);    // add to front  

3. #### Look at front (no remove)  
        q.peek();            // null if empty  
        dq.peekFirst();  
        dq.peekLast();       // look at end  

4. #### Remove from front  
        q.poll();            // null if empty  
        dq.pollFirst();  
        dq.pollLast();       // remove from end  

5. #### Use as Stack  
        dq.push(x);          // add to front  
        dq.pop();            // remove from front  
        dq.peek();  

6. #### Empty / Size  
        q.isEmpty();  
        q.size();  

7. #### Loop over Queue  
        for (int x : q) {  
            System.out.println(x);  
        }  

8. #### Priority Queue (Heap)  
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();  
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());  
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);  // custom compare  

> Note: `offer/peek/poll` return null/false on failure; `add/element/remove` throw exceptions.

## LinkedHashSet
HashSet that keeps insertion order. add / remove / contains are O(1).

1. #### Create LinkedHashSet  
        Set<Integer> set = new LinkedHashSet<>();  

2. #### Add item  
        set.add(x);          // returns false if already present  

3. #### Check item exist  
        set.contains(x);  

4. #### Remove item  
        set.remove(x);  

5. #### Get first item (oldest inserted)  
        set.iterator().next();       // check !set.isEmpty() first  

6. #### Empty / Size  
        set.isEmpty();  
        set.size();  

7. #### Loop over LinkedHashSet (insertion order)  
        for (int x : set) {  
            System.out.println(x);  
        }  

> Related: `LinkedHashMap<K,V>` keeps insertion order for maps; `new LinkedHashMap<>(16, 0.75f, true)` keeps access order (LRU cache).

## Loops  
1. #### Loop over hashmap  
        Map<String,String> gfg = new HashMap<String,String>();
        for (String name : gfg.keySet()) 
        {
            String url = gfg.get(name);
            System.out.println("Key = " + name + ", Value = " + url);
        }
