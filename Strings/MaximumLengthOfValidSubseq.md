int n = nums.size();
        vector<int> r(n);
        for (int i = 0; i < n; ++i) {
            r[i] = nums[i] % k;
        }

        vector<vector<int>> l(k);
        for (int i = 0; i < n; ++i) {
            l[r[i]].push_back(i);
        }

        int m = 0;
        for (const auto& group : l) {
            m = max(m, (int)group.size());
        }

        for (int x = 0; x < k; ++x) {
            for (int y = x + 1; y < k; ++y) {
                const auto& a = l[x];
                const auto& b = l[y];
                int i = 0, j = 0;
                int sx = 0, sy = 0;

                while (i < a.size() || j < b.size()) {
                    if (j == b.size() || (i < a.size() && a[i] < b[j])) {
                        int nx = sy > 0 ? sy + 1 : 1;
                        sx = max(sx, nx);
                        ++i;
                    } else {
                        int ny = sx > 0 ? sx + 1 : 1;
                        sy = max(sy, ny);
                        ++j;
                    }
                }

                m = max(m, max(sx, sy));
            }
        }

        return m;
