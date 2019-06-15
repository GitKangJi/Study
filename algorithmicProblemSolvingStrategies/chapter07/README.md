7장 분할 정복
================

## 01. 도입
#### &nbsp;분할 정복(Divide & Conquer)은 가장 유명한 알고리즘 디자인 패러다임으로, 각개 격파라는 말로 간단히 설명할 수 있다. 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 이용해 계산하고, 각 부분 문제의 답으로부터 전체 문제의 답을 계산해 낸다. 분할 정복이 일반적인 재귀 호출과 다른 점은 문제를 한 조각과 나머지 전체로 나누는 대신 거의 같은 크기의 부분 문제로 나누는 것이다. 분할 정복을 사용하는 알고리즘들은 대개 세 가지의 구성 요소를 갖고 있다.
#### 1. 문제를 더 작은 문제로 분할하는 과정(Divide)
#### 2. 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정(Merge)
#### 3. 더이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제(Base case)
#### 분할 정복은 많은 경우 같은 작업을 더 빠르게 처리해 준다.

* ### 예제: 수열의 빠른 합과 행렬의 빠른 제곱
#### &nbsp;1부터 n까지의 합을 재귀 호출을 이용해 계산하는 recursiveSum() 함수를 작성했었다. 여기서 분할 정복을 이용해 똑같은 일을 하는 fastSum() 함수를 만들어 본다. 편의상 n은 짝수라고 가정한다. 1부터 n까지 합을 구할 때 (1 + 2 + ... + (n/2)) + ((n/2 + 1) + ... + n)으로 나타낼 수 있다. 첫 번째 부분은 fastSum(n / 2)로 나타낼 수 있지만, 두 번째 부분 문제는 그렇지 않다. 두 번째 부분을 바꿔보면 (n/2 + 1) + (n/2 + 2) + ... + (n/2 + n/2) = (n/2) * (n/2) + (1 + 2 + 3 + ... + (n/2)) = (n/2)^2 + fastSum(n/2)로 나타낼 수 있다. 따라서 fastSum(n) = 2 * fastSum(n/2) + (n/2)^2 (n이 짝수 일때) 로 나타낼 수 있다.
```c++
// 필수 조건: n은 자연수
// 1 + 2 + 3 + ... + n을 반환한다.
int fastSum(int n) {
  // 기저 사례
  if(n == 1) return 1;
  if(n % 2 == 1) return fastSum(n - 1) + n;
  return 2 * fastSum(n/2) + (n/2) * (n/2);
}
```

* ### 시간 복잡도 분석
#### &nbsp;fastSum()과 recursiveSum()은 반복문이 없기 때문에 순전히 함수가 호출되는 횟수에 비례한다. recursiveSum()은 n번 함수의 호출이 필요하다. 하지만 fastSum()은 호출될 때마다 최소한 두 번에 한 번 꼴로 n이 절반으로 줄어든다. 그러니 fastSum()의 호출 횟수가 훨씬 적으리란 것을 쉽게 예상할 수 있다. fastSum()의 알고리즘 실행 시간은 O(lgn)이 된다.

* ### 행렬의 거듭제곱
#### &nbsp;n * n 크기의 행렬 A가 주어질 때 거듭제곱(power)을 계산하는 알고리즘 자체는 어려울 것이 없지만, m이 매우 클 때 A^m을 구하는 것은 꽤나 시간이 오래 걸리는 작업이다. 행렬의 곱셈에는 O(n^3)번의 연산이 필요하다. 여기서 분할 정복을 이용하면 눔깜짝할 새에 이 값을 구할 수 있다. A^m을 구하는데 필요한 m개의 조각을 절반으로 나눈다.
```c++
// 정방행렬을 표현하는 SquareMatrix 클래스가 있다고 가정한다.
class SquareMatrix;
// n*n 크기의 항등 행렬(identity matrix)을 반환하는 함수
SquareMatrix identity(int n);
// A^m을 반환한다.
SquareMatrix pow(const SquareMatrix& A, int m) {
  // 기저 사례: A^0 = I
  if(m == 0) return identity(A.size());
  if(m % 2 > 0) return pow(A, m - 2) * A;
  SquareMatrix half = pow(A, m / 2);
  // A^m = (A^(m/2) * (A^(m/2))
  return half * half;
}
```

* ### 나누어 떨어지지 않을 때의 분할과 시간 복잡도
#### &nbsp;m이 홀수일 때, A^m = A*A^(m-1)로 나누지 않고, 좀더 절반에 가깝게 나누는 게 좋지 않을까라는 생각을 할 수도 있습니다. 그러나 이 문제에서 이 방식은 오히려 알고리즘을 더 느리게 만든다. 좀더 절반에 가깝게 나눠서 계산하는 경우는 결국 m-1번 곱셈을 하는 것과 다를바가 없다. 반면 원래 하던 방식으로 하면 시간 복잡도는 O(lgn)이 된다.

* ### 예제: 접합 정렬과 퀵 정렬
#### &nbsp;정렬 알고리즘 중 가장 널리 쓰이는 것이 병합 정렬(Merge sort)와 퀵 정렬(Quick sort)인데, 이 두 알고리즘은 모두 분할 정복 패러다임을 기반으로 해서 만들어진 것이다. 같은 문제를 해결하는 알고리즘이라도 어떤 식으로 분할하느냐에 따라 다른 알고리즘이 될 수 있으며, 이들 모두는 분할 정복 패러다임을 사용한 알고리즘이라고 할 수 있다.

* ### 시간 복잡도 분석
#### &nbsp;병합 정렬과 퀵 정렬은 본질적으로 비슷한 형태의 알고리즘이기 때문에, 시간 복잡도를 비슷한 방법으로 분석할 수 있다. 병합 정렬의 수행 시간은 병합 과정에 의해 지배된다. 그런데 아래 단계로 내려갈수록 부분 문제의 수는 두 배로 늘고 각 부분 문제의 크기는 바능로 줄어들기 때문에, 한 단계 내에서 모든 병합에 필요한 총 시간은 O(n)으로 항상 일정하다. 단계의 수에 n을 곱하면 병합 정렬에 필요한 전체 시간을 얻을 수 있다. 문제의 크기는 항상 거의 절반으로 나누어 지기 때문에, 필요한 단게의 수는 O(lgn)이 된다. 따라서 병합 정렬의 시간 복잡도는 O(nlgn)이다. 퀵 정렬의 경우 대부분의 시간을 차지하는 것은 주어진 문제를 두 개의 부분 문제로 나누는 파티션 과정이다. 파티션에는 주어진 수열의 길이에 비례하는 시간이 걸린다. 사실 병합 정렬에서의 병합 과정과 다를 것이 없다. 하지만 최악의 경우에는 O(n^2)이 된다. 그래서 대부분의 퀵 정렬 구현은 가능한 한 절반에 가까운 분할을 얻기 위해 좋은 기준을 뽑는 다향한 방법들을 사용한다.

* ### 예제: 카라츠바의 빠른 곱셈 알고리즘
#### &nbsp;수백, 수만 자리는 되는 큰 숫자들을 다룰 때 주로 사용된다.
```c++
// num[]의 자릿수 올림을 처리한다.
void normalize(vector<int>& num) {
  num.push_back(0);
  // 자리수 올림을 처리한다.
  for(int i = 0; i < num.size(); ++i) {
    if(num[i] < 0) {
      int borrow = (abs(num[i]) + 9) / 10;
      num[i + 1] -= borrow;
      num[i] += borrow * 10;
    } else {
      num[i + 1] += num[i] / 10;
      num[i] %= 10;
    }
  }
  while(num.size() > 1 && num.back() == 0) num.pop_back();
}
// 두 긴 자연수의 곱을 반환한다.
// 각 배열에는 각 수의 자릿수가 1의 자리에서부터 시작해 저장되어 있다.
// 예: multiply({3, 2, 1}, {6, 5, 4}) = 123 * 456 = 56088 = {8, 8, 0, 6, 5}
vector<int> multiply(const vector<int>& a, const vector<int>& b) {
  vector<int> c(a.size() + b.size() + 1, 0);
  for(int i = 0; i < a.size(); ++i)
    for(int j = 0; j < b.size(); ++j)
      c[i + j] += a[i] * b[j];
  normalize(c);
  return c;
}
```
#### 두 정수의 길이가 모두 n이라고 할 때 O(n^2)이다. 이것보다 빠른 알고리즘이 카라츠바 알고리즘이다. 만약 a, b가 256자리 수라면 a = a1 * 10^128 + a0와 b = b1 * 10^128 + b0라고 표현할 수 있다. 따라서 a * b = (a1 * 10^128 + a0) * (b1 * 10^128 + b0) = a1 * b1 * 10^256 + (a1 * b0 + a0 * b1) * 10^128 + a0 * b0으로 표현할 수 있다. 큰 정수 두개를 한 번 곱하는 대신, 절반 크기로 나눈 작은 조각을 네 번 곱한다. 이때 카라츠바가 발견한 것은 a * b를 이전과 같이 네 번 대신 세 번의 곱셈으로만 이 값을 계산할 수 있다는 것이다. 다음은 카라츠바 알고리즘의 구현이다.
```c++
// a += b * (10^k);를 구현한다.
void addTo(vector<int>& a, const vector<int>& b, int k);
// a -= b;를 구현한다. a >= b를 가정한다.
void subFrom(vector<int>& a, const vector<int>& b);
// 두 긴 정수의 곱을 반환한다.
vector<int> karatsuba(const vector<int>& a, const vector<int>& b) {
  int an = a.szie(), bn = b.size();
  // a가 b보다 짧을 경우 둘을 바꾼다.
  if(an < bn) return karatsuba(b, a);
  // 기저 사례: a나 b가 비어 있는 경우
  if(an == 0 || bn == 0) return vector<int>();
  // 기저 사례: a가 비교적 짧은 경우 O(n^2)곱셈으로 변경한다.
  if(an <= 50) return multiply(a, b);
  int half = an / 2;
  // a와 b를 밑에서 half 자리와 나머지로 분리한다.
  vector<int> a0(a.begin(), a.begin() + half);
  vector<int> a1(a.begin() + half, a.end());
  vector<int> b0(b.begin(), b.begin() + min<int>(b.size(), half));
  vactor<int> b1(b.begin() + min<int>(b.size(), half), b.end());
  // z2 = a1 * b1
  vector<int> z2 = karatsuba(a1, b1);
  // z0 = a0 * b0
  vector<int> z0 = karatsuba(a0, b0);
  // a0 = a0 + a1; b0 = b0 + b1
  addTo(a0, a1, 0);
  addTo(b0, b1, 0);
  // z1 = (a0 * b0) - z0 - z2;
  vector<int> z1 = karatsuba(a0, b0);
  subFrom(z1, z0);
  subFrom(z1, z2);
  // ret = z0 + z1 * 10^half + z2* 10^(half*2)
  vector<int> ret;
  addTo(ret, z0, 0);
  addTo(ret, z1, half);
  addTo(ret, z2, half + half);
  return ret;
}
```

* ### 시간 복잡도 분석
#### &nbsp;최종 시간 복잡도는 O(n^lg3)이 된다.

## 02. 문제: 쿼드 트리 뒤집기(문제 ID: QUADTREE, 난이도: 하)
#### &nbsp;대량의 좌표 데이터를 메모리 안에 압축해 저장하기 위해 사용하는 여러 기법 중 쿼드 트리(quad tree)란 것이 있습니다. 주어진 공간을 항상 4개로 분할해 재귀적으로 표현하기 때문에 쿼드 트리라는 이름이 붙었는데, 이의 유명한 사용처 중 하나는 검은 색과 흰 색밖에 없는 흑백 그림을 압축해 표현하는 것입니다. 쿼드 트리는 2^N × 2^N 크기의 흑백 그림을 다음과 같은 과정을 거쳐 문자열로 압축합니다.
#### 01. 이 그림의 모든 픽셀이 검은 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 b가 됩니다.
#### 02. 이 그림의 모든 픽셀이 흰 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 w가 됩니다.
#### 03. 모든 픽셀이 같은 색이 아니라면, 쿼드 트리는 이 그림을 가로 세로로 각각 2등분해 4개의 조각으로 쪼갠 뒤 각각을 쿼드 트리 압축합니다. 이때 전체 그림의 압축 결과는 x(왼쪽 위 부분의 압축 결과)(오른쪽 위 부분의 압축 결과)(왼쪽 아래 부분의 압축 결과)(오른쪽 아래 부분의 압축 결과)가 됩니다. 예를 들어 그림 (a)의 왼쪽 위 4분면은 xwwwb로 압축됩니다.
#### 그림 (a)와 그림 (b)는 16×16 크기의 예제 그림을 쿼드 트리가 어떻게 분할해 압축하는지를 보여줍니다. 이때 전체 그림의 압축 결과는 xxwww bxwxw bbbww xxxww bbbww wwbb가 됩니다. 쿼드 트리로 압축된 흑백 그림이 주어졌을 때, 이 그림을 상하로 뒤집은 그림 을 쿼드 트리 압축해서 출력하는 프로그램을 작성하세요.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 1초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야만 한다.

* ### 입력
#### &nbsp;첫 줄에 테스트 케이스의 개수 C (C<=50)가 주어집니다. 그 후 C줄에 하나씩 쿼드 트리로 압축한 그림이 주어집니다. 모든 문자열의 길이는 1,000 이하이며, 원본 그림의 크기는 2^20 × 2^20 을 넘지 않습니다.

* ### 출력
#### &nbsp;각 테스트 케이스당 한 줄에 주어진 그림을 상하로 뒤집은 결과를 쿼드 트리 압축해서 출력합니다.

* ### 개인적 풀이
#### &nbsp;문자열 x가 나오면 재귀적인 함수 호출을 통해서 더이상 x가 나오지 않고 4개의 문자열이 나오면 상하를 바꿔주면 된다.

## 03. 풀이: 쿼드 트리 뒤집기
#### &nbsp;단순한 알고리즘에서 시작해서 최적화해 나간다.

* ### 쿼드 트리 압축 풀기
#### &nbsp;문자열 s의 압축을 해제해서 N * N 크기의 배열에 저장하는 함수 decompress()를 구현한다고 한다. 만약 첫 글자가 x라면 decompress()는 s의 나머지 부분을 넷으로 쪼개 재귀 호출한다. 다음과 같은 형의 decompress()를 작성하면 된다.
```c++
char decompressed[MAX_SIZE][MAX_SIZE];
// s를 압축 해제해서 decompressed[y..y + size - 1][x..x + size - 1]구간에 쓴다.
void decompress(const string& s, int y, int x, int size);
```

* ### 압축 문자열 분할하기
#### &nbsp;decompress() 함수에서 '필요한 만큼 가져다 쓰도록' 하게 만든다. s를 통째로 전달하는 것이 아니라, s의 한 글자를 가리키는 포인터를 전달한다.
```c++
char decompressed[MAX_SIZE][MAX_SIZE];
void decompress(string::iterator& it, int y, int x, int size) {
  // 한 글자를 검사할 때마다 반복자를 한 칸 앞으로 옮긴다.
  char head = *(it++);
  // 기저 사례: 첫 글자가 b 또는 w인 경우
  if(head == 'b' || head == 'w') {
    for(int dy = 0; dy < size; ++dy)
      for(int dx = 0; dx < size; ++dx)
        decompressed[y + dy][x + dx] = head;
  } else {
    // 네 부분을 각각 순서대로 압축 해제한다.
    int half = size / 2;
    decompress(it, y, x, half);
    decompress(it, y, x + half, half);
    decompress(it, y + half, x, half);
    decompress(it, y + half, x + half, half);
  }
}
```

* ### 압축 다 풀지 않고 뒤집기
#### &nbsp;곧이 곧대로 압축을 풀기에는 이 문제에서 다루는 그림들은 너무 크다. 2^20 * 2^20크기의 거대한 그림은 1테라바이트나 된다. decompress()의 기저 사례를 생각해 본다. 천체가 한가지 색인 경우는 뒤집어도 똑같다. 아닌경우는 네 부분을 각각 상하로 뒤집은 결과를 얻은 뒤, 이들을 병합해 답을 얻어야 한다. 각각의 네 부분을 상하로 뒤집고, 위 두 조각과 아래 두 조각을 바꾸면 전체가 뒤집힌 그림이 된다.
```c++
string reverse(string::iterator& it) {
  char head = *it;
  ++it;
  if(head == 'b' || head == 'w')
    return string(1, head);
  string upperLeft = reverse(it);
  string upperRight = reverse(it);
  string lowerLeft = reverse(it);
  string lowerRight = reverse(it);
  // 각각 위와 아래 조각들의 위치를 바꾼다.
  return string("x") + lowerLeft + lowerRight + upperLeft + upperRight;
}
```

* ### 내가 짠 코드와 비교해 보기
#### &nbsp;이번에는 코드를 짜지 못했다. 답안을 보니 생각보다 간단한 코드로 구현이 가능하다. iterator를 사용하는 부분은 나중에도 유용하게 사용될 것 같다.

* ### 시간 복잡도 분석
#### &nbsp;reverse()함수는 한 번 호출될 때마다 주어진 문자열의 한 글자씩을 사용한다. 따라서 함수가 호출되는 횟수는 문자열의 길이에 비례하므로 O(n)이 된다.