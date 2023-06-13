## DFS
$O(V + E)$

```cpp
std::vector<std::vector<int>> adjacent_vert_list_;
std::vector<int> used;

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
> Ациклический граф
```cpp
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
void dfs(int curr, std::vector<std::pair<int, int>> &vct, std::vector<int> &tin, std::vector<int> &up, std::vector<int> &used, int prev) {
        used[curr] = 1;
        ++time_;
        tin[curr] = time_;
        up[curr] = time_;
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
DFS из $u$, развернуть ребра и снова DFS из $u$.

Компоненты сильной связности и конденсация:  
Разворот графа и DFS в порядке обратном топологической сортировке изначального графа.

