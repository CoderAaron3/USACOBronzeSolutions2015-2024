# Problem 1: Promotion Counting
**Problem Statement: [https://usaco.org/index.php?page=viewproblem2&cpid=591](url)**

**Idea:**

The number of participants promoted from each tier equals the change in the number of participants in the next tier after the contest.

- Promotions from Gold → Platinum = difference between platinum participants after and before the contest.

- Promotions from Silver → Gold = change in gold participants + those who moved from gold to platinum (since those promotions free up gold spots).

- Promotions from Bronze → Silver = change in silver participants + all promotions from higher tiers (since those must ultimately trace back to bronze promotions).

**Code:**
```c++
#include <bits/stdc++.h>
using namespace std;

// Initialize variables
int bronze1, bronze2;
int silver1, silver2;
int gold1, gold2;
int plat1, plat2;

int main() {
    freopen("promote.in", "r", stdin);
    freopen("promote.out", "w", stdout);

    // Input the number of participants before and after for each tier
    cin >> bronze1 >> bronze2;
    cin >> silver1 >> silver2;
    cin >> gold1 >> gold2;
    cin >> plat1 >> plat2;

    // Calculate promotions for each tier
    int goldToPlat = plat2 - plat1; // Gold → Platinum
    int silverToGold = (gold2 - gold1) + goldToPlat; // Silver → Gold
    int bronzeToSilver = (silver2 - silver1) + silverToGold; // Bronze → Silver

    // Output results
    cout << bronzeToSilver << endl;
    cout << silverToGold << endl;
    cout << goldToPlat << endl;
}
```
