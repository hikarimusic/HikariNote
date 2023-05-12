Greedy Algorithm
================

Activity Selection
------------------

:Time Complexity: :math:`O(N\log N)`
:Auxiliary Space: :math:`O(N)`

.. code-block:: c++

    #include <bits/stdc++.h>
    using namespace std;

    void solve(int N, vector<pair<int,int>> act) {
        vector<int> sol(N);
        sort(act.begin(), act.end(), [](pair<int,int> a, pair<int,int> b){
            return a.second < b.second;
        });
        int tf{};
        for (int i=0; i<N; ++i) {
            if (act[i].first<tf)
                continue;
            sol[i] = 1;
            tf = act[i].second;
        }
        for (int i=0; i<N; ++i) {
            if (sol[i])
                cout << act[i].first << ' ' << act[i].second << '\n';
        }
    }

    int main() {
        int N{6};
        vector<pair<int,int>> act{{ 5, 9 }, { 1, 2 }, { 3, 4 },{ 0, 6 }, { 5, 7 }, { 8, 9 }};
        solve(N, act);
        return 0;
    }