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
int N; // Number of rooms
vector<int>rooms; // Cows in each room (0-indexed)
int main(){
    // Standard I/O.
    freopen("cbarn.in", "r", stdin);
    freopen("cbarn.out", "w", stdout);
    cin>>N;
    rooms.assign(N, 0);
    for(int i = 0;i<N;i++){
        cin>>rooms[i]; // Read cow counts
    }
    int ans = INT_MAX; // Store the best (minimum) total distance
    // Outer loop: Try every room 'i' as the starting exit door (O(N))
    for(int i = 0;i<N;i++){
        int curr = 0; // Total distance for door 'i'
        // Inner loop: Iterate over all N rooms starting after the door, simulating the circle
        // The loop runs from i+1 up to N+i
        for(int j = i+1;j<N+i;j++){
            //j % N gives the true circular index (0 to N-1) of the room
            int currroom = rooms[j % N]; 
            // Calculate distance: (cows in room) * (steps from door i)
            // j - i gives the steps taken (distance)
            curr += currroom * (j - i); 
        }
        // Update the global minimum answer
        ans = min(ans, curr); 
    }
    cout<<ans<<endl; // Output minimum total distance
}
