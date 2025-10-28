# Problem 1: Milk Pails

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=615](url)**

Idea:

The idea for this problem is to try all possible number of X pails that we dump into M.

We have two pails (X and Y) and a target capacity (M). Since X is the smaller capacity, we can't use it more than M/X times. So, the strategy is simple:

- Iterate all possible counts for the X pail. We check 0, 1, 2... up to the max times the X pail can be used.
- Maximize the $Y$ pail: For each X count, we figure out the max number of Y pails we can dump in without going over M.
- Find the Best Combo: We keep track of which combination of X and Y gets us closest to M. 
It's essentially a O(M/X) simulation to find the optimal mix.

Code:
```c++
#include <bits/stdc++.h>
using namespace std;
// X and Y are pail sizes, M is the target capacity
int X, Y, M;
int main(){
    // Standard file I/O for USACO.
    freopen("pails.in", "r", stdin);
    freopen("pails.out", "w", stdout);
    // Grab inputs
    cin>>X>>Y>>M;
    // sum holds the current total for i * X.
    int sum = 0;
    // ans stores the minimum difference: M - (total milk). Initialize high (to M).
    int ans = M; 
    // Loop: Brute force the number of X pails used (from 0 up to max possible).
    for(int i = 0;i<=M / X;i++){
        sum = i * X; // Total milk from X pails.
        // Calculate max total milk from Y pails without going over M.
        // (M - sum) is the remaining space. Dividing by Y gives max count.
        // Multiplying by Y again gives the actual milk volume.
        int total = ((M - sum) / Y) * Y;
        // Check if this combination is better, ans minimizes the gap (M - total milk).
        ans = min(ans, M - (total+sum));
    }
    // Output closest milk volume is M - ans
    cout<<M-ans<<endl;
}
