# Problem 2: Circular Barn

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=616](url)**

**Idea:**

This problem is about finding the optimal exit location to minimize the total walking distance for all the cows. Since the barn is circular and $N$ (the number of rooms) is small, we can use a direct O(N^2) simulation.

- Brute Force the Door: We try every single room as the potential exit door.
- Calculate Total Distance: For each chosen door, we iterate through every other room clockwise.
- Distance Calculation: The distance a cow walks is simply its distance from the door. We multiply the number of cows in a room by this distance and sum it up. Detail: If the room goes past N we will be out of bounds for the rooms array, so we should take the modulo of the current room with N. 
- Find Minimum: The minimum total distance across all possible door locations is our answer.

**Code:**

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
