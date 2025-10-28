# Problem 1: Diamond Collector

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=639](url)**

**Idea:**

Since N is small (up to 1,000), a brute-force approach is perfect. The key constraint is $D_{max} - D_{min} \le K$. 

- Sort First: We must first sort all the diamonds by size. This lets us use a simple sweep since the smallest diamond will always be the starting point, $D_{min}$, and the largest will be $D_{max}$.
- Brute Force Start: We iterate through every sorted diamond, treating it as the guaranteed smallest diamond ($D_{min}$) in the collection
- Find the Endpoint: For each $D_{min}$, we scan forward to find the furthest diamond that still satisfies the $D_{max} \le D_{min} + K$ rule.
- Maximize Count: We simply count how many diamonds fit this window and track the absolute maximum count found.

**Code:**

```c++
#include <bits/stdc++.h>
using namespace std;

int N, K; // N: number of diamonds, K: max size difference allowed
vector<int>diamonds;

int main(){
    // Setup file I/O.
    freopen("diamond.in", "r", stdin);
    freopen("diamond.out", "w", stdout);

    // Read input N and K.
    cin>>N>>K;
    diamonds.assign(N, 0);
    for(int i = 0;i<N;i++){
        cin>>diamonds[i]; // Read all diamond sizes
    }
    //Sorting makes the linear sweep possible O(N log N)
    sort(diamonds.begin(), diamonds.end()); 
    int ans = 0; // The final max number of diamonds collected
    // Outer loop O(N): Iterate, assuming diamonds[i] is the smallest (D_min).
    for(int i = 0;i<N;i++){
        int curr = 0; // Count (num of diamonds) for the current collection
        // Inner loop O(N): Scan forward to find the D_max endpoint
        for(int j = i;j<N;j++){
            // Check the constraint: D_max (diamonds[j]) - D_min (diamonds[i]) > K
            if(diamonds[j] - diamonds[i] > K){
                break; // Found the limit, stop scanning for this start diamond
            }
            curr++; // This diamond fits
        }
        ans = max(ans, curr); // Update the best collection size found so far
    }
    cout<<ans<<endl; // Print the final result
}
