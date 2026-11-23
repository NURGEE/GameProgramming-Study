# 2주차( for, while, do while문, for each문, 함수)

for문 형식

for(1. 초기화(1회 실행하는 코드); 2. 반복할 조건; 3. 반복을 하고 나서 실행할 내용)

for(int i = 0; i < 5; i++)
{
반복할 내용물
}

**for문의 1,2,3번 생략 가능 ⇒ 무한 반복 가능**

<u>**for문에 초기화 한 값은 for문 밖에 출력 X**</u>

**while문 형식**

while(조건) 

{

반복할 내용물

}

**do while문 형식**

do
{
   반복할 내용물
} while(조건);

**for each문**

   for each(요소의 자료형 요소의 이름 in 리스트, 배열)

{

  반복할 내용

}

**함수 선언**

**자료형(반환 상태) 함수 이름();**
main.cpp 기준으로 main보다는 위에 작성
단, 함수 내에 작성 불가능

**함수 정의**

**자료형(반환형태) 함수이름()
{
     함수가 수행하는 내용
 } -> ; 이 붙지 않음**

선언 보다는 아래에 작성
함수 내에 작성 불가능

**함수 호출**

반환 형태를 생략하고
함수 이름();

**반환**

**int A();**

**int A()
{
int nData = 0;
return nData;
//코드가 실행 중에 return을 만나면, 뒤의 내용을 실행 안함.**

**}**

**void**

형태가 없음을 의미
반환형으로 적을 경우에는 return을 생략 가능 or return;

**매개변수(인자)**

1. 함수의 소괄호 안에 작성하는 변수
2. 변수의 개수 제한X	
3. 변수와 변수 구분은 ,로 작성
4. 호출하는 쪽에서 값을 입력해 주어야 함
5. 함수 내에서 사용가능

함수 선언과 동시에 정의

void A()
{

```c
};
main.cpp 기준으로 main보다는 ㅜ이에 작성
단, 함수 내에 작성 불가능

```

**매개 변수의 기본 값 설정 : 선언 부분에 있는 매개변수에 = 값**

**호출하는 쪽에서 입력하지 않고 생략 가능
기본 값을 가지는 변수들은 모두 뒤에 작성**

**int main()**

**{**

**int nSumResult = Sum(0, 1);
printf("nSumResult : %d\n", nSumResult);
nSumResult = Sum(10, 20);
printf("nSumResult : %d\n", nSumResult);
nSumResult = Sum(15, 30);
printf("nSumResult : %d\n", nSumResult);**

**}**

**int Sum(int nValue1, int nValue2)
{
int nResult = nValue1 + nValue2;
return nResult;
}**

**전역 변수 : main() 밖에 있는 변수를 적용 하면 전역 변수라함.** 

**지역 변수 : main() 안에 있는 변수를 정의 한 것이 지역 변수라함.**

