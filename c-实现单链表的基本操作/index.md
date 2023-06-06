# C++实现单链表的基本操作


<!--more-->

### 单链表节点结构体

```cpp
struct ListNode {
      int val;										//数据域
      ListNode *next;								//指针域
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };
```
 **链表遍历输出（方便测试）**

```cpp
void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {
        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}
```
### 一、头结点插入法创建单链表

```cpp
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {//输入666结束链表的创建

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}
```
#### 头插法测试代码

```cpp
#include<iostream>	//strin and stdout 的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {
        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}

int main() {
    ListNode* head = List_Head_insert(); //倒序插入不推荐
    Print_LinkNode(head);
}
```
**输入**

```cpp
1
2
3
4
5
666
```
**输出**

```cpp
5 -> 4 -> 3 -> 2 -> 1
```
### 二、尾结点插入法创建单链表

```cpp
ListNode* List_Tail_insert() {
    ListNode* head = new ListNode();
    ListNode* p = head;
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        p -> next = newnode;
        p = p -> next;  // p = newnode;

        cin >> x;
    }
    return head;
}
```
#### 尾插法测试代码 
```cpp
#include<iostream>	//strin and stdout 的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 尾结点插入法
ListNode* List_Tail_insert() {
    ListNode* head = new ListNode();
    ListNode* p = head;
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        p -> next = newnode;
        p = p -> next;  // p = newnode;

        cin >> x;
    }
    return head;
}

void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {
        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}

int main() {
    ListNode* head = List_Tail_insert();//正序插入
    Print_LinkNode(head);
}
```
**输入**

```cpp
1
2
3
4
5
666
```
**输出**

```cpp
1 -> 2 -> 3 -> 4 -> 5
```
### 三、按序号查找

```cpp
//按序号查找(输入有序号一定不要忘记非法输入检验与序号越界检验！！！)
ListNode* Selectbyindex(int index,ListNode* head) {
    if(index < 1)return nullptr;    //非法输入检查不能忘！
    ListNode* p = head -> next;     //尽量不要改变输入的指针域
                                    //有头结点！！要从第一个元素开始head -> next！！！
    int i = 1;
    while(p != nullptr && i < index) {  //防止index超过链表长度要进行非空检验!

        p = p -> next;
        i++;

    }
    return p;
}
```
#### 按序号查找测试代码

```cpp
#include<iostream>	//strin and stdout 的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

//按序号查找(输入有序号一定不要忘记非法输入与序号越界检验！！！)
ListNode* Selectbyindex(int index,ListNode* head) {
    if(index < 1)return nullptr;    //非法输入检查不能忘！
    ListNode* p = head -> next;     //尽量不要改变输入的指针域
                                    //有头结点！！要从第一个元素开始head -> next！！！
    int i = 1;
    while(p != nullptr && i < index) {  //防止index超过链表长度要进行非空检验!

        p = p -> next;
        i++;

    }
    return p;
}


int main() {
	int index;
	ListNode* head = List_Tail_insert();//正序插入
    cout << "请输入要查询的节点序号" <<endl;
    cin >> index;
    ListNode* res = Selectbyindex(index,head);
    cout << res -> val;
}
```
**输入**

```cpp
1
5
3
6
7
666
2
```
**输出**

```cpp
5
```
### 四、按值查找
```cpp
//按值查找
ListNode* Selectbyvalue(int value,ListNode* head) {
    ListNode* p = head -> next;
    while(p != nullptr && p -> val != value) { //每次都能取出val值，不需要再加中间变量保存每一次的val了！！

        p = p -> next;

    }
    return p;
}
```
#### 按值查找测试代码

```cpp
#include<iostream>	//strin and stdout 的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

//按值查找
ListNode* Selectbyvalue(int value,ListNode* head) {
    ListNode* p = head -> next;
    while(p != nullptr && p -> val != value) { //每次都能取出val值，不需要再加中间变量保存每一次的val了！！

        p = p -> next;

    }
    return p;
}

int main() {
    int value;
	ListNode* head = List_Tail_insert();//正序插入
    cout << "请输入要查询的节点的值" <<endl;
    cin >> value;
    ListNode* res = Selectbyvalue(value,head);
    cout << res -> val;
}
```
**输入**

```cpp
1
5
3
6
7
666
5
```
**输出**

```cpp
5
```
### 五、按序号插入

```cpp
//按序号插入
void Insertbyindex(int index,ListNode* head,ListNode* node) {
    if(index < 1)return;
    //采用后插法
    ListNode* p = head;//若插入第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        if(i == index - 1){
        
            //通常采用的是后插法，需要找到插入位置的前驱节点来后插
            //前插法与之相反，但可以将对节点的前插操作转化为后插操作，即找到节点的前节点后插。
            node -> next = p -> next;
            p -> next = node;

        }

        //另一种后插实现前插的方法是，将node插入p的后面，再交换node和p的val。
        //if(i == index) {
        
            //node -> next = p -> next;
            //p -> next = node;
            //int temp = node -> val;
            //node -> val = p -> val;
            //p -> val = temp;

        //}

        p = p -> next;
        i++;
    }
}
```
#### 按序号插入测试代码

```cpp
#include<iostream>	//strin and stdout 的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

//按序号插入
void Insertbyindex(int index,ListNode* head,ListNode* node) {
    if(index < 1)return;
    //采用后插法
    ListNode* p = head;//若插入第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        if(i == index - 1){

            //通常采用的是后插法，需要找到插入位置的前驱节点来后插
            //前插法与之相反，但可以将对节点的前插操作转化为后插操作，即找到节点的前节点后插。
            node -> next = p -> next;
            p -> next = node;

        }

        //另一种后插实现前插的方法是，将node插入p的后面，再交换node和p的val。
        //if(i == index) {

            //node -> next = p -> next;
            //p -> next = node;
            //int temp = node -> val;
            //node -> val = p -> val;
            //p -> val = temp;

        //}

        p = p -> next;
        i++;
    }
}

void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {
        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}

int main() {
    int index;
	ListNode* head = List_Tail_insert();//正序插入
    cout << "请输入要插入的节点序号" <<endl;
    cin >> index;
    ListNode* node = new ListNode(4);
    Insertbyindex(index,head,node);
    Print_LinkNode(head);
}
```
**输入**

```cpp
1
2
3
5
666
4
```
**输出**

```cpp
1 -> 2 -> 3 -> 4 -> 5
```
### 	六、按序号删除

```cpp
void Deletebyindex(int index,ListNode* head) {
    if(index < 1)return;
    //找到其前驱节点
    ListNode* p = head;//若删除第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        找到前驱节点删除改节点
        if(i == index - 1) {

            ListNode* delnode = p -> next;
            p -> next = delnode -> next;
            free(delnode);//c++没有垃圾收集机制，要手动释放不用的堆内存，否则容易内存泄漏！

        }

        //另一种方法是找到该节点，将该节点值与后驱节点值替换，删除后驱节点。
        //if(i == index) {

            //ListNode* afternode = p -> next;
            //p -> next = afternode -> next;
            //p -> val = afternode -> val;
            //free(afternode);

        //}
        
        p = p -> next;
        i++;
    }
}
```
#### 	按序号删除测试代码

```cpp
#include<iostream>	//strin and stdout 的头文件
#include<cstdlib>   //free的头文件
using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };
  
// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

void Deletebyindex(int index,ListNode* head) {
    if(index < 1)return;
    //找到其前驱节点
    ListNode* p = head;//若删除第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        找到前驱节点删除改节点
        if(i == index - 1) {

            ListNode* delnode = p -> next;
            p -> next = delnode -> next;
            free(delnode);//c++没有垃圾收集机制，要手动释放不用的堆内存，否则容易内存泄漏！

        }

        //另一种方法是找到该节点，将该节点值与后驱节点值替换，删除后驱节点。
        //if(i == index) {

            //ListNode* afternode = p -> next;
            //p -> next = afternode -> next;
            //p -> val = afternode -> val;
            //free(afternode);

        //}
        
        p = p -> next;
        i++;
    }
}
void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {
        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}

int main() {
    int index;
	ListNode* head = List_Tail_insert();//正序插入
    cout << "请输入要删除的节点序号" <<endl;
    cin >> index;
    ListNode* node = new ListNode(4);
    Deletebyindex(index,head);
    Print_LinkNode(head);
} 	
```
**输入**

```cpp
1
2
3
4
5
666
3
```
**输出**

```cpp
1 -> 2 -> 4 -> 5
```
### 汇总代码

```cpp
#include<iostream>	//strin and stdout 的头文件
#include<cstdlib>   //free的头文件

using namespace std;

struct ListNode {
      int val;
      ListNode *next;
      ListNode() : val(0), next(nullptr) {}         //节点默认构造方法
      ListNode(int x) : val(x), next(nullptr) {}    //节点初始化val的构造方法
  };

// 头结点插入法
ListNode* List_Head_insert() {
    ListNode* head = new ListNode();
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        newnode -> next = head -> next;
        head -> next = newnode;

        cin >> x;
    }
    return head;
}

// 尾结点插入法
ListNode* List_Tail_insert() {
    ListNode* head = new ListNode();
    ListNode* p = head;
    int x;
    cin >> x;
    while(x != 666) {

        ListNode* newnode = new ListNode(x);
        p -> next = newnode;
        p = p -> next;  // p = newnode;

        cin >> x;
    }
    return head;
}

//遍历链表输出
void Print_LinkNode(ListNode* head) {
    ListNode* p = head;
    while(p != nullptr) {

        p = p -> next;
        cout << p -> val;
        if(p -> next != nullptr)
            cout  << " -> ";
    }
}

//按序号查找(输入有序号一定不要忘记非法输入与序号越界检验！！！)
ListNode* Selectbyindex(int index,ListNode* head) {
    if(index < 1)return nullptr;    //非法输入检查不能忘！
    ListNode* p = head -> next;     //尽量不要改变输入的指针域
                                    //有头结点！！要从第一个元素开始head -> next！！！
    int i = 1;
    while(p != nullptr && i < index) {  //防止index超过链表长度要进行非空检验!

        p = p -> next;
        i++;

    }
    return p;
}

//按值查找
ListNode* Selectbyvalue(int value,ListNode* head) {
    ListNode* p = head -> next;
    while(p != nullptr && p -> val != value) { //每次都能取出val值，不需要再加中间变量保存每一次的val了！！

        p = p -> next;

    }
    return p;
}

//按序号插入
void Insertbyindex(int index,ListNode* head,ListNode* node) {
    if(index < 1)return;
    //采用后插法
    ListNode* p = head;//若插入第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        if(i == index - 1){

            //通常采用的是后插法，需要找到插入位置的前驱节点来后插
            //前插法与之相反，但可以将对节点的前插操作转化为后插操作，即找到节点的前节点后插。
            node -> next = p -> next;
            p -> next = node;

        }

        //另一种后插实现前插的方法是，将node插入p的后面，再交换node和p的val。
        //if(i == index) {

            //node -> next = p -> next;
            //p -> next = node;
            //int temp = node -> val;
            //node -> val = p -> val;
            //p -> val = temp;

        //}

        p = p -> next;
        i++;
    }
}

//按序号删除
void Deletebyindex(int index,ListNode* head) {
    if(index < 1)return;
    //找到其前驱节点
    ListNode* p = head;//若删除第一个序号的位置，为了保证其有前驱节点要从head开始而不是head -> next
    int i = 0;//相应的i改成0开始而不是1
    while(p != nullptr) {
        找到前驱节点删除改节点
        if(i == index - 1) {

            ListNode* delnode = p -> next;
            p -> next = delnode -> next;
            free(delnode);//c++没有垃圾收集机制，要手动释放不用的堆内存，否则容易内存泄漏！

        }

        //另一种方法是找到该节点，将该节点值与后驱节点值替换，删除后驱节点。
        //if(i == index) {

            //ListNode* afternode = p -> next;
            //p -> next = afternode -> next;
            //p -> val = afternode -> val;
            //free(afternode);

        //}
        
        p = p -> next;
        i++;
    }
}

int main() {
    int index,value;
    //ListNode* head = List_Head_insert(); //倒序插入不推荐
    //Print_LinkNode(head);

    ListNode* head = List_Tail_insert();//正序插入
    //Print_LinkNode(head);

    //cout << "请输入要查询的节点序号" <<endl;
    //cin >> index;
    //ListNode* res = Selectbyindex(index,head);
    //cout << res -> val;

    //cout << "请输入要查询的节点的值" <<endl;
    //cin >> value;
    //ListNode* res = Selectbyvalue(value,head);
    //cout << res -> val;

    //cout << "请输入要插入的节点序号" <<endl;
    //cin >> index;
    //ListNode* node = new ListNode(4);
    //Insertbyindex(index,head,node);
    //Print_LinkNode(head);

    cout << "请输入要删除的节点序号" <<endl;
    cin >> index;
    ListNode* node = new ListNode(4);
    Deletebyindex(index,head);
    Print_LinkNode(head);
}



```

