# Problem 2: Speeding Ticket
**Problem Statement: https://usaco.org/index.php?page=viewproblem2&cpid=568**

Idea:

Code:

```c++
#include <bits/stdc++.h>
using namespace std;
int N, M;
vector<pair<int, int>>road;
vector<pair<int, int>>bessie;
int main(){
    freopen("speeding.in", "r", stdin);
    freopen("speeding.out", "w", stdout);
    cin>>N>>M;
    road.assign(N, {0, 0});
    bessie.assign(M, {0, 0});
    for(int i = 0;i<N;i++){
        cin>>road[i].first>>road[i].second;
    }
    for(int i = 0;i<N;i++){
        cin>>bessie[i].first>>bessie[i].second;
    }
    int currroad = 0, currbessie = 0, lastroad = 0, lastbessie= 0;
    int ans = 0;
    for(int i = 1;i<=100;i++){
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
