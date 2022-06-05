# Topic wise Concepts
1. String [Link](#string)
2. Array [Link](#array)
3. Collection [Link](#collection)

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

## Loops  
1. #### Loop over hashmap  
        Map<String,String> gfg = new HashMap<String,String>();
        for (String name : gfg.keySet()) 
        {
            String url = gfg.get(name);
            System.out.println("Key = " + name + ", Value = " + url);
        }
   
