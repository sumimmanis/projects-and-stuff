cpp```
void dfs(std::vector<std::vector<int>> &adjacent_vert_list_, std::vector<bool> &used, int curr) {
    used[curr] = true;
    for (auto i : adjacent_vert_list_[curr]) {
        if (!used[i]) {
            dfs(adjacent_vert_list_, used, i);
        }
    }
}
```
