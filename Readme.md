# BFS Algrorithm

```c
#include "harness.h"
#include <queue>
#include <set>
#include <stack>

vector<Cell> solve(vector<vector<char>>& grid, Cell start, Cell goal){
  vector<Cell> visited;

  queue<Cell> q;
  set<Cell> seen;

  q.push(start);
  seen.insert(start);
  visited.push_back(start);

  int dr[] = {-1, 1, 0, 0};
  int dc[] = { 0, 0,-1, 1};

  while(!q.empty()){
    Cell cur = q.front();
    q.pop();

    if(cur == goal) break;

    for(int k = 0; k < 4; k++){
      int nr = cur.first + dr[k];
      int nc = cur.second + dc[k];

      if(!inBounds(nr, nc)) continue;
      if(isWall(grid[nr][nc])) continue;

      Cell nxt = {nr, nc};

      if(seen.count(nxt)) continue;

      seen.insert(nxt);
      came_from[nxt] = cur;
      q.push(nxt);
      visited.push_back(nxt);
    }
  }
  return visited;
}
```
# DFS Algorithm

```c
#include "harness.h"
#include <queue>
#include <set>
#include <stack>

vector<Cell> solve(vector<vector<char>>& grid, Cell start, Cell goal){
  vector<Cell> visited;
  stack<Cell> stk;        // DFS uses stack instead of queue
  set<Cell> seen;

  stk.push(start);
  seen.insert(start);

  int dr[] = {-1, 0, 0, 1};
  int dc[] = { 0,-1, 1, 0};

  while (!stk.empty()) {

    Cell cur = stk.top();   // DFS takes from TOP (most recently added)
      stk.pop();

      visited.push_back(cur); // mark as explored when popped, not when pushed

      if (cur == goal) break;

      for (int k = 0; k < 4; k++) {
        int nr = cur.first  + dr[k];
        int nc = cur.second + dc[k];

        if (!inBounds(nr, nc)) continue;
        if (isWall(grid[nr][nc])) continue;

            Cell nxt = {nr, nc};
            if (seen.count(nxt)) continue;

            seen.insert(nxt);
            came_from[nxt] = cur;   // record parent for path reconstruction
            stk.push(nxt);
        }
    }
  return visited;
}
```

# Dijkstra's ALgorithm

```c
#include "harness.h"
#include <queue>
#include <set>
#include <stack>

vector<Cell> solve(vector<vector<char>>& grid, Cell start, Cell goal){
  vector<Cell> visited;
  priority_queue<pair<int,Cell>, vector<pair<int,Cell>>, greater<pair<int,Cell>>> pq;
  map<Cell, int> dist;

  dist[start] = 0;
  pq.push({0, start});

  int dr[] = {-1, 1, 0, 0};
  int dc[] = { 0, 0,-1, 1};

  while (!pq.empty()) {

    auto top = pq.top();
    pq.pop();

    int curCost = top.first;
    Cell cur = top.second;

    if (curCost > dist[cur]) continue;
      visited.push_back(cur);

      if (cur == goal) break;
      for (int k = 0; k < 4; k++) {
        int nr = cur.first  + dr[k];
        int nc = cur.second + dc[k];

        if (!inBounds(nr, nc)) continue;
        if (isWall(grid[nr][nc])) continue;

        Cell nxt = {nr, nc};
        int newCost = curCost + cellCost(grid[nr][nc]);

        if (!dist.count(nxt) || newCost < dist[nxt]) {
          dist[nxt] = newCost;
          came_from[nxt] = cur;
          pq.push({newCost, nxt});
        }
      }
    }

  return visited;
}
```
