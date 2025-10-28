# Problem 2: Circular Barn

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=616](url)**

Idea:

Code:

```c++
#include <bits/stdc++.h>
using namespace std;
int N;
vector<int>rooms;
int main(){
    cin>>N;
    rooms.assign(N, 0);
    for(int i = 0;i<N;i++){
        cin>>rooms[i];
    }
    int ans = INT_MAX;
    for(int i = 0;i<N;i++){
        int curr = 0;
        for(int j = i+1;j<N+i;j++){
            int currroom = rooms[j % N];
            curr += currroom * (j - i);
        }
        ans = min(ans, curr);
    }
    cout<<ans<<endl;
}
