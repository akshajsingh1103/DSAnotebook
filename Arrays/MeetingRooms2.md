```
class Solution {
public:

int mostBooked(int n, vector<vector<int>>& meetings) {
    sort(meetings.begin(), meetings.end());
    priority_queue<int, vector<int>, greater<int>> available;
    for (int i = 0; i < n; ++i) {
        available.push(i);
    }
    using P = pair<long long, int>;
    priority_queue<P, vector<P>, greater<P>> busy;
    vector<int> roomCount(n, 0);

    for (auto& meeting : meetings) {
        long long start = meeting[0], end = meeting[1];
        long long duration = end - start;
        while (!busy.empty() && busy.top().first <= start) {
            available.push(busy.top().second);
            busy.pop();
        }

        if (!available.empty()) {
            int room = available.top();
            available.pop();
            ++roomCount[room];
            busy.emplace(end, room);
        } else {
            auto [freeTime, room] = busy.top();
            busy.pop();
            ++roomCount[room];
            busy.emplace(freeTime + duration, room);
        }
    }
    int maxMeetings = 0, resultRoom = 0;
    for (int i = 0; i < n; ++i) {
        if (roomCount[i] > maxMeetings) {
            maxMeetings = roomCount[i];
            resultRoom = i;
        }
    }

    return resultRoom;
}

};
```
