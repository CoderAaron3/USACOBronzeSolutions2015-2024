# Problem 3: Contaminated Milk
**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=569](url)**

Idea:

Code:
```c++
#include <bits/stdc++.h>
using namespace std;
//initialize variables
int N, M, D, S;
vector<int>p, m, t;
vector<int>sick;
vector<vector<int>>persontomilk;
int main(){
    freopen("badmilk.in", "r", stdin);
    freopen("badmilk.out", "w", stdout);
    //input and size arrays
    cin>>N>>M>>D>>S;
    p.assign(D, 0);
    m.assign(D, 0);
    t.assign(D, 0);
    sick.assign(N, -1);
    persontomilk.assign(N, vector<int>(M, 101));
    //input arrays
    for(int i = 0;i<D;i++){
        cin>>p[i]>>m[i]>>t[i];
        //store by the person p[i] drank milk m[i] earliest at t[i] and previously stored time
        persontomilk[p[i]-1][m[i]-1] = min(t[i], persontomilk[p[i]-1][m[i]-1]);
    }
    for(int i = 0;i<S;i++){
        int a, b;
        cin>>a>>b;
        sick[a-1] = b;
    }
    int ans = 0;
    for(int i = 0;i<M;i++){
        bool flag = false;
        int curr = 0;
        for(int j = 0;j<N;j++){
            // 🔹 FIX 2: condition reversed — milk invalid if drank >= sickness time
            if(sick[j] != -1 && persontomilk[j][i] >= sick[j]){
                flag = true;
                break;
            }
            else{
                if(persontomilk[j][i] != 101) curr++; // 🔹 FIX 3: count only if drank
            }
        }
        if(flag) continue;
        ans = max(ans, curr);  // 🔹 FIX 4: take max instead of overwrite+break
    }
    cout<<ans<<endl;
}
