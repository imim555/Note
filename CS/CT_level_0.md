
# 대소문자 바꿔서 출력하기

영어 알파벳으로 이루어진 문자열 `str`이 주어집니다. 각 알파벳을 대문자는 소문자로 소문자는 대문자로 변환해서 출력하는 코드를 작성해 보세요.

### 코드
```python
str = input()
result = ''

for i in str:
    if i.isupper() :
        result += i.lower()
    else:
        result += i.upper()

print(result)
```

```
print(''.join(x.upper() if x == x.lower() else x.lower() for x in input()))
```


# 특수문자 출력하기


다음과 같이 출력하도록 코드를 작성해 주세요.

```
!@#$%^&*(\'"<>?:;
```


```python
print(r'!@#$%^&*(\'"<>?:;')

print("\n") //줄바꿈
print("\t") //수평 탭(Tab)
print("\\") // \ 백슬래시
print("\"") // " 큰 따옴표
print("\'") // ' 작은 따옴표

```


# 덧셈식 출력하기


두 정수 `a`, `b`가 주어질 때 다음과 같은 형태의 계산식을 출력하는 코드를 작성해 보세요.


```
a, b = map(int, input().strip().split(' '))
print('{} + {} = {}'.format(a,b,a+b))
```


# 문자열 붙여서 출력하기


두 개의 문자열 `str1`, `str2`가 공백으로 구분되어 입력으로 주어집니다.  
입출력 예와 같이 `str1`과 `str2`을 이어서 출력하는 코드를 작성해 보세요.

```
print(input().strip().replace(' ', ''))
```


# 문자열 돌리기

문자열 `str`이 주어집니다.  
문자열을 시계방향으로 90도 돌려서 아래 입출력 예와 같이 출력하는 코드를 작성해 보세요.

```
print('\n'.join(input()))
```


# 홀짝 구분하기


자연수 `n`이 입력으로 주어졌을 때 만약 `n`이 짝수이면 "`n` is even"을, 홀수이면 "`n` is odd"를 출력하는 코드를 작성해 보세요.


```
a = int(input())
print('{} is odd'.format(a)) if a%2 else print('{} is even'.format(a))  
```



```
N = int(input())
print(f"{N} is {'even' if N % 2 == 0 else 'odd'}")
```


# 문자열 겹쳐쓰기

문자열 `my_string`, `overwrite_string`과 정수 `s`가 주어집니다. 문자열 `my_string`의 인덱스 `s`부터 `overwrite_string`의 길이만큼을 문자열 `overwrite_string`으로 바꾼 문자열을 return 하는 solution 함수를 작성해 주세요.

```
def solution(my_string, overwrite_string, s):
	return my_string[:s] + overwrite_string + my_string[s + len(overwrite_string):]

```


