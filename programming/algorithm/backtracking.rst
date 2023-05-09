Backtracking 
============

Knight's Tour
-------------

:Time Complexity: :math:`O(M^{N^2})`
:Auxiliary Space: :math:`O(N^2)`

.. code-block:: c++
    
    #include <bits/stdc++.h>
    using namespace std;

    bool safe(int x, int y, vector<vector<int>>& sol, int N) {
        return (x>=0 && x<=N-1 && y>=0 && y<=N-1 && sol[x][y]==0);
    }

    bool util(int x, int y, int c, vector<vector<int>>& sol, int N, int M, vector<int>& dx, vector<int>& dy) {
        if (c==N*N)
            return true;
        for (int i=0; i<N; ++i) {
            if (safe(x+dx[i], y+dy[i], sol, N)) {
                sol[x+dx[i]][y+dy[i]] = c+1;
                if (util(x+dx[i], y+dy[i], c+1, sol, N, M, dx, dy))
                    return true;
                sol[x+dx[i]][y+dy[i]] = 0;
            }
        }
        return false;
    }

    void solve(int N, int M, vector<int>& dx, vector<int>& dy) {
        vector<vector<int>> sol(N, vector<int>(N));
        sol[0][0] = 1;
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

    int main() {
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

    bool safe(int x, int y, vector<vector<int>>& sol, int N, vector<vector<int>>& maz) {
        return (x>=0 && x<=N-1 && y>=0 && y<=N-1 && sol[x][y]==0 && maz[x][y]==1);
    }

    bool util(int x, int y, vector<vector<int>>& sol, int N, vector<vector<int>>& maz, int M, vector<int>& dx, vector<int>& dy) {
        if (x==N-1 && y==N-1)
            return true;
        for (int i=0; i<M; ++i) {
            if (safe(x+dx[i], y+dy[i], sol, N, maz)) {
                sol[x+dx[i]][y+dy[i]] = 1;
                if (util(x+dx[i], y+dy[i], sol, N, maz, M, dx, dy))
                    return true;
                sol[x+dx[i]][y+dy[i]] = 0;
            }
        }
        return false;
    }

    void solve(int N, vector<vector<int>>& maz, int M, vector<int>& dx, vector<int>& dy) {
        vector<vector<int>> sol(N, vector<int>(N));
        sol[0][0] = 1;
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

N Queen
--------

:Time Complexity: :math:`O(N!)`
:Auxiliary Space: :math:`O(N^2)`

.. code-block:: c++

    #include <bits/stdc++.h>
    using namespace std;

    bool safe(int r, int c, vector<vector<int>>& sol, int N) {
        int x{}, y{};
        for (x=r-1, y=c; x>=0; --x) {
            if (sol[x][y]!=0)
                return false;
        }
        for (x=r-1, y=c-1; x>=0 && y>=0; x--, y--) {
            if (sol[x][y]!=0)
                return false;
        }
        for (x=r-1, y=c+1; x>=0 && y<=N-1; x--, y++) {
            if (sol[x][y]!=0)
                return false;
        }
        return true;
    }

    bool util(int r, vector<vector<int>>& sol, int N) {
        if (r==N)
            return true;
        for (int c=0; c<N; ++c) {
            if (safe(r, c, sol, N)) {
                sol[r][c] = 1;
                if (util(r+1, sol, N))
                    return true;
                sol[r][c] = 0;
            }
        }
        return false;
    }

    void solve(int N) {
        vector<vector<int>> sol(N, vector<int>(N));
        if (util(0, sol, N)) {
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
        solve(N);
        return 0;
    }

Subset Sum
----------

:Time Complexity: :math:`O(2^N)`
:Auxiliary Space: :math:`O(N)`

.. code-block:: c++

    #include <bits/stdc++.h>
    using namespace std;

    void util(int x, int s, vector<int>& sol, int N, vector<int>& Set, int S) {
        if (s==S) {
            for (int i=0; i<N; ++i) {
                if (sol[i])
                    cout << Set[i] << ' ';
            }
            cout << '\n';
        }
        for (int i=x; i<N; ++i) {
            if (s+Set[i]>S)
                break;
            sol[i] = 1;
            util(i+1, s+Set[i], sol, N, Set, S);
            sol[i] = 0;
        }
    }

    void solve(int N, vector<int>& Set, int S) {
        sort(Set.begin(), Set.end());
        vector<int> sol(N);
        util(0, 0, sol, N, Set, S);
    }

    int main() {
        int N{8};
        vector<int> Set{15, 22, 14, 26, 32, 9, 16, 8};
        int S{53};
        solve(N, Set, S);
        return 0;
    }

M Coloring
----------

:Time Complexity: :math:`O(M^N)`
:Auxiliary Space: :math:`O(N)`

.. code-block:: c++

    #include <bits/stdc++.h>
    using namespace std;

    bool safe(int x, int c, vector<int>& sol, vector<vector<int>>& adj) {
        for (int y : adj[x]) {
            if (sol[y]==c)
                return false;
        }
        return true;
    }

    bool util(int x, vector<int>& sol, int N, vector<vector<int>>& adj, int M) {
        if (x==N)
            return true;
        for (int c=1; c<=M; ++c) {
            if (safe(x, c, sol, adj)) {
                sol[x] = c;
                if (util(x+1, sol, N, adj, M))
                    return true;
                sol[x] = 0;
            }
        }
        return false;
    }

    void solve(int N, vector<vector<int>>& adj, int M) {
        vector<int> sol(N);
        if (util(0, sol, N, adj, M)) {
            for (int i=0; i<N; ++i)
                cout << sol[i] << ' ';
            cout << '\n';
        }
        else
            cout << -1 << '\n';
    }

    int main() {
        int N{10};
        vector<vector<int>> adj{
            {1, 4, 5},
            {0, 2, 6},
            {1, 3, 7},
            {2, 4, 8},
            {0, 3, 9},
            {0, 7, 8},
            {1, 8, 9},
            {2, 5, 9},
            {3, 5, 6},
            {4, 6, 7}
        };
        int M{3};
        solve(N, adj, M);
        return 0;
    }

Hamiltonian Cycle
-----------------

:Time Complexity: :math:`O(N!)`
:Auxiliary Space: :math:`O(N)`

.. code-block:: c++

    #include <bits/stdc++.h>
    using namespace std;

    bool safe(int x, int y, vector<int>& sol, int N, vector<vector<bool>>& adj) {
        if (!adj[x][y])
            return false;
        for (int i=0; i<N; ++i) {
            if (sol[i]==y)
                return false;
        }
        return true;
    }

    bool util(int x, int c, vector<int>& sol, int N, vector<vector<bool>>& adj) {
        if (c==N)
            return true;
        for (int y=0; y<N; ++y) {
            if (safe(x, y, sol, N, adj)) {
                sol[c] = y;
                if (util(y, c+1, sol, N, adj))
                    return true;
                sol[c] = 0;
            }
        }
        return false;
    }

    void solve(int N, vector<vector<bool>>& adj) {
        vector<int> sol(N, -1);
        sol[0] = 0;
        if (util(0, 1, sol, N, adj)) {
            for (int i=0; i<N; ++i)
                cout << sol[i] << ' ';
            cout << sol[0] << '\n';
        }
        else
            cout << -1 << '\n';
    }

    int main() {
        int N{6};
        vector<vector<bool>> adj{
            {0, 1, 0, 0, 1, 1},
            {1, 0, 1, 1, 1, 0},
            {0, 1, 0, 1, 0, 0},
            {0, 1, 1, 0, 1, 1},
            {1, 1, 0, 1, 0, 1},
            {1, 0, 0, 1, 1, 0}
        };
        solve(N, adj);
        return 0;
    }