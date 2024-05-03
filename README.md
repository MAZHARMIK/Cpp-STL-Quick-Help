# C++ STL Quick Help
It contains C++ STLs usage and quick help with easy to understand comments and examples (copy+paste to use).
I learned these while solving different kinds of Leetcode Questions.  
I will be using "int, string etc" for ease and not complex entities like pairs, structs etc 😉. You can replace it with any data structure
If you are confused with the syntax or description, see the example. I am sure that will clear things BECAUSE I have specifically chosen  
:mag_right: "EASY + IMPORTANT + MOST USED" examples.
Last but not least, I have added Leetcode Qns also which can be easily solved using STLs

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
priority_queue< pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>> > pq; //min_heap of pairs
priority_queue< pair<int, int>, vector<pair<int, int>>, greater<> > pq;               //min_heap of pairs
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

### :memo: std::accumulate(begin_iterator, end_iterator, initial_sum, lambda) :heavy_plus_sign:
```c++
lambda : Binary operation taking an element of type <initial_sum> as first argument and an
            element in the range as second, and which returns a value that can be assigned to type T.

Example-1 : 

auto lambda = [&](int s, long n) {
    return s + n*n; //sums the square of numbers
    //You can call any other function inside as well
};

int sum = 0;
vector<int> nums{1, 3, 2, 5};
sum = accumulate(begin(nums), end(nums), 0, lambda);

cout << sum; //39

Example-2 : Handling 2-D matrix
//Summming all elements row by row
auto lambda = [&](int sum, vector<int> vec) {
    sum = sum + accumulate(begin(vec), end(vec), 0);
    return sum;
};

int result =  accumulate(matrix.begin(), matrix.end(), 0, lambda);


Beautiful example and usage :
Leetcode-1577 (My Approach - https://leetcode.com/problems/number-of-ways-where-square-of-number-is-equal-to-product-of-two-numbers/discuss/1305961/C%2B-(A-very-simple-Two-Sum-like-approach)

Leetcode-1572 (My Approach - https://leetcode.com/problems/matrix-diagonal-sum/discuss/3498479/Using-C%2B%2B-STL-%3A-accumulate)

```

### :memo: min_element(begin_iterator, end_iterator), max_element(begin_iterator, end_iterator), minmax_element(begin_iterator, end_iterator) :astonished:
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

### :memo: upper_bound(), lower_bound() in sorted vector, ordered set, ordered map :outbox_tray:
```c++

For vector:
vector<int> vec{10,20,30,30,20,10,10,20};
vector<int>::iterator up  = upper_bound(begin(vec), end(vec), 35);//returns iterator to first element "greater" than 35
vector<int>::iterator low = lower_bound(begin(vec), end(vec), 35);//returns iterator to first element "greater or equal" to 35
cout << "upper_bound at position " << (up - vec.begin()) << '\n';
cout << "lower_bound at position " << (low- vec.begin()) << '\n';

For set:
st.upper_bound(35); //returns iterator to first element "greater" than 35
st.lower_bound(35); //returns iterator to first element "greater or equal" than 35

For map:
mp.upper_bound(35); //returns iterator to first element "greater" than 35
mp.lower_bound(35); //returns iterator to first element "greater or equal" than 35

Benefit : You didn't have to write binary search (in case of vector),
JAVA's tree_map equivalent in C++ (in case of map or set)
There are amazing applications or problems that can be solved using the above concepts.
Example : My Calendar I (Leetcode - 729) -
         You can find it in my interview_ds_algo repository as well B-)
```

### :memo: std::rotate 🌀
```c++
vector<int> vec{1, 2, 3, 4};
int n = vec.size();
int k = 2;

rotate(vec.begin(), vec.begin()+k, vec.end());   //Left Rotate by K times

rotate(vec.begin(), vec.begin()+n-k, vec.end()); //Right Rotate by K times

```

### :memo: To check if some rotation of string s can become string t🌀
```c++

string s = "abcde";
string t = "cdeab";

cout << (s.length() == t.length() && (s+s).find(t) != string::npos) << endl;

```

### :memo: std::next_permutation ➡️
```c++
It gives the next lexicographically greater permutation.
So, if the container is already the greatest permutation (descending order), it returns nothing.

vector<int> vec{1, 2, 3, 4};
    
if(next_permutation(begin(vec), end(vec)))
    cout << "Next permutation available" << endl;

for(int &x : vec)
    cout << x << " ";
    
//Output : 1, 2, 4, 3

Also see : std::prev_permutation() - It gives just the previous lexicographically smaller permutation.
But I have never encountered any question where it's required till now. So you can skip it.
    Leetcode - 31  : Next Permutation
    etc.
```


### :memo: std::stringstream :fast_forward:
```c++
Usage:
1) Converting string to number
2) Count number of words in a string

Example-1
    string s = "12345";
    stringstream ss(s);
 
    // The object has the value 12345 and stream
    // it to the integer x
    int x = 0;
    ss >> x;
    cout << x;
    
Exmaple-2
    stringstream s(ss);
    string word; // to store individual words
  
    int count = 0;
    while (s >> word)
        count++;
    cout << count;
    NOTE: It will tokenize words on the basis of ' ' (space by default) characters
Example-3
    It can be used very well to extract numbers from string.
    string complex = "1+1i";
    stringstream ss(complex);
    char justToSkip;
    int real, imag;
    ss >> real >> justToSkip >> imag >> justToSkip;
    cout << real << ", " << imag; //output : 1, 1
    
    Other application on this STL :
    Leetcode - 151  : Reverse Words in a String
    Leetcode - 186  : Reverse Words in a String II
    Leetcode - 557  : Reverse Words in a String III
    Leetcode - 1108 : Defanging an IP Address
    Leetcode - 1816 : Truncate Sentence
    Leetcode - 884  : Uncommon Words from Two Sentences
    Leetcode - 537  : Complex Number Multiplication (Example-3 above)
    Leetcode - 165  : Compare Version Numbers
    etc.
```


### :memo: std::transform(InputIterator first1, InputIterator last1, OutputIterator result, UnaryOperation op) :robot:
```c++
Applies an operation sequentially to the elements of one (1) or
two (2) ranges and stores the result in the range that begins at result.
Uage :
1) Convert all letters of a string to lower case
2) Convert all letters of a string to upper case

Example : 
    string line = "Hello world, this is MIK";

    transform(begin(line), end(line), begin(line), ::tolower);

    cout << line << endl;

    transform(begin(line), end(line), begin(line), ::toupper);

    cout << line << endl;

```

### :memo: std::regex_replace :pager:
```c++
It converts a regular expression given by user to desired expression given by user.

Example : 
    Ex-1 - Remove all vowels from a string.
    string s = "mika";
    auto rgx = regex("[aeiouAEIOU]");
    cout << regex_replace(s, rgx, "");
    
    Ex-2 - Replace all '.' to "[.]"
    string s = "1.2.3.4";
    auto rgx = regex("\\.");
    regex_replace(s, rgx, "[.]");
    
    Note : You can write smart regex for achieving amazing replacements.
    Qns on Leetcode:
    Leetcode - 1108 : Defanging an IP Address
    Leetcode - 1119 : Remove Vowels from a String
    etc.
```

### :memo: std::count_if :1234:
```c++
counts the number of elements satisfying a given condition (given by comparator function or lambda)

Example : 
    vector<int> vec{1, 3, 2, 0, 5, 0};

    auto lambda = [&](const auto& i) {
        return i == 0;
    };

    cout << count_if(begin(vec), end(vec), lambda); //output : 2
    
    Note : You can write any kind of lambda/comparator functions for matching your required condition
    Qns on Leetcode:
    Leetcode - 1773 : Count Items Matching a Rule
    etc.
```

### :memo: std::copy_if :1234:
```c++
Copies the elements to a container
how copy_if function works : in this function you have to pass four parameters 
copy_if(begin iterator , end iterator , destination , condition)
			
    eg :    vector<int> from_vec = {1,2,3,4,5,6,7,8,9,10};
            vector<int> to_vec;
            //here i want to copy all the number from from_vec vector to to_vec vector which are divisible by 2 .
            
            copy_if(from_vec.begin(), from_vec.end(), back_inserter(to_vec),[](int n){return n%2==0;});
            
            for(auto it : to_vec) 
                cout<<it<<" ";
            o/p : 2 4 6 8 10
Example : 
    Note : You can write any kind of lambda/comparator functions for matching your required condition
    Qns on Leetcode:
    Leetcode - 1796 : Second Largest Digit in a String
    etc.
```


### :memo: Writing lambda for upper_bound or lower_bound for vector<pair<int, string>> :1234:
```c++
Example-1 : 
        //Let's say you want upper_bound for a variable timestamp, take it in a pair (because it's a vector of pair)
        pair<int, string> ref = make_pair(timestamp, "");
            
        auto lambda = [](const pair<int, string>& p1, const pair<int, string>& p2) {
            return p1.first < p2.first;
        };
        
        auto it = upper_bound(begin(my_vector), end(my_vector), ref, lambda);
	
Example-2 : 
        //Let's say you want to find upper_bound of a value in a non-increasing vector.
	vector<int> vec{1, 0, -1, -2}
	int idx = upper_bound(begin(vec), end(vec), 0, greater<int>()) - begin(vec);
	Output will be index of -1 (i.e. 2)
	
	Qns on Leetcode:
    	Leetcode - 981 : Time Based Key-Value Store
	Leetcode - 744 : Find Smallest Letter Greater Than Target
	Leetcode - 1351 : Count Negative Numbers in a Sorted Matrix
    
```


### :memo: Writing lambda for unordered_map to make life simple :1234:
```c++
Example : 
        //Let's say, you want to store different evaluate logic for different operator "+", "-", "*", "/"
	unordered_map<string, function<int (int, int) > > mp = {
            { "+" , [] (int a, int b) { return a + b; } },
            { "-" , [] (int a, int b) { return a - b; } },
            { "*" , [] (int a, int b) { return a * b; } },
            { "/" , [] (int a, int b) { return a / b; } }
        };
	
	//Simply use it like below :-
	int result = mp["+"](1, 2); //This will return 1+2 i.e. 3
	
	Qns on Leetcode: 150
	Leetcode - : Evaluate Reverse Polish Notation
    
```




### :memo: std::set_difference and std::back_inserter :1234:
```c++
set_difference -> Copies the elements from the sorted s1 which are not found in the sorted s2 to a container in sorted order
back_inserter -> Can be used to add elements to the end of a container
Example : 
        set<int> st1, st2;
	vector<int> v1;
	//Find difference in between set1 and set2 and put unique element of set1 in v1
	set_difference(begin(st1), end(st1), begin(st2), end(st2), back_inserter(v1));
	
	Qns on Leetcode: 2215
	Leetcode - : Find the Difference of Two Arrays
    
```
