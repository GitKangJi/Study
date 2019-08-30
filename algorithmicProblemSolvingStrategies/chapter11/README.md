11장 조합 탐색
==============

## 01. 도입
#### &nbsp;지금까지 다룬 동적 계획법이나 분할 정복 등의 디자인 패러다임은 적절히 적용될 때는 매우 유용하지만, 모든 문제에 적용할 수 없다. 대개 답을 만드는 과정을 여러 개의 선택으로 나눈 뒤, 재귀 호출을 이용해 각각의 선택지를 채워가는 형태로 구현되곤 한다. 대부분 문제에서 탐색 공간의 크기는 문제의 규모에 따라 기하급수적으로 증가한다. 완전 탐색을 포함해, 이렇게 유한한 크기의 탐색 공간을 뒤지면서 답을 찾아 내는 알고리즘들을 조합 탐색(Combinatorial Search)이라고 부른다. 조합 탐색에는 다양한 최적화 기법이 있으며, 이들은 접근 방법은 다르지만 모두 기본적으로 모두 최적해가 될 가능성이 없는 답들을 탐색하는 것을 방지하여 만들어 봐야 할 답의 수를 줄이는 것을 목표로 한다.

## 02. 조합 탐색 기법들
#### &nbsp;여행하는 외원판 문제를 푸는 완전 탐색 알고리즘에 이 기법들을 적용하면서 변화하는 수행 속도를 관찰해 본다.

* ### 조합 탐색 뼈대의 구현
#### &nbsp;n이 16만 돼도 완전 탐색은 시간 초과(750시간)로 작동하지 않는다.

* ### 최적해보다 나빠지면 그만두기
#### &nbsp;현재 상태의 답이 지금까지 구한 최적해와 같거나 더 나쁠 떄 탐색을 중단한다.

* ### 간단한 휴리스틱을 이용한 가지치기
#### &nbps;휴리스틱을 이용해 답의 남은 부분을 어림짐작하는 가지치기를 이요하면 좀더 똑똑하게 가지치기를 할 수 있다. 휴리스틱을 이용한 가지치기는 남은 조각들을 푸는 최적해를 찾기는 오래 걸리더라도, 이 값을 적당히 어림짐작하기는 훨씬 빠르게 할 수 있다는 점을 이용해 가지치기를 수행한다.

* ### 휴리스틱 함수 작성하기
#### &nbsp;휴리스틱 함수를 만드는 좋은 방법 중 하나는 문제의 제약 조건을 일부 없앤 더 단순한 형태의 문제를 푸는 것이다.

* ### 단순한 휴리스틱 함수의 구현
#### &nbsp;아직 방문하지 않은 도시들에 대해 인접한 간선 중 가장 짧은 간선의 길이를 더한다.

* ### 가까운 도시부터 방문하기
#### &nbsp;도시를 번호 순서대로 방문하는 대신, 더 가까운 것부터 방문하면 좋은 답을 더 빨리 찾아낼 수 있는 경우가 있다.

* ### 지나온 경로를 이용한 가지치기
#### &nbsp;지금까지 만든 경로가 시작 상태에서 현재 상태까지 도달하는 최적해가 아니라고 하면 탐색을 계속할 이유가 없다.

* ### MST휴리스틱을 이용한 가지치기의 구현
#### &nbsp;최소 스패닝 트리를 이용한다.

* ### 마지막 단계 메모이제이션하기
#### &nbsp;조합 탐색 과정에서 같은 상태를 두 번 이상 맞닥뜨리는 것은 흔하다. 이런 비효율을 메모이제이션으로 제거한다.

## 03. 문제: 게임판 덮기 2(문제 ID: BOARDCOVER2, 난이도: 하)
#### &nbsp;H×W 크기의 게임판과 한 가지 모양의 블록이 여러 개 있습니다. 게임판에 가능한 많은 블록을 올려놓고 싶은데, 게임판은 검은 칸과 흰 칸으로 구성된 격자 모양을 하고 있으며 이 중에서 흰 칸에만 블록을 올려놓을 수 있습니다. 이때 블록들은 자유롭게 회전해서 놓을 수 있지만, 서로 겹치거나, 격자에 어긋나게 덮거나, 검은 칸을 덮거나, 게임판 밖으로 나가서는 안 됩니다. 위 그림은 예제 게임판과 L 자 모양의 블록으로 이 게임판을 덮는 방법을 보여줍니다. 게임판에는 15개의 흰 칸이 있고, 한 블록은 네 칸을 차지하기 때문에 그림과 같이 최대 세 개의 블록을 올려놓을 수 있지요. 게임판과 블록의 모양이 주어질 때 최대 몇 개의 블록을 올려놓을 수 있는지 판단하는 프로그램을 작성하세요.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 2초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야 한다.

* ### 입력
#### &nbsp;입력의 첫 줄에는 테스트 케이스의 수 T (T≤100)가 주어집니다. 각 테스트 케이스의 첫 줄에는 게임판의 크기 H, W (1≤H, W≤10), 그리고 블록의 모양을 나타내는 격자의 크기 R, C (1≤R, C≤10)가 주어집니다. 다음 H줄에는 각각 W 글자의 문자열로 게임판의 정보가 주어집니다. 문자열의 각 글자는 게임판의 한 칸을 나타내며, #은 검은 칸, 마침표는 흰 칸을 의미합니다. 다음 R줄에는 각 C 글자의 문자열로 블록의 모양이 주어집니다. 이 문자열에서 #은 블록의 일부, 마침표는 빈 칸을 나타냅니다. 각 게임판에는 최대 50개의 흰 칸이 있으며, 각 블록은 3개 이상 10개 이하의 칸들로 구성됩니다. 변을 맞대고 있는 두 변이 서로 연결되어 있다고 할 때, 블록을 구성하는 모든 칸들은 서로 직접적 혹은 간접적으로 연결되어 있습니다.

* ### 출력
#### &nbsp;각 테스트 케이스마다 게임판에 놓을 수 있는 최대의 블록 수를 출력합니다.

## 04. 풀이: 게임판 덮기2
#### &nbsp;이 문제에서는 같은 상태를 여러 번 방문하는 것을 방지해 수행 속도를 빠르게 하는 효과가 있다.

* ### 전처리
#### &nbsp;블록의 형태가 미리 정해져 있지 않은 이 문제에서는 첫 번째 칸에 대한 상대 좌표의 목록으로 미리 저장하는게 불가능 하다. 따라서 입력받은 블록을 회전하고 각 칸의 상대 좌표를 계산하는 작업을 전처리 과정에서 수행해야 한다.
```c++
// 블록의 각 회전된 형태를 상대 좌표의 목록으로 저장해둔다.
vector<vector<pair<int, int> > > rotations;
// 블록의 크기
int blockSize;
// 2차원 배열 arr을 시계방향으로 90도 돌린 결과를 반환한다.
vector<string> rotate(const vector<string>& arr){
	vector<string> ret(arr[0].size(), string(arr.size(), ' '));
	for(int i = 0; i < arr.size(); i++){
		for(int j = 0; j < arr[0].size(); j++){
			ret[j][arr.size() - i - 1] = arr[i][j];
	return ret;
}
// block의 네가지 회전 형태를 만들고 이들의 상대 좌표의 목록으로 변환한다.
void generateRotations(vector<string> block) {
	rotations.clear();
	rotations.resize(4);
	for(int rot = 0; rot < 4; rot++){
		int originY = -1; int originX = -1;
		for(int i = 0; i < block.size(); i++)
			for(int j = 0; j < block[i].size(); j++)
				if(block[i][j] == '#'){
					if(originY == -1){
						// 가장 윗줄 맨 왼쪽에 있는 칸이 '원점'이 된다.
						originY = i;
						originX = j;
					}
					// 각 칸의 위치를 원점으로부터의 상대좌표로 표현한다.
					rotations[rot].push_back(make_pair(i - originY, j - originX));
				}
			// 블록을 시계 방향으로 90도 회전한다.
			block = rotate(block);
		}
	
	// 네 가지 회전 형태 중 중복이 있을 경우 이를 제거한다.
	sort(rotations.begin(), rotations.end());
	rotations.erase(unique(rotations.begin(), rotations.end()), rotations.end());
	// 블록이 몇 칸 짜리인지 저장해 둔다.
	blockSize = rotations[0].size();
}
```

* ### 완전 탐색
#### &nbsp;모든 칸을 덮을 수 없는 경우가 있기 때문에 현재 칸을 항상 덮는 것이 아니라 빈 채로 남겨 두는 경우도 고려해야 한다. (y, x)를 덮지 않기로 결정한 경우에도 막아 둔다.
```c++
// 게임판의 정보
int boardH, boardW;
vector<string> board;
// 게임판의 각 칸이 덮였는지를 나타낸다. 1이면 검은 칸이거나 이미 덮은 칸, 0이면 빈칸
int covered[10][10];
// 지금까지 찾은 최적해
int best;
// (y, x)를 왼쪽 위칸으로 해서 주어진 블록을 delta = 1이면 놓고, -1이면 없댄다. 블록을 놓을 때 이미 놓인 블록이나 검은 칸과 겹치면 거짓을, 아니면 참을 반환한다.
bool set(int y, int x, const vector<pair<int,int> >& cells, int delta) {
	bool ok = true;
	for(int i = 0; i < cells.size(); i++) {
		int cy = y + cells[i].first, cx = x + cells[i].second;
		if(cy < 0 || cx < 0 || cy >= boardH || cx >= boardW)
			ok = false;
		else {
			covered[cy][cx] += delta;
			if(covered[cy][cx] > 1) ok = false;
		}
	}
	return ok;
}

// placed : 지금까지 놓은 블록의 수
void search(int placed){
	// 아직 채우지 못한 빈 칸 중 가장 윗줄 왼쪽에 있는 칸을 찾는다.
	int y = -1, x = -1;
	for(int i = 0; i < boardH; i++){
		for(int j = 0; j < boardW; j++)
			if(covered[i][j] == 0){
				y = i;
				x = j;
				break;
			}
		if(y != -1) break;
	}

	// 기저사례 : 게임판의 모든 칸을 처리한 경우
	if(y == -1){
		best = max(best, placed);
		return;
	}

	// 이 칸을 덮는다.
	for(int i = 0; i < rotations.size(); i++){
		if(set(y, x, rotations[i], 1))
			search(placed + 1);
		set(y, x, rotations[i], -1);
	}

	// 이 칸을 덮지 않고 '막아'둔다.
	covered[y][x] = 1;
	search(placed);
	covered[y][x] = 0;
}

int solve(){
	best = 0;
	// covered 배열을 초기화 한다.
	for(int i = 0; i < boardH; i++){
		for(int j = 0; j < boardW; j++){
			covered[i][j] = (board[i][j] == '#' ? 1 : 0);
		}
	}
	search(0);
	return best;
}
```

* ### 가지 치기
#### &nbsp;가지 치기를 위해서는 현재 상태에서 최선을 다해도 최적해보다 많은 블록을 내려놓을 수 없다는 것을 알아야 한다. 우리가 놓을 수 있는 블록의 수는 단순히 남은 빈 칸의 수를 블록의 크기로 나눈 것이 된다. 이 값은 항상 우리가 실제 놓을 수 있는 블록의 수 이상이기 때문에, 우리가 얻을 수 있는 답의 상한이 된다. 이 답의 상한이 현재 찾은 최적해 이하라면 더이상 탐색을 수행할 필요가 없다.

## 05. 문제: 알러지가 심한 친구들(문제 ID: ALLERGY, 난이도: 중)
#### &nbsp;집들이에 n 명의 친구를 초대하려고 합니다. 할 줄 아는 m 가지의 음식 중 무엇을 대접해야 할까를 고민하는데, 친구들은 각각 알러지 때문에 못 먹는 음식들이 있어서 아무 음식이나 해서는 안 됩니다. 만들 줄 아는 음식의 목록과, 해당 음식을 못 먹는 친구들의 목록이 다음과 같은 형태로 주어진다고 합시다. 각 친구가 먹을 수 있는 음식이 최소한 하나씩은 있도록 하려면 최소 몇 가지의 음식을 해야 할까요? 위 경우라면 다 같이 먹을 수 있는 음식이 없기 때문에 결국 두 가지 이상 음식을 해야 합니다. 피자와 탕수육, 혹은 잡채와 닭강정처럼 두 개 이상의 음식을 선택해야만 모두가 음식을 먹을 수 있지요. 친구들의 정보가 주어질 때 최소한 만들어야 하는 요리의 가지수를 계산하는 프로그램을 작성하세요.

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 5초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야 한다.

* ### 입력
#### &nbsp;입력의 첫 줄에는 테스트 케이스의 수 T (T <= 50 ) 가 주어집니다. 각 테스트 케이스의 첫 줄에는 친구의 수 n (1 <= n <= 50) 과 할 줄 아는 음식의 수 m (1 <= m <= 50) 이 주어집니다. 다음 줄에는 n 개의 문자열로 각 친구의 이름이 주어집니다. 친구의 이름은 10 자 이하의 알파벳 소문자로만 구성된 문자열입니다. 그 후 m 줄에 하나씩 각 음식에 대한 정보가 주어집니다. 각 음식에 대한 정보는 해당 음식을 먹을 수 있는 친구의 수와 각 친구의 이름으로 주어집니다. 모든 친구는 하나 이상의 음식을 먹을 수 있다고 가정해도 좋습니다.

* ### 출력
#### &nbsp;각 테스트 케이스마다 한 줄에 만들어야 할 최소의 음식 수를 출력합니다.

## 06. 풀이: 알러지가 심한 친구들
#### &nbsp;이 문제는 유명한 NP 완비 문제 중 하나다. 이 문제를 해결할 수 있는 가장 직관적인 방법은 음식을 선택하는 모든 경우의 수를 하나하나 만들어 보는 것이다. 

* ### 너무 느리다
#### &nbsp;이 방법은 너무 느리다. 그래서 여러 최적화 기법을 생각할 수 있다. 하지만 이 문제를 푸는 간단한 방법은 탐색의 형태를 바꾸는 것이다. 매 재귀 호출마다 아직 먹을 음식이 없는 친구를 하나 찾은 뒤 이 친구를 위해 어떤 음식을 만들지 결정하는 것이다.
```c++
int n, m;
// canEat[i] : i번 친구가 먹을 수 있는 음식의 집합
// eaters[i] : i번 음식을 먹을 수 있는 친구들의 집합
vector<int> canEat[50], eaters[50];
int best;

// chosen : 지금까지 선택한 음식의 수
// edible[i] : 지금까지 고른 음식 중 i번 친구가 먹을 수 있는 음식의 수
void search(vector<int>& edible, int chosen){
	// 간단한 가지치기
	if(chosen >= best) return;

	//아직 먹을 음식이 없는 첫 번째 친구를 찾는다.
	int first = 0;

	while(first < n && edible[first] > 0) first++;

	// 모든 친구가 먹을 음식이 있는 경우 종료한다.
	if(first == n){
		best = chosen; 
		return;
	}

	for(int i = 0; i < canEat[first].size(); i++){
		int food = canEat[first][i];
		for(int j = 0; j < eaters[food].size(); j++){
			edible[eaters[food][j]]++;
		}

		search(edible, chosen + 1);
		for(int j = 0; j < eaters[food].size(); j++){
			edible[eaters[food][j]]++;
		}
	}
}
```

* ### 왜 더 빠른가?
#### &nbsp;search()는 항상 모든 친구가 먹을 음식이 있는 조합만을 찾아낸다. 또한 한 번 호출될 때마다 항상 음식을 하나 하게 되고, 음식을 하면 그 음식이 필요한 친구가 반드시 있다는 의미다.

* ### 더 최적화 하기
#### &nbsp;좀더 좋은 답을 초반에 찾아내서 가지치기를 더 잘할 수 있다.

## 07. 문제: 카쿠로(문제 ID: KAKURO2, 난이도: 중)
#### &nbsp;카쿠로는 흔히 십자말풀이 수학 버전이라고 불리는 논리 퍼즐이다. 카쿠로는 위와 같이 생긴 정사각형의 게임판을 가지고 하는 퍼즐로, 이 게임판의 각 칸은 흰 칸이거나, 검은 칸이거나, 힌트 칸이다. (힌트 칸은, 대각선으로 갈라져 있고 숫자가 씌여 있는 칸을 말한다) 모든 흰 칸에 적절히 숫자를 채워 넣어 규칙을 만족시키는 것이 이 게임의 목표이다. 카쿠로의 규칙은 다음과 같다.
#### 모든 흰 칸에는 1 부터 9 까지의 정수를 써넣어야 한다.
#### 세로로 연속한 흰 칸들의 수를 모두 더하면, 그 칸들의 바로 위에 있는 힌트 칸의 왼쪽 아래에 씌인 숫자가 나와야 한다.
#### 가로로 연속한 흰 칸들의 수를 모두 더하면, 그 칸들의 바로 왼쪽에 있는 힌트 칸의 오른쪽 위에 씌인 숫자가 나와야 한다.
#### 이 때 한 줄을 이루는 연속된 흰 칸들에는 같은 수를 두 번 넣을 수 없다. 예를 들어, 두 개의 흰 칸의 숫자를 더해서 4가 나와야 한다고 하면, 1+3=4 이거나 3+1=4 만이 가능하고, 2+2=4 는 불가능하다.
#### 알고스팟 멤버들은 모의고사를 준비하면서 카쿠로를 푸는 프로그램을 작성하는 문제를 내려고 하는데, 입력 파일 만들기가 너무 힘들어서 입력 파일을 만드는 문제부터 먼저 내기로 했다.

## 08. 풀이: 카쿠로

* ### 제약 충족 문제
#### &nbsp;제대로 만든 카쿠로 퍼즐에는 항상 답이 하나뿐이라 최적화 문제는 아니다. 이와 같이 특정한 제약에 해당하는 답을 찾는 문제들을 제약 충족 문제라고 부른다.

* ### 제약 전파
#### &nbsp;답의 일부를 생성하고 나면 문제의 조건에 의해 다른 조각의 답에 대해 알게 되는 것을 제약 전파(Constraint Propagation)라고 부른다. 제약 전파를 이용하면 실제 탐색해야 할 탐색 공간의 수가 크게 줄어든다.

* ### 채울 순서 정하기
#### &nbsp;채울 순서 정하기는 두 가지의 문제를 합쳐 부르는 말이다. 변수 순서 정하기(Variable Ordering)는 어느 빈 칸부터 채워나갈지를 결정하는 문제고, 값 순서 정하기(Value Ordering)는 이 빈 칸에 어떤 숫자부터 채워나갈 것인지를 말한다.

* ### 후보의 수 계산하기
#### &nbsp;변수 순서 정하기를 위해 흔히 쓰는 좋은 방법은, 해당 칸에 들어갈 수 있는 후보의 수가 가장 작은 칸부터 채워나가는 방법이다.

* ### 후보의 수 빠르게 계산하기
#### &nbsp;메모이제이션하거나, 탐색을 수행하기 전에 미리 계산해 두면 더 빠르게 계산이 가능하다.