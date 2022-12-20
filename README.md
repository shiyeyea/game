主函数文件
#include<stdio.h>
#include"game.h"
void menu()
	{
		printf("****************************\n");
		printf("****  1.play   0. exit  ****\n");
		printf("****************************\n");
	}
//游戏实现
void game()
{
	char ret=0;
	//数组存放走出的棋盘信息
	char arr[SYY][CYY]={0};//全部空格     定义格式  syy、cyy是自己控制格式
	//初始化棋盘
	initarr(arr, SYY, CYY);
	//打印棋盘
	displayarr(arr, SYY, CYY);
	//下棋
	while(1)
	{
		//玩家下棋
		playermove(arr,SYY,CYY);
		displayarr(arr,SYY,CYY);
		//判断玩家是否赢
		ret = iswin(arr,SYY,CYY);
		if(ret !='c' )
		{
			break;
		}
		//电脑下棋
		computermove(arr,SYY,CYY);
		displayarr(arr,SYY,CYY);
		//判断电脑是否赢
		ret = iswin(arr,SYY,CYY);
		if(ret !='c')
		{
			break;
		}
	}
	if(ret !='*')
	{
		printf("玩家赢\n");
	}
	else if(ret != '#')
	{
		printf("电脑赢\n");
	}
	else
	{
		printf("平局\n");
	}
}
void test()
{
	int input=0;
	do{
		menu();
		printf("输入数字:>");
		scanf("%d",&input);
		switch(input)
		{
		case 1:
			game();//猜数字游戏
			break;
		case 0:
			printf("退出游戏\n");break;
		default :
			printf("选择错误\n");break;
		}

	}while(input);
}
int main()
{
	test();
	return 0;
}
解释函数文件
#define _CRT_SECURE_NO_WARNINGS 1
#include"game.h"
void initarr(int arr[SYY][CYY],int syy,int cyy)
{
	int i=0;
	int j=0;
	for(i=0;i<syy;i++)
	{
		for(j=0;j<cyy;j++)
		{
			arr[i][j]= ' ';
		}
	}
}
void displayarr(char arr[SYY][CYY],int syy,int cyy)
{
	int i=0;
	for(i=0;i<syy;i++)
	{
	int j=0;
	for(j=0;j<cyy;j++)
	{
			//1.打印一行的数据
			printf(" %c ",arr[i][j]);
			if(j<cyy-1)
				printf("|");
	}
	printf("\n");
		//2.打印分割行
		if(i<syy-1)
		{
			for(j=0;j<cyy;j++)
			{
				printf("---");
				if(j<cyy-1)
					printf("|");
			}
		}
		printf("\n");
	}
	
}
void playermove(char arr[SYY][CYY],int syy, int cyy)
{
	int x=0;
	int y=0;
	printf("玩家走：\n");
	while(1)
	{
		printf("请输入要下的坐标：");
		scanf("%d%d",&x,&y);
		//判断下x,y坐标的合法性
		if(x>=1&&x<=syy&&y>=1&&y<=cyy)
		{
			if(arr[x-1][y-1]=' ')
			{
				arr[x-1][y-1]='*';
				break;
			}
			else
			{
				printf("该坐标被占用\n");
			}
		}
		else
		{
			printf("坐标非法，请重新输入！\n");
		}
	}
}
void computermove(char arr[SYY][CYY],int syy,int cyy)
{
	int x=0;
	int y=0;
	printf("电脑走：\n");
	while(1)
	{
		x=rand()%syy;
		y=rand()%cyy;
		if(arr[x][y]==' ')
		{
			arr[x][y]='#';
			break;
		}
	}
}
//返回1，表示棋盘满了
//返,0，表示棋盘没满
int isfull(char arr[SYY][CYY],int syy,int cyy)
{
	int i=0;
	int j=0;
	for(i=0;i<syy;i++)
	{
		for(j=0;j<cyy;j++)
		{
			if(arr[i][j]==' ')
			{
				return 0;//棋盘没满
			}
		}
	}
	return 1;//棋盘满了
}
char iswin(char arr[SYY][CYY],int syy,int cyy)
{
	int i=0;
	//横三排
	for(i=0;i<syy;i++)
	{
		if(arr[i][0]==arr[i][1]&&arr[i][1]==arr[i][2]&&arr[i][1]!=' ')
			return arr[i][1];
	}
	//竖三排
	for(i=0;i<cyy;i++)
	{
		if(arr[0][i]==arr[1][i]&&arr[1][i]==arr[2][i]&&arr[1][i]!=' ')
			return arr[1][i];
	}
	//对角线
	if(arr[0][0]==arr[1][1]&&arr[1][1]==arr[2][2]&&arr[1][1]!=' ')
		return arr[1][1];
	if(arr[2][0]==arr[1][1]&&arr[1][1]==arr[0][2]&&arr[1][1]!=' ')
		return arr[1][1];
	//判断平局 
	if(1==isfull(arr,SYY,CYY))
	{
		return 'q';
	}
	return 'c';
}
头文件（引函数）
#define SYY 3
#define CYY 3
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<time.h>
//声明
void initarr(char arr[SYY][CYY],int syy,int cyy);
void displayarr(char arr[SYY][CYY],int syy,int cyy);
void playermove(char arr[SYY][CYY],int syy,int cyy);
void computermove(char arr[SYY][CYY],int syy,int cyy);
//游戏中的四种状态
//玩家赢 --'*'
//电脑赢 --'#'
//平局 --'q' 
//继续 --'c'
char iswin(char arr[SYY][CYY],int syy,int cyy);
