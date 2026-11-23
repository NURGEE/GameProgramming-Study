# 4주차(포인터, 깊은 복사, 얕은 복사, CallbyValue, CallByReference, 동적 할당, 연결 리스트)

```
****포인터 : 자료형에는 상관없이 데이터의 메모리 '주소 값을 저장'
printf할 때 format : %p**

**포인터 변수
자료형* 변수이름 = 값;
ex)
변수 : int nData = 0;
포인터 : int* pData = nullptr; (NULL) (nullptr-> 공통으로 제공하는 키워드 NULL과 같음)
void* pData2 = nullptr;**

**NULL; -> 다른 컴퓨터에 비쥬얼 스튜디오 버전에 따라 값이 다를수도 있음
nullptr -> 현재 비쥬얼 스튜디오 기준 파란색 : c++프로젝트에서 공통적으로 제공되어 동일하게 사용 가능**

**ex)
int nData = 0;
int* pData = nullptr;**

```
**if(pData == nullptr)
{
    pData = &nData; &변수이름: 해당 변수의 주소값
}**

```

**메모리의 주소 값을 알았으니 접근을 해야함
접근 : 그 메모리에 있는 데이터를 얻겠다
*포인터 변수 이름 == 이미 담겨있는 주소 값을 가지고 있는 변수의 데이터
*pData           == nData**

**printf("nData의 값 : %d\n", nData);
printf("pData의 값 : %d\n", *pData);
int nData2 = nData;
printf("nData2의 값 : %d\n", nData2);**

**깊은 복사 : 메모리 자체를 받은것
ex) nData2 = nData;  →   이 후에 nData를 바꾸면 nData2에는 영향이 없음.
파일을 복사 + 붙여넣기**

**얕은 복사 : 메모리의 주소를 받은것
ex) pData = &nData;  → 이 후에 nData를 바꾸면 주소를 받은 것이기 때문에 영향을 받음.
파일의 바로가기**

**void CallByvalue(int nData); -> 일반 
void CallByReference1(int* pData); -> 포인터
void CallByReference2(int& nData); -> 참조**

**const int nData = 10;
int nData2 = 2;
int nData3 = 3;
const int* pData = &nData2;
int* const pData2 = &nData2;**

**pData = &nData3;
pData2 = &nData3; // int* const : error**

***pData = 10;    //const int** : error
*pData2 = 20;**

**포인터 변수이름만 작성하면 주소 값을 의미
포인터 변수이름 앞에 *를 붙이면 그 주소 값에 있는 변수의 값을 의미
pData == &nData
*pData == nData****

```

```
**동적 (메모리)할당
C : malloc함수
자료형* pData = (자료형*)malloc(자료형크기);
ex) int* pData = (int*)malloc(sizeof(int));

C++ : new 키워드 (생성자를 자동으로 호출해준다)
int* pData2 = new int();

동적 할당한 메모리를 해제
C : free함수
free(pData);

delete 키워드
delete pData2;

※ <u>malloc을 사용하였으면 free로 해제</u>
   <u>new를 사용하였으면 delete로 해제</u>

<u>malloc 장점 크기 변경 가능</u> 나중에 개발할때 생각해서 쓰는게 좋을듯
new는 크기 변경 불가능

※ tip! : <u>할당하는 코드를 적었으면 해제해야할 타이밍 부분에 해제하는 코드부터 작성</u>

ex 1) 
	int* pdata = nullptr;
	if(pData == nullptr)
		{
			pData = new int();
			*pData = 10;
		}
	else
		{
			*pData = 10;
		}
delete pData;

ex 2) ex1과 상관없는 코드

	if(pData != nullptr)
		{
			delete pData;
		}
		else
		{
			null이 아닐때 쓸 내용;
		}
질문1. 여기서 만약 ex1과 2가 연결 되어있다면?
-> 해결 : delete 다음 줄에 null, nullptr로 초기화 해주면 됨.
질문2. 여기서 만약 delete를 하지 않으면?

int* pData = nullptr;
int nData = 0;
pData = &pData;
<u>free(pData);</u> // 정적 할당 했을때만 지워야함. 위에 코드는 컴퓨터가 알아서 메모리 해제               
                해 줄거임.**

```

```

**struct 키워드 : 구조체

1. 사용자가 정의한 새로운 자료형
2. 특정 형태가 정해지지 않음
3. 함수내에 작성X

struct 구조체 이름
{
    멤버 변수 (int nData ->int m_nData)
};

ex) Person 구조체
struct stPerson
{
    char m_szName[100];
    int m_nAge;
    int m_nHeight;
    int m_nWeight;
};

int main()
{
	<u>Person 변수이름1 = Person();</u>
	<u>Person* 변수이름2 = new Person();</u>

	변수이름1.Person의 멤버변수 = 초기화;
	변수이름2->Person의 멤버변수 = 초기화;
	delete 변수이름2;
}

구조체 응용 : 연결 리스트

struct stNode
{
    //각 노드의 구분자
    int m_nID;

    // 다음 노드를 알기 위한 구조체 형태의 포인터
    stNode* m_pNextNode;

};

int main()
{
  // H
  stNode nodeHead = stNode();
  // T
  stNode nodeTail = stNode();

  nodeHead.m_nID = -1;
  nodeHead.m_pNextNode = &nodeTail;

  nodeTail.m_nID = -1;
  nodeTail.m_pNextNode = nullptr;

  //P
  stNode* pCurrentNode = &nodeHead;

  //새로생성한 노드
  stNode* pCreatedNode = new stNode();
  //1. 새로생성한 노드의 N은 P의 N이다.
  pCreatedNode->m_nID = pCreatedNode->m_nID + 1;
  //2. P의 N은 새로생성한 노드다.
  pCurrentNode->m_pNextNode = pCreatedNode;
  //3. P는 새로생성한 노드다.
  pCurrentNode = pCreatedNode;

}

코드 수정

int main()
{
	// H
	stNode nodeHead = stNode();
	// T
	stNode nodeTail = stNode();

	nodeHead.m_nID = -1;
	nodeHead.m_pNextNode = &nodeTail;

	/*nodeTail.m_nID = -1;*/
	// 위 주석 대신 이 코드 사용
		nodeHead.m_pNextNode->m_nID = nodeHead.m_nID + 1; // nodeHead.m_pNextNode->m_nID                     == nodeTail.m_nID
// nodeHead. m_pNextNode->m_nID = nodeHead.m_nID +1; == nodeTail.m_nID = nodeHead.m_nID +1;

nodeTail.m_pNextNode = nullptr;

//P
stNode* pCurrentNode = &nodeHead;

//새로생성한 노드
stNode* pCreatedNode = new stNode();
//1. 새로생성한 노드의 N은 P의 N이다.
pCreatedNode->m_nID = pCreatedNode->m_nID + 1;
//2. P의 N은 새로생성한 노드다.
pCurrentNode->m_pNextNode = pCreatedNode;
//3. P는 새로생성한 노드다.
pCurrentNode = pCreatedNode;

//코드 추가
pCurrentNode->m_pNextNode->m_nID = pCurrentNode->m_nID + 1;

	
	

}**
```

****

