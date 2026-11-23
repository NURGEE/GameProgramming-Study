# 3주차(배열, 정렬, const, define)

**배열(변수) 선언**

**자료형 변수이름[개수];**

**자료형 변수이름[개수] = {66, 23 , 10, 5}; 입력한 순서대로 요소에 값이 복사됨**

**자료형 변수이름[개수] = {66,23};  개수가 숫자보다 많을 경우 나머지는 0으로 초기화**

**자료형 변수이름[개수] = {}; 전부 0으로 초기화**

**배열의 요소에 접근해야 데이터를 얻을 수 있음**

**2차원 배열 등 많음 ex) 자료형 변수이름[개수][개수];**

**요소에 접근하는 방법 : 변수이름[요소 번호]**

**요소의 번호의 범위 : 0이상 개수 미만**

**변수 이름[요소번호] = 10;**

**(변수이름[요소번호]++;**

**int nData = 변수 이름[요소번호];** 

**ex) int arrDatas[4] = {1, 2, 3, 4};**

**memset(arrDatas, 0 , sizeof(16)) → arrDatas를 16바이트의 메모리를 0으로 초기화** 

**memset(변수의 주소값, 0, 메모리사이즈)**

**sizeof() ()안에 변수명, 자료형 등 넣어도 됨.**

**sizeof(변수이름) or sizeof(자료형)**

**int arrDatas[100];**

**ex) int nArraySize = sizeof(arrDatas);**

**printf(”%d\n”, nArraySize);**

**ex) int nIntSize = sizeof(int);**

**int nArrayLength = nArraySize / nIntSize;**

**printf(”%d\n”, nArrayLength);**

**정렬 : 정렬 되지 않은 배열 같은 것들을 낮은, 높은 순으로 정렬하는 방식**

**정렬 종류 : 선택, 버블, 삽입, 등 있음**

**Src : 복사를 받을 대상**

**Dst : 복사할 값**

**memcpy_s(Src의 주소값, Src의 크기, Dst의 주소값, Dst의 크기);**

**memcpy_s(szText, 10, szText2, 10); //memory copy**

**strcpy_s(Src의 주소값, 복사할 크기, 문자열);**

**strcpy_s(szText,10,szText2);**

**printf(”%s\n”, szText);**

**strlen(szText); = 문자열 길이 출력(변수명)**

**Const 키워드 : 변수의 값을 고정하겠다. ( 변수의 상수화) → 함수 선언, 매개변수 가능**

**Static 키워드 : 뭔가를 정적으로 바꾼다. 즉, 프로그램 실행될 때부터 종료할 때까지 메모리해제가 이뤄지지 않음. → static 자료형 변수명 = 초기화; 를 하면 한번만 초기화 해주고 다음 반복에는 초기화를 하지 않는다.** 

**ex) void Statictext()**

**{**

**int nData = 0; → 초기화가 계속됨.**

**nData++;**

**printf(”nData : %d\n”, nData);**

**static int nStaticdata = 0; → static은 초기화 한번만 사용.**

**nStaticData++;**

**printf(”nStataicData : %d\n”, nStaticData);**

**}**

<u>**#define</u> ARRAY_SIZE 10 → ARRAY_SIZE 이름을 뒤에 있는 값으로 사용하겠다.**

