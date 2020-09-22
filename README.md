//深度优先
#include<stdio.h>
#define DM 15
#define PATH 20
#define CLOSE 100000
typedef int BOOL;
#define TRUE  1
#define FALSE 0
int a[3][3];
int b[CLOSE][3][3]; 
int c[PATH][3][3]; 
int i0,j0;
void DFS(int i0,int j0);
void exchange(int *a,int *b)
{
	int temp;
	temp=*a;
	*a=*b;
	*b=temp;
}
int blank_left(int i0,int j0)
{ 
	exchange(&a[i0][j0-1],&a[i0][j0]);
} 
int blank_right(int i0,int j0)
{ 
	exchange(&a[i0][j0+1],&a[i0][j0]);
}
int blank_up(int i0,int j0)
{ 
	exchange(&a[i0-1][j0],&a[i0][j0]);
}
int blank_down(int i0,int j0)
{
	exchange(&a[i0+1][j0],&a[i0][j0]);
}
void show_matrix(int a[3][3])
{ 
	int i,j;
	for(i=0;i<3;i++)
	{
		for(j=0;j<3;j++)
		{
			printf("%d ",a[i][j]);
		}
		printf("\n");
	}
	printf("\n");
}
void save_matrix(int b[3][3],int a[3][3])
{
	int i,j;
	for(i=0;i<3;i++)
	{
		for(j=0;j<3;j++)
		{
			b[i][j]=a[i][j];
		}
	}
}
BOOL ComparePath(int num)
{ 
	if(a[0][0]==c[num][0][0]&&
	a[0][1]==c[num][0][1]&&
	a[0][2]==c[num][0][2]&&
	a[1][0]==c[num][1][0]&&
	a[1][1]==c[num][1][1]&&
	a[1][2]==c[num][1][2]&&
	a[2][0]==c[num][2][0]&&
	a[2][1]==c[num][2][1]&&
	a[2][2]==c[num][2][2])
		return TRUE;
	else
		return FALSE;
}
BOOL CompareClose(int num)
{ 
	if(a[0][0]==b[num][0][0]&&
	a[0][1]==b[num][0][1]&&
	a[0][2]==b[num][0][2]&&
	a[1][0]==b[num][1][0]&&
	a[1][1]==b[num][1][1]&&
	a[1][2]==b[num][1][2]&&
	a[2][0]==b[num][2][0]&&
	a[2][1]==b[num][2][1]&&
	a[2][2]==b[num][2][2])
		return TRUE;
	else
		return FALSE;
}
BOOL CompareAll(int close,int path)
{
	int i;
	for(i=0;i<path;i++)
	{
		if(ComparePath(i)==TRUE)//在表Path中 
			return TRUE;
	}
	for(i=0;i<close;i++)//在表close中 
	{
		if(CompareClose(i)==TRUE)
			return TRUE;
	}
	return FALSE;
} 
BOOL IsTravel(int f,int close,int path,int i0,int j0)
{
	BOOL flag=FALSE;
	if(f==0)
	{
		blank_left(i0,j0);
		if(CompareAll(close,path)==TRUE)
			flag=TRUE;
		blank_right(i0,j0-1);
	}
	else if(f==1)
	{
		blank_up(i0,j0);
		if(CompareAll(close,path)==TRUE)
			flag=TRUE;
		blank_down(i0-1,j0);
	}
	else if(f==2)
	{
		blank_right(i0,j0);
		if(CompareAll(close,path)==TRUE)
			flag=TRUE;
		blank_left(i0,j0+1);
	}
	else if(f==3)
	{
		blank_down(i0,j0);
		if(CompareAll(close,path)==TRUE)
			flag=TRUE;
		blank_up(i0+1,j0);
	}
	return flag;
} 
BOOL FINAL()
{
	if(a[0][0]==1&&
	a[0][1]==2&&
	a[0][2]==3&&
	a[1][0]==8&&
	a[1][1]==0&&
	a[1][2]==4&&
	a[2][0]==7&&
	a[2][1]==6&&
	a[2][2]==5)
		return TRUE;
	else
		return FALSE;
}



int main()
{
	int i,j;
	int i0=0,j0=1;
	FILE * fp;
	fp=fopen("a.txt","r");
	for(i=0;i<3;i++)
	{
	for(j=0;j<3;j++)
		{
			fscanf(fp,"%d",&a[i][j]);
		}
	}	
	DFS(i0,j0); 
	return 0;
}

void DFS(int i0,int j0)
{
	int deep=0; 
	int close=0;
	int path=0;
	
	int i;
while(TRUE)
{
	if(j0!=0&&IsTravel(0,close,path,i0,j0)==FALSE&&path<=DM)
	{ 
		save_matrix(c[path],a);
		path++;
		deep++;
		
		blank_left(i0,j0);
		j0--;
	}
	else if(i0!=0&&IsTravel(1,close,path,i0,j0)==FALSE&&path<=DM)
	{
		save_matrix(c[path],a);
		path++;
		deep++;
		
		blank_up(i0,j0);
		i0--;
	}
	else if(j0!=2&&IsTravel(2,close,path,i0,j0)==FALSE&&path<=DM)
	{
		save_matrix(c[path],a);
		path++;
		deep++;
		
		blank_right(i0,j0);
		j0++;
	}
	else if(i0!=2&&IsTravel(3,close,path,i0,j0)==FALSE&&path<=DM)
	{
		save_matrix(c[path],a);
		path++;
		deep++;
		
		blank_down(i0,j0);
		i0++;
	}
	else
	{
		save_matrix(b[close],a);
		close++;//存入close表
		path--;
		save_matrix(a,c[path]);
		int m,n;
		for(m=0;m<3;m++)
		{
			for(n=0;n<3;n++)
				if(a[m][n]==0)
				{
					i0=m;
					j0=n;
				}
		 } 
	}
	if(FINAL()==TRUE)
	{
		printf("FIND!\n");	
		break;
	}
}
	for(i=0;i<path;i++)
	{
		show_matrix(c[i]);
	}
	show_matrix(a);
}


(m=0;m<3;m++)
		{
			for(n=0;n<3;n++)
				if(a[m][n]==0)
				{
					i0=m;
					j0=n;
				}
		 } 
	}
	if(FINAL()==TRUE)
	{
		printf("FIND!\n");	
		break;
	}
}
	for(i=0;i<path;i++)
	{
		show_matrix(c[i]);
	}
	show_matrix(a);
}
//广度优先
#include <iostream>
#include <string>
#include <cstring>
#include <cmath>
#include <vector>
#include <queue>
#include <set>
using namespace std;
#define N 9
int jc[N+1]={1,1,2,6,24,120,720,5040,40320,362880};
typedef struct data
{
    int arr[N];
    int hash;
    int pos;
    int step;
}Node;
 
int dir[4][2]={
    {0,1},
    {1,0},
    {0,-1},
    {-1,0}
};
int cantor(int arr[N])
{
    int i,j;
    int sum=0;
    for( i=0;i<N;i++)
    {
        int nmin=0;
        for(j=i+1;j<N;j++)
        {
            if(arr[i]>arr[j])
                nmin++;
        }
        sum+=(nmin*jc[N-i-1]);
 
    }
 
    return sum;
}
void swap(int *arr,int i,int j)
{
    int t=arr[i];
    arr[i]=arr[j];
    arr[j]=t;
}
void printArray(int * arr)
{
    int i,j;
    for (i=0;i<N;i++)
    {
        if(i%3==0)
            cout<<"\n";
        cout<<arr[i]<<" ";
    }
    cout<<endl;
}
void copyArray(int src[N],int target[N])
{
    int i;
    for(i=0;i<N;i++)
    {
        target[i]=src[i];
    }
 
}
int bfs(int arr[N],int sHash,int tHash)
{
    if(sHash==tHash)
        return 0;
    int i,j;
    queue<Node> q;
    set<int> setHash;
    Node now,next;
    copyArray(arr,now.arr);
    int pos=0;
    for (i=0;i<N;i++)
    {
        if(arr[i]==0)
            break;
        pos++;
    }
    now.hash=sHash;
    now.step=0;
    next.step=0;
    now.pos=pos;
    q.push(now);
    setHash.insert(now.hash);
    while(!q.empty())
    {
        now=q.front();
        q.pop();
        for (i=0;i<4;i++)
        {
            int offsetX=0,offsetY=0;
            offsetX=(now.pos%3+dir[i][0]);
            offsetY=(now.pos/3+dir[i][1]);
 
            if(offsetX>=0&&offsetX<3&&offsetY<3&&offsetY>=0)
            {
                copyArray(now.arr,next.arr);
                next.step=now.step;
 
                next.step++;
                swap(next.arr,now.pos,offsetY*3+offsetX);
 
                next.hash=cantor(next.arr);
                next.pos=(offsetY*3+offsetX);
                int begin=setHash.size();
                setHash.insert(next.hash);
                int end=setHash.size();
 
                if(next.hash==tHash){
 
                    return next.step;
                }
 
                if(end>begin)
                {
 
                    q.push(next);
                }
 
            }
        }
 
    }
    return -1;
}
int inversion(int arr[N])
{
    int sum=0;
    for(int i=0;i<N;++i)
    {
        for(int j=i+1;j<N;++j)
        {
            if(arr[j]<arr[i]&&arr[j]!=0)
            {
                sum++;
            }
        }
    }
 
    return sum;
 
}
int main(int argc, char **argv)
{
    int i,j;
    string s="123456780";
    string t="123456078";
    int is[N],it[N];
    for (i=0;i<9;i++)
    {
        if (s.at(i)>='0'&&s.at(i)<='8')
        {
            is[i]=s.at(i)-'0';
        }else
        {
            is[i]=0;
        }
        if (t.at(i)>='0'&&t.at(i)<='8')
        {
            it[i]=t.at(i)-'0';
        }else
        {
            it[i]=0;
        }
    }
    int sHash,tHash;
 
    sHash=cantor(is);
    tHash=cantor(it);
    int inver1=inversion(is);
    int inver2=inversion(it);
    if((inver1+inver2)%2==0)
    {
        int step=bfs(is,sHash,tHash);
        cout<<step<<endl;
    }
    else
    {
        cout<<"无法从初始状态到达目标状态"<<endl;
    }
 
    return 0;
}
