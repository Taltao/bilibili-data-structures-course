## C语言单链表代码

```c
/*
* 作者：程序员小涛
* C语言单链表代码
* b站配套视频教程 https://www.bilibili.com/video/BV1dT4y1F7a5/
* C语言版本C99，如果编译器版本过低会无法编译
* 在CodeBlocks和VS2019上测试过
* 看代码遇到问题可以加QQ: 1497161158
*/

#include <stdio.h>
#include <stdlib.h> // 导入库用于调用malloc函数动态分配内存

// 定义链表节点
typedef struct Node
{
  int value;
  struct Node* next;
} Node;

// 从头遍历链表依次展示链表节点
void display_linklist(Node* head)
{
  // 如果链表的头节点是NULL，则链表为空，跳过后面的代码
  if (head == NULL) {
    printf("链表为空\n");
    return;
  }
  // 定义一个临时变量指向链表的头指针，防止改掉链表头指针后无法访问链表
  Node* temp = head;
  int i = 1;
  // 如果链表走到结尾以后，链表节点的next指针指向的是NULL，也就是我们初始化的时候设置的NULL
  // 如果temp == NULL那就意味着链表到末尾了，循环结束
  while (temp != NULL)
  {
    // 输出链表节点的value值
    printf("链表的第%d个元素的值是:%d\n", i, temp->value);
    i++;
    temp = temp->next; // 链表向后走一位
  }
  printf("\n");
}

// 向链表中间插入元素，index是插入到第几个节点后，value是插入元素的值
void insert(Node* head, int index, int value)
{
  Node* temp = head;
  for (int i = 0; i < index - 1; i++)
  {
    temp = temp->next; // 移动链表到要操作的元素那里
  }
  // 创建新的链表节点
  Node* node = (Node*)malloc(sizeof(Node));
  node->value = value;
  node->next = temp->next; // 将新节点的next指向当前节点的后继节点
  temp->next = node; // 将当前节点的next指针指向新添加的元素
}

// 创建一个有10个元素的链表，因为要在函数内操作头指针，所以用了二级指针，不然无法保留指针指向的改变
void init_linklist(Node** head)
{
  // 定义一个新节点用于指向链表当前的元素，防止改变头指针指向的位置
  Node* temp = *head;
  for (int i = 0; i < 10; i++)  // 创建10个链表节点元素
  {
    //给链表节点元素动态分配内存，sizeof(Node)用于计算Node结构体占用的内存字节大小
    Node* node = (Node*)malloc(sizeof(Node));
    // 为简化操作不考虑内存分配失败的情况
    node->value = i + 1; // 给链表节点元素赋值，为了简单，给它赋值为当前的循环次数，这个可以是任意值
    node->next = NULL; // 给链表节点元素的next指针初始化
    if (*head == NULL)  // 判断当前赋值的链表节点是不是第一个，如果是第一个要特殊处理
    {
      *head = node; // 链表开头元素用来记录链表的起始地址
      // 我们用一个新的变量指向链表的头指针
      // 防止之后的赋值操作把链表头指针改了就找不到链表开头的位置
      // 自然也无法访问到链表了
      temp = *head;
    }
    else
    {
      // 如果当前不是第一个元素，那就把当前元素指针的next指向新创建的链表节点
      temp->next = node;
      // 然后把当前的元素指针向后移动一位
      temp = temp->next;
    }
  }
}

// 删除指定位置链表的元素
void delete_node(Node** head, int index)
{
  Node* temp = *head;
  // 如果删除的是链表的头节点
  // 直接将链表的头节点修改为头节点的后继节点即可
  if (index == 0) {
    *head = (*head)->next;
    free(temp); // 释放链表的第一个节点的内存
    return; // 跳过后面的代码
  }
  // 如果不是删除第一个节点，那就将链表移动到指定的位置再删除
  for (int i = 0; i < index - 1; i++)
  {
    temp = temp->next; // 移动链表到指定位置
  }
  // 将当前节点的next指针指向删除元素的后继节点
  // temp->next是要删除的节点
  // next_node->next是要删除的节点后面的节点
  Node* next_node = temp->next;
  temp->next = next_node->next;
  free(next_node);
}

// 清空链表并释放内存
void delete_linklist(Node** head)
{
  Node* temp = *head;
  Node* next_node;
  while (temp != NULL)
  {
    // 用一个变量存储要删除元素的后继节点
    // 因为不存储的话一旦删除该节点后就找不到后继节点了
    next_node = temp->next;
    free(temp); // 释放当前节点的内存
    temp = next_node; // 将链表向后移动一位
  }
  // 将头指针重置为NULL，表示链表为空
  // 因为涉及在函数里修改指针的指向，所以用了二级指针
  *head = NULL;
}


int main()
{
  Node* head = NULL; //定义链表的头节点
  init_linklist(&head); // 初始化链表

  printf("展示初始化后的链表元素\n");
  display_linklist(head); // 展示初始化后的链表元素

  insert(head, 3, 999); // 向第三个节点后插入一个值为999的节点
  printf("展示插入后的结果\n");
  display_linklist(head); // 展示插入后的结果

  delete_node(&head, 0); // 删除第一个链表节点
  printf("展示删除指定位置节点后的结果\n");
  display_linklist(head); // 展示删除指定位置节点后的结果

  delete_linklist(&head); // 删除链表
  printf("展示删除链表后的结果\n");
  display_linklist(head); // 展示删除链表后的结果

  return 0;
}


```