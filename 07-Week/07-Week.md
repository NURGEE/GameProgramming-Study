# 7주차(양방향 연결리스트 연습, 2차원 배열)

양방향 연결리스트 구조체, 생성, 삭제 연습

```
#include <iostream>

struct stNode {
	int index;
	stNode* next;
	stNode* prev;
};

void DeleteNode();

int main()
{
	stNode nodeHead;
	stNode nodeTail;

	nodeHead.index = -1;
	nodeTail.index = nodeHead.index + 1;
	nodeHead.next = &nodeTail;
	nodeHead.prev = nullptr;
	nodeTail.prev = &nodeHead;
	nodeTail.next = nullptr;

	stNode* conNode = &nodeHead;
	conNode->prev = nodeTail.prev;
	int num = 0;
	scanf_s("%d", &num);
	
	for (int i = 0; i < num; i++)
	{
		stNode* newNode = new stNode;
		
		newNode->next = conNode->next;
		newNode->prev = conNode;
		newNode->index = newNode->next->index;
		newNode->next->index = newNode->index + 1;
		//conNode->next->prev = newNode;
		newNode->prev->next = newNode;

		//conNode->next = newNode;
		newNode->next->prev = newNode;

		conNode = newNode;

	}
	
	conNode = &nodeHead;
	/*for (int i = 0; i < num ; i++)
	{
		printf("현재 노드의 주소값 %p, 현재 노드의 인덱스 %d, 이전 노드의 주소값 %p, 다음 노드의 주소 값 : %p\n", &conNode, conNode->index, &conNode->prev, &conNode->next);
		conNode = conNode->next;
	}*/
	while (conNode != nullptr)
	{
		printf("현재 노드의 주소값 %p, 현재 노드의 인덱스 %d, 이전 노드의 주소값 %p, 다음 노드의 주소 값 : %p\n", &conNode, conNode->index, &conNode->prev, &conNode->next);
		conNode = conNode->next;
	}

	conNode = &nodeHead;
	/*for (int i = 0; i < num; i++)
	{
		if (conNode->next != nullptr)
		{
			stNode* delNode = conNode->next;
			conNode->next = conNode->next->next;

			
			delete delNode;
		}
	}*/
	conNode = &nodeHead;
	while (conNode->next != &nodeTail)
	{
		stNode* delNode = conNode->next;
		conNode->next = conNode->next->next;
		conNode->next->prev = conNode;
		while (conNode->next->next != nullptr)
		{
			conNode->next->index = conNode->index + 1;
			conNode = conNode->next;
		}
		conNode->next->index = conNode->index + 1;
		delete delNode;

	}
	

}

void DeleteNode()
{

}

```

배열
포인터
int arrValues[5] = { 10,20,30,40,50 };

int* pArray = new int[5];
pArray[0]

arrValues : 배열 첫 번째 요소의 주소 값
int* pValues = arrValues;

/* printf("pValue : %d\n", *pValues);
pValues++;*/
//arrValues[0] == 11;

포인터에 증감연산자를 사용하면 저장된 주소값 +1 의미가 아닌
포인터의 자료형태의 크기만큼 주소값에 더해줌

5개 짜리 배열 arrValues에서
첫 번쨰 요소의 주소 값 : 0x12345678 16진수
두번째 요소 : 0x1234567C
세 번째 요소 : 0x12345680
네 번째 요소 : 0x12345684
다섯 번째 요소 : 0x12345688

//2차원 배열
int arrValues[5][3] = { {1,2,3}, {4,5,6,}, {7,8,9}, {10,11,12}, {13,14,15} };
printf("%d\n", arrValues[0][0]);

