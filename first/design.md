#Design
##146. LRU Cache
###题目：
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: **get** and **set**.

**get(key)** - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.

**set(key, value)** - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.
###思路：
这个题目的重点在于在

###代码：

```
class LRUCache{
public:
    LRUCache(int capacity) {
        cap = capacity;
    }
    
    int get(int key) {
        auto it = hash.find(key);
        if (it == hash.end()) return -1;
        LRU.erase(it->second.second);
        LRU.push_front(key);
        it->second.second = LRU.begin();
        return it->second.first;
    }
    
    void set(int key, int value) {
        auto it = hash.find(key);
        if (it != hash.end()) {
            LRU.erase(it->second.second);
            LRU.push_front(key);
            it->second.second = LRU.begin();
        }
        else {
            if (LRU.size() == cap) {
                hash.erase(LRU.back());
                LRU.pop_back();
            }
            LRU.push_front(key);
            
        }
        hash[key] = {value, LRU.begin()};
    }
private:
    unordered_map<int, pair<int, list<int>::iterator>> hash;
    list<int> LRU;
    int cap;
};
```
