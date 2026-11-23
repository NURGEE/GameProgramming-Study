# 8주차(이중(다중)포인터, 지렁이 게임 만들기)

```
// 20240427.cpp : 이 파일에는 'main' 함수가 포함됩니다. 거기서 프로그램 실행이 시작되고 종료됩니다.
//

#include <iostream>

int main()
{
	// 5 * n 형태의 배열 ( 포이터 + 1차원 배열)
	int* pValues[5] = {};
	for (int i = 0; i < 5; i++)
	{
		pValues[i] = new int[5];
	}

	// m * n 형태의 배열( 2중 포인터 사용)
	int** pValues2 = nullptr;
	pValues2 = new int* [5];
	for (int i = 0; i < 5; i++)
	{
		pValues2[i] = new int[5];
	}

	//1. pValues의 메모리 해제
	for (int i = 0; i < 5; i++)
	{
		delete[] pValues[i]; 
		// 초기화 까지 하면 
		pValues[i] = nullptr;
	}

	//2. pValues2의 메모리 해제
	for (int i = 0; i < 5; i++)
	{
		// 배열 i번째에 있는 내용을 먼저 지우고(그렇지 않으면 메모리 누수생김)
		delete[] pValues2[i]; 
		// 초기화 까지 하면 
		pValues2[i] = nullptr;
	}
	//이제는 배열을 지운다.
	delete[] pValues2;
	pValues2 = nullptr;
	}

```

지렁이 게임 

현재 진행도 

![Untitled](%EA%B2%8C%EC%9E%84%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(C++)/Untitled%201.png)

```
#include <iostream>
#include <time.h>
#include <conio.h>
#include <Windows.h>

// 맵 넓이
#define MAP_WIDTH 50
// 맵 높이
#define MAP_HEIGHT 25 

//키보드 방향키 설정
#define LEFT_ARROW_KY 75
#define RIGHT_ARROW_KY 77
#define UP_ARROW_KY 72
#define DOWN_ARROW_KY 80

//맵의 아이템 최대 개수
#define MAX_ITEM_COUNT 2
//다음 프레임 대기 시간
#define DELAY_TIME 50

struct stWormBody
{
	//몸통 x좌표
	int m_nPositionX;
	//몸통 y좌표
	int m_nPositionY;

	//진행 방향
	int m_nDirection;

	//다음 노드
	stWormBody* m_pNextBody;
	//이전 노드 
	stWormBody* m_pPrevBody;
};

struct stItem
{
	// x좌표
	int m_nPositionX;
	// y좌표
	int m_nPositionY;
	// 다음 아이템
	stItem* m_pNextItem;

};

// 콘솔창의 커서 위치 이동
void SetCursorPosition(int nX, int nY);

// 초기화
void Initialize(); // Init();

// 기본 맵 그리기
void SetDefaultMap();

// 게임화면 전시
void DrawGame(stWormBody* pHead, stWormBody* pTail);

// 지렁이
// head, tail을 제외한 지렁이 body 출력
void PrintWorm(stWormBody* pHead, stWormBody* pTail);

// 키보드 입력 및 지렁이 위치 변경
void MoveWorm(stWormBody* pHead, stWormBody* pTail);

// 이전 프레임의 지렁이 마지막 몸통 제거 
void HidePreviousFrame(stWormBody* pTail);

// 지렁이 몸통 추가
void AddBody(stWormBody* pTail);

// 아이템

// 아이템 생성
void CreateItem(stItem* pDropTable);

// 아이템 출력
void PrintItem(stItem* pItemList);

// 아이템 삭제
void RemoveItem(stItem* pItemList, stItem* pTargetItem);
// 충돌
bool IsHitItem(stItem* pItemList, stWormBody* pFirstBody);

int main()
{
	stWormBody sHead = stWormBody();
	stWormBody sTail = stWormBody();

	Initialize();
	sHead.m_pNextBody = new stWormBody();
	sHead.m_pPrevBody = nullptr;
	sHead.m_pNextBody->m_pPrevBody = &sHead;
	sHead.m_pNextBody->m_pNextBody = &sTail;

	sTail.m_pPrevBody = sHead.m_pNextBody;
	sTail.m_pNextBody = nullptr;

	sHead.m_pNextBody->m_nPositionX = MAP_WIDTH / 2;
	sHead.m_pNextBody->m_nPositionY = MAP_HEIGHT / 2;

	DrawGame(&sHead, &sTail);

	/*
		MoveWorm(&sHead, &sTail);
		PrintWorm(&sHead, &sTail);
		Sleep(DELAY_TIME);
	*/

}

void SetCursorPosition(int nX, int nY)
{
	COORD cursorPosition = COORD();
	cursorPosition.X = nX + 30;
	cursorPosition.Y = nY + 3;

	//Handle : 윈도우 창에 대한 포인터
	HANDLE hHandle = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleCursorPosition(hHandle, cursorPosition);
}

void Initialize()
{
	system("mode con cols=100 lines=40 | title 지렁이게임"); // 콘솔창 크기 랑 타이틀
	time_t t = time(NULL);
	srand(t);
	SetDefaultMap();

}

void SetDefaultMap()
{
	system("cls");
	for (int i = 1; i < MAP_WIDTH; i++) //가로에 대한 맵 구현 모서리 부분은 제외
	{
		SetCursorPosition(i, 0);
		printf("─");
		SetCursorPosition(i, MAP_HEIGHT);
		printf("─");
	}

	for (int i = 1; i < MAP_HEIGHT; i++) //세로에 대한 맵 구현 모서리 부분 제외
	{
		SetCursorPosition(0, i);
		printf("│");
		SetCursorPosition(MAP_WIDTH, i);
		printf("│");
	}

	SetCursorPosition(0, 0);
	printf("┌");
	SetCursorPosition(MAP_WIDTH, 0);
	printf("┐");
	SetCursorPosition(0, MAP_HEIGHT);
	printf("└");
	SetCursorPosition(MAP_WIDTH, MAP_HEIGHT);
	printf("┘");

}

void PrintWorm(stWormBody* pHead, stWormBody* pTail)
{
	stWormBody* pCurrentBody = pHead->m_pNextBody;
	while (pCurrentBody != nullptr && pCurrentBody != pTail)
	{
		SetCursorPosition(pCurrentBody->m_nPositionX, pCurrentBody->m_nPositionY);
		printf("0");
		pCurrentBody = pCurrentBody->m_pNextBody;
	}
}

void MoveWorm(stWormBody* pHead, stWormBody* pTail)
{
	HidePreviousFrame(pTail);
	stWormBody* pFirstbody = pHead->m_pNextBody;
	//키보드 눌림 감지
	if (_kbhit() != 0)
	{
		//입력 받은 키 값
		char cKey = _getch();
		switch (cKey)
		{
		case LEFT_ARROW_KY:
			if (pFirstbody->m_nDirection != RIGHT_ARROW_KY)
			{
				pFirstbody->m_nDirection = LEFT_ARROW_KY;
			}
			else
			{

			}
			break;
		case RIGHT_ARROW_KY:
			if (pFirstbody->m_nDirection != LEFT_ARROW_KY)
			{
				pFirstbody->m_nDirection = RIGHT_ARROW_KY;
			}
			else
			{

			}
			break;
		case UP_ARROW_KY:
			if (pFirstbody->m_nDirection != DOWN_ARROW_KY)
			{
				pFirstbody->m_nDirection = UP_ARROW_KY;
			}
			else
			{

			}
			break;
		case DOWN_ARROW_KY:
			if (pFirstbody->m_nDirection != UP_ARROW_KY)
			{
				pFirstbody->m_nDirection = DOWN_ARROW_KY;
			}
			else
			{

			}
			break;
		default:
			break;
		}
	}
	else
	{

	}

	stWormBody* pCurrentbody = pTail->m_pPrevBody;

	while (pCurrentbody != nullptr && pCurrentbody != pHead && pCurrentbody != pFirstbody)
	{
		pCurrentbody->m_nDirection = pCurrentbody->m_pPrevBody->m_nDirection;

		pCurrentbody->m_nPositionX = pCurrentbody->m_pPrevBody->m_nPositionX;
		pCurrentbody->m_nPositionY = pCurrentbody->m_pPrevBody->m_nPositionY;

		pCurrentbody = pCurrentbody->m_pPrevBody;
	}

	if (pFirstbody->m_nDirection == LEFT_ARROW_KY)
	{
		pFirstbody->m_nPositionX--;
	}
	else if (pFirstbody->m_nDirection == RIGHT_ARROW_KY)
	{
		pFirstbody->m_nPositionX++;
	}
	else if (pFirstbody->m_nDirection == UP_ARROW_KY)
	{
		pFirstbody->m_nPositionY--;
	}
	else if (pFirstbody->m_nDirection == DOWN_ARROW_KY)
	{
		pFirstbody->m_nPositionY++;
	}
	else
	{
		//현재는 처리 없음 필요시 다른 키에 대한 처리 추가
	}
}

//HidePreviousFrame 함수를 MoveWorm의 어느 부분에서 호출해야하나? -> body들의 포지션이 변경되기 전이면 어디든 가능
void HidePreviousFrame(stWormBody* pTail)
{
	stWormBody* pLastBody = pTail->m_pPrevBody;

	if (pLastBody != nullptr && pLastBody->m_pPrevBody != nullptr)
	{
		SetCursorPosition(pLastBody->m_nPositionX, pLastBody->m_nPositionY);
		printf(" ");
	}
	else
	{

	}
}

void CreateItem(stItem* pDropTable)
{
	if (pDropTable != nullptr)
	{
		stItem* pItem = new stItem();
		pItem->m_pNextItem = pDropTable->m_pNextItem;
		pDropTable->m_pNextItem = pItem; // 앞에다 추가
		pItem->m_nPositionX = 3 + rand() % (MAP_WIDTH - 3);
		pItem->m_nPositionY = 3 + rand() % (MAP_HEIGHT - 3);
		//rand() 함수에 원하는 랜덤값의 최대값 + 1 을 % 연산 하면 0 ~ 원하는 랜덤값의 최대값 나옴.

	}
	else
	{

	}
}

void DrawGame(stWormBody* pHead, stWormBody* pTail)
{
	stWormBody* pFirstBody = pHead->m_pNextBody;
	stItem sItemList = stItem();

	sItemList.m_pNextItem = nullptr;
	int nItemCount = 0;

	while (true)
	{
		MoveWorm(pHead, pTail);

		bool isHit = IsHitItem(&sItemList, pHead);

		if (isHit)
		{
			AddBody(pTail);
			nItemCount--;
		}
		else
		{

		}

		if (nItemCount < MAX_ITEM_COUNT)
		{
			CreateItem(&sItemList);
			nItemCount++;
		}
		PrintWorm(pHead, pTail);
		PrintItem(&sItemList);
		Sleep(DELAY_TIME);
	}
}

void PrintItem(stItem* pItemList)
{
	stItem* pItem = pItemList->m_pNextItem;
	while (pItem != nullptr)
	{
		SetCursorPosition(pItem->m_nPositionX, pItem->m_nPositionY);
		printf("C");
		pItem = pItem->m_pNextItem;
	}
}

bool IsHitItem(stItem* pItemList, stWormBody* pHead)
{
	bool isHit = false;

	stItem* pCurrentItem = pItemList->m_pNextItem;
	stWormBody* pCurrentBody = pHead->m_pNextBody;

	while (pCurrentBody != nullptr && pCurrentItem != nullptr && pCurrentBody->m_pNextBody != nullptr && !isHit)
	{
		if (pCurrentItem->m_nPositionX == pCurrentBody->m_nPositionX && pCurrentItem->m_nPositionY == pCurrentBody->m_nPositionY)
		{
			isHit = true;
			RemoveItem(pItemList, pCurrentItem);
			pCurrentItem = nullptr;
		}
		else
		{
			pCurrentItem = pCurrentItem->m_pNextItem;
		}
	}

	return isHit;
}

void AddBody(stWormBody* pTail)
{
	stWormBody* pBody = new stWormBody();
	pBody->m_pPrevBody = pTail->m_pPrevBody;
	pBody->m_pNextBody = pTail;

	pBody->m_pPrevBody->m_pNextBody = pBody;
	pBody->m_pNextBody->m_pPrevBody = pBody;

	pBody->m_nPositionX = pBody->m_pPrevBody->m_nPositionX;
	pBody->m_nPositionY = pBody->m_pPrevBody->m_nPositionY;
	pBody->m_nDirection = pBody->m_pPrevBody->m_nDirection;

	if (pBody->m_nDirection == LEFT_ARROW_KY)
	{
		pBody->m_nPositionX++;
	}
	else if (pBody->m_nDirection == RIGHT_ARROW_KY)
	{
		pBody->m_nPositionX--;
	}
	else if (pBody->m_nDirection == UP_ARROW_KY)
	{
		pBody->m_nPositionY++;
	}
	else if (pBody->m_nDirection == DOWN_ARROW_KY)
	{
		pBody->m_nPositionY--;;
	}
	else
	{

	}
}
void RemoveItem(stItem* pItemList, stItem* pTargetItem)
{
	stItem* pCurrentItem = pItemList;
	while (pCurrentItem->m_pNextItem != nullptr)
	{
		if (pCurrentItem->m_pNextItem == pTargetItem)
		{
			pCurrentItem->m_pNextItem = pTargetItem->m_pNextItem;
			delete pTargetItem;
			pTargetItem = nullptr;
		}
		else
		{
			pCurrentItem = pCurrentItem->m_pNextItem;
		}
		
	}
}
```

```

```

