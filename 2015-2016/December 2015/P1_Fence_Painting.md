**Problem Statement: https://usaco.org/index.php?cpid=567&page=viewproblem2**

Idea:

There are 2 situations for this problem: 
1. Overlap between 2 intervals, where we would find the overlap by taking the minimum of the 2 endpoints (end of overlap) and subtracting by the maximum of the 2 start points (start of overlap)
2. There is no overlap, so overlap = 0
The answer is then simply the sum of the 2 intervals minus the overlap

*Tip: Draw out a few cases to prove/understand the interval formula if you are struggling

Code:
```c++
#include <bits/stdc++.h>
using namespace std;
//intialize variables
int a, b, c, d;
int main(){
    freopen("paint.in", "r", stdin);
    freopen("paint.out", "w", stdout);
    cin>>a>>b>>c>>d;//input
    int overlap = max(0, min(b, d) - max(a, c));//calculate overlap between two intervals
    cout<<(b-a) + (d-c) - overlap<<endl;//sum of two intervals - overlap
}
