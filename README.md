# C++ STL Quick Help
It contains C++ STLs usage and quick help with easy to understand comments and examples (copy+paste to use)
I will be using "int" for ease. You can replace it with any data structure

### :memo:Different ways of using priority_queue (i.e. heap) :mount_fuji:

- Default declarations
```c++
priority_queue<int> pq;                            //creates max-heap
priority_queue<int, vector<int>> pq;               //creates max-heap
```
<br>

- writing comparator function for priority_queue
```c++
1. Using in-built comparator provided by C++ : 

priority_queue<int, vector<int>, greater<int>> pq;  //creates min-heap
```
```c++
2. Using user defined comparator as a structure

struct comp {
    bool operator()(int &a, int &b) {
        return a<b; //max-heap
        return a>b; //min-heap
    }
};

priority_queue<int, vector<int>, comp> pq;  //usage
```

```c++
3. Using user defined comparator as a function

static bool comp(int &a, int &b) {
    return a<b; //max-heap
    return a>b; //min-heap
}

priority_queue<int, vector<int>, function<bool(int&, int&)> > pq(comp);   //usage
```
```c++
4. Using lambda function

auto comp = [](int &a, int &b) {
    return a<b; //max-heap
    return a>b; //min-heap 
};

priority_queue<int, vector<int>, decltype(comp) > pq(comp);   //usage

NOTE :
You can receive parameters inside [] as well i.e. auto comp = [some_parameters]
Ex : You want to access a map inside this lambda function
unordered_map<int, int> mp;

auto comp = [&mp](int &a, int &b) {
    return mp[a] < mp[b]; //etc.
};

```
