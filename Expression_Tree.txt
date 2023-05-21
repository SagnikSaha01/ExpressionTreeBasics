ENode.java:
----------
public class ENode {
    char data;
    int root;
   ENode right = null;
   ENode left = null;
  // int depth;
   public ENode(char d){
      // depth = dep;
       data = d;
   }
    
    
    
    
    
}

ExpressionTree.java:
-------------------
public class ExpressionTree {
   public ENode start;
   public ExpressionTree(char d){
       start = new ENode(d);
       if(d == 'f' || d == 'n'){
          
           start.right = new ENode('0');
       }
   }
   public void add(char d){
      
       start = recursiveAdd(start,d);
   }
   public ENode recursiveAdd(ENode current, char d){
     
       if(current == null){
           
           current = new ENode(d);
           if(d == 'f' || d == 'n'){
              
               current.right = new ENode('0');
               
           }else if(d != '+' || d != '-' || d != '*' || d != '%' || d != 'h' || d != 'n' || d != 'f'){
              
               current.root = Character.getNumericValue(d);
              
           
           }
           return current;
       }
       if(d == '+' || d == '-' || d == '*' || d == '%' || d == 'h' || d == 'f' || d == 'n'){
            if(current.left == null){
                current.left = recursiveAdd(current.left,d);
            }else if(current.left.data == '+' || current.left.data == '-' || current.left.data == '*' || current.left.data == '%' || current.left.data == 'h' || current.left.data == 'n' || current.left.data == 'f' ){
                if(checkEmpty(current.left) == true){
                    current.left = recursiveAdd(current.left,d);
                }else{
                    current.right = recursiveAdd(current.right,d);
                }
            }else if(current.right == null){
                current.right = recursiveAdd(current.right,d);
            }else if(current.right.data == '+' || current.right.data == '-' || current.right.data == '*' || current.right.data == '%' || current.right.data == 'h' || current.right.data == 'n' || current.right.data == 'f'){
                if(checkEmpty(current.right) == true){
                    current.right = recursiveAdd(current.right,d);
                }else{
                    current.right = recursiveAdd(current.right,d);
                }
            }
       
       }else{
             if(current.left == null){
                   current.left = recursiveAdd(current.left,d);
               }else if(current.left.data == '+' || current.left.data == '-' || current.left.data == '*'|| current.left.data == '%' || current.left.data == 'h'  || current.left.data == 'n' || current.left.data == 'f' ){
                  
                   if(checkEmpty(current.left) == true){
                       current.left = recursiveAdd(current.left,d);
                       
                   }else{
                       current.right = recursiveAdd(current.right,d);
                   }
                   
               }else if(current.right == null || (current.right.data == '+' || current.right.data == '-' || current.right.data == '*'|| current.right.data == '%' || current.right.data == 'h'|| current.right.data == 'n' || current.right.data == 'f')){
                   current.right = recursiveAdd(current.right,d);
               }
           
           
           
       }
   
       return current;
   }
   public boolean checkEmpty(ENode current){
       if(current.left == null || current.right == null){
        return true;  
       }else if(current.left.data == '+' || current.left.data == '-' || current.left.data == '*'|| current.left.data == '%' || current.left.data == 'h' || current.left.data == 'n' || current.right.data == 'f'  ){
            return checkEmpty(current.left);
       }else if( current.right.data == '+' || current.right.data == '-' || current.right.data == '*' || current.right.data == '%' || current.right.data == 'h'|| current.right.data == 'n' || current.right.data == 'f'){
           
           return checkEmpty(current.right);
       }else{
           return false;
       }
       
   }
   
   public void calc(){
       calculate(start);
       
   }
   
   public boolean calculate(ENode current){
      
       
       if(current.data == '+' || current.data == '-' || current.data == '*' || current.data == '%' || current.data == 'h' || current.data == 'n' || current.data == 'f'){
           if((current.left.data == '+' || current.left.data == '-' || current.left.data == '*' || current.left.data == '%' || current.left.data == 'h' || current.left.data == 'f' || current.left.data == 'n') && (current.right.data == '+' || current.right.data == '-' || current.right.data == '*' || current.right.data == 'h' || current.right.data == '%' || current.right.data == 'n' || current.right.data == 'f')){
           calculate(current.left);
           calculate(current.right);
           compute(current);
           return true;
           }else if(current.left.data == '+' || current.left.data == '-' || current.left.data == '*' || current.left.data == '%' || current.left.data == 'h' || current.left.data == 'f' || current.left.data == 'n'){
           
           
            calculate(current.left); 
            
         
            compute(current);
             //System.out.println(current.root);
            return true;
           }else if(current.right.data == '+' || current.right.data == '-' || current.right.data == '*' || current.right.data == 'h' || current.right.data == '%' || current.right.data == 'f' || current.right.data == 'n'){
            
            calculate(current.right);   
            
             compute(current);
             //System.out.println(current.root);
             return true;
               
           }else{
             
           compute(current);
           return true;
           }
        
       }
     
       return true;
   }
   
   public boolean compute(ENode current){
                
               if(current.data == '+'){
                   current.root = current.left.root + current.right.root;
               }else if(current.data == '-'){
                   current.root = current.left.root - current.right.root;
               }else if(current.data == '*'){
                   current.root = current.left.root * current.right.root;
               }else if(current.data == '%'){
                   current.root = current.left.root % current.right.root;
               }else if(current.data == 'h'){
                   current.root = (int)(Math.sqrt((current.left.root * current.left.root)+(current.right.root * current.right.root)));
               }else if(current.data == 'n'){
                   current.root = current.left.root * (-1);
               }else if(current.data == 'f'){
                   current.root = current.left.root;
                   for(int x = current.left.root-1; x > 0; x--){
                       current.root = current.root * x;
                   }
               }
       
       return true;
   }
   
   public void ans(){
       System.out.println("The answer is: "+start.root);
   }
   
   public void print(){
       prefixPrint(start);
   }
   public boolean prefixPrint(ENode current){
       if(current == null){
           return true;
       }
        System.out.print(current.data + " ");
        prefixPrint(current.left);
        prefixPrint(current.right);
        
        return true;
       
       
   }
}

MyProgram.java:
--------------
import java.util.Scanner;

public class MyProgram
{
    public static void main(String[] args)
    {
        
        System.out.println("Enter input");
        Scanner in = new Scanner (System.in);
        String input = in.nextLine();
        ExpressionTree tree = new ExpressionTree(input.charAt(0));
        for(int x = 1; x < input.length(); x++){
        
       tree.add(input.charAt(x));
        }
     tree.calc();
        
    // tree.print();
        
    tree.ans();
        //+*23+45 --> 15
        //+7-*528 --> 9
        //-*+4325 --> 9
        //+3*52 --> 13
        //-+95*23 --> 8
        //+*3-82-9++32*54 -->2
    }
}

