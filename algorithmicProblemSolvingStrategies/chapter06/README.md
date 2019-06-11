6장 무식하게 풀기
================

## 01. 도입
#### &nbsp;대부분의 사람들이 가장 많이 하는 실수는 쉬운 문제를 어렵게 푸는 것이다. '무식하게 푼다(brute-force)'는 컴퓨터의 빠른 계산 능력을 이용해 가능한 경우의 수를 일일이 나열하면서 답을 찾는 방법을 의미한다. 이렇게 가능한 방법을 전부 만들어 보는 알고리즘들을 가리켜 흔히 완전 탐색(exhaustive search)라고 부른다.

## 02. 재귀 호출과 완전 탐색

* ### 재귀 호출
#### &nbsp;컴퓨터가 수행하는 많은 작업들은 대개 작은 조각들로 나누어 볼 수 있다. 그런데 우리가 들여다보는 범위가 작아지면 작아질수록 각 조각들의 형태가 유사해지는 작업들을 많이 볼 수 있다. for과 같은 반복문이 좋은 예이다. 이런 작업을 구현할 때 유용하게 사용되는 개념이 바로 재귀 함수(recursive function), 혹은 재귀 호출(recursion)이다. 다음은 1부터 n까지의 합을 반환하는 함수 sum()의 구현이다.
```c++
int sum(int n) {
  int ret = 0;
  for(int i = 1; i <= n; ++i)
    ret += i;
  return ret;
}

int recursiveSum(int n) {
  if(n == 1) return 1;
  return n + recursiveSum(n - 1);
}
```
#### 모든 재귀 함수는 더이상 쪼개지지 않는 최소한의 작업에 도달했을 때 갑을 곧장 반환하는 조건문을 포함해야 한다. 이때 쪼개지지 않는 가장 작은 작업들을 가리켜 재귀 호출의 기저 사례(base case)라고 한다. 문제의 특성에 따라 재귀 호출은 코딩을 훨씬 간편하게 해 줄 수 있는 강력한 무기가 된다.

* ### 예제: 중첩 반복문 대체하기
#### &nbsp;0번부터 차례대호 번호 매겨진 n개의 원소 중 네 개를 고르는 모든 경우를 출력하는 코드를 작성해 보자. 물론 4중 for문을 써서 이것을 간단하게 할 수 있다. 하지만 네 개가 아닌 일곱 개를 골라야 한다면 for문을 일곱번 중첩해야한다. 그러면 코드가 길고 복잡해지는데다, 골라야 할 원소의 수가 입력에 따라 달라질 수 있는 경우에는 사용할 수 없다는 문제가 있다. 다음은 재귀 함수로 구현을 한 코드다.
```c++
// n: 전체 원소의 수
// picked: 지금까지 고른 원소들의 번호
// toPick: 더 고를 원소의 수
void pick(int n, vector<int>& picked, int toPick) {
  // 기저 사례
  if(toPick == 0) {
    printPicked(picked);
    return;
  }
  // 고를 수 있는 가장 작은 번호를 계산한다.
  int smallest = picked.empty() ? 0 : picked.back() + 1;
  // 이 단계에서 원소 하나를 고른다.
  for(int next = smallest; next < n; ++next) {
    picked.push_back(next);
    pick(n, picked, toPick - 1);
    picked.pop_back();
  }
}
```
#### 이와 같이 재귀 호출을 이용하면 특정 조건을 만족하는 조합을 모두 생성하는 코드를 쉽게 작성할 수 있다. 때문에 재귀 호출은 완전 탐색을 구현할 때 아주 유용한 도구다.

* ### 예제: 보글 게임(문제 ID: BOGGLE, 난이도: 하)
#### nbsp;보글(Boggle) 게임은 그림 (a)와 같은 5x5 크기의 알파벳 격자인 게임판의 한 글자에서 시작해서 펜을 움직이면서 만나는 글자를 그 순서대로 나열하여 만들어지는 영어 단어를 찾아내는 게임입니다. 펜은 상하좌우, 혹은 대각선으로 인접한 칸으로 이동할 수 있으며 글자를 건너뛸 수는 없습니다. 지나간 글자를 다시 지나가는 것은 가능하지만, 펜을 이동하지않고 같은 글자를 여러번 쓸 수는 없습니다. 예를 들어 그림의 (b), (c), (d)는 각각 (a)의 격자에서 PRETTY, GIRL, REPEAT을 찾아낸 결과를 보여줍니다. 보글 게임판과 알고 있는 단어들의 목록이 주어질 때, 보글 게임판에서 각 단어를 찾을 수 있는지 여부를 출력하는 프로그램을 작성하세요.

> hasWord(y, x, word) = 보글 게임판의 (y, x)에서 시작하는 단어 word의 존재 여부를 반환한다.

* ### 문제의 분할
#### &nbsp;hasWord()가 하는 일을 가장 자연스럽게 조각내는 방법은 각 글자를 하나의 조각으로 만드는 것이다. 시작 위치 (x, y)에서 인접한 여덟 칸을 모두 시도하면서 답을 찾으면 된다.

* ### 기저 사례의 선택
#### &nbsp;첫번째, 위치 (x, y)에 있는 글자가 원하는 단어의 첫 글자가 아닌 경우 항상 실패한다. 두번째, 원하는 단어가 한 글자인 경우 항상 성공이다. 두 조건 간의 순서는 변하면 안된다.

* ### 구현
```c++
// 내가 짜본 말도 안되는 코드...
bool hasWord(int x, int y, const string& word) {
  // 없는 좌표인 경우
  if(x < 0 || 4 < x || y < 0 || 4 > y) return false;
  // 기저 사례
  if(word.length() == 0) return true;

  // 지금 있는 좌표의 단어와 word의 첫번째 단어와 일치를 확인한다.
  if(board[x][y] == word.at(0)) {
    // 주위 8개의 칸을 검사한다.
    for(int additionalX = -1; additionalX < 2; additionalX++) {
      for(int additionalY = -1; additionalY < 2; additionalY++) {
        // 현재 칸이 아닌 경우만 검사한다.
        if(!(additionalX == 0 && additionalY == 0)) {
          result = hasWord(x + additionalX, Y + additionalY, word.erase(0, 1));
          if(result == true) {
            // 존재하는 경우
            return true;
          }
        }
      }
    }
  } else {
    // 지금 칸이 같지 않은 경우.
    return false;
  }
}
```
```c++
const int dx[8] = { -1, -1, -1, 1, 1, 1, 0, 0 };
const int dy[8] = { -1, 0, 1, -1, 0, 1, -1, 1 };

bool hasWord(int y, int x, const string& word) {
  // 기저 사례 1: 시작 위치가 범위 밖이면 무조건 실패
  if(!inRange(y, x)) return false;
  // 기저 사례 2: 첫 글자가 일치하지 않으면 실패
  if(board[y][x] != word[0]) return false;
  // 기저 사례 3: 단어 길이기 1이면 성공
  if(word.size() == 1) return true;
  // 인접한 여덟 칸을 검사한다
  for(int direction = 0; direction < 8; ++direction) {
    int nextY = y + dy[direction], nextX = x + dx[direction];
    // 다음 칸이 범위 안에 있는지, 첫 글자는 일치하는지 확인할 필요가 없다.
    if(hasWord(nextY, nextX, word.substr(1)))
      return true;
  }
  return false;
}
```

* ### 시간 복잡도 분석
#### &nbsp;시간 복잡도를 계산하는 것은 비교적 단순하다. 모든 경우의 수를 세어보기만 하면 된다.

* ### 완전 탐색 레시피
#### &nbsp;어떤 식으로 문제에 처음 접근해야 할지에 대한 대략적인 지침이다.
#### 01. 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한 답의 수에 정확히 비례한다.
#### 02. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눈다. 각 선택은 답의 후보를 만드는 과정의 한 조각이 된다.
#### 03. 그중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성한다.
#### 04. 조각이 하나밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로, 이것을 기저 사례로 선택해 처리한다.

* ### 이론적 배경: 재귀 호출과 부분 문제
#### &nbsp;보글 게임에서 해당 단어를 이 위치에서 찾을 수 있는지 알기 위해 최대 아홉가지 정보를 알아야 한다. 주위 8가지 글자를 검사하는 경우는 형식이 같은 또 다른 문제다. 문제를 구성하는 조각들 중 하나를 뺐기 때문에, 이 문제들은 원래 문제의 일부라고 말할 수 있다. 이런 문제들을 원래 문제의 부분 문제라고 한다.

## 03. 문제: 소풍(문제 ID: PICNIC, 난이도: 하)
#### &nbsp;안드로메다 유치원 익스프레스반에서는 다음 주에 율동공원으로 소풍을 갑니다. 원석 선생님은 소풍 때 학생들을 두 명씩 짝을 지어 행동하게 하려고 합니다. 그런데 서로 친구가 아닌 학생들끼리 짝을 지어 주면 서로 싸우거나 같이 돌아다니지 않기 때문에, 항상 서로 친구인 학생들끼리만 짝을 지어 줘야 합니다. 각 학생들의 쌍에 대해 이들이 서로 친구인지 여부가 주어질 때, 학생들을 짝지어줄 수 있는 방법의 수를 계산하는 프로그램을 작성하세요. 짝이 되는 학생들이 일부만 다르더라도 다른 방법이라고 봅니다. 예를 들어 다음 두 가지 방법은 서로 다른 방법입니다.
#### (태연,제시카) (써니,티파니) (효연,유리)
#### (태연,제시카) (써니,유리) (효연,티파니)

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 1초 내에 실행되어야 하고, 64MB 이하의 메모리만을 사용해야 한다.

* ### 입력
#### &nbsp;입력의 첫 줄에는 테스트 케이스의 수 C (C <= 50) 가 주어집니다. 각 테스트 케이스의 첫 줄에는 학생의 수 n (2 <= n <= 10) 과 친구 쌍의 수 m (0 <= m <= n*(n-1)/2) 이 주어집니다. 그 다음 줄에 m 개의 정수 쌍으로 서로 친구인 두 학생의 번호가 주어집니다. 번호는 모두 0 부터 n-1 사이의 정수이고, 같은 쌍은 입력에 두 번 주어지지 않습니다. 학생들의 수는 짝수입니다.

* ### 출력
#### &nbsp;각 테스트 케이스마다 한 줄에 모든 학생을 친구끼리만 짝지어줄 수 있는 방법의 수를 출력합니다.

* ### 개인적 풀이
#### &nbsp;만약 10P10으로 모든 경우의 수를 계산할 때는 10! * 50 = 1.8억 이렇게 경우의 수만 1억이 넘는다. 따라서 이렇게 풀면 안된다. 우리에게는 친구 쌍이 주어지기 때문에 이것을 이용해 만들 수 있는 경우의 수를 생각해보면 될 것 같다.
```c++
// 어렵다....너무 어렵다...
int combinationFriend(int numberOfStudents, const vector<int>& friendList, const vector<int>& selectedFriends) {
  // 기저 사례
  if(selectedFriends.length() == numberOfStudents) { return 1; }
  int result = 0;
  for(int friendListIndex = 0; friendListIndex < friendList.length(); friendListIndex + 2) {
    if(checkStudentsDuplecation(selectedFriends, { friendList[friendListIndex], friendList[friendListIndex + 1] })) {
      // selectedFriends에 friendList[friendListIndex], friendList[friendListIndex + 1]를 추가하고, friendList에서 앞에 추가한 값을 빼고, 새롭게 combinationFriend를 호출한다.
    } else {
      // 조합을 다 못만들었는데 친구들이 중복되면 0을 반환
      return 0;
    }
  }
}
```

## 04. 풀이: 소풍

* ### 완전 탐색
#### &nbsp;완전 탐색을 이용해 조합을 모두 만들어 보는 것이다. 재귀 호출을 이용해 코드를 작성해 보자.

* ### 중복으로 세는 문제
#### &nbsp; 이 코드를 예제 입력을 이용해 실행해 보면 전혀 다른 답이 나온다. 이유는 한 경우를 중복으로 여러번 세고 있기 때문이다.
```c++
int n;
bool areFriends[10][10];
// taken[i] = i 번째 학생이 짝을 이미 찾았으면 true, 아니면 false
int countPairings(bool taken[10]) {
  // 기저 사례: 모든 학생이 찾을 찾았으면 한 가지 방법을 찾았으니 종료한다.
  bool finished = true;
  for(int i = 0; i < n; ++i) if(!taken[i]) finished = false;
  if(finished) return 1;
  int ret = 0;
  // 서로 친구인 두 학생을 찾아 짝을 지어 준다.
  for(int i = 0; i < n; ++i)
    for(int j = 0; j < n; ++j)
      if(!taken[i] && !taken[j] && areFriends[i][j]) {
        taken[i] = taken[j] = true;
        ret += countPairings(taken);
        taken[i] = taken[j] = false;
      }
    return ret;
}
```
#### 이 코드에는 두가지 문제점이 있다. 같은 학생 쌍을 두 번 짝지어 준다. 다른 순서로 학생들을 짝지어 주는 것을 서로 다른 경우로 세고 있다. 이 상황을 해결하기 위한 좋은 방법은 항상 특정 형태를 갖는 답만을 세는 것이다. 흔히 사용하는 방법으로는 같은 답 중에서 사전순으로 가장 먼저 오는 답 하나만을 세는 것이 있다. 이 속성을 강제하기 위해서 각 단계에서 남아 있는 학생들 중 가장 번호가 빠른 학생의 짝을 찾아 주도록 하면 됩니다.
```c++
int n;
bool areFriends[10][10];
// taken[i] = i 번째 학생이 짝을 이미 찾았으면 true, 아니면 false
int countPairings(bool taken[10]) {
  // 남은 학생들 중 가장 번호가 빠른 학생을 찾는다.
  int firstFree = -1;
  for(int i = 0; i < n; ++i) {
    if(!taken[i]) {
      firstFree = i;
      break;
    }
  }
  // 기저 사례: 모든 학생이 짝을 찾았으면 한 가지 방법을 찾았으니 종료한다.
  if(firstFree == -1) return 1;
  int ret = 0;
  // 이 학생과 짝지을 학생을 결정한다.
  for(int pairWith = firstFree + 1; pairWith < n; ++pairWith) {
    if(!taken[pairWith] && areFriends[firstFree][pairWith]) {
      taken[firstFree] = taken[pairWith] = true;
      ret += countPairings(taken);
      taken[firstFree] = taken[pairWith] = false;
    }
  }
  return ret;
}
```

* ### 답의 수의 상한
#### &nbsp;프로그램을 짜기 전에 답의 수가 얼마나 될지 예측해 보고 모든 답을 만드는 데 시간이 얼마나 오래 걸릴지를 확인해야 한다.