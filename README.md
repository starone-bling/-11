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
#include<stdio.h>
struct node
{
    int xy[3][3];
    int dir;
};
struct node sh[102], end;
int count = 1;
 
void init()
{
    printf("输入起始节点的位置:n");
    int i, j;
    for (i = 0; i < 3; i++)
        for (j = 0; j < 3; j++)
            scanf("%d", &sh[0].xy[i][j]);
    sh[0].dir = -1;
    printf("输入目标节点的位置:n");
    for (i = 0; i < 3; i++)
        for (j = 0; j < 3; j++)
            scanf("%d", &sh[101].xy[i][j]);
    sh[101].dir = -1;
}
int loction(int num)
{
    int i;
    for (i = 0; i < 9; i++)
        if (sh[num].xy[i / 3][i % 3] == 0) return i;
}
long long sign(int num)
{
    long long  sum;
    sum = sh[num].xy[0][0]*100000000 + sh[num].xy[0][1]*10000000 + sh[num].xy[0][2]*1000000 + sh[num].xy[1][0]*100000 + sh[num].xy[1][1]*10000 + sh[num].xy[1][2]*1000 + sh[num].xy[2][0]*100 + sh[num].xy[2][1]*10 + sh[num].xy[2][2];
    return sum;
}
 
void mobile(int num)
{
 
    int temp;
    int loc;
    int up = 1, down = 1, left = 1, right = 1;
    loc = loction(num);
    int stand = sh[num].dir;
    if (loc / 3 != 0 && stand != 1)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3 - 1][loc % 3];
        sh[count].xy[loc / 3 - 1][loc % 3] = temp;
        sh[count].dir = 3;
        count++;
    };
    if (loc / 3 != 2 && stand != 3)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3 + 1][loc % 3];
        sh[count].xy[loc / 3 + 1][loc % 3] = temp;
        sh[count].dir = 1;
        count++;
    }
    if (loc % 3 != 0 && stand != 0)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3][loc % 3 - 1];
        sh[count].xy[loc / 3][loc % 3 - 1] = temp;
        sh[count].dir = 2;
        count++;
    }
    if (loc % 3 != 2 && stand != 2)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3][loc % 3 + 1];
        sh[count].xy[loc / 3][loc % 3 + 1] = temp;
        sh[count].dir = 0;
        count++;
    }
 
}
void display(int num)
{
    int i, j;
    for (i = 0; i < 3; i++)
    {
        for (j = 0; j < 3; j++)
            printf("%d ", sh[num].xy[i][j]);
        printf("n");
    }
}
 
int search()
{
    int i = 0;
    while (1)
    {
        printf("n");
        display(i);
        printf("n");
        if (i == 100)
        {
            printf("超出了上限次数n");
            return 0;
        }
        if (sign(i) == sign(101))
        {
            printf("在第%d次找到了", i);
            display(i);
            return i;
        }
        mobile(i);
        i++;
    }
}
 
int main()
{
    init();
    search();
    return 0;
}
