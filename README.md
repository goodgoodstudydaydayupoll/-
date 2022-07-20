#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
typedef int ElemType;
typedef struct
{
	ElemType* elem;//整形指针
	int Tablelen;//存储动态数组里边元素的个数
}SSTable;
int Search_Seq(SSTable ST, ElemType key)
{
	ST.elem[0] = key;//让零号元素为哨兵
	int i;
	for (i = ST.Tablelen - 1; ST.elem[i] != key; --i)
	return i;
}
void ST_Init(SSTable& ST, int len)//随机数生成
{
	ST.Tablelen = len + 1;//多申请了一个位置，为了存哨兵
	ST.elem = (ElemType*)malloc(sizeof(ElemType) * ST.Tablelen);
	int i;
	srand(time(NULL));//随机数生成
	for (i = 0; i < ST.Tablelen; i++)//为啥这里零号位置也随机了数据，为折半查找服务
	{
		ST.elem[i] = rand() % 100;
	}
}
void ST_print(SSTable ST)
{
	for (int i = 0; i < ST.Tablelen; i++)
	{
		printf("%3d", ST.elem[i]);
	}
	printf("\n");
}
int Binary_Search(SSTable L, ElemType key)
{
	int low = 0, high = L.Tablelen - 1, mid;
	while (low <= high)
	{
		mid = (low + high) / 2;
		if (L.elem[mid] == key)
			return mid;
		else if (L.elem[mid] > key)
			high = mid - 1;
		else
			low = mid + 1;
	}
	return -1;
}
int compare(const void* left, const void* right)//left,right是任意两个元素的地址值
{
	return *(ElemType*)left - *(ElemType*)right;
}
int main()
{
	SSTable ST;
	ElemType key;
	int pos;
	ST_Init(ST, 10);
	ST_print(ST);
	printf("请输入要搜素的key值:\n");
	scanf("%d", &key);
	pos = Search_Seq(ST, key);
	if (pos)
	{
		printf("查找成功 位置为 %d\n", pos);
	}
	else
	{
		printf("查找失败\n");
	}
	qsort(ST.elem, ST.Tablelen, sizeof(int), compare);
	ST_print(ST);
	printf("二分查找，请输入要查找的key值：\n");
	scanf("%d", &key);
	pos = Binary_Search(ST, key);
	if (pos != -1)
	{
		printf("查找成功 位置为 %d\n", pos);
	}
	else
	{
		printf("查找失败\n");
	}
	system("pause");
	//return 0;
}
