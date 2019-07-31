10장 탐욕법
===========

## 1. 도입
#### &nbsp;탐욕법(greedy method)는 가장 직관적인 알고리즘 설계 패러다임 중 하나다. 완전 탐색이나 동적 계획법 알고리즘은 모든 선택지를 고려해 보고 그중 전체 답이 가장 좋은 것을 찾는다. 하지만 탐욕법은 각 단계마다 지금 당장 가장 좋은 방법만을 선택한다. 탐욕적 알고리즘은 많은 경우 최적해를 찾지 못하기 때문에 다음과 같은 경우에 사용한다.
#### 1. 탐욕법을 사용해도 항상 최적해를 구할 수 있는 문제를 만난 경우, 탐욕법은 동적 계획법보다 수행 시간이 훨씬 빠르기 때문에 유용한다.
#### 2. 시간이나 공간적 제약으로 인해 다른 방법으로 최적해를 찾기 너무 어렵다면 최적해 대신에 적당히 괜찮은 답을 찾는 것으로 타협할 수 있다. 탐욕법은 이럴 때 최적은 아니지만 임의의 답보다는 좋은 답을 구하는 용도로 유용하게 쓰인다.

* ### 예제: 회의실 예약
#### &nbsp;탐욕법이 유용하게 사용되는 문제 중 유명한 예로 활동 선택 문제가 있다. 회사에 회의실이 하나밖에 없는데, n개의 팀이 각가 회의하고 싶은 시간을 제출했다고 한다. 두 팀이 회의실을 같이 쓸 수는 없기 때문에 이 중 서로 겹치지 않을 회의들만을 골라내서 진행해야 한다. 최대 몇 개나 선택할 수 있을까?

* ### 무식하게 풀 수 있을까?
#### &nbsp;이 문제를 무식하게 푸는 방법은 모든 부분 집합을 하나하나 만들어 보며 그중 회의들이 겹치지 않는 답들을 걸러내고 그중 가장 큰 부분 집합을 찾아낸다. 하지만 집합의 크기가 n일때 n이 30만 돼도 시간 안에 문제를 풀기는 힘들다.

* ### 탐욕적 알고리즘의 구상
#### &nbsp;이 문제를 해결하는 탐욕적인 방법은 길이와 상관없이 가장 먼저 끝나는 회의부터 선택하는 것이다.

* ### 정당성의 증명: 탐욕적 선택 속성
#### &nbsp;탐욕적 알고리즘의 정당성 증명은 많은 경우 일정한 패턴을 가진다. 처음으로 증명해야 할 속성은 동적 계획법처럼 답의 모든 부분을 고려하지 않고 탐욕적으로만 선택하더라도 최적해를 구할 수 있다는 것이다. 이 속성은 탐욕적 선택 속성(greedy choice property)이라고 부른다.
#### 앞에서 제안한 알고리즘의 경우, 탐욕적 선택 속성이 성립하려면 다음 조건이 성립해야 한다.
#### 가장 종료 시간이 빠른 회의(Smin)를 포함하는 최적해가 반드시 존재한다.

* ### 최적 부분 구조
#### &nbsp;항상 최적의 선택만을 내려서 전체 문제의 최적해를 얻을 수 있음을 보여야 한다. 탐욕법의 정당성을 위해 증명해야 할 두 번쨰 속성은 최적 부분 구조(optimal substructure)이다. 부분 문제의 최적해에서 전체 문제의 최적해를 만들 수 있음을 보여야한다.

* ### 구현
#### &nbsp;처음에 모든 회의를 종료 시간의 오름차순으로 정렬해 두면 쉽고 빠르게 구현가능하다. 겹치는 회의를 지울 필요 없이, 정렬된 배열을 순회하면서 첫 번째 회의와 겹치지 않는 회의를 찾는다.
```c++
// 각 회의는 [begin,end) 구간 동안 회의실을 사용한다
int n;
int begin[100], end[100];
int schedule() {
	// 일찍 끝나는 순서대로 정렬한다
	vector<pair<int,int> > order;
	for(int i = 0; i < n; i++)
		order.push_back(make_pair(end[i], begin[i]));
	sort(order.begin(), order.end());
	// earliest: 다음 회의가 시작할 수 있는 가장 빠른 시간
	// selected: 지금까지 선택한 회의의 수
	int earliest = 0, selected = 0;
	for(int i = 0; i < order.size(); ++i) {
		int meetingBegin = order[i].second, meetingEnd = order[i].first;
		if(earliest <= meetingBegin) {
			// earliest 를 마지막에 회의가 끝난 시간 이후로 갱신한다
			earliest = meetingEnd;
			++selected;
		}
	}

	return selected;
}
```

* ### 난 동적 계획법으로 풀었는데?
#### &nbsp;물론 이 문제도 동적 계획법으로 풀 수 있다. 하지만 다른 문제에서 동적 계획법에 필요한 메모리나 시간이 과도하게 크기 때문에 탐욕법을 사용한다.

* ### 예제: 출전 순서 정하기(문제 ID: MATCHORDER, 난이도: 하)
#### &nbsp;전세계 최대의 프로그래밍 대회 알고스팟 컵의 결승전이 이틀 앞으로 다가왔습니다. 각 팀은 n명씩의 프로 코더들로 구성되어 있으며, 결승전에서는 각 선수가 한 번씩 출전해 1:1 경기를 벌여 더 많은 승리를 가져가는 팀이 최종적으로 우승하게 됩니다. 각 팀의 감독은 대회 전날, 주최측에 각 선수를 출전시킬 순서를 알려 주어야 합니다. 결승전 이틀 전, 한국팀의 유감독은 첩보를 통해 상대 러시아팀의 출전 순서를 알아냈습니다. 이 대회에서는 각 선수의 실력을 레이팅(rating)으로 표현합니다. 문제를 간단히 하기 위해 1:1 승부에서는 항상 레이팅이 더 높은 선수가 승리하고, 레이팅이 같을 경우 우리 선수가 승리한다고 가정합시다. 표와 같이 출전 순서를 정했다고 하면 한국팀은 2번, 3번, 5번, 6번 경기에서 승리해 전체 네 경기를 이기게 됩니다. 그러나 대신 4번 경기와 1번 경기에 나갈 선수를 바꾸면 1번 경기만을 제외하고 모든 경기에 승리할 수 있지요. 상대방 팀 선수들의 순서를 알고 있을 때, 어느 순서대로 선수들을 내보내야 승수를 최대화할 수 있을까요?

* ### 시간 및 메모리 제한
#### &nbsp;프로그램은 1초 안에 실행되어야 하며, 64MB 이하의 메모리를 사용해야 한다.

* ### 입력
#### &nbsp;입력의 첫 줄에는 테스트 케이스의 수 C (C≤50)가 주어집니다. 각 테스트 케이스의 첫 줄에는 각 팀 선수의 수 N(1≤N≤100)가 주어집니다. 그 다음 줄에는 N개의 정수로 러시아팀 각 선수의 레이팅이 출전 순서대로 주어지며, 그 다음 줄에는 N개의 정수로 한국팀 각 선수의 레이팅이 무순으로 주어집니다. 모든 레이팅은 1 이상 4000 이하의 정수입니다.

* ### 출력
#### &nbsp;각 테스트 케이스마다 한 줄에 한국팀이 얻을 수 있는 최대 승수를 출력합니다.

* ### 개인적 풀이
#### &nbsp;한국팀 레이팅을 오름 차순으로 배열하고, 러시아팀 레이팅과 비교해서 가장 차이가 적으면서 러시아 레이팅 이상의 값을들 배치해주면 될 것 같다.