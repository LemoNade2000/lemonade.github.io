---
title: "2022 Credit Suisse Global Coding Challenge Problems / Solutions"
date: 2022-10-23 16:00:00
categories:
tags:
---
### About the challenge
Credit Suisse hosted a algorithm contest again this year. 
This was my first time participating in this particular competition, and there were some aspects that the contest differed from other competitions.

There are total 9 problems that you have to solve, 3 for each difficulties of easy, normal, hard.

It runs for more than two weeks, so you have more than enough time to tackle every problem, and try to optimize it. I spent about a week tackling problems, then rest was about optimizing the runtime.

![Leaderboard](https://github.com/LemoNade2000/lemonade2000.github.io/blob/master/_posts/Let'sGo.PNG)

There were 1690 contestants this year, about 200 from SEA. I was placed 30th in the global leaderboard, and 8th in SEA. Also I was first among contestants from my school :)

I recommend it for people who are just getting into the competitive programming, or problem solving (just like me!), since the difficulties in solving the problems are quite lenient compared to other well-known competitions.

In this post, I'll try to go through my approaches in each problems, and by no means it is a exhaustive editorial of the contest. If there are any inconsistencies, please let me know and I'll try to fix it.

### 1. Banker and Stocks (Easy)

#### My Thoughts

For this problem, you have to decide whether a number has more even-numbered divisors, or odd-numbered divisors. For example, 10 has divisors {1, 2, 5, 10}, so it has 2 even divisors and odd divisors, so you would output "PASS" in this case.

First, I thought it was a very straightforward problem. 

My first instinct was to do a prime factorization for each integer that is going to be given.

However, this approach will take very long time, as the input integer can be quite large. The first observation that I made while thinking about this problem was that every odd number is going to have 0 divisors that is an even number, since you cannot get a odd number by multiplying an even number to any number. Thus, if the number is an odd number, you can safely assume that it has more odd divisors.
Secondly, for a number to have more even number divisors, you need to have at least one pair that is made of two even numbers. Only one pair of such condition is enough to tell you that a number has more even number divisors, since you cannot multiply two odd numbers to get even number. Therefore, if a number is divisible by 4, you can assume that it has more even number divisors.
Lastly, even numbers that are not divisible by 4 are going to have equal number of both divisors.
Then, I manipulated some parts of the code in order to optimize every arithmetic.

#### Entire Code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
#define MOD 1000000007

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    ll st;
    cin >> st;

    ll a, b;
    a = b = st;
    if(st & 1){
        cout << "SELL";
        return 0;
    }
    else if(!(a & 1) && !((b >> 1) & 1)){
        cout << "BUY";
        return 0;
    }
    else{
        cout << "PASS";
        return 0;
    }
    return 0;
}
```

### 2. File Reorganization (Easy)

#### My thoughts

For this problem, you have to decide the length of longest palindrome given that you can pick and reorder any number of alphabets from a string. For example, given "acbbaadc", you can pick "bcaaacb" or any other palindromes that use 3 'a', 2 'b', and 2'c'. 

My approach was to keep a count of alphabets, and also keep track whether there is an alphabet that has odd number count. If there is at least one alphabet that has odd number count, then we can use that alphabet odd number of times in the middle, like "aaa" in our exmaple. Other than that, all I had to do was to increase the answer by 2 whenever an alphabet reached count 2.


#### Entire Code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
#define MOD 1000000007

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    string str;
    cin >> str;
    vector<int> cnt = vector<int>(30);
    int ans = 0;
    int oneCnt = 0;
    for(int i = 0; i < str.size(); i++){
        cnt[str[i] - 'a']++;
        if(cnt[str[i] - 'a'] == 2){
            ans += 2;
            oneCnt--;
            cnt[str[i] - 'a'] = 0;
        }
        else{
            oneCnt++;
        }
    }
    if(oneCnt){
        ans++;
    }
    cout << ans << "\n";
    return 0;
}
```

### 3. Sorting Share Prices (Easy)

#### My Thoughts

For this problem, you have to decide whether you can sort the list of numbers. However, you can only reverse the order of sub-sequence if the sum of elements in the subsequence does not exceed threshhold number M. For example, if threshhold is 4 and the list is {3, 4, 3, 2, 1}, then you cannot sort the list because you cannot reverse {4, 3}.

My approach was to check if there exists any pair of elements that are not sorted and their sum exceeds M. For this, I only kept track of the maximum number that the list contained as I checked through the sequence, and if there exists a number that exceeds M when summed with maximum value, then I wouldn't be able to reverse the order of those two pairs, which would make the sorting impossible. If no such pair is found, I am guaranteed to be able to sort the sequence.

#### Entire Code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
#define MOD 1000000007

vector<int> arr;
int N, M;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> N >> M;
    arr = vector<int>(N);
    int maxi = -1;
    for(int i = 0; i < N; i++){
        cin >> arr[i];
        if(arr[i] > maxi){
            maxi = arr[i];
        }
        else if(arr[i] < maxi){
            if(arr[i] + maxi > M){
                cout << "0";
                return 0;
            }
        }
    }
    cout << "1";
    return 0;
}
```

### 4. Time Intervals (Medium)

#### My Thoughts

For this problem, you were given list of employees along with their shifts. Then, you had to output the exhaustive list of time intervals and list of names of people who were working in that shift. For example, if A works {1, 5} and B works {2, 6}, Then, you have to output 
```
1 2 A
2 5 A B
5 6 B
```
Also, the names had to be sorted lexicographically.

I approached this problem by creating a class "Info" that contained information of a person either entering or exiting their shift. Then, I kept track of every time interval where changes were being made, since the window was very small (Employees only have 20 hours of time window combined). For each time interval, I created a vector containing information about that specific time. Then, I iterated through all time slots from 0 to 21, then kept the list of people who were working by making a ordered set. Conveniently, this also takes care of sorting lexicographically.

During this problem, I faced a problem of not being able to enter a newline character. Also, I found a test case which did not fit the problem constraint at the beginning, so the staffs were able to change the problem statement.

#### Entire Code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
#define MOD 1000000007

int N;
vector<string> names;
vector<pii> times;

struct Info{
    bool goingIn; 
    int idx;
}; 

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> N;
    vector<vector<Info>> schedule;
    names = vector<string>(N);
    schedule = vector<vector<Info>>(22);
    vector<int> cnt(22, 0);
    for(int i = 1; i <= 21; i++){
        schedule[i] = vector<Info>(0);
    }
    for(int i = 0; i < N; i++){
        cin >> names[i];
    }
    int ansC = 0;
    for(int i = 0; i < N; i++){
        int u, v;
        cin >> u >> v;
        schedule[u].push_back({true, i});
        schedule[v].push_back({false, i});
        if(cnt[u] == 0){
            cnt[u]++;
            ansC++;
        }
        if(cnt[v] == 0){
            cnt[v]++;
            ansC++;
        }
    }
    set<string> active;
    int prev = -1;
    cout << ansC - 1;
    for(int i = 1; i <= 21; i++){
        if(schedule[i].empty()){
            continue;
        }
        if(prev == -1){
            prev = i;
            for(auto info : schedule[i]){ 
                if(info.goingIn){
                    active.insert(names[info.idx]);
                }
                else{
                    active.erase(names[info.idx]);
                }
            }
            continue;
        }
        cout << "\n" << prev << " " << i << " " << active.size();
        if(active.empty()){
            cout << " ";
        }
        for(auto j : active){
            cout << " " << j;
        }
        for(auto info : schedule[i]){
            if(info.goingIn){
                active.insert(names[info.idx]);
            }
            else{
                active.erase(names[info.idx]);
            }
        }
        prev = i;
    }
    return 0;
}
```

### Machine Learning Model Manager Program (Medium)

#### My Thoughts

First problem that utilizes an algorithm following an observation!

For this problem, you are given a sequence of integers. Then, you have to find all subsequence such that the sum of subsequence do not exceed a threshhold number, M. For example, if you are given M = 10 and {5, 4, 9, 7, 6}, then your combinations can be |{5}, {4}, {9}, {7}, {6}, {5, 4}| = 6.

For this problem, I used prefix sum to keep track of sums up until k-th element. Then, since there are no negative elements, what I had to do was find the smallest j where prefixSum[k] - prefixSum[j] < M. First, I thought about doing a binary search to find this smallest j. About a day later, I came up with two-pointer strategy for this problem. The observation that you have to make is that the sequence of j_1, j_2, ... , j_N is a increasing sequence. Thus, you can set one pointer as your k, and one as your j, and check through all prefix sum. Essentially, this becomes a O(N) problem.

#### Entire Code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
#define MOD 1000000007

int C;
int N;
vector<int> arr;
vector<int> prefix;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> C;
    cin >> N;
    arr = vector<int>(N);
    prefix = vector<int>(N + 1, 0);
    int sum = 0;
    for(int i = 0; i < N; i++){
        cin >> arr[i];
        prefix[i + 1] = prefix[i] + arr[i];
    }
    int lIdx = 0;
    int i = 1;
    while(i <= N){
        while(prefix[i] - prefix[lIdx] >= C){
            lIdx++;
            if(lIdx == i) break;
        }
        sum += i - lIdx;
        i++;
    }
    cout << sum << "\n";
    return 0;
}
```

### Reporting System (Medium)

#### My Thoughts

For this problem, you are given N reports, which then reports to other reports (a report 'jump'). Then, you need to determine how many report 'jumps' each reports need in order to return back to original report. You are given that only one jump is made to a report and from the report. For example, {2, 1, 3, 5, 4} gives {2, 2, 1, 2, 2}.

First, you are guaranteed to be given N jumps, one for each report. If you consider a report a node of a graph, and a jump an edge, then this becomes quite clear that it is finding the size of cycles that each nodes are in. How do we know if all nodes have cycles? It has N edges, so there has to be a cycle, and there are only one edge that comes in and out from each node, so you can guarantee that every graph has a cycle that it belongs to.

Second part of this problem is about how you implement the solution. I used another well known algorithm, union-find. Since every node in a cycle is going to have same number of jumps, you only need to keep track of the parent of each nodes, and how many child does that parent has, since the number of child + 1 is the same as the jumps that it will make. 

Last improvement to the algorithm is to use path compression and union by rank. This increased my score by few points. You can find more about this optimization at:
https://iq.opengenus.org/union-find-optimizations/

#### Entire Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define MOD 1000000007

typedef long long ll;
typedef long double ld;
typedef pair<int, int> pii;

struct Node{
    int idx;
    int parent;
    int cnt;
    int rank;
    Node(){
        this->idx = 0;
        this->parent = 0;
        this->cnt = 1;
        this->rank = 1;     
    }
};

int N;
vector<Node> nodes;

int find(int n){
    if(nodes[n].parent == n){
        return n;
    }
    else return nodes[n].parent = find(nodes[n].parent);
}

int unionNode(int u, int v){
    u = find(u);
    v = find(v);
    if(u == v){
        return 0;
    }
    if(nodes[u].rank > nodes[v].rank){
        swap(u, v);
    }
    nodes[v].cnt += nodes[u].cnt;
    nodes[u].parent = v;
    if(nodes[u].rank == nodes[v].rank){
        nodes[v].rank++;
    }
    return 0;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> N;
    nodes = vector<Node>(N + 1);
    for(int i = 1; i <= N; i++){
        nodes[i].parent = i;
        nodes[i].idx = i;
    }
    for(int i = 1; i <= N; i++){
        int n;
        cin >> n;
        unionNode(i, n);
    }
    for(int i = 1; i <= N; i++){
        cout << nodes[find(i)].cnt << " ";
    }
    return 0;
}
```

### Fraudulent Transactions (Hard)

#### My Thoughts

For this problem, you are given N customers and M transactions that they make. If a customer makes any transaction that at some point comes back to the customer, than the transaction is considered ineligible. Your objective is to find if the list of transactions are whether eligible or ineligible. For example,
```
4 5 // 4 People, 5 Transactions
1 2 // 1 to 2
2 3 // 2 to 3
2 4 // 2 to 4
3 5 // 3 to 5
4 1 // 4 to 1, which is fraudulent!
```

Thus, the above example is ineligible.

My approach was to again treat it like a graph. Then, it was again finding a cycle within the graph. However, you could have transactions that directly go from 1 to 1, which is not fraudulent. 

I used DFS and array of processing nodes to check whether a graph has an active cycle. If you process DFS for unvisited nodes and in the process comes in contact with a processing node, then you can say that the graph has a cycle.

#### Entire Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define MOD 1000000007

typedef long long ll;
typedef long double ld;
typedef pair<int, int> pii;

int N, M;
vector<vector<int>> adj;
vector<bool> visited;
vector<bool> process;

bool dfs(int node){
    process[node] = true;
    for(auto dest : adj[node]){
        if(process[dest]){
            return false;
        }
        else if(!visited[dest]){
            if(!dfs(dest)){
                return false;
            }
        }
    }
    visited[node] = true;
    process[node] = false;
    return true;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> N >> M;
    adj = vector<vector<int>>(N + 1);
    visited = vector<bool>(N + 1, false);
    process = vector<bool>(N + 1, false);
    for(int i = 0; i < M; i++){
        int u, v;
        cin >> u >> v;
        if(u == v){
            continue;
        }
        adj[u].push_back(v);
    }
    for(int i = 1; i <= N; i++){
        if(!visited[i]){
            if(!dfs(i)){
                cout << "Ineligible";
                return 0;
            }
        }
    }
    cout << "Eligible";
    return 0;
}
```

### Risk Mitigation (Hard)

#### My Thoughts

*** First question where I had spent a lot of time thinking about. ***

The problem is quite interesting in my opinion, although some might consider it classic.

You are given a sequence of N risk values. You can do M projects that has to be disjoint in terms. If you start your project at i and end at j, you mitigate risk[j] - risk[i]. Your job is to find the maximum mitigated risk that can be done by M projects. For example, if N = 5 and M = 2, risk = {5, 8, 2, 4, 7}, answer = 8 - 5 + 7 - 2 = 8.

First, I thought it was a problem that can be solved by greedy approach. My intuition was to compress the risk values into sequence alternating increase and decrease. For example, {5, 8, 2, 4, 7} becomes {3, -6, 5}. Then, greedily adding M highest increases seems to give a solution. However, this fails when you have increases that are bigger than decrease. For example, {0, 100, 80, 120} with M = 1. If you use greedy approach, you get max(100, -20, 40) = 100. However, you can start at t = 0 and end at t = 3, which then gives 120 - 0 = 120.

My next approach was to use dynamic programming. The problem has few states with it : the number of projects that you can do, the time that one is in, and whether you are currently doing a project. Then, the problem becomes finding the maximum dp value with dp[M][T][X] = maximum risk mitigated with M project left, at t-th element in compression, with X = false if you are currently doing project, and true otherwise. I thought I could still use the compression method that I made above while tackling greedy approach. DP recursion formula I came up with is following

```cpp
dp[i][j][k] = // doing project
	if(k == 1)
		max(
			dp[i][j + 2][0] + compressedRisk[j] // You end the project, and move on to next risk-mitigating term
			dp[i][j + 2][1] + compressedRisk[j] + compressedRisk[j + 1] // You continue the project, and accept the negative risk mitigation at j + 1
		)
	if(k == 0) // not doing project
		max( 
			dp[i][j + 2][0] // You continue to move on without doing any project
			dp[i - 1][j + 2][1] + compressedRisk[j] + compressedRisk[j + 1] // You start project, and continue to do it until the next term
			dp[i - 1][j + 2][0] + compressedRisk[j] // You start and end immediately, effectively mitigating risk at time t. 
		)
```

I can add 2 to j since I know the compressed risk is alternating in signs.

I optimized it using some memoization technique at the end. It took me about 2 days to formulate the idea and optimize it.

#### Entire Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define MOD 1000000007

typedef long long ll;
typedef long double ld;
typedef pair<int, int> pii;

int N, limit;
vector<int> arr;
vector<vector<vector<int>>> dp; // dp[i][j] : with i-segments left, maximum from index j
vector<int> compact;

int maximumSubseq(int start, int end){
    int ret = 0;
    int sum = 0;
    for(int i = start; i <= end; i++){
        sum += compact[i];
        ret = max(ret, sum);
        if(sum < 0){
            sum = 0;
        }
    }
    return ret;
}

int fillDP(int cond, int start, int end, int chance){
    if(start > end || chance == 0){
        return 0;
    }
    else if(start == end){
        return compact[start];
    }
    if(dp[cond][start][chance] != -1){
        return dp[cond][start][chance];
    }
    if(cond == 0){
        if(chance == 1){
            dp[cond][start][chance] = maximumSubseq(start, end);
            return dp[cond][start][chance];
        }
        dp[cond][start][chance] = max({fillDP(0, start + 2, end, chance),
        fillDP(1, start + 2, end, chance - 1) + compact[start] + compact[start + 1],
        fillDP(0, start + 2, end, chance - 1) + compact[start]
        });
        return dp[cond][start][chance];
    }
    else if(cond == 1){
        dp[cond][start][chance] = max(
        fillDP(1, start + 2, end, chance) + compact[start] + compact[start + 1],
        fillDP(0, start + 2, end, chance) + compact[start]
        );
        return dp[cond][start][chance];
    }
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> limit >> N;
    arr = vector<int>(N);
    for(int i = 0; i < N; i++){
        cin >> arr[i];
    }
    if(limit == 0){
        cout << "0\n";
        return 0;
    }
    int increasing = 0; // 0 - neutral 1 - inc -1 - dec
    int nextIncrease = 0;
    int prev = arr[0];
    int s = 0;
    for(int i = 1; i < N; i++){
        if(arr[i] > prev){
            nextIncrease = 1;
        }
        else if(arr[i] == prev){
            continue;
        }
        else{
            nextIncrease = -1;
        }
        if(increasing != nextIncrease){
            if(s != 0){
                compact.push_back(increasing * s);
            }
            s = 0;
            increasing = nextIncrease;
        }
        s += abs(arr[i] - prev);
        prev = arr[i];
    }
    if(s != 0){
        compact.push_back(increasing * s);
    }
    int start = 0;
    int end = compact.size() - 1;
    if(compact.empty()){
        cout << "0\n";
        return 0;
    }
    if(compact[0] < 0){
        start++;
    }
    if(compact[end] < 0){
        end--;
    }
    if((end - start + 2) / 2 <= limit){
        int sum = 0;
        for(;start <= end; start += 2){
            sum  += compact[start];
        }
        cout << sum;
        return 0;
    }
    dp = vector<vector<vector<int>>>(2);
    for(int i = 0; i < 2; i++){
        dp[i] = vector<vector<int>>(end + 1);
        for(int j = 0; j <= end; j++){
            dp[i][j] = vector<int>(limit + 1, -1);
        }
    }
    cout << fillDP(0, start, end, limit);
    return 0;
}
```

### Minimum Cost to Decrypt Matrix (Hard)

*** Second question that took me majority of the time for the contest, and probably the hardest question in the contest. ***

The problem starts with giving you a N x N square board filled with '-' and '$'. You can use c - cost square to cover c x c square of the board. The objective is to find a minimum cost that you need to spend in order to cover all '$' on the board.

For example,

```
--$--
$$$--
-----
-----
----$
```
You need 3 costs to cover the group at the left corner, and 1 cost to cover the single '$' at the last row. It is guaranteed that N <= 50. 

My first approach to get partial score was to just guess N for each test cases. It is guaranteed that you can cover all targets using N x N square. It gave me about 30 points, which I think is quite large...?

Then, I changed my approach to finding a D&C strategy for the problem. I thought you would be able to find the optimal solution if you can divide up the board into parts that contained targets. However, at first, you would need to choose wisely how you are going to divide the board. I chose dividing at each points, which made me spend O(N^2) at least. I thought this was necessary because you cannot know which strategy is going to be the best. However, once you divide the board horizontally and vertically for the first time, you can simply divide the smaller pieces in the middle, since I know that I'm going to exhaustively check all the divide points, and I am going to check every possible combinations at the board at the end. Therefore, I spent O(NlogN) for each iterations. My theoretical time complexity to this problem is O(N^3 logN), which I think some people could improve it to O(N^3) using efficient methods in finding the outermost targets given a rectangle.

I again used some optimization techniques same as the previous problems.

#### Entire Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define MOD 1000000007

typedef long long ll;
typedef long double ld;
typedef pair<int, int> pii;

int N;
string matrix[55];
vector<int> rowDollar[55];
vector<int> colDollar[55];
int dp[50][50][50][50];

int findMin(int rowStart, int rowEnd, int colStart, int colEnd){
    if(rowStart >= N || colStart >= N || rowEnd < 0 || colEnd < 0){
        return 0;
    }
    int &ret = dp[rowStart][rowEnd][colStart][colEnd];
    if(ret != -1){
        return ret;
    }
    if(rowStart > rowEnd || colStart > colEnd){
        return ret = 0;
    }
    if(rowStart == rowEnd || colStart == colEnd){
        int cnt = 0;
        if(rowEnd == rowStart){
            cnt = upper_bound(rowDollar[rowStart].begin(), rowDollar[rowStart].end(), colEnd) - lower_bound(rowDollar[rowStart].begin(), rowDollar[rowStart].end(), colStart);
        }
        if(colEnd == colStart){
            cnt = upper_bound(colDollar[colStart].begin(), colDollar[colStart].end(), rowEnd) - lower_bound(colDollar[colStart].begin(), colDollar[colStart].end(), rowStart);
        }
        return ret = cnt;
    }
    int iMax, iMin;
    int jMax, jMin;
    iMax = jMax = -1;
    iMin = jMin = 60;
    int cnt = 0;
    for(int i = rowStart; i <= rowEnd; i++){
        if(rowDollar[i].empty()){
            continue;
        }
        auto it = lower_bound(rowDollar[i].begin(), rowDollar[i].end(), colStart);
        if(it == rowDollar[i].end() || *it > colEnd) continue;
        else {
            iMin = i;
            cnt++;
        }

        if(iMin != 60){
            break;
        }
    }
    for(int i = rowEnd; i >= rowStart; i--){
        if(rowDollar[i].empty()){
            continue;
        }
        auto it = lower_bound(rowDollar[i].begin(), rowDollar[i].end(), colStart);
        if(it == rowDollar[i].end() || *it > colEnd) continue;
        else {
            iMax = i;
            cnt++;
        }
        if(iMax != -1){
            break;
        }
    }
    for(int i = colStart; i <= colEnd; i++){
        if(colDollar[i].empty()){
            continue;
        }
        auto it = lower_bound(colDollar[i].begin(), colDollar[i].end(), rowStart);
        if(it == colDollar[i].end() || *it > rowEnd) continue;
        else {
            jMin = i;
            cnt++;
        }
        if(jMin != 60){
            break;
        }
    }
    for(int i = colEnd; i >= colStart; i--){
        if(colDollar[i].empty()){
            continue;
        }
        auto it = lower_bound(colDollar[i].begin(), colDollar[i].end(), rowStart);
        if(it == colDollar[i].end() || *it > rowEnd) continue;
        else{
            jMax = i;
            cnt++;
        }
        if(jMax != -1){
            break;
        }
    }
    if(cnt <= 2){
        return ret = 2;
    }
    int width = jMax - jMin + 1;
    int height = iMax - iMin + 1;
    ret = max(width, height);
    int erMid = (iMin + iMax) / 2;
    int ecMid = (jMin + jMax) / 2;
    int intLeft = findMin(iMin, iMax, jMin, ecMid) + findMin(iMin, iMax, ecMid + 1, jMax);
    int intRight = findMin(iMin, erMid, jMin, jMax) + findMin(erMid + 1, iMax, jMin, jMax);
    ret = min({ret, intLeft, intRight});
    return ret;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    cin >> N;
    memset(dp, -1, sizeof(dp));
    for(int i = 0; i < N; i++){
        cin >> matrix[i];
    }
    int iMax, iMin;
    int jMax, jMin;
    iMax = jMax = -1;
    iMin = jMin = 60;

    deque<int> r, c;
    
    for(int i = 0; i < N; i++){
        for(int j = 0; j < N; j++){
            if(matrix[i][j] == '$'){
                rowDollar[i].push_back(j);
                r.push_back(i);
            }
            if(matrix[j][i] == '$'){
                colDollar[i].push_back(j);
                c.push_back(i);
            }
        }
    }
    if(r.size() <= 2){
        cout << r.size();
        return 0;
    }
    iMin = r.front();
    iMax = r.back();
    jMin = c.front();
    jMax = c.back();
    int erMid = (iMin + iMax) / 2;
    int ecMid = (jMin + jMax) / 2;
    int ans = N;
    for(int i = iMin; i <= iMax; i++){
        for(int j = jMin; j <= jMax; j++){
            ans = min(ans, findMin(iMin, i, jMin, j) + findMin(i + 1, iMax, jMin, j) + findMin(iMin, i, j + 1, jMax) + findMin(i + 1, iMax, j + 1, jMax));
        }
    }
    cout << ans;
    return 0;
}
```

## Conclusion

I would say CS's GCC is one-of-its-kind competition in CP. There are not many algorithm competitions that give contestants more than 2 weeks to solve problems. Moreover, there are many contests that break ties with runtime, but not many use it at calculating the score. It is very unorthodox (although I'm not even an year into CP) for a contest to utilize these rules, although I enjoyed it since I really like to think over a problem for long time and try to come up with a better solution. 
The quality of contest organization was quite high in my opinion, since you could expect timely reply of staff members regarding questions. However, one thing I think it lacked was quality in test cases. Along with my strategy of simply guessing N for the last problem, many problems seemed to be lacking in exhaustive inspection in problem statements and test cases on the edge.