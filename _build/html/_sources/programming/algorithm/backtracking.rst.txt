Backtracking 
============

Knight's Tour
-------------

:Time Complexity: :math:`O(M^{N^2})`
:Auxiliary Space: :math:`O(N^2)`

.. code-block:: c++
    
    #include <bits/stdc++.h>
    using namespace std;

    bool util(int x, int y, int c, vector<vector<int>>& sol, int N, int M, vector<int>& dx, vector<int>& dy) {
        if (x<0 || x>N-1 || y<0 || y>N-1 || sol[x][y]!=0)
            return false;
        sol[x][y] = c;
        if (c==N*N)
            return true;
        for (int i=0; i<M; ++i) {
            if (util(x+dx[i], y+dy[i], c+1, sol, N, M, dx, dy))
                return true;
        }
        sol[x][y] = 0;
        return false;
    }

    void solve(int N, int M, vector<int>& dx, vector<int>& dy) {
        vector<vector<int>> sol(N, vector<int>(N));
        if (util(0, 0, 1, sol, N, M, dx, dy)) {
            for (int i=0; i<N; ++i) {
                for (int j=0; j<N; ++j)
                    cout << setw(3) << sol[i][j];
                cout << '\n';
            }
        }
        else
            cout << -1 << '\n';
    }

    int main () {
        int N{8};
        int M{8};
        vector<int> dx{2, 1, -1, -2, -2, -1, 1, 2};
        vector<int> dy{1, 2, 2, 1, -1, -2, -2, -1};
        solve(N, M, dx, dy);
        return 0;
    }

Rat in a Maze
-------------

:Time Complexity: :math:`O(M^{N^2})`
:Auxiliary Space: :math:`O(N^2)`

.. code-block:: c++
    
    #include <bits/stdc++.h>
    using namespace std;

    bool util(int x, int y, vector<vector<int>>& sol, int N, vector<vector<int>>& maz, int M, vector<int>& dx, vector<int>& dy) {
        if (x<0 || x>N-1 || y<0 || y>N-1 || sol[x][y]!=0 || maz[x][y]==0)
            return false;
        sol[x][y] = 1;
        if (x==N-1 && y==N-1)
            return true;
        for (int i=0; i<M; ++i) {
            if (util(x+dx[i], y+dy[i], sol, N, maz, M, dx, dy))
                return true;
        }
        sol[x][y] = 0;
        return false;
    }

    void solve(int N, vector<vector<int>>& maz, int M, vector<int>& dx, vector<int>& dy) {
        vector<vector<int>> sol(N, vector<int>(N));
        if (util(0, 0, sol, N, maz, M, dx, dy)) {
            for (int i=0; i<N; ++i) {
                for (int j=0; j<N; ++j)
                    cout << setw(2) << sol[i][j];
                cout << '\n';
            }
        }
        else
            cout << -1 << '\n';
    }

    int main() {
        int N{8};
        vector<vector<int>> maz{
            {1, 0, 1, 1, 1, 1, 1, 1},
            {1, 0, 1, 0, 0, 0, 1, 0},
            {1, 0, 1, 1, 1, 1, 1, 1},
            {1, 0, 1, 0, 1, 0, 0, 0},
            {1, 0, 1, 0, 1, 1, 1, 0},
            {1, 0, 1, 1, 0, 1, 1, 0},
            {1, 0, 0, 1, 0, 0, 1, 1},
            {1, 1, 1, 1, 1, 1, 0, 1}
        };
        int M{4};
        vector<int> dx{1, -1, 0, 0};
        vector<int> dy{0, 0, 1, -1};
        solve(N, maz, M, dx, dy);
    }