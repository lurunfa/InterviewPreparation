数据结构相关笔记
==
## 栈
![](./res/stack.png)
> 来自[csdn博客](https://blog.csdn.net/javazejian/article/details/53362993)的介绍

栈满足先进后出规则，并且在实现的时候只操作栈顶元素，因此效率比较高。
``` java
public interface Stack<T> {

    /**
     * 判断是否为空栈
     * @return 是否为空栈
     */
    boolean isEmpty();

    /**
     * 入栈
     * @param t 入栈元素
     */
    void push(T t);

    /**
     * 获取栈顶元素，未出栈
     * @return 栈顶元素
     */
    T peek();

    /**
     * 出栈 返回栈顶元素
     * @return 栈顶元素
     */
    T pop();
}
```
根据内部存储数据的方式不同，分为顺序栈（以数组或者存储数据）和链式栈（以链表），其中对于顺序栈，需要主要在实现时注意top指针的移动并且需要进行数组扩容。而对于链式栈，需要在push的时候考虑有可能因为pop吧原的空栈底出栈导致栈底为空的情况
### 栈的应用
- 符号匹配
- 中缀表达式转换为后缀表达式
- 计算后缀表达式
- 实现函数的嵌套调用
- HTML和XML文件中的标签匹配
- 网页浏览器中已访问页面的历史记录
下面简要说一下

- 符号匹配主要引用于计算字符串表达式，设定一系列的出入栈规则，当遇到左括号时入栈，遇到右括号的时候出栈下面是主要代码节选
``` java
int i=0;
      while(i<expstr.length())
      {
          char ch=expstr.charAt(i);
          i++;
          switch(ch)
          {
              case '(': stack.push(ch+"");//左括号直接入栈
                  break;
              case ')': if (stack.isEmpty() || !stack.pop().equals("(")) //遇见右括号左括号直接出栈
                  return "(";
          }
      }
```
补充中缀表达式转后缀表达式[链接](https://blog.csdn.net/javazejian/article/details/53362993)

## 二叉树
### 为什么用树
1. 原文来自此[链接](https://blog.csdn.net/cai2016/article/details/52589952)
,总结本文的开头，首先重点都是将<font face="黑体" color=#0099ff>有序</font>数组以及链表的优点相结合，对于<font face="黑体" color=#0099ff>有序</font>数组，可以快速进行二分法的查找，但是对于在数组中（不包括尾部插入，删除和修改）插入和删除则速度较慢。对于链表，在链表中节点的更新，插入和删除都很快。因此二叉树，则可以完美的结合二者的优势。（在有序规则下的二叉树）
2. 代码片段
    1. 首先像链表一样定义节点TreeNode,一个节点（可类似于链表定义为泛型）有三个数据项，有左子节点，右子节点。
    2. Tree定义，包含一个根节点，主要是加入了有序的定义，定义了插入节点的规则。
``` java
/**
 * 树节点
 *
 * @author kevin
 * @date 2018/8/30
 * @since 0.1.0
 **/
public class TreeNode<T> {

    public T t;
    /**
     * int类型的节点数据
     */
    public int iData;
    /**
     * double类型的节点数据
     */
    public double dData;
    /**
     * 左节点
     */
    public TreeNode<T> leftNode;
    /**
     * 右节点
     */
    public TreeNode<T> rightNode;

}
    
```
``` java
/**
 * 树
 *
 * @author kevin
 * @date 2018/8/30
 * @since 0.1.0
 **/
public class Tree<T> {
    /**
     * 树的头节点
     */
    private TreeNode<T> root;

    /**
     * 需要定义插入树的规则，才能使树更好的发挥作用,先以iData为标准，一个节点的头节点的值大于此节点的左子节点，小于右侧子节点
     * @param t 数据t
     * @param iData 判别数据i
     * @param dData 数据d
     */
    public void insert(T t,int iData,double dData){
        //创建新节点
        TreeNode<T> newNode = new TreeNode<>();
        newNode.t = t;
        newNode.dData = dData;
        newNode.iData = iData;
        if (root == null){
            root = newNode;
            return;
        }
        TreeNode<T> current = root;
        TreeNode<T> parent;
        //按照规则，如果数据大于当前节点，则寻找当前节点的右边，若为空，则当插入到前节点的右边子节点，停止。若右边子节点不为空，则吧当前节点改为当前节点重复，若数据小于当前节点，则寻找当前节点的左边，若为空，则当插入到前节点的左边子节点，停止。若右边子节点不为空，则吧当前节点改为当前节点重复
        while (true){
            parent = current;
            if (parent.iData<iData){
                current = current.leftNode;
                if (current == null){
                    parent.leftNode = newNode;
                    return;
                }
            }else{
                current = current.rightNode;
                if (current == null){
                    parent.rightNode = newNode;
                    return;
                }
            }
        }
    }

    /**
     * 也是根据插入的规则定义的查询方法，定义当前节点，从根节点开始，如果当前节点的key不等于key,则判断大于，则把当前节点的右节点变为当前节点，若小于，则把当前节点的左节点变为当前节点。若当前节点为空，则退出循环（表示查不到），当查到后，也退出循环，返回。
     * @param key 所要查找节点的key
     * @return 节点
     */
    public TreeNode<T> find(int key){
        TreeNode<T> current = this.root;
        if (current ==null){
            return null;
        }
        while (current.iData!=key){
            if (current.iData<key){
                current = current.leftNode;
            }else{
                current = current.rightNode;
            }
            if (current == null){
                return null;
            }
        }
        return current;
    }
}   
```

