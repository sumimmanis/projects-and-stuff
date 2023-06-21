## Binary search
```cpp
while (l < r) {    //важно что тут сразу все плохо, а потом хорошо
        int mid = (l + r) / 2;
        if (good(mid)) r = mid;
        else l = mid + 1;
    }
```

