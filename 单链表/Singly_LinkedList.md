<<<<<<< HEAD
# <center><font size = 50>单链表</font></center></center>
## 链表的理解与应用:
> <p style ="text-indent:2em">链表像它的字面意思,就是许多节点连成的链,不过连接点是每个节点末尾的指针,其指向Null(尾节点)或是下一个节点的头部.</p>
><p style ="text-indent:2em">其中,单链表包含头节点(1号),首元节点(头节点之后的),头指针(指向首元节点或指向头节点的),一个链表可以没有头节点,但是一定要有头指针
</p>

### 一.创建节点
>>**源码**:定义链表的节点
>>>
    typedef struct node{    
        int n;          //数域//
        struct node *next;          //指针域,指针指向next//
    }NODE;
### 二.使用函数生成节点
>>**源码**:声明一块足够大小的内存空间(malloc),对数域进行赋值,使得节点尾部的指针指向NULL
>>>
    NODE *create_node(int value_n){
        NODE *p = malloc(sizeof(NODE));
        p->n = value_n;    //特殊情况//
        p->next = NULL;
        return p;
    }
### 三.生成链表的主函数
>>**源码**:在主函数中,应当包含头指针(初始为空),在成功创建其他节点时,让头节点的尾指针指向新创建的节点头部
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);  //让头节点的尾指针指向新创建的节点头部//
    }
### 四.给函数增长
#### 头插法:
>>**示意图如下**:
![头插法示意图](无标题.png)
>>**源码实例**:根据图不难想出,新的头插法函数需要的两个变量分别为:"原head指针","节点编号"
>>>   
    NODE *push_front(NODE *head,int n){
        NODE *new = create_node(n);
        new->next = head;   //step2
        head = new;
        return head;    //为了下一次循环的完整,注意将head的值传回//
    }
>>**renew**:同时,在主函数的部分对head进行及时的赋值
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);
        head = push_front(head,1);    //new change//
    }
#### 尾插法:
>联想头插法,想要使用尾插法,则必须找到尾节点(即:尾指针next指向NULL的节点)
>>**源码**:找出尾节点
>>>
    NODE *end = head;   //*end为一个临时指针,以免head直接被操作导致其被修改//
    while (end->next != NULL){
        end = end->next;    //end->next存储的就是本届点的下一节点位置//
    }   
>找出尾节点后,直接把新创建的节点连接原尾节点//
>>**源码(尾插法完整版)**:
>>>
    NODE *push_back(NODE *head,int n){
        NODE *new_node = create_node(n);
        //创建新节点后,链接找出尾节点所需的代码,也可以将其直接做成函数形式,以便调用,本处不展示//
        NODE *end = head;   //*end为一个临时指针,以免head直接被操作导致其被修改//
        if (head == NULL){
            return new_node;
        }else{
        while (end->next != NULL){
            end = end->next;    //end->next存储的就是本节点的下一节点位置//
        }
        }
        end->next = new_node;    //让最后一个节点的尾指针next指向新创建的new节点,将其链接//
        return head;
    }
        //此时,end指向的就是尾节点的头部地址//
>>**renew**:同时,在主函数的部分对head进行及时的赋值
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);
        push_back(NODE* head,1);    //不同于头插法,尾插法无需调整head,也不需要返回新节点new的
        地址,因为push_back函数自带查找最后一个节点
        的功能,但后续学习为了减少计算量,或许需要返回new的地址.
    }
#### 中间插法:
>中间插法给我的第一印象就是它与头插法非常神似,只不过使用头插法时,新节点new的前一个节点必是头节点head.询问AI(包括Kimi,deepseek)后我得到了肯定的答复,给出与deepseek的交流记录:

>>Q:单链表中，能否认为中间插法是头插法的变式？（将头插法的头节点替换成新节点所在位置的前一个节点）

>>A:
您的说法完全正确，而且是一种非常深刻的理解。 我们可以这样来详细阐述
为什么说中间插法是“变式”？
对比上面的代码，我们可以得出一个精辟的结论：
中间插法的代码，就是将头插法中的固定头节点 head，替换成了一个通用的“前驱节点” prev_node。
头插法是 中间插法 的一个特例，即 prev_node 恒等于 head 的情况。
中间插法是 头插法 的一个泛化，它将插入位置从固定的头部扩展到了链表的任意有前驱节点的位置。

***所以这里直接参考头插法就是了,辅助理解的图像也可以说基本完全一致***
#### 节点删除(内嵌节点的查找):
>节点的删除函数共需要完成两个任务:
1.保证链表的连续性
2.删除目标节点
示意图如下:![删除节点](无标题2.png)
**注:这里不写删除的节点就是头节点head的情况**

    NODE *list_delete(NODE *head,int n){
        NODE *prev = head;
        NODE *p = prev->next;
        if (head != NULL && head->n == n) {
        NODE *temp = head;
        head = head->next;
        free(temp);
        return head;
        }
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //执行step1//
                prev->next = p->next;
                //执行step2//
                //若只进行查找,不删除节点,那么之后只需要return p;即可//
                free(p);
            }   //如果此时的n找到了,那就进行删除操作,并且停止遍历,因为此时p已经是空指针//
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        
    }
**当前delete函数已经能够实现删除节点并保持链表完整性的操作,但可能会出现算力浪费和效果不明显的情况,因此尝试改进:
>1.如果内层if函数被执行,那么就打断整个循环
>>i.return head;
>>ii. break;
这两种方式虽然都能打断循环,但i方法能给予函数返回值,所以选择i方法
>2.如果给定链表的头节点的下一个节点为空,那么就不执行下一步,直接返回head

>>renew:

    NODE *list_delete(NODE *head,int n){  
        if (head == NULL){
        return head;
    }
        NODE *prev = head;
        NODE *p = prev->next; //p为当前节点,prev为当前节点的前一个节点
        if (head->n == n) {
        NODE *temp = head;
        head = head->next;
        free(temp);
        return head;
    
    }
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //执行step1//
                prev->next = p->next;
                //执行step2//
                //若只进行查找,不删除节点,那么之后只需要return p;即可//
                free(p);
                printf("删除成功,delete函数结束"); //new change//
                return head; //new change//
            }   //如果此时的n找到了,那就进行删除操作//
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        printf("未找到目标节点,delete节点失败");
        return head;    //new change 循环结束,能运行到这一步必定表示if函数没有起作用//
    }

#### 节点的修改
>节点的修改有两种形式
>>1.直接替换节点(调用delete函数与中间插入函数)
2.修改目标节点数域的值

>>源码1:

    NODE *add_middle(NODE *p,NODE *prev,int n,int new_n){
        NODE *new = create_node(new_n);
            new->next = p->next;
            prev->next= new;
            printf("替换成功\n");
            return prev;
    }
**这里的add_middle并非中间插法函数,形象地来说,它是使一个节点并联在目标节点p之上的函数
(需要联想最简单的并联电路图帮助记忆与理解),为了减少重复思考的时间,这里直接给出示意图:**

![add_middle](无标题3.png)

**replace函数本质上个将p移动到目标节点的函数,其替换效果来源于add_middle函数与delete函数的套用,整体并未使用新语法或是新思想

    NODE *replace_node(NODE *head,int n,int new_n){
        NODE *prev = head;
        NODE *p = prev->next;
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //prev->next = p->next;//
                printf("已找到该节点\n");
                //此时,p就是头插法中的head->next,即存储目标节点的地址//
            add_middle(p,prev,n,new_n);
            return prev;
            delete(head,n);
            break;
            }
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        printf("未找到该节点,请重新输入");
        return head;
        //此处可以用循环嵌套整体,但这里不展示//
    }

**果真如此吗? 如果你认同我对于replace_node函数的功能实现为了方便可以直接嵌套delete函数,那说明你忽略了delete函数的作用不止删除,它还有维系链表完整性的作用(使目标节点前的节点指向目标节点指向的节点),可能造成类似并联电路短路的情况,并且经过replace_node函数的遍历,我们已经找到了p的位置,而delete函数内部也存在遍历的相关代码,这样做会增加计算量**
>>**renew:**        

NODE *replace_node(NODE *head, int n, int new_n) {
    // 边界条件处理：如果链表为空，直接返回NULL
    if (head == NULL) {
        printf("链表为空\n");
        return NULL;
    }

    // 特殊情况处理：如果目标节点是头节点
    if (head->n == n) {
        printf("目标节点是头节点，直接替换头节点\n");
        // 创建新节点作为新的头节点
        NODE *new_node = create_node(new_n);
        if (new_node == NULL) {
            printf("内存分配失败！\n");
            return head; // 内存分配失败，返回原链表
        }
        // 让新头节点指向原头节点的后继节点
        new_node->next = head->next;
        // 释放原头节点
        free(head);
        // 返回新的头节点指针
        return new_node;
    }

    // 初始化两个指针用于遍历链表
    // prev: 指向当前检查节点的前一个节点，初始为头节点
    // p:    指向当前正在检查的节点，初始为第二个节点（head->next）
    NODE *prev = head;
    NODE *p = head->next; // 关键修复：明确定义p，从第二个节点开始检查

    // 遍历链表查找目标节点
    // 循环条件：p != NULL 确保能检查到最后一个节点（包括尾节点）
    while (p != NULL) {
        // 检查当前节点p的数据是否等于目标值n
        if (p->n == n) {
            printf("已找到目标节点，开始替换\n");
            
            // 创建新节点
            NODE *new_node = create_node(new_n);
            if (new_node == NULL) {
                printf("内存分配失败！\n");
                return head; // 内存分配失败，返回原链表
            }
            
            // 执行"并联替换"操作（三步法）：
            // 1. 新节点的next指向p节点的next（新节点接管p的后继链）
            new_node->next = p->next;
            // 2. prev节点的next指向新节点（前驱节点链接到新节点）
            prev->next = new_node;
            // 3. 释放原节点p的内存
            free(p);
            
            printf("节点替换成功\n");
            return head; // 返回头指针（头指针未改变）
        }
        
        // 当前节点不是目标，双指针同步后移：
        // prev移动到p的位置，p移动到下一个节点
        prev = p;
        p = p->next;
    }

    // 循环结束未找到目标节点
    printf("未找到值为 %d 的节点\n", n);
    return head; // 链表未改变，返回原头指针
}


>>****测试****
     int main(){
        NODE *head = NULL;
        head = create_node(0);
        push_back(head,1); 
        push_back(head,2);
        push_back(head,3);
        push_back(head,4);
        printf("请输入希望替换的节点");
        int n;
        scanf("%d",&n);
        delete(head,n);
        replace_node(head,n-1);
        return 0;
    }

>>输出结果:
  正在执行任务: C:/Windows/System32/cmd.exe /d /c .\build\Debug\outDebug.exe 
请输入希望替换的节点2
已找到该节点
替换成功
删除成功,delete函数结束 *  终端将被任务重用，按任意键关闭。 
**注意:以上的节点替换操作(增添节点,插入节点,删除旧节点)逻辑繁琐,效率较低,在此使用只是为了将已经写好的delete函数,add_midddle函数,place_node函数重复利用.**
#### 内存的释放
>对于小项目来说：
短期运行的程序：如果程序很快结束，操作系统会回收所有内存，内存泄漏的影响确实不明显
简单程序：对于功能简单、内存分配少的程序，即使有内存泄漏，可能也不会立即出现问题
学习阶段：在初学阶段，主要关注算法和逻辑，内存管理可能不是最高优先级
但是存在风险：
习惯养成：如果在小项目中忽视内存管理，会养成不良的编程习惯
程序行为不可预测：即使在小项目中，内存泄漏也可能导致程序在长时间运行时出现问题
调试困难：内存问题往往难以调试，及早养成好习惯可以避免后续的调试痛苦
在日后的学习中：
大型项目：在复杂的系统中，内存泄漏会累积并最终导致程序崩溃
嵌入式系统：在资源受限的环境中，内存管理至关重要
专业开发：在企业级开发中，内存泄漏是严重的质量问题
**简而言之,虽然我们目前的小项目不会怎么看到内存释放的功效,但最好养成正确释放内存的习惯**
>> **这里区分一个重要概念:内存与储存
内存（Memory）- RAM
硬件形态：内存条、芯片
技术类型：DRAM、SDRAM、DDR系列
作用：临时工作空间，CPU直接访问
特点：速度快、容量小、断电数据丢失
存储（Storage）
硬件形态：硬盘、固态硬盘、U盘、存储卡
技术类型：HDD（机械硬盘）、SSD（固态硬盘）、Flash
作用：长期数据保存
特点：速度较慢、容量大、断电数据保留
 老百姓说："我手机内存是128G"
实际含义：存储容量（Storage）
正确说法："我手机存储空间是128G"
手机真实内存：通常4-16GB RAM
又比如:你的电脑装载16G+16G的内存条,那么你的电脑内存就是32G,装载512G+1TB的硬盘,那么你的电脑储存就是1.5T(由于厂家与计算机算法不同,实际小于1.5T)**

>回到链表内存的释放:
>结合之前所学的遍历链表,你可能会想到这样的释放内存方式:

    void free_list(NODE*head){
        NODE *current = head;   //让current指向head//
        while(current =! NULL){
            free(current);
            current = current->next;    //清除current所在节点后,将
            current通过节点的尾指针next后移//
        }
    }
>这种逻辑看似没问题,实则当current被free时,current->next就已经不存在了,因此,我们需要增加第二个指针,让它在current被free前,将current->next保存起来,这时不难联想到使用在delete函数中的双指针技巧

    void free_list(NODE *head){
        NODE *current = head;
        NODE *next_node;
        while (current != NULL) {
        next_node = current->next;  // 保存下一个节点的地址
        free(current);              // 释放当前节点
        current = next_node;        // 移动到下一个节点
    }
    printf("链表内存已成功释放\n");
    head = NULL; // 释放头指针
    }

#### 至此,所有单链表的基础知识复习与应用已经整理完毕,编译后运行无误 能达到预期效果####



=======
# <center><font size = 50>单链表</font></center></center>
## 链表的理解与应用:
> <p style ="text-indent:2em">链表像它的字面意思,就是许多节点连成的链,不过连接点是每个节点末尾的指针,其指向Null(尾节点)或是下一个节点的头部.</p>
><p style ="text-indent:2em">其中,单链表包含头节点(1号),首元节点(头节点之后的),头指针(指向首元节点或指向头节点的),一个链表可以没有头节点,但是一定要有头指针
</p>

### 一.创建节点
>>**源码**:定义链表的节点
>>>
    typedef struct node{    
        int n;          //数域//
        struct node *next;          //指针域,指针指向next//
    }NODE;
### 二.使用函数生成节点
>>**源码**:声明一块足够大小的内存空间(malloc),对数域进行赋值,使得节点尾部的指针指向NULL
>>>
    NODE *create_node(int value_n){
        NODE *p = malloc(sizeof(NODE));
        p->n = value_n;    //特殊情况//
        p->next = NULL;
        return p;
    }
### 三.生成链表的主函数
>>**源码**:在主函数中,应当包含头指针(初始为空),在成功创建其他节点时,让头节点的尾指针指向新创建的节点头部
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);  //让头节点的尾指针指向新创建的节点头部//
    }
### 四.给函数增长
#### 头插法:
>>**示意图如下**:
![头插法示意图](无标题.png)
>>**源码实例**:根据图不难想出,新的头插法函数需要的两个变量分别为:"原head指针","节点编号"
>>>   
    NODE *push_front(NODE *head,int n){
        NODE *new = create_node(n);
        new->next = head;   //step2
        head = new;
        return head;    //为了下一次循环的完整,注意将head的值传回//
    }
>>**renew**:同时,在主函数的部分对head进行及时的赋值
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);
        head = push_front(head,1);    //new change//
    }
#### 尾插法:
>联想头插法,想要使用尾插法,则必须找到尾节点(即:尾指针next指向NULL的节点)
>>**源码**:找出尾节点
>>>
    NODE *end = head;   //*end为一个临时指针,以免head直接被操作导致其被修改//
    while (end->next != NULL){
        end = end->next;    //end->next存储的就是本届点的下一节点位置//
    }   
>找出尾节点后,直接把新创建的节点连接原尾节点//
>>**源码(尾插法完整版)**:
>>>
    NODE *push_back(NODE *head,int n){
        NODE *new_node = create_node(n);
        //创建新节点后,链接找出尾节点所需的代码,也可以将其直接做成函数形式,以便调用,本处不展示//
        NODE *end = head;   //*end为一个临时指针,以免head直接被操作导致其被修改//
        if (head == NULL){
            return new_node;
        }else{
        while (end->next != NULL){
            end = end->next;    //end->next存储的就是本节点的下一节点位置//
        }
        }
        end->next = new_node;    //让最后一个节点的尾指针next指向新创建的new节点,将其链接//
        return head;
    }
        //此时,end指向的就是尾节点的头部地址//
>>**renew**:同时,在主函数的部分对head进行及时的赋值
>>>
    int main(){
        NODE *head = NULL;
        head = create_node(0);
        push_back(NODE* head,1);    //不同于头插法,尾插法无需调整head,也不需要返回新节点new的
        地址,因为push_back函数自带查找最后一个节点
        的功能,但后续学习为了减少计算量,或许需要返回new的地址.
    }
#### 中间插法:
>中间插法给我的第一印象就是它与头插法非常神似,只不过使用头插法时,新节点new的前一个节点必是头节点head.询问AI(包括Kimi,deepseek)后我得到了肯定的答复,给出与deepseek的交流记录:

>>Q:单链表中，能否认为中间插法是头插法的变式？（将头插法的头节点替换成新节点所在位置的前一个节点）

>>A:
您的说法完全正确，而且是一种非常深刻的理解。 我们可以这样来详细阐述
为什么说中间插法是“变式”？
对比上面的代码，我们可以得出一个精辟的结论：
中间插法的代码，就是将头插法中的固定头节点 head，替换成了一个通用的“前驱节点” prev_node。
头插法是 中间插法 的一个特例，即 prev_node 恒等于 head 的情况。
中间插法是 头插法 的一个泛化，它将插入位置从固定的头部扩展到了链表的任意有前驱节点的位置。

***所以这里直接参考头插法就是了,辅助理解的图像也可以说基本完全一致***
#### 节点删除(内嵌节点的查找):
>节点的删除函数共需要完成两个任务:
1.保证链表的连续性
2.删除目标节点
示意图如下:![删除节点](无标题2.png)
**注:这里不写删除的节点就是头节点head的情况**

    NODE *list_delete(NODE *head,int n){
        NODE *prev = head;
        NODE *p = prev->next;
        if (head != NULL && head->n == n) {
        NODE *temp = head;
        head = head->next;
        free(temp);
        return head;
        }
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //执行step1//
                prev->next = p->next;
                //执行step2//
                //若只进行查找,不删除节点,那么之后只需要return p;即可//
                free(p);
            }   //如果此时的n找到了,那就进行删除操作,并且停止遍历,因为此时p已经是空指针//
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        
    }
**当前delete函数已经能够实现删除节点并保持链表完整性的操作,但可能会出现算力浪费和效果不明显的情况,因此尝试改进:
>1.如果内层if函数被执行,那么就打断整个循环
>>i.return head;
>>ii. break;
这两种方式虽然都能打断循环,但i方法能给予函数返回值,所以选择i方法
>2.如果给定链表的头节点的下一个节点为空,那么就不执行下一步,直接返回head

>>renew:

    NODE *list_delete(NODE *head,int n){  
        if (head == NULL){
        return head;
    }
        NODE *prev = head;
        NODE *p = prev->next; //p为当前节点,prev为当前节点的前一个节点
        if (head->n == n) {
        NODE *temp = head;
        head = head->next;
        free(temp);
        return head;
    
    }
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //执行step1//
                prev->next = p->next;
                //执行step2//
                //若只进行查找,不删除节点,那么之后只需要return p;即可//
                free(p);
                printf("删除成功,delete函数结束"); //new change//
                return head; //new change//
            }   //如果此时的n找到了,那就进行删除操作//
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        printf("未找到目标节点,delete节点失败");
        return head;    //new change 循环结束,能运行到这一步必定表示if函数没有起作用//
    }

#### 节点的修改
>节点的修改有两种形式
>>1.直接替换节点(调用delete函数与中间插入函数)
2.修改目标节点数域的值

>>源码1:

    NODE *add_middle(NODE *p,NODE *prev,int n,int new_n){
        NODE *new = create_node(new_n);
            new->next = p->next;
            prev->next= new;
            printf("替换成功\n");
            return prev;
    }
**这里的add_middle并非中间插法函数,形象地来说,它是使一个节点并联在目标节点p之上的函数
(需要联想最简单的并联电路图帮助记忆与理解),为了减少重复思考的时间,这里直接给出示意图:**

![add_middle](无标题3.png)

**replace函数本质上个将p移动到目标节点的函数,其替换效果来源于add_middle函数与delete函数的套用,整体并未使用新语法或是新思想

    NODE *replace_node(NODE *head,int n,int new_n){
        NODE *prev = head;
        NODE *p = prev->next;
        //考虑到链表的长度未知,这里直接套用找到尾节点的思路进行循环//
        while(p != NULL){
            //遍历链表,直到把指针p移动到目标节点,再进行删除//
            if(p->n == n){
                //prev->next = p->next;//
                printf("已找到该节点\n");
                //此时,p就是头插法中的head->next,即存储目标节点的地址//
            add_middle(p,prev,n,new_n);
            return prev;
            delete(head,n);
            break;
            }
            else{
                prev = prev->next;
                p = prev->next;
            }   //如果没找到,那就让p和prev整体后移//
        }
        printf("未找到该节点,请重新输入");
        return head;
        //此处可以用循环嵌套整体,但这里不展示//
    }

**果真如此吗? 如果你认同我对于replace_node函数的功能实现为了方便可以直接嵌套delete函数,那说明你忽略了delete函数的作用不止删除,它还有维系链表完整性的作用(使目标节点前的节点指向目标节点指向的节点),可能造成类似并联电路短路的情况,并且经过replace_node函数的遍历,我们已经找到了p的位置,而delete函数内部也存在遍历的相关代码,这样做会增加计算量**
>>**renew:**        

NODE *replace_node(NODE *head, int n, int new_n) {
    // 边界条件处理：如果链表为空，直接返回NULL
    if (head == NULL) {
        printf("链表为空\n");
        return NULL;
    }

    // 特殊情况处理：如果目标节点是头节点
    if (head->n == n) {
        printf("目标节点是头节点，直接替换头节点\n");
        // 创建新节点作为新的头节点
        NODE *new_node = create_node(new_n);
        if (new_node == NULL) {
            printf("内存分配失败！\n");
            return head; // 内存分配失败，返回原链表
        }
        // 让新头节点指向原头节点的后继节点
        new_node->next = head->next;
        // 释放原头节点
        free(head);
        // 返回新的头节点指针
        return new_node;
    }

    // 初始化两个指针用于遍历链表
    // prev: 指向当前检查节点的前一个节点，初始为头节点
    // p:    指向当前正在检查的节点，初始为第二个节点（head->next）
    NODE *prev = head;
    NODE *p = head->next; // 关键修复：明确定义p，从第二个节点开始检查

    // 遍历链表查找目标节点
    // 循环条件：p != NULL 确保能检查到最后一个节点（包括尾节点）
    while (p != NULL) {
        // 检查当前节点p的数据是否等于目标值n
        if (p->n == n) {
            printf("已找到目标节点，开始替换\n");
            
            // 创建新节点
            NODE *new_node = create_node(new_n);
            if (new_node == NULL) {
                printf("内存分配失败！\n");
                return head; // 内存分配失败，返回原链表
            }
            
            // 执行"并联替换"操作（三步法）：
            // 1. 新节点的next指向p节点的next（新节点接管p的后继链）
            new_node->next = p->next;
            // 2. prev节点的next指向新节点（前驱节点链接到新节点）
            prev->next = new_node;
            // 3. 释放原节点p的内存
            free(p);
            
            printf("节点替换成功\n");
            return head; // 返回头指针（头指针未改变）
        }
        
        // 当前节点不是目标，双指针同步后移：
        // prev移动到p的位置，p移动到下一个节点
        prev = p;
        p = p->next;
    }

    // 循环结束未找到目标节点
    printf("未找到值为 %d 的节点\n", n);
    return head; // 链表未改变，返回原头指针
}


>>****测试****
     int main(){
        NODE *head = NULL;
        head = create_node(0);
        push_back(head,1); 
        push_back(head,2);
        push_back(head,3);
        push_back(head,4);
        printf("请输入希望替换的节点");
        int n;
        scanf("%d",&n);
        delete(head,n);
        replace_node(head,n-1);
        return 0;
    }

>>输出结果:
  正在执行任务: C:/Windows/System32/cmd.exe /d /c .\build\Debug\outDebug.exe 
请输入希望替换的节点2
已找到该节点
替换成功
删除成功,delete函数结束 *  终端将被任务重用，按任意键关闭。 
**注意:以上的节点替换操作(增添节点,插入节点,删除旧节点)逻辑繁琐,效率较低,在此使用只是为了将已经写好的delete函数,add_midddle函数,place_node函数重复利用.**
#### 内存的释放
>对于小项目来说：
短期运行的程序：如果程序很快结束，操作系统会回收所有内存，内存泄漏的影响确实不明显
简单程序：对于功能简单、内存分配少的程序，即使有内存泄漏，可能也不会立即出现问题
学习阶段：在初学阶段，主要关注算法和逻辑，内存管理可能不是最高优先级
但是存在风险：
习惯养成：如果在小项目中忽视内存管理，会养成不良的编程习惯
程序行为不可预测：即使在小项目中，内存泄漏也可能导致程序在长时间运行时出现问题
调试困难：内存问题往往难以调试，及早养成好习惯可以避免后续的调试痛苦
在日后的学习中：
大型项目：在复杂的系统中，内存泄漏会累积并最终导致程序崩溃
嵌入式系统：在资源受限的环境中，内存管理至关重要
专业开发：在企业级开发中，内存泄漏是严重的质量问题
**简而言之,虽然我们目前的小项目不会怎么看到内存释放的功效,但最好养成正确释放内存的习惯**
>> **这里区分一个重要概念:内存与储存
内存（Memory）- RAM
硬件形态：内存条、芯片
技术类型：DRAM、SDRAM、DDR系列
作用：临时工作空间，CPU直接访问
特点：速度快、容量小、断电数据丢失
存储（Storage）
硬件形态：硬盘、固态硬盘、U盘、存储卡
技术类型：HDD（机械硬盘）、SSD（固态硬盘）、Flash
作用：长期数据保存
特点：速度较慢、容量大、断电数据保留
 老百姓说："我手机内存是128G"
实际含义：存储容量（Storage）
正确说法："我手机存储空间是128G"
手机真实内存：通常4-16GB RAM
又比如:你的电脑装载16G+16G的内存条,那么你的电脑内存就是32G,装载512G+1TB的硬盘,那么你的电脑储存就是1.5T(由于厂家与计算机算法不同,实际小于1.5T)**

>回到链表内存的释放:
>结合之前所学的遍历链表,你可能会想到这样的释放内存方式:

    void free_list(NODE*head){
        NODE *current = head;   //让current指向head//
        while(current =! NULL){
            free(current);
            current = current->next;    //清除current所在节点后,将
            current通过节点的尾指针next后移//
        }
    }
>这种逻辑看似没问题,实则当current被free时,current->next就已经不存在了,因此,我们需要增加第二个指针,让它在current被free前,将current->next保存起来,这时不难联想到使用在delete函数中的双指针技巧

    void free_list(NODE *head){
        NODE *current = head;
        NODE *next_node;
        while (current != NULL) {
        next_node = current->next;  // 保存下一个节点的地址
        free(current);              // 释放当前节点
        current = next_node;        // 移动到下一个节点
    }
    printf("链表内存已成功释放\n");
    head = NULL; // 释放头指针
    }

#### 至此,所有单链表的基础知识复习与应用已经整理完毕,编译后运行无误 能达到预期效果####



>>>>>>> 3fc4bfd7786997d369dac2c591dd80e5fdfbee95
