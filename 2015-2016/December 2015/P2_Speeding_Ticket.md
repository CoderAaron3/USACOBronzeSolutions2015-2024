# Problem 2: Speeding Ticket
**Problem Statement: https://usaco.org/index.php?page=viewproblem2&cpid=568**

Idea:

- Simulate Bessie’s journey mile by mile along the 100-mile road.

- For each mile, keep track of which segment of the road and which segment of Bessie’s route we are currently in.

- Compare Bessie’s speed with the road’s speed limit at that mile and record the maximum amount she exceeds the limit.

- As we move along the road, update the current segment whenever we reach the end of one.

Code:

```c++
#include <bits/stdc++.h>
using namespace std;
//initalize variables and arrays
int N, M;
vector<pair<int, int>>road;
vector<pair<int, int>>bessie;
int main(){
    freopen("speeding.in", "r", stdin);
    freopen("speeding.out", "w", stdout);
    cin>>N>>M;
    road.assign(N, {0, 0});
    bessie.assign(M, {0, 0});
    //input road by pair of {segment length, speed limit}
    for(int i = 0;i<N;i++){
        cin>>road[i].first>>road[i].second;
    }
    //input bessie by pair of {segment length, speed}
    for(int i = 0;i<M;i++){
        cin>>bessie[i].first>>bessie[i].second;
    }
    //intialize variables for curr road segment and bessie segment and last bessie and road segment
    int currroad = 0, currbessie = 0, lastroad = 0, lastbessie= 0;
    int ans = 0;
    //do stimulation mile by mile
    for(int i = 1;i<=100;i++){
        //update ans if bessie exceeds speeed limit by greater amount
        ans = max(ans, max(0, bessie[currbessie].second - road[currroad].second));
        if(lastbessie + bessie[currbessie].first <= i){
            lastbessie = i;
            currbessie++;
        }
        if(lastroad + road[currroad].first <= i){
            lastroad = i;
            currroad++;
        }
    }
    cout<<ans<<endl;
}
