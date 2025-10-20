# Problem 1: Promotion Counting
**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=591](url)

Idea:

Code:
```c++
#include <bits/stdc++.h>
using namespace std;
int bronze1, bronze2;
int silver1, silver2;
int gold1, gold2;
int plat1, plat2;
int main(){
    freopen("promote.in", "r", stdin);
    freopen("promote.out", "w", stdout);
    cin>>bronze1>>bronze2;
    cin>>silver1>>silver2;
    cin>>gold1>>gold2;
    cin>>plat1>>plat2;
    int goldtoplat = plat2 - plat1;
    int silvertogold = goldtoplat + (gold2 - gold1);
    int bronzetosilver = silvertogold + (silver2-silver1);
    cout<<bronzetosilver<<endl;
    cout<<silvertogold<<endl;
    cout<<goldtoplat<<endl;
}
```
