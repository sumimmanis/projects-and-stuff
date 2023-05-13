## BFS
$O(V + E)$  
Можно реализовать итерируясь по $n + 1$ массивам, где $n$ - максимальная длина ребра.

#### Лабиринт

```cpp
int bfs() {
    std::queue<std::pair<int, int>> q;
    

    q.emplace(x1_, y1_);


    if (x1_ == x2_ && y1_ == y2_) {
        return 0;
    }
    
    std::vector<std::pair<int, int>> offsets{{-1, 0}, {1,  0}, {0,  -1}, {0,  1}};
    
    while (!q.empty()) {
        auto [i, j] = q.front();
        q.pop();
        for (auto [di, dj]: offsets) {
            if (i + di < 0 || i + di >= matrix_.size() || j + dj < 0 || j + dj >= matrix_[i].size()) {
                continue;
            }
            if (matrix_[i + di][j + dj] == 0) {
                matrix_[i + di][j + dj] = matrix_[i][j] + 1;
                q.emplace(i + di, j + dj);
            }

            if(i + di == x2_ && j + dj == y2_) {
                return matrix_[i + di][j + dj];
            }
        }
    }
    return -1;
}
```

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

#### Вершины на кратчайших путях

Вершина $x$ лежит на кратчайшем пути из $u$ в $v$ если $d(xu) + d(xv) == d(uv)$. Запускаем два BFS и перебираем все вершины.

#### Большие графы

В большх графах лучше поочередно ходить от обеих вершин и проверять пересечеие множеств достижимых.
