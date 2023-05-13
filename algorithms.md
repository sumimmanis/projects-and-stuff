## DFS
$O(V + E)$

```cpp
std::vector<std::vector<int>> adjacent_vert_list_;

void dfs(std::vector<int> &used, int curr=0, int colour=1) {
    used[curr] = colour;
    for (auto next : adjacent_vert_list_[curr]) {
        if (!used[next]) {
            dfs(used, next, colour);
        }
    }
}
```

#### Классификация ребер
```cpp
void dfs(std::vector<int> &used, int curr = 0) {
    used[curr] = 1;
    for (auto next: adjacent_vert_list_[curr]) {
        if (!used[next]) {
            dfs(used, next, 1);
        } else if (used[curr] == 1) {
            //обратное
        } else {
            //прямое или перекрестное если отрезки времени не пересекаются
        }
    }
    used[curr] = 2;
}
```

#### Топологическа сортировка (рассеянные профессор)
> ациклический граф
```cpp
int dim_;
std::vector<std::vector<int>> adjacent_vert_list_;

std::list<int> foo() {
    std::list<int> vct;
    std::vector<int> used(dim_, 0);
    for (int i = 0; i < dim_; ++i) {
        if (!used[i]) {
            dfs(i, vct, used);
        }
    }
    return vct;
}

void dfs(int curr, std::list<int> &vct, std::vector<int> &used) {
    used[curr] = 1;
    for (auto next : adjacent_vert_list_[curr]) {
        if (!used[next]) {
            dfs(next, vct, used);
        }
    }
    vct.push_front(curr);
}
```

#### Поиск мостов
```cpp
int time_ = 0;
std::vector<std::vector<int>> adjacent_vert_list_;

void dfs(int curr, std::vector<std::pair<int, int>> &vct, std::vector<int> &tin, std::vector<int> &up, std::vector<int> &used, int prev) {
        used[curr] = 1;
        ++time_;
        tin[curr] = time_;
        min[curr] = time_;
        for (auto next : adjacent_vert_list_[curr]) {
            if (next == prev) {
                continue;
            }
            if (!used[next]) {
                dfs(next, vct, tin, up, used, curr);
                up[curr] = std::min(up[next], up[curr]);
                if (up[next] > tin[curr]) {
                    vct.push_back(std::make_pair(curr, next));
                }
            } else {
                up[curr] = std::min(up[curr], tin[next]);
            }
        }
    }
```

#### Точки сочленения
Поиск мостов но знак $<$ в проверке и отдельно проверить корень.


#### Сильная связность
Проверка на сильную связность:  
&nbsp;&nbsp;&nbsp;&nbsp; DFS из $u$, развернуть ребра и снова DFS из $u$.

Компоненты сильной связности и конденсация:  
&nbsp;&nbsp;&nbsp;&nbsp;  Разворот графа и DFS в порядке обратном топологической сортировке изначального графа.

## BFS
$O(V + E)$  
Можно реализовать итерируясь по $n + 1$ массивам, где $n$ - максимальная длина ребра.

#### Лабиринт

```cpp
#define wall (-1)

std::vector<std::vector<int>> matrix_;
int x1_, x2_, y1_, y2_;

int bfs() {
    std::queue<std::pair<int, int>> q;
    if (matrix_[x1_][y1_] != wall) {
        q.emplace(x1_, y1_);
    }

    if (x1_ == x2_ && y1_ == y2_) {
        return 0;
    }
    std::vector<std::pair<int, int>> offsets{{-1, 0},
                                             {1,  0},
                                             {0,  -1},
                                             {0,  1}};
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
std::vector<std::vector<int>> adjacent_vert_list_;
std::vector<int> belongs_;    //1 или 2, если ребро между 1 и 2 то длина ребра один

void bfs(int v = 0) {
    std::deque<int> q;
    std::vector<int> dist(dim_, INT32_MAX);
    std::vector<int> used(dim_, 0);
    std::vector<int> prev(dim_, -1);
    used[v] = 1;
    dist[v] = 0;
    q.push_back(v);
    while (!q.empty()) {
        auto curr = q.front();
        q.pop_front();
        for (auto next: adjacent_vert_list_[curr]) {
            if (belongs_[next] + belongs_[curr] == 3) {
                if (dist[next] > dist[curr] + 1) {
                    dist[next] = dist[curr] + 1;
                    prev[next] = curr;
                }

                if (!used[next]) {
                    q.push_back(next);
                    used[next] = 1;
                }
            } else {
                if (dist[next] > dist[curr]) {
                    dist[next] = dist[curr];
                    prev[next] = curr;
                }
                if (!used[next]) {
                    q.push_front(next);
                    used[next] = 1;
                }
            }
        }
    }
}
```

#### Вершины на кратчайших путях

Вершина $x$ лежит на кратчайшем пути из $u$ в $v$ если $dist(xu) + dist(xv) == dist(uv)$. Запускаем два BFS и перебираем все вершины.

#### Большие графы

В большх графах лучше поочередно ходить от обеих вершин и проверять пересечеие множеств достижимых.
