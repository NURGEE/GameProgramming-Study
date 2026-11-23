# 10주차(지렁이게임, 함수 오버로딩, 클래스, 객체지향)

**함수 오버로딩(오버로드)**

**: 함수 이름이 같으며 매개변수 개수가 다른 것.**

**ex) void A();**

**void A(int a);** 

**이런 식으로 내용이 다르고 이름이 같은 함수 생성 가능.**

**(반환 형도 오버 로드 와 같은 식임)**

지렁이 게임 완성

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

// 커서 보이기 여부 설정
void SetCursorVisible(bool isVisible);

// 지렁이
// head, tail을 제외한 지렁이 body 출력
void PrintWorm(stWormBody* pHead, stWormBody* pTail);

// 키보드 입력 및 지렁이 위치 변경
void MoveWorm(stWormBody* pHead, stWormBody* pTail);

// 이전 프레임의 지렁이 마지막 몸통 제거 
void HidePreviousFrame(stWormBody* pTail);

// 지렁이 몸통 추가
void AddBody(stWormBody* pTail);

// 지렁이 몽통 메모리 해제
void DeleteBodies(stWormBody* pHead);

// 아이템

// 아이템 생성
void CreateItem(stItem* pItemList);
void CreateItem(stItem* pItemList, stWormBody* pHead);

// 아이템 출력
void PrintItem(stItem* pItemList);

// 아이템 삭제
void RemoveItem(stItem* pItemList, stItem* pTargetItem);
// 아이템 충돌
bool IsHitItem(stItem* pItemList, stWormBody* pFirstBody);
// 아이템 메모리 해제
void DeleteItems(stItem* pItemList);

// 벽 충돌
bool IsGameOver(stWormBody* pHead);

// 점수 출력
void PrintScore(const int nScore);

int main()
{
	char cKey = ' ';
	do
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

		while (_kbhit())
		{
			cKey = _getch();
		}
		Sleep(DELAY_TIME);
		cKey = _getch();
	} while (cKey != 'q' && cKey != 'Q');

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
	SetCursorVisible(false);
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
		while (_kbhit())
		{
			cKey = _getch();
		}
	

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

void CreateItem(stItem* pItemList)
{
	if (pItemList != nullptr)
	{
		stItem* pItem = new stItem();
		pItem->m_pNextItem = pItemList->m_pNextItem;
		pItemList->m_pNextItem = pItem; // 앞에다 추가
		pItem->m_nPositionX = 3 + rand() % (MAP_WIDTH - 3);
		pItem->m_nPositionY = 3 + rand() % (MAP_HEIGHT - 3);
		//rand() 함수에 원하는 랜덤값의 최대값 + 1 을 % 연산 하면 0 ~ 원하는 랜덤값의 최대값 나옴.

	}
	else
	{

	}
}

void CreateItem(stItem* pItemList, stWormBody* pHead)
{
	if (pItemList != nullptr)
	{
		bool isSamePosition = false;
		int nX = 0;
		int nY = 0;
		do
		{
			isSamePosition = false;
			nX = 3 + rand() % (MAP_WIDTH - 3);
			nY = 3 + rand() % (MAP_HEIGHT - 3);

			stItem* pCurrentItem = pItemList->m_pNextItem;
			while (!isSamePosition && pCurrentItem != nullptr)
			{
				if (pCurrentItem->m_nPositionX == nX && pCurrentItem->m_nPositionY == nY)
				{
					isSamePosition = true;
				}
				else
				{
					pCurrentItem = pCurrentItem->m_pNextItem;
				}
			}

			stWormBody* pCurrentBody = pHead->m_pNextBody;
			while (!isSamePosition && pCurrentBody->m_pNextBody != nullptr)
			{
				if (pCurrentBody->m_nPositionX == nX && pCurrentBody->m_nPositionY == nY)
				{
					isSamePosition = true;
				}
				else
				{
					pCurrentBody = pCurrentBody->m_pNextBody;
				}
			}
		} while (isSamePosition);

		stItem* pItem = new stItem();
		pItem->m_pNextItem = pItemList->m_pNextItem;
		pItemList->m_pNextItem = pItem;
		pItem->m_nPositionX = nX;
		pItem->m_nPositionY = nY;

		// rand() 함수에 원하는 랜덤값의 최대값 + 1을 %연산 하면 0~ 원하는 랜던값의 최대값
	}
	else
	{

	}
}

void DrawGame(stWormBody* pHead, stWormBody* pTail)
{
	stWormBody* pFirstBody = pHead->m_pNextBody;
	//item 리스트의 head
	stItem sItemList = stItem();

	sItemList.m_pNextItem = nullptr;
	int nItemCount = 0;
	int nScore = 0;
	bool isGameOver = false;

	while (!isGameOver)
	{
		MoveWorm(pHead, pTail);
		isGameOver = IsGameOver(pHead);

		
		if (isGameOver)
		{
			//게임 종료
		}
		else
		{
			bool isHit = IsHitItem(&sItemList, pHead);
			if (isHit)
			{
				AddBody(pTail);
				nItemCount--;
				nScore++;
			}
			else
			{
			}

			if (nItemCount < MAX_ITEM_COUNT)
			{
				CreateItem(&sItemList, pHead);
				nItemCount++;
			}
			else
			{
			}

			PrintWorm(pHead, pTail);
			PrintItem(&sItemList);
			PrintScore(nScore);
			Sleep(DELAY_TIME);
		}
	}

	system("cls");
	printf("Game Over\n");
	printf("score : %d\n", nScore);
	printf("press any key to restart\n");
	printf("press Q to Quit");

	DeleteBodies(pHead);
	DeleteItems(&sItemList);
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
	while (pCurrentItem->m_pNextItem != nullptr && pTargetItem != nullptr)
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

bool IsGameOver(stWormBody* pHead)
{
	bool isHit = false;
	if (pHead != nullptr && pHead->m_pNextBody->m_pNextBody != nullptr)
	{
		stWormBody* pFirstBody = pHead->m_pNextBody;
		if (pFirstBody->m_nPositionX <= 1 || pFirstBody->m_nPositionX >= MAP_WIDTH ||
			pFirstBody->m_nPositionY <= 1 || pFirstBody->m_nPositionY >= MAP_HEIGHT)
		{
			isHit = true;
		}
		else
		{
			// FirstBody를 제외한 나머지 몸통
			stWormBody* pBody = pFirstBody->m_pNextBody;
			while (pBody->m_pNextBody != nullptr)
			{
				if (pFirstBody->m_nPositionX == pBody->m_nPositionX &&
					pFirstBody->m_nPositionY == pBody->m_nPositionY)
				{
					isHit = true;
				}
				else
				{
				}
				pBody = pBody->m_pNextBody;
			}
		}
	}
	else
	{
		//1. haed가 nullptr인 경우
		//2. head 다음 몸통이 없고 바로 tail인 경우
		isHit = true;
	}
	return isHit;
}
void DeleteBodies(stWormBody* pHead)
{
	if (pHead != nullptr)
	{
		stWormBody* pBody = pHead->m_pNextBody;
		while (pBody != nullptr && pBody->m_pNextBody != nullptr)
		{
			pBody->m_pNextBody->m_pPrevBody = pBody->m_pPrevBody;
			pBody->m_pPrevBody->m_pNextBody = pBody->m_pNextBody;
			delete pBody;
			pBody = pHead->m_pNextBody;
		}
	}
	else
	{
	
	}
}

void DeleteItems(stItem* pItemList)
{
	stItem* pItem = pItemList->m_pNextItem;
	if (pItem != nullptr)
	{
		while (pItem->m_pNextItem != nullptr)
		{
				while (pItem->m_pNextItem->m_pNextItem != nullptr)
				{
					pItem = pItem->m_pNextItem;
				}
				delete pItem->m_pNextItem;
				pItem->m_pNextItem = nullptr;
				pItem = pItemList;
		}
	}
}

void PrintScore(const int nScore)
{
	SetCursorPosition(MAP_WIDTH + 3, 3);
	printf("Score : %d", nScore);
}

void SetCursorVisible(bool isVisible)
{
	CONSOLE_CURSOR_INFO cursorInfo;
	cursorInfo.bVisible = isVisible;
	cursorInfo.dwSize = 1;

	HANDLE handle = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleCursorInfo(handle, &cursorInfo);
}
```

```
클래스
C++ 객체 지향 언어 : 큰 틀이 되는 무언가?

접근 지정자 : 클래스 멤버 요소에 접근이 가능한 범위를 지정.
	1) 외부에서 접근 가능한 대상 (public)
	2) 내부에서만 접근 가능한 대상 (private)
	3) 외부에서는 접근 불가능, 내부와 자식에서는 접근 가능(protected)
	생성자, 소멸자

상속 : 상속 받은 대상을 자식이라 부르고, 상속을 한 대상을 부모라함.
	1) 일반적으로 부모 클래스는 자식클래스의 기능 접근 못함.
	2) 자식 클래스는 접근지정자 특징 1,3번 해당하면 접근 가능.
	함수 오버로드와 비슷한 단어가 나옴
	가상함수
	순수 가상 함수

프렌드 키워드 : 특정 대상에 한하여 접근이 가능

싱글턴 : 객체 지향 언의 모순
		하나의 인스턴스를 생성해서 어디서 누가 접근하든 그 인스턴스만 접근함.

오퍼레이터 : 클래스에 대한 연산자를 정의한다.

클래스 외 용어?

namespace : 해당 대상을 접근하기전에 특정 이름으로 그룹 지어서 그 이름을 통해서 접근하도록 지정

템플릿 : 함수의 매개변수를 특정 자료형으로 지정하지 않고 사용자가 입력한 자료형으로 사용

	
```

