# Problem 3: Load Balancing

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=617](url)**

Idea:

- Trying every single coordinate is a classic Time Limit Exceeded (TLE) trap since the coordinate bounds ($B$) are huge.The key realization is that moving a fence (a vertical or horizontal line) only changes which quadrant a cow belongs to when the fence crosses that cow's location.
- Coordinate Reduction: Instead of checking every coordinate up to $B$, we only need to test fence locations right next to a cow's coordinate (e.g., $X_{cow} + 1$). This is because any split between $X_{cow}$ and $X_{next\_cow}$ is redundant.
- Brute Force Cow-Based Splits: We iterate over every cow's $X$ coordinate and every cow's $Y$ coordinate, setting up a potential fence intersection.$O(N^2)$
- Sweep: This shrinks the problem from a massive $O(B^2)$ down to a tiny $O(N^2)$ search, which is fast enough!
- Optimal Balance: For each intersection, we find the quadrant with the most cows (the worst quadrant) and then track the minimum of those worst-case values as our final answer.


Code:

```c++
#include <bits/stdc++.h>
using namespace std;
int N, B;
vector<pair<int, int>>cows;
int main(){
    // Standard I/O setup.
    freopen("balancing.in", "r", stdin);
    freopen("balancing.out", "w", stdout);
    cin>>N>>B;
    cows.assign(N, {0, 0});
    for(int i = 0;i<N;i++){
        cin>>cows[i].first>>cows[i].second; // Read cow coordinates.
    }
    int ans = N; // Initialize minimum max-quadrant size to N.
    // Outer loops: Iterate over all cow coordinates for the fence lines. O(N^2).
    for(int i = 0;i<N;i++){
        for(int j = 0;j<N;j++){
            // Set fence coordinates 1 unit past a cow's location.
            int xcoord = cows[i].first + 1;
            int ycoord = cows[j].second + 1;
            int quad1 = 0, quad2 = 0, quad3 = 0, quad4 = 0;
            // Innermost loop: Iterate over all N cows (k) to check quadrant. O(N).
            for(int k = 0;k<N;k++){ 
                // Check quadrant based on fence position (xcoord, ycoord).
                if(cows[k].first < xcoord && cows[k].second < ycoord){
                    quad1++; // Bottom-Left
                }
                else if(cows[k].first < xcoord && cows[k].second > ycoord){
                    quad2++; // Top-Left
                }
                else if(cows[k].first > xcoord && cows[k].second > ycoord){
                    quad3++; // Top-Right
                }
                else{
                    quad4++; // Bottom-Right
                }
            }
            // Find the worst-case (maximum) quadrant for this fence.
            int curr = max(quad1, max(quad2, max(quad3, quad4)));
            // Update the overall minimum worst-case.
            ans = min(ans, curr);
        }
    }
    cout<<ans<<endl;
}
