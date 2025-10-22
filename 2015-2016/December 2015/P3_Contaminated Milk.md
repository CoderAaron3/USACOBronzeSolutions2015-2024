# Problem 3: Contaminated Milk
**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=569](url)**

**Idea:**

The idea is to check for every milk type whether that milk could be the bad milk (since M is very small) and check for every person who got sick whether they got sick or not after they drank the milk being considered. 

We want to figure out which milk type could be contaminated. For each milk type, we check all people who got sick. If anyone drank that milk after getting sick or never drank it, that milk cannot be bad. If the milk is possible, we count everyone who drank it, since they could potentially get sick. Finally, we pick the milk that could make the most people sick.

**Code:**
```c++
#include <bits/stdc++.h>
using namespace std;
int N, M, D, S; // N = people, M = milk types, D = drinking events, S = sickness events
vector<int> p, m, t; // Drinking events: person, milk, time
vector<int> sick;     // sick[p] = time person p got sick (-1 if not sick)
vector<vector<int>> persontomilk; // persontomilk[p][m] = earliest time person p drank milk m

int main(){
    freopen("badmilk.in", "r", stdin);
    freopen("badmilk.out", "w", stdout);

    cin>>N>>M>>D>>S;

    // Initialize arrays
    p.assign(D, 0);
    m.assign(D, 0);
    t.assign(D, 0);
    sick.assign(N, -1); // default -1 = person not sick
    persontomilk.assign(N, vector<int>(M, 101)); //101 = never drank

    // Record drinking events
    for(int i = 0;i<D;i++){
        cin>>p[i]>>m[i]>>t[i];
        // Store earliest time this person drank this milk
        persontomilk[p[i]-1][m[i]-1] = min(t[i], persontomilk[p[i]-1][m[i]-1]);
    }

    // Record sickness events
    for(int i = 0;i<S;i++){
        int a, b;
        cin>>a>>b;
        sick[a-1] = b; // time person a got sick
    }

    int ans = 0; // maximum number of people who could get sick

    // Check each milk type
    for(int i = 0;i<M;i++){
        bool flag = false; // true if milk cannot be contaminated
        int curr = 0;      // counts people who drank this milk

        for(int j = 0;j<N;j++){
            // If sick person drank this milk at or after getting sick, milk is invalid
            if(sick[j] != -1 && persontomilk[j][i] >= sick[j]){
                flag = true;
                break;
            }
            // Count everyone who drank this milk (ignore those who never drank it)
            else if(persontomilk[j][i] != 101) {
                curr++;
            }
        }

        // Update answer if this milk is valid
        if(!flag) ans = max(ans, curr);
    }
    cout<<ans<<endl;
}
