# Java语言生成算数表达式二叉树


<!--more-->


几个月前数据结构的课设，抽到了一个比较简单的题目。想着用java语言来实现一下（学的时候是C语言版的数据结构）。毕竟在java项目中，万物皆可对象化，一个类型的数据创建为一个对象，其他函数可以来调用对象实例化是非常香的，所以我打算用java语言来完成本次课设。Java语言来写一个数据结构项目是很吃香的，毕竟java作为一个成熟的高级语言，它里面封装好了许多可以直接用的非常简便的函数，甚至还有封装好的数据结构比如“栈”，在java里我只需要Stack<node> stack = new Stack<node>;就可以直接生成的一个栈。
		没想到网上找不到任何能参考的代码。java语言没有指针把我给难倒了，毕竟我所学的数据结构大量用到了指针功能，最终一个多星期还是写出来了，至于指针我调用了大量的递归，导致代码的可读性和执行效率降了一大截。后来了解了可以用引用类来替代指针（属实是当时java没学好）。为了让老师能看懂代码做了流程图来解释代码。
		![在这里插入图片描述](https://img-blog.csdnimg.cn/07d6b1bb182543b5ae079ac694377973.png)
具体代码如下

 - 先创建目录tree
 - 创建一个名为Node的java.class文件来建立叶子结点的类，里面有三个成员变量，数据域、左孩子、右孩子。并自动生成了所有成员变量的set和get函数。
Node.java
```go
package tree;

public class Node {
    Object data = null;//数据
    Node left;//左节点
    Node right = null;//右节点

    public Node(){
    }

    public Node(Object data) {
        super();
        this.data = data;
    }

    public Object getData() {
        return data;
    }
    
    public void setData(Object data) {
        this.data = data;
    }
    
    public Node getLeft() {
        return left;
    }
    
    public void setLeft(Node left) {
        this.left = left;
    }
    
    public Node getRight() {
        return right;
    }
    
    public void setRight(Node right) {
        this.right = right;
    }
    
}
```

 - 创建二叉树的类，在里面把各种操作都写成了成员方法。
towtree.java
```java
package tree;

import javax.script.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;
import java.util.Stack;

public class towtree<E> {
    private Node root;// 根节点
    private String result = "";// 储存后缀表达式
    Stack<Node> list = new Stack<Node>();// 生成树栈
    Stack<Double>stack = new Stack<Double>();// 计算结果栈

    /*
    * 生成二叉树函数
    * */

    public void add(String str) {
        String[] sarray = str.split("");
        Node node = new Node();
        if (sarray.length == 1) {
            String key = sarray[0];
            root = new Node();
            root.setData(key);
        }
        else {
//            debugcreattree(0,sarray,node);
            creattree(0,sarray,node);
        }

    }

    // 让根节点等于最后一个节点
    public void creattree(int i,String[] str,Node node) {
        String key = str[i];
        char ch = key.charAt(0);
        if ((ch >= 'A' && ch <= 'Z')||(ch >= 'a' && ch <= 'z')||(ch >= '0' && ch <= '9')) {// 如果是字母或数字
            if (node.getData() == null) {                                                  // 进入判断字母或数字语句
                node.setData(key);                                                         // 进入赋值语句
                Node point = list.pop();                                                   // 进入出栈语句
                if (point.getRight() == null ) {                                           // 进入右子树不存在并初始化右子树语句
                    Node node1 = new Node();
                    point.setRight(node1);
                    list.push(point);                                                      // 进入入栈语句
                    int j = i + 1;
                    if (j == str.length)
                        return;
                    creattree(j,str,point.getRight());
                }
                else {                                                                      // 进入右子树存在出栈语句
                    list.push(point);                                                       // 进入入栈语句
                    while (true) {
                        Node pop = list.pop();                                              // pop进入出栈语句
                        if (list.isEmpty())break;
                        Node top = list.pop();                                              // top进入出栈语句
                        if (top.getRight() == null){                                        // 进入右子树为空语句
                            int j = i + 1;
                            if (j == str.length) return;
                            if (top.getLeft().getData() == null ) {                         // 顶栈左子树未赋值并赋值pop语句
                                top.setLeft(pop);
                                list.push(top);                                             // top进入入栈语句
                                creattree(j,str,top.getLeft());
                                break;
                            }
                            creattree(j,str,top.getRight());
                            break;
                        }
                        else if (top.getRight().getData() == null) {
                            if (top.getLeft().getData() == null ) {                         // 进入顶栈左子树未赋值并赋值pop语句
                                top.setLeft(pop);
                                list.push(top);                                             // top进入入栈语句
                                int j = i + 1;
                                if (j == str.length) return;
                                creattree(j,str,top.getRight());
                                break;
                            }else {                                                         // 进入顶栈右子树存在未赋值并赋值pop语句
                                top.setRight(pop);
                                list.push(top);                                             // top进入入栈语句
                                }
                            if (list.isEmpty())break;
                            }
                        }
                    }
                }
        } else {                                                                            // 如果是运算符
            if (i == 0) {
                node.setData(key);
                root = node;
                Node node1 = new Node();
                node.setLeft(node1);
                list.push(node);                                                            // 进入入栈语句
                creattree(1,str,node.getLeft());
            } else {
                Node point = list.pop();                                                    // 进入出栈语句
                if (point.getRight() == null ) {
                    Node node1 = new Node();
                    point.setRight(node1);
                    list.push(point);                                                       // 进入入栈语句
                }else {
                    list.push(point);                                                       // 进入入栈语句
                }
                Node node1 = new Node();
                node1.setData(key);
                int j = i + 1;
                if (j == str.length) return;
                Node node2 = new Node();
                node1.setLeft(node2);
                list.push(node1);                                                           // 进入入栈语句
                creattree(j,str,node1.getLeft());
            }
        }
    }

    /*
    * 调试生成二叉树函数
    * */
    public void debugcreattree(int i,String[] str,Node node) {
        String key = str[i];
        char ch = key.charAt(0);
        if ((ch >= 'A' && ch <= 'Z')||(ch >= 'a' && ch <= 'z')||(ch >= '0' && ch <= '9')) {
            if (node.getData() == null) {
                node.setData(key);
                System.out.println(key+"进入赋值语句");
                Node point = list.pop();
                System.out.println(point.getData().toString()+"进入出栈语句");
                if (point.getRight() == null ) {
                    System.out.println(point.getData().toString()+"进入右子树不存在并初始化右子树语句");
                    Node node1 = new Node();
                    point.setRight(node1);
                    list.push(point);
                    System.out.println(point.getData().toString()+"进入入栈语句");
                    int j = i + 1;
                    if (j == str.length)
                        return;
                    debugcreattree(j,str,point.getRight());
                }
                else {
                    list.push(point);
                    while (true) {
                        Node pop = list.pop();
                        System.out.println(pop.getData().toString()+"出栈");
                        if (list.isEmpty())break;
                        Node top = list.pop();
                        System.out.println(top.getData().toString()+"出栈");
                        System.out.println("pop="+pop.getData().toString()+"top="+top.getData().toString());
                        System.out.println("top.getRight = " + top.getRight());
                        if (top.getRight() == null){
                            System.out.println(top.getData().toString()+"进入右子树不存在并初始化右子树语句");
                            int j = i + 1;
                            if (j == str.length) return;
                            if (top.getLeft().getData() == null ) {
                                System.out.println(key+"进入顶栈左子树未赋值并赋值pop语句");
                                top.setLeft(pop);
                                list.push(top);
                                System.out.println(top.getData().toString()+"入栈");
                                debugcreattree(j,str,top.getLeft());
                                break;
                            }
                            debugcreattree(j,str,top.getRight());
                            break;
                        }
                        else if (top.getRight().getData() == null) {
                            if (top.getLeft().getData() == null ) {
                                System.out.println(key+"进入顶栈左子树未赋值并赋值pop语句");
                                top.setLeft(pop);
                                list.push(top);
                                System.out.println(top.getData().toString()+"入栈");
                                int j = i + 1;
                                if (j == str.length) return;
                                debugcreattree(j,str,top.getRight());
                                break;
                            }else {
                                System.out.println(key+"进入顶栈右子树存在未赋值并赋值pop语句");
                                top.setRight(pop);
                                list.push(top);
                                System.out.println(top.getData().toString()+"入栈");
                            }
                            if (list.isEmpty())break;
                        }
                    }
                }
            }
        } else {//如果是运算符
            if (i == 0) {
                node.setData(key);
                root = node;
                Node node1 = new Node();
                node.setLeft(node1);
                list.push(node);
                System.out.println(key+"进入入栈语句");
                debugcreattree(1,str,node.getLeft());
            } else {
                Node point = list.pop();
                System.out.println(point.getData().toString()+"进入出栈语句");
                if (point.getRight() == null ) {
                    System.out.println(point.getData().toString()+"进入右子树不存在并初始化右子树语句");
                    Node node1 = new Node();
                    point.setRight(node1);
                    list.push(point);
                    System.out.println(point.getData().toString()+"进入入栈语句");
                }else {
                    list.push(point);
                    System.out.println(point.getData().toString()+"进入入栈语句");
                }
                Node node1 = new Node();
                node1.setData(key);
                list.push(node1);
                System.out.println(key+"进入入栈语句");
                int j = i + 1;
                if (j == str.length) return;
                Node node2 = new Node();
                node1.setLeft(node2);
                debugcreattree(j,str,node1.getLeft());
            }
        }
    }

    /**
     * 递归   中序遍历
     */
    public void output() {
        if (root == null)
        System.out.println("请先执行步骤1");
        else output(root);
    }
    
    public void output(Node node) {

        if (node.getLeft() != null) {
            System.out.print("(");
            output(node.getLeft());
        }
        System.out.print(node.getData());
        if (node.getRight() != null) {
            output(node.getRight());
            System.out.print(")");
        }
    }

    /**
    * 查找元素并赋值
    * */
    public void assign(String v,String c) {
        if(root == null)
            System.out.println("请先执行步骤1");
        else {
            assign(v,c,root);
            System.out.println("赋值后表达式为：");
            output(root);
            System.out.println();
        }
    }
    
    public void assign(String v,String c,Node node) {// 中序遍历二叉树遇到所寻字母后进行赋值
        if (node.getLeft() != null) {
            assign(v,c,node.getLeft());
        }
        if(node.getData().toString().equals(v)) {
            node.setData(c);
            return;
        }
        if (node.getRight() != null) {
            assign(v,c,node.getRight());
        }
    }

    /**
    * 返回赋值后的后序表达式并计算
    * */
    public void backtoresult(){
        backtoresult(root);
        if (result == "")
            System.out.println("请先执行步骤1");
        else {
            for (int i = 0; i < result.length(); i++) {
                char key = result.charAt(i);
                if (key >= '0' && key <= '9')// 如果是数字就入栈
                    stack.push(Double.parseDouble(String.valueOf(key)));
                else {
                    if (stack.size() > 1 ) {// 如果是运算符就取两个顶栈元素进行计算并把结果入栈
                        Double pop = stack.pop();
                        Double top = stack.pop();// 注意这里左边是top,右边是pop如-91=>91-左边是9-右边1
                        Double s =  jisuan(top,pop,String.valueOf(key));
                        stack.push(s);
                    }
                }
            }
            Double jisuan = stack.pop();
            System.out.println(jisuan);
            result = "";
        }
    }

    /**
    * 构造出后缀表达式result
    * */
    public void backtoresult(Node node) {
        if (node.getLeft() != null) {
            backtoresult(node.getLeft());
        }
        if (node.getRight() != null) {
            backtoresult(node.getRight());
        }
        String key = node.getData().toString();
        char ch = key.charAt(0);
        if ((ch >= 'A' && ch <= 'Z')||(ch >= 'a' && ch <= 'z')){
            result += '0';
        } else result += node.getData().toString();
    }

    /**
    * 计算函数
    * */
    public double jisuan(double a, double b, String f) {
        switch (f) {
            case "*":
                return a * b;
            case "/":
                if(b==0){
                    System.out.println("除数为0异常返回计算值0，计算值可能出现错误");
                    return 0;
                }
                else return a / b;
            case "+":
                return a + b;
            case "-":
                return a - b;
            case "^": {
                double sum = 1;
                for (int i = 0; i < b; i++) {
                    sum *= a;
                }
                return sum;
            }
        }
        return 0;
    }

    /**
    * 拼接表达式函数
    * */
    public void compound (String p,String e1,String e2){
        Node newroot = new Node();
        newroot.setData(p);
        Node left;
        Node right;
        Node node1 = new Node();
        Node node2 = new Node();
        String[] sarray1 = e1.split("");
        String[] sarray2 = e2.split("");
        root = null;
        creattree(0,sarray1,node1);
        left = root;
        newroot.setLeft(left);
        root = null;
        creattree(0,sarray2,node2);
        right = root;
        newroot.setRight(right);
        output(newroot);
    }
}
```

 - 建一个主类来实现菜单页面，并实例化二叉树类，在主方法里调用二叉树类的成员方法来实现各个基本功能。
twochatree.java

```java
package tree;
import java.util.InputMismatchException;
import java.util.Scanner;

public class towchatree{
    private static towtree tt = new towtree();
    private static int chance = 1;
    private String E;
    private String result = "";

    public static void ReadExpre(String exp) {
        tt.add(exp);
    }

    public static void WriteExpre() {
        tt.output();;
        System.out.println("");
    }

    public static void Assign(String v,String c) {
        tt.assign(v,c);
    }

    public static void Value() {
        tt.backtoresult();
    }

    public static void CompoundExpr(String p,String e1,String e2) {
        tt.compound(p, e1, e2);
    }

    public static void main(String[] args) {
        String expression = "";
        for (int i = 0; i < 1000; i++) {
            System.out.println("算术表达式与二叉树程序 请选择你要进行的操作");
            System.out.println("以字符序列的形式输入语法正确的前缀表达式并构造表达式----1");
            System.out.println("用带括弧的中缀表达式输出表达式----------------------2");
            System.out.println("对某一变量的赋值，变量的初值为0---------------------3");
            System.out.println("对算术表达式求值----------------------------------4");
            System.out.println("构造一个新的复合表达式-----------------------------5");

            while (true) {
                try {
                    Scanner input = new Scanner(System.in);
                    chance = input.nextInt();
                    if (chance > 0 && chance <=5) break;
                    else System.out.println("输入超过系统范围，请重新输入！");
                } catch (InputMismatchException e) {
                    System.out.println("非法输入，请重新输入！");
                }
            }
            switch (chance) {
                case 1:
                    System.out.println("输入语法正确的前缀表达式");
                    Scanner expre = new Scanner(System.in);
                    expression = expre.next();
                    ReadExpre(expression);
                    break;
                case 2:
                    WriteExpre();
                    break;
                case 3:
                    System.out.println("请输入要赋值的元素和要赋给的值，未赋值的元素默认值为0");
                    Scanner scanner = new Scanner(System.in);
                    String V = scanner.next();
                    String c = scanner.next();
                    Assign(V, c);
                    break;
                case 4:
                    Value();
                    break;
                case 5:
                    System.out.println("请依次输入运算符，表达式1，表达式2");
                    Scanner com = new Scanner(System.in);
                    String p = com.next();
                    String e1 = com.next();
                    String e2 = com.next();
                    CompoundExpr(p, e1, e2);
                    break;
            }
            System.out.println("请选择下一步操作");
        }
    }
}

```
勉强用java实现了前缀算数表达式生成二叉树，写的不好望各位见谅。

