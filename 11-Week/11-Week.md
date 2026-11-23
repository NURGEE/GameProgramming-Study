# 11주차(클래스)

꿀팁? : 마우스로 하면 작업속도가 느려짐 그래서 단축키 활용

현재 파일에서 마지막 으로 봤던 파일로 갈때 : ctrl(짧게) + tab

해당 정의? 선언?으로 가는 단축키 : F12

정의 자동 생성 : ctrl + . 안되면 윈도우 한영키 바꾸기

생성자, 소멸자 함수(public)

20240518.cpp

```
#include <iostream>
#include "Cnode.h"

int main()
{
	CNode cNode = CNode("a");
	CNode cNodeB = CNode("b");
	CNode cNodeC = CNode("c");
	CNode cNodeD = CNode("d");
	CNode cNodeE = CNode("e");
	printf("\n");

	char szName[100];
	for (int i = 0; i < 5; i++)
	{
		sprintf_s(szName, "%c", 'g' + i); // printf로 복사 szName은 복사 받을곳, %c부분 바뀜
		CNode* pNode = new CNode(szName);

		delete pNode;
	}
	printf("\n");
}
```

CNode.cpp

```
#include "CNode.h"
#include <iostream>

//CNode::CNode()
//{
//	printf("CNode의 생성자 호출\n");
//}

CNode::CNode(const char* pName)
{
	strcpy_s(szName, 100, pName);
	printf("%s의 생성자 호출\n", szName);
}

CNode::~CNode()
{
	//printf("CNode의 소멸자 호출\n"); 

	/*char cInput;
	scanf_s("%c", &cInput, 1);*/

	printf("%s의 소멸자 호출\n", szName);
}

```

CNode.h

```
#pragma once

// 주로 헤더파일에 작성 됨
// 여러 파일에서 include했을 때 컴퓨터의 파일에 대한 중복 처리를 막음.

class CNode
{
	//생성자 : 함수와 형태가 유사하지만 반환 형태를 작성하지 않음.

public: // 이 줄 아래로는 전부 public에 포함 됨.
		// public은 외부에서 접근 가능함.
	
	
	//생성자는 클래스의 이름 + 괄호 + ;
	//메모리 공간에 CNode라는 클래스의 변수가 할당되면 호출되는 애
	//CNode(); 
	CNode(const char* pName);

	//소멸자 : 메모리에서 해제될 때 호출
	~CNode(); 

	char szName[100];
};
```

```

```

```

```

```

```

```

```