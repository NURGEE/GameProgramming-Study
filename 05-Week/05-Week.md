# 5주차(연결리스트 문제(구조체, 생성, 삭제))

**연결리스트 작성해보기**

```
struct stNode // 연결리스트 구조체
{
int m_nIndex;
stNode* m_pNextNode;
};

int main()
{
stNode nodeHead;
stNode nodeTail;

**nodeHead.m_nIndex = -1;
nodeHead.m_pNextNode = &nodeTail;
nodeHead.m_pNextNode->m_nIndex = nodeHead.m_nIndex + 1;
nodeHead.m_pNextNode->m_pNextNode = nullptr;

stNode* pCurrentNode = &nodeHead;

int nNodeCount = 3;
<u>/*scanf_s("%d", &nNodeCount);*/</u> 주석 처리한부분 틀린부분 

for (int i = 0; i < nNodeCount; i++)
{
	stNode* pNode = new stNode();
	pNode->m_pNextNode = pCurrentNode->m_pNextNode;
	
	<u>/*pNode->m_nIndex = pCurrentNode->m_pNextNode->m_nIndex + 1;*/</u>
	
	pNode->m_nIndex = pCurrentNode->m_pNextNode->m_nIndex;
	pCurrentNode->m_pNextNode->m_nIndex++;
	
	/*
	<u>pCurrentNode->m_pNextNode = pNode->m_pNextNode;
	pCurrentNode->m_pNextNode = pNode;</u>
	*/
	
	pCurrentNode->m_pNextNode = pNode;
	pCurrentNode = pNode;

}

pCurrentNode = &nodeHead;
for (int i = 0; i < nNodeCount + 2; i++) // stNode* pNode = &nodeHead; pNode != nullptr; pNode = pNode->m_pNextNode 로 해도 됨.
{
	printf("node index : %d, adress : %p, next : %p\\n", pCurrentNode->m_nIndex, pCurrentNode, pCurrentNode->m_pNextNode);
}
}**

```

```

```

---

// 모든 노드 삭제 head의 인덱스는 -1로 고정, tail의 인덱스는 개수
// 뒤에서부터 삭제

pCurrentNode = &nodeHead;
while (pCurrentNode->m_pNextNode != &nodeTail)
{
while (pCurrentNode->m_pNextNode->m_pNextNode != &nodeTail)
{
pCurrentNode = pCurrentNode->m_pNextNode;
} 

*// pCurrentNode->m_pNextNode->m_pNextNode == &nodeTail;
// pCurrentNode->m_pNextNode == tail 이전의 마지막 노드
// pCurrentNode tail의 전 전 노드
stNode* pDeleteNode = pCurrentNode->m_pNextNode;
// pCurrentNode->m_pNextNode = pCurrentNode->m_pNextNode->m_pNextNode;
pCurrentNode->m_pNextNode = pDeleteNode->m_pNextNode;
pCurrentNode->m_pNextNode->m_nIndex--;
delete pDeleteNode;

pCurrentNode = &nodeHead;

*/

// 앞에서부터 삭제
pCurrentNode = &nodeHead;
while (pCurrentNode->m_pNextNode != &nodeTail)
{
stNode* pDeleteNode = pCurrentNode->m_pNextNode;
pCurrentNode->m_pNextNode = pCurrentNode->m_pNextNode->m_pNextNode;
//pCurrentNode->m_pNextNode->m_nIndex = pCurrentNode->m_nIndex + 1;

```
while (pCurrentNode->m_pNextNode->m_pNextNode != nullptr)
{
	pCurrentNode->m_pNextNode->m_nIndex = pCurrentNode->m_nIndex + 1;
	pCurrentNode = pCurrentNode->m_pNextNode;
}
pCurrentNode->m_pNextNode->m_nIndex = pCurrentNode->m_nIndex + 1;

delete pDeleteNode;
pCurrentNode = &nodeHead;

```

}

```
struct stNode 
{
    int m_nIndex;
    stNode* m_pNextNode;
};

int main()
{
    stNode HeadNode;
    stNode TailNode;

    /*HeadNode.m_pNextNode = &TailNode;
    HeadNode.m_nIndex = -1;
    TailNode.m_pNextNode = nullptr;
    TailNode.m_nIndex = 0;*/
   
    HeadNode.m_nIndex = -1;
    HeadNode.m_pNextNode = &TailNode;
    HeadNode.m_pNextNode->m_nIndex = HeadNode.m_nIndex+1;
    HeadNode.m_pNextNode->m_pNextNode = nullptr;

    stNode* pCurrentNode = &HeadNode;

    int nCountNode = 0;
    scanf_s("%d", &nCountNode);
    
    //삽입
    for (int i = 0; i < nCountNode; i++)
    {
        stNode* newNode = new stNode();
        newNode->m_pNextNode = pCurrentNode->m_pNextNode;
        newNode->m_nIndex = pCurrentNode->m_pNextNode->m_nIndex;
        pCurrentNode->m_pNextNode->m_nIndex++;
        pCurrentNode->m_pNextNode = newNode;
        pCurrentNode = newNode;
    }

    pCurrentNode = &HeadNode;
    //출력
    for (int i = 0; i < nCountNode + 2; i++)
    {
        printf("index : %d adress : %p next adress : %p\n", pCurrentNode->m_nIndex, pCurrentNode ,pCurrentNode->m_pNextNode);
        pCurrentNode = pCurrentNode->m_pNextNode;
    }

    //삭제
    pCurrentNode = &HeadNode;
    
    for (int i = 0; i < nCountNode + 2; i++)
    {
        
        if (HeadNode.m_pNextNode != &TailNode)
        {
            stNode* delNode = pCurrentNode->m_pNextNode;
            pCurrentNode->m_pNextNode = pCurrentNode->m_pNextNode->m_pNextNode;

            pCurrentNode->m_pNextNode->m_nIndex = pCurrentNode->m_nIndex + 1;           
          
            delete delNode;
            //pCurrentNode = &HeadNode;
        }
    }
}
```

