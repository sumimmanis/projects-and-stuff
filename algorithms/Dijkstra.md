## Dijkstra

```cpp
std::vector<int> dijkstra() {
    std::set<std::pair<int, int>> set;
    std::vector<int> prev(dim_, -1);
    set.insert({0, x_});
    dist[x_] = 0;
    while(!set.empty()) {
        auto [r, curr] = *set.begin();
        set.erase(set.begin());
        for (auto [next, dst] : adjacent_vert_list_[curr]) {
            if (dist[next] > dist[curr] + dst) {
                if (dist[next] != INT32_MAX) {
                    set.erase(set.find({dist[next], next}));
                }
                dist[next] = dist[curr] + dst;
                prev[next] = curr;
                set.insert({dist[next], next});
            }
        }
    }
  return prev;
}
```
