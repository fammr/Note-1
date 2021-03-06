
```java
package tree;

import java.util.Scanner;

class CBTType{							//定义二叉树结点类型
	String data;						//元素结点数据
	CBTType left;						//本结点的左子树结点的引用
	CBTType right;						//本结点的右子树结点的引用
}


public class TreeList {
	static final int MAXLEN=3;
	static Scanner input= new Scanner(System.in);


	//初始化动作：先申请内存，然后输入一个根结点数据，并将指向左子树和右子树的引用设置为空
	CBTType InitTree(){								//初始化二叉树的根
		CBTType node;
		if((node=new CBTType())!=null){					//申请内存
			System.out.print("请先输入一个根结点数据：\n");
			node.data=input.next();
			node.left=null;
			node.right=null;
			if(node!=null){								//如果二叉树根结点不为空
				return node;
			}else{
				return null;
			}
		}
		return null;
	}
	
	//添加结点:添加结点时除了要输入结点数据外，还要指定其父结点，以及添加的结点是作为左子树还是右子树
	void AddTreeNode(CBTType treeNode){				//treeNode为二叉树的根结点，传入根结点去查找该二叉树.
													//此方法先申请内存，然后输入二叉树结点数据，并设置左右子树为空。接着指定父结点
		CBTType pnode,parent;		//pnode作为新增结点，parent作为父结点	
		String data;				//需要添加的结点数据
		int menusel;				
		
		if((pnode=new CBTType())!=null){		//为新增结点分配内存
			System.out.print("输入二叉树结点数据: \n");
			pnode.data=input.next();
			pnode.left=null;
			pnode.right=null;
			
			System.out.print("输入该结点的父结点数据：");		//输入父结点数据
			data=input.next();
			
			parent=TreeFindNode(treeNode,data);				//根据上面输入的父结点数据，查找指定数据的结点
			if(parent==null){  							//如果没找到
				System.out.print("未找到该父结点！\n");
				pnode=null;										//释放创建的结点内存
				return;
			}
			//选择项
			System.out.printf("1.添加该结点到左子树\n 2.添加该结点到右子树\n 非1或2则退出");
			do{
				menusel=input.nextInt();				//输入选择项
				if(menusel==1||menusel==2){
					if(parent==null){					//刚刚是查找指定数据的结点，没找到父结点
						System.out.print("不存在父结点，请先设置父结点!\n");
					}else{
						switch(menusel){
						case 1:								//添加到左子树结点
							if(parent.left!=null){					//左子树不为空
								System.out.print("左子树结点不为空！\n");
							}else{
								parent.left=pnode;
							}
							break;
						case 2:							//添加到右子树结点
							if(parent.right!=null){			//右子树不为空
								System.out.print("左子树结点不为空！\n");
							}else{
								parent.right=pnode;
							}
							break;
						default:
							System.out.print("无效参数！\n");
						}
					}
				}
			}while(menusel!=1&&menusel!=2);
		}
		
	}
	//查找操作。遍历二叉树每一个结点，逐个对比数据，当找到目标数据就返回该数据所在结点的引用
	CBTType TreeFindNode(CBTType treeNode,String data){		//查找结点.treeNode为待查找的二叉树的根结点，输入参数data为待查找的结点数据。
															//先判断根结点是否为空，然后向左右子树递归查找。如果当前结点的数据与查找数据相等，则返回当前结点的引用.
		CBTType ptr;				//用一个假结点来的得到查询到的结点数据，遍历查询对比
		if(treeNode==null){
			return null;
		}
		else{										
			if(treeNode.data.equals(data)){
				return treeNode;
			}
			else {										//分别向左右子树递归查找
				if((ptr=TreeFindNode(treeNode.left,data))!=null){		//找	左子树结点
					return ptr;
				}else if((ptr=TreeFindNode(treeNode.right,data))!=null){	//找右子树结点
					return ptr;
				}else{
					return null;
				}
			}
		}
	}
	
	//显示结点数据操作：就是显示当前结点的数据内容。p为待显示结点。
	void TreeNodeData(CBTType p){				//显示结点数据
		System.out.printf("%s",p.data);				//输出结点数据
	}
	/**
	 * 	前序遍历，也叫先根遍历，遍历的顺序是，根，左子树，右子树
	 *	中序遍历，也叫中根遍历，顺序是 左子树，根，右子树
	 * 	后序遍历，也叫后根遍历，遍历顺序，左子树，右子树，根
	 * @param treeNode
	 */
	
	//先序遍历：首先访问根结点然后遍历左子树，最后遍历右子树。在遍历左、右子树时，仍然先访问根结点，然后遍历左子树，最后遍历右子树，如果二叉树为空则返回。按递归的思路来遍历整个二叉树
	//这里的treeNode为我二叉树根结点
	void DLRTree(CBTType treeNode){
		if(treeNode!=null){
			TreeNodeData(treeNode);				//显示结点的数据
			DLRTree(treeNode.left);				//采用递归思想！！！！！。本方法调用本方法自身。此递归出口就是在不断遍历时，直到一个子树结点为空。
			DLRTree(treeNode.right);			//递归前进段是：一开始根结点的左子树结点不为空，右子树结点也不为空。如果不为空，就会用子树结点替换掉根结点。
		}
	}
	
	//中序遍历:首先遍历左子树，然后访问根结点，最后遍历右子树。在遍历左、右子树时，仍然先遍历左子树，再访问根结点，最后遍历右子树
	void LDRTree(CBTType treeNode){
		if(treeNode!=null){
			LDRTree(treeNode.left);				//中序遍历左子树
			TreeNodeData(treeNode);				//显示结点数据	
			LDRTree(treeNode.right);			//中序遍历右子树
		}
	}
	
	//后序遍历：首先遍历左子树，然后遍历右子树，最后访问根结点，在遍历左、右子树时，仍然先遍历左子树，然后遍历右子树，最后遍历根结点。
	// treeNode为需要遍历的二叉树根结点
	void LRDTree(CBTType treeNode){
		if(treeNode!=null){
			LRDTree(treeNode.left);				//后序遍历左子树		
			LRDTree(treeNode.right);			//后序遍历右子树
			TreeNodeData(treeNode);				//显示结点数据
		}
	}
	
	//按层遍历二叉树（最直观二等遍历算法）：处理第一层即根结点，再处理第一层根结点的左右子树结点（即系第二层）
	//treeNode为需要遍历的二叉树根结点。处理时，先从根结点开始，将每层的结点放进队列。
	void LevelTree(CBTType treeNode){
		CBTType p;
		CBTType[] q=new CBTType[MAXLEN];			//定义个一个顺序队列（循环队列）
		int head=0,tail=0;							//队头队尾。tail指向的是下一个可存结点的位置。
		
		if(treeNode!=null){						//如果二叉树不为空
							
			q[tail]=treeNode;					//将二叉树根引用进队列。意思是：因为结点存在，所以把他放进一个数组
			tail=(tail+1)%MAXLEN;		//我们不断把指针往后移动一个位置，到最后就转到数组头部
		}
		while(head!=tail){	
			p=q[head];					//获取队首元素				
			TreeNodeData(p);					//处理队首元素
			head=(head+1)%MAXLEN;				//出队的时候，头指针就加1，不断把指针往后移动一个位置，到最后就转到数组头部
			
			if(p.left!=null){
				q[tail]=p.left;					//如果该结点存在于它的左子树结点就把他放进队列。将左子树结点引进队列。此时第一轮循环是：左子树结点存在，取余得到序号，所以放进队列中保存。
				tail=(tail+1)%MAXLEN;			//树的左子树结点进入队列后，tail就往后移动。第一次循环时意思就是：头结点的左子树结点存在，（tail+1）%MAXLEN为2，存在，所以第一轮左子树结点为序号2
								
			}
			if(p.right!=null){					//如果该结点存在它的右子树结点
				q[tail]=p.right;				//将右子树结点引进队列。此时第一轮循环是：右子树结点存在，取余得到序号，所以放进队列中保存。
				tail=(tail+1)%MAXLEN;			//树的右子树结点进入队列后，tail就往后移动。第一次循环时意思就是：头结点的右子树结点存在，（tail+1）%MAXLEN为2，存在，所以第一轮右子树结点为序号3
			}
		}		
	}
	
	//查询二叉树的深度
	int TreeDepth(CBTType treeNode){
		int depleft,depright;
		if(treeNode==null){
			return 0;		//对于空树，深度为0
		}else{
							//这里使用递归，是递归到最后一层的。然后在最后一层运行以下代码，判断左深度和右深度。接着返回到倒数第二层，再到倒数第三层....最后到第一层（根结点）
							//这里列下递归过程：先递归左子树深度，到最后一层，然后递归得到右子树深度，右子树遍历完后，接着就是遍历左子树啦，遍历左子树的时候比较高度if(depleft>depright)，
							//然后就是左子树的从底层遍历回到根结点，最终return出一个深度
			depleft=TreeDepth(treeNode.left);			//左子树深度（递归调用）
			depright=TreeDepth(treeNode.right);			//右子树深度（递归调用）
			//两者遍历完后得到左子树和右子树的深度，然后就比较高度
			if(depleft>depright){
				return depleft+1;
			}else{
				return depright+1;
			}


//			if(depleft<depright){
//				return depright+1;
//			}else{
//				return depleft+1;
//			}
		}
	}
	//清空二叉树（将二叉树变成空树）。
	//treeNode是待清空的二叉树的根结点。方法按照递归来清空左子树和右子树，并且使用赋值null操作来释放当前结点所占内存。
	void ClearTree(CBTType treeNode){
		if(treeNode!=null){
			ClearTree(treeNode.left);				//递归左子树，清空左子树
			ClearTree(treeNode.right);				//递归右子树，清空右子树
			treeNode=null;							//释放当前结点所占内存
		}
	}


	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CBTType root=null;					//root为指向二叉树根结点的引用
		int menusel;			
		TreeList t=new TreeList();
		root=t.InitTree();					//初始化
		do{								//添加结点
			System.out.print("请选择菜单添加二叉树的结点\n");
			System.out.print("0.退出\t");					//显示菜单
			System.out.print("1.添加二叉树结点\n");
			menusel=input.nextInt();
			switch(menusel){
			case 1:
				t.AddTreeNode(root);					//添加结点
				break;
			case 0:
				break;
			default:
					;
			}
		}while(menusel!=0);		
		
		//遍历
		do{
			System.out.print("请选择菜单遍历二叉树，输入0表示退出：\n");
			System.out.print("1.先序遍历DLR\t");
			System.out.print("2.中序遍历LDR\t");
			System.out.print("3.后序遍历LRD\t");
			System.out.print("4.按层遍历\n");
			menusel=input.nextInt();
			switch(menusel){
			case 0:
				break;
			case 1:							//先序遍历
				System.out.print("\n先序遍历DLR的结果：");
				t.DLRTree(root);
				System.out.print("\n");
				break;
			case 2:							//中序遍历
				System.out.print("\n中序LDR遍历的结果：");
				t.LDRTree(root);
				System.out.print("\n");
				break;
			case 3:
				System.out.print("\n后序遍历LRD的结果：");
				t.LRDTree(root);
				System.out.print("\n");
				break;
			case 4:
				System.out.print("\n按层遍历的结果是：");
				t.LevelTree(root);
				System.out.print("\n");
				break;
			default:
				;	
			}
		}while(menusel!=0);
		//查出深度
		System.out.printf("\n 二叉树深度为：%d\n",t.TreeDepth(root));
		
		t.ClearTree(root);
		root=null;
	}
}
```