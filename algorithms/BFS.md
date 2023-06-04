## BFS
$O(V + E)$  
Можно реализовать итерируясь по $n + 1$ массивам, где $n$ - максимальная длина ребра.

#### Кратчайший путь в 0-1 графе 

```cpp
void bfs(int v = 0) {
    std::deque<int> q;
    std::vector<int> dist(dim_, INT32_MAX), used(dim_, 0), prev(dim_, -1);
    
    used[v] = 1;
    dist[v] = 0;
    q.push_back(v);
    while (!q.empty()) {
        auto curr = q.front();
        q.pop_front();
        for (auto next: adjacent_vert_list_[curr]) {
            //если 0 то push_front, если 1 то push_back
        }
    }
}
```

#### Большие графы

В большх графах лучше поочередно ходить от обеих вершин и проверять пересечеие множеств достижимых.

------------------------------------------------------------
[выход из лабиринта](Contests/Maze.md)
