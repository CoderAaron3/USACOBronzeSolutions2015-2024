# Problem 1: Milk Pails

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=615](url)**

Idea:

Code:
```c++
#include <bits/stdc++.h>
using namespace std;
int X, Y, M;
int main(){
    freopen("pails.in", "r", stdin);
    freopen("pails.out", "w", stdout);
    cin>>X>>Y>>M;
    int sum = 0;
    int ans = M;
    for(int i = 0;i<=M / X;i++){
        sum = i * X;
        int total = ((M - sum) / Y) * Y;
        ans = min(ans, M - (total+sum));
    }
    cout<<M-ans<<endl;
}
