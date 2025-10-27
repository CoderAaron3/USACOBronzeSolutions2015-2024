# Problem 3: Mowing The Field

**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=593](url)**

Idea:

The idea for this problem is to directly stimulate the mowing of FJ. 
- Since FJ can travel 1000 steps maximum in one direction, we use a 2005x2005 grid to represent the field and safely contain all possible coordinates.
- Every time FJ steps on a square, that square gets time-stamped with the current step count (the total time elapsed).
- Find quickest revisit: If we step onto a square that already has a time stamp, immediately calculate the difference in time
Final Result: The smallest time difference we find is the answer. If the cow never steps on the same square twice, the answer is just $-1$. Easy peasy!

Code:

```c++
#include <bits/stdc++.h>
using namespace std;
int N;
// Stores movements: {Direction, Steps}
vector<pair<char, int>>sequence;
// Grid to track last visit time (steps). 2005x2005 for coordinate offset.
vector<vector<int>>lastmow;

int main(){
    // File I/O setup.
    freopen("mowing.in", "r", stdin);
    freopen("mowing.out", "w", stdout);
    
    cin>>N;
    sequence.assign(N, {'.', 0});
    // Init grid to -1 (unvisited).
    lastmow.assign(2005, vector<int>(2005, -1));
    for(int i = 0;i<N;i++){
        cin>>sequence[i].first>>sequence[i].second;
    }
    
    // Start at (1000, 1000). 'ans' tracks min revisit time.
    int currposx = 1000, currposy = 1000, ans = 1005, currtime = 0;

    // Iterate through N movement commands.
    for(int i = 0;i<N;i++){
        
        // --- WEST Movement ---
        if(sequence[i].first == 'W'){
            // Process each step.
            for(int j = 0;j<sequence[i].second;j++){
                // If spot is visited, calculate interval.
                if(lastmow[currposx][currposy] != -1){
                    // Update minimum answer.
                    ans = min(ans, (currtime - lastmow[currposx][currposy]));
                }
                // Record current time.
                lastmow[currposx][currposy] = currtime;
                
                currposx--; // Move West (X-).
                currtime++; // Time increases per step.
            }
        }
        
        // --- EAST Movement ---
        else if(sequence[i].first == 'E'){
            for(int j = 0;j<sequence[i].second;j++){
                if(lastmow[currposx][currposy] != -1){
                    ans = min(ans, (currtime - lastmow[currposx][currposy]));
                }
                lastmow[currposx][currposy] = currtime;
                
                currposx++; // Move East (X+).
                currtime++;
            }
        }
        
        // --- NORTH Movement ---
        else if(sequence[i].first == 'N'){
            for(int j = 0;j<sequence[i].second;j++){
                if(lastmow[currposx][currposy] != -1){
                    ans = min(ans, (currtime - lastmow[currposx][currposy]));
                }
                lastmow[currposx][currposy] = currtime;
                
                currposy++; // Move North (Y+).
                currtime++;
            }
        }
        
        // --- SOUTH Movement ---
        else{ // 'S'
            for(int j = 0;j<sequence[i].second;j++){
                if(lastmow[currposx][currposy] != -1){
                    ans = min(ans, (currtime - lastmow[currposx][currposy]));
                }
                lastmow[currposx][currposy] = currtime;
                
                currposy--; // Move South (Y-).
                currtime++;
            }
        }
    }
    
    // If 'ans' is unchanged, no revisit occurred.
    if(ans == 1005){
        cout<<"-1"<<endl;
        return 0;
    }
    
    // Output the smallest time interval.
    cout<<ans<<endl;
}
