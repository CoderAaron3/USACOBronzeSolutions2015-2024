# Problem 2: Angry Cows

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=592](url)**

**Idea:**

To find the largest group of haybales that can be destroyed, we must try starting the explosion at every single haybale.

- For each haybale We check how far the explosion spreads left and right, as the blast hits surrounding haybales.

- The key rule is that if a new haybale is hit and destroyed, it acts as the new anchor and increases the required blast radius by 1 for the next stage.

- The chain reaction stops when the increasing radius can no longer reach any new, unexploded haybales in that direction.

- The total span of destroyed haybales is calculated for each starting point, and we take the maximum span found as the answer.

**Code:**

```c++
#include <bits/stdc++.h>
using namespace std;

int N;
vector < int > haybales;

int main() {
    // Standard USACO file I/O setup.
    freopen("angry.in", "r", stdin);
    freopen("angry.out", "w", stdout);
    cin >> N;
    haybales.assign(N, 0);
    // Read haybale positions.
    for (int i = 0; i < N; i++) {
        cin >> haybales[i];
    }
    // Crucial step: Sort the positions for distance calculations.
    sort(haybales.begin(), haybales.end());
    // 'ans' stores the maximum number of exploded haybales.
    int ans = 0;
    // Iterate through every haybale as the initial explosion center (Brute Force).
    for (int i = 0; i < N; i++) {
        // 'lasthaybale': Index of the bale anchoring the current blast (starts at i).
        int lasthaybale = i;
        // 'currhaybale': Index being checked, moves to find the boundary.
        int currhaybale = i;
        // 'currradius': Current blast radius, starts at 1.
        int currradius = 1;
        // 'left' and 'right': Final indices of the destroyed range.
        int left = i;
        int right = i; 
        // --- Simulate explosion spreading LEFT ---
        while (currhaybale >= 0) {
            // Find the furthest haybale to the left still within 'currradius' of 'lasthaybale'.
            while (currhaybale >= 0 && haybales[lasthaybale] - haybales[currhaybale] <= currradius) {
                currhaybale--;
            }
            // Move back one step to the actual new boundary.
            currhaybale++;
            // Check if the explosion stopped (the boundary didn't move).
            if (lasthaybale == currhaybale) {
                left = currhaybale;
                break;
            }
            // Set the new anchor for the next blast.
            lasthaybale = currhaybale;
            // Increase the radius for the cascading effect (R -> R+1).
            currradius++;
        }
        // --- Simulate explosion spreading RIGHT (reset variables) ---
        // Reset anchor and search index to the center 'i'.
        lasthaybale = i;
        currhaybale = i;
        // Reset radius.
        currradius = 1;
        while (currhaybale < N) {
            // Find the furthest haybale to the right still within 'currradius' of 'lasthaybale'.
            while (currhaybale < N && haybales[currhaybale] - haybales[lasthaybale] <= currradius) {
                currhaybale++;
            }
            // Move back one step to the actual new boundary.
            currhaybale--;
            // Check if the explosion stopped.
            if (lasthaybale == currhaybale) {
                right = currhaybale;
                break;
            }
            // Set the new anchor for the next blast.
            lasthaybale = currhaybale;
            // Increase the radius for the cascading effect (R -> R+1).
            currradius++;
        }
        // Calculate total exploded count (span) and update overall maximum answer.
        ans = max(ans, right - left + 1);
    }
    cout << ans << endl;
}
