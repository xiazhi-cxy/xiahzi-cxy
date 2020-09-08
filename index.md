# 1，还原二叉树
![还原二叉树](https://img-blog.csdnimg.cn/20191210195111493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjcyMjUx,size_16,color_FFFFFF,t_70)

> 先序遍历的特点：先遍历根结点，再遍历左子树，最后再遍历右子树
> 中序遍历的特点：先遍历左子树，再遍历根结点，最后再遍历右子树
> 后序遍历的特点：先遍历左子树，再遍历右子树，最后再遍历根结点

1. 举例
先序遍历：A B D F G H I E C
中序遍历：F D H G I B E A C
如上，根据先序遍历以及中序遍历的特点，也就是说先序遍历的第一个为根结点，在中序遍历中找到根结点，则在中序遍历中根结点左边为左子树，根结点右边为右子树。
如下图，根据上述分解完后再将分解出的序列看作一组新的先序序列和中序序列，继续分解，直到序列中只有一个元素
![图解](https://img-blog.csdnimg.cn/20191210201111399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjcyMjUx,size_16,color_FFFFFF,t_70)

```cpp
#include<iostream>
using namespace std;
int g = 1;
typedef struct Node {
	char data;
	struct Node *left, *right;
}Node;
Node* restore(char *a, char *b,int n)   
{ 
	Node *p = (Node*)malloc(sizeof(Node));
	if (n == 0) return NULL;    //递归边界
	p->data = *a;        //插入新结点
	p->left = NULL;
	p->right = NULL;
	if (n == 1) return p;     //递归边界
	int k;
	for(int i=0;i<n;i++)    //寻找根结点
		if(*a==*(b+i))
		{
			k = i; break;
		}
	p->left = restore(a + 1, b, k);    //左子树分解
	p->right = restore(a + k + 1, b + k + 1, n - k - 1);   //右子树分解
	return p;
}
void preorder(Node *bt)     //先序遍历二叉树
{
	if (g == 1)
		printf("先序遍历序列为：\n");
	g = g + 1;
	if (bt)
	{
		printf("%6c", bt->data);
		preorder(bt->left);
		preorder(bt->right);
	}
	else
		if (g == 2) printf("空树\n");
}
int height(Node *p)    //深度的计算
{
	if (p == NULL)
		return 0;
	int Max = height(p->left);      
	if (Max < height(p->right))    //比较左右子树的深度，返回较大的值
		Max = height(p->right);
	return Max + 1;
}
int main()
{
	Node *p;
	char a[101], b[101];
	int n;
	cin >> n;
	cin >> a >> b;
	if (strcmp(a, b))
	{
		p = restore(a, b, n);
		preorder(p);
	}
	else
		cout << n << endl;
	cout << endl;
	cout << height(p) << endl;
	return 0;
}
```
# 二叉搜索树
![二叉搜索树](https://img-blog.csdnimg.cn/2019121020244862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjcyMjUx,size_16,color_FFFFFF,t_70)

> 二叉搜索树的原则：任意一个结点的左子树均小于该结点，任意一个结点的右子树均大于该结点。
> 特点：二叉搜索树的中序遍历为非递增序列。

**二叉搜索树其实质也就是有序数列的二分搜索的过程。**

直接给代码，

```cpp
#include<iostream>
# include <cstdio>
# include <cstring>
# include <algorithm>
using namespace std;
int cnt;
int a[1000];
struct node {
	int data;
	node *l, *r;
};
node *newnode() {     //生成结点
	node *t;
	t = (node *)malloc(sizeof(node));
	t->l = t->r = NULL;
	return t;
}
void jianshu(node *T, int d) {    //插入结点
	if (d>T->data) {
		if (!T->r) {
			T->r = newnode();
			T->r->data = d;
			return;
		}
		else {
			jianshu(T->r, d);
		}
	}
	else if (d<T->data) {
		if (!T->l) {
			T->l = newnode();
			T->l->data = d;
			return;
		}
		else {
			jianshu(T->l, d);
		}
	}
	else
		return;
}
void bianli(node *T) {   //先序遍历二叉树
	if (T) {
		a[cnt++] = T->data;
		bianli(T->l);
		bianli(T->r);
	}
}
int main() {
	char flage;
	int num, i;
	node *T;
	T = newnode();
	cin >> num;
	flage = getchar();
	T->data = num;
	while (flage == ' ') {   //输入序列
		cin >> num;
		jianshu(T, num);
		flage = getchar();
	}
	cnt = 0;
	bianli(T);
	for (i = 0; i <= cnt - 1; i++) {   //输出序列
		if (i != cnt - 1) { 
			printf("%d ", a[i]);
		}
		else {
			printf("%d", a[i]);
		}
	}
	return 0;
}
```

希望能对你有一定的帮助，望大佬指正
