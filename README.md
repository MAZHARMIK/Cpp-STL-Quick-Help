# C++ STL Quick Help
It contains C++ STLs usage and quick help with easy to understand comments and examples (copy+paste to use).  
I will be using "int, string etc" for ease and not complex entities like pairs, structs etc 😉. You can replace it with any data structure

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

### :memo: When and why to use std::move() :arrow_left:
```c++
/*
    To efficiently transfer the resources from source to target.
    By efficient, I mean no usage of extra space and time for creating copy.
*/
Examples :
    string source = "MIK";
    string target = "";
    target = std::move(source);
    cout << " source = " << source << endl;
    cout << "target = "  << target << endl;
    /*
        output :
        source = 
        target = "MIK"
    */
    
    vector<string> v;
    string str = "example";
    v.push_back(std::move(str));
    /*
    After this, str becomes empty i.e. ""
    And while moving str inside v, no extra copy of str was done implicitly.
    */

    vector<int> temp{1, 2, 3};
    vector<vector<int>> result;
    result.push_back(std::move(temp));
    /*
    This allows no copy of "temp" being created.
    It ensures that the contents of "temp"
    will be moved into the "result".  This is less
    expensive, also means temp will now be empty.
    */
```

### :memo: std::accumulate(begin_iterator, end_iterator, initial_sum) :heavy_plus_sign:
```c++
int sum = 0;
vector<int> nums{1, 3, 2, 5};
sum = accumulate(begin(nums), end(nums), 0);

cout << sum; //11

Benefit : You didn't have to write for loop to find the sum
```

### :memo: \*min_element(begin_iterator, end_iterator), \*max_element(begin_iterator, end_iterator), minmax_element(begin_iterator, end_iterator) :astonished:
```c++
vector<int> nums{1, 3, 2, 5};

int minimumValue = *min_element(begin(nums), end(nums)); //1
int maximumValue = *max_element(begin(nums), end(nums)); //5
                OR,
        auto itr  = minmax_element(begin(nums), end(nums));
int minimumValue  = *itr.first;  //remember, first is minimum  //1
int maximumValue  = *itr.second; //remember, second is maximum //5


Benefit : You didn't have to write for loop to find the max or min element
```

### :memo: \*upper_bound(), \*lower_bound in sorted vector, ordered set, ordered map :outbox_tray:
```c++

For vector:
vec.upper_bound(begin(vec), end(vec), 35); //returns iterator to first element "greater" than 35

vec.lower_bound(begin(vec), end(vec), 35); //returns iterator to first element "greater or equal" to 35

For set:
st.upper_bound(35); //returns iterator to first element "greater" than 35
st.lower_bound(35); //returns iterator to first element "greater" than 35

For map:
mp.upper_bound(35); //returns iterator to first element "greater" than 35
mp.lower_bound(35); //returns iterator to first element "greater" than 35

Benefit : You didn't have to write binary search (in case of vector),
JAVA's tree_map equivalent in C++ (in case of map or set)
There are amazing applications or problems that can be solved using the above concepts.
Example : My Calendar I (Leetcode - 729) - You can find it in my interview_ds_algo repository as well B-)
```
