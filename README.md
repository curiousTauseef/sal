SAL - simple algorithms and datastructures library
===

A C++ header only library containing efficient algorithms and data structures implemented simply.  

Simplicity here refers to how close the implementation matches the core concepts of each algorithm,
rather than the triviality of each algorithm.

<br>
Features
---
decoupled algorithms so most files are standalone 
- contest friendly 
- paste specific functions in without the rest of the library

<br>
simple implementation of efficient algorithms 
- learning friendly
- engineered for readability

<br>
Motivation
---
I enjoyed some features of the boost library (namely variant), but found the library too cumbersome as a whole.  
Using it in a set up environment was fine, but it was impossible to understand conceptual slices of it.  
It wasn't the best resource to learn the concepts from, and you couldn't (in most cases) use functions from it for contests.  

The focus for this library is on representing the concepts 

Table of contents
===
For examples, look at algotest.cpp and datatest.cpp
Algorithms
---
###### [sal/algo/numerics.h --- numeric](#numeric)
- modular exponentiation
- integer exponentiation
- fibonacci generation
- cyclic number generation and detection
- greatest common denominator
- totient (number coprimes below)
- checking for perfect square and powers of

###### [sal/algo/perm.h --- permutation and combination](#perm)
- kth permutation of an indexable sequence
- all permutations (non-distinct)
- all permutations (distinct)
- generate all possible combinations using each element in sequence
- count ways to generate a sum using a set number of elements of given value

###### [sal/algo/prime.h --- prime generation and manipulation](#prime)
- sequence of primes
- nth prime
- prime closest to a number
- prime after a number
- count primes below a number
- check if prime

###### [sal/algo/search.h --- basic searching, substring matching, and finding longest common features](#search)
- binary search on sorted sequence
- intersection of a set of sets
- order statistic select 
- matching word inside sentence
- longest common substring
- longest common subsequence
- longest common subsequence length
- suffix array (data structure for substring searches)

###### [sal/algo/sort.h --- comparison, distributive, and hybrid sorts](#sort)
- partition
- bubble sort
- insertion sort
- merge sort
- quick sort
- heap sort
- counting sort
- radix sort
- tim sort
- patience sort

###### [sal/algo/string.h --- edit distances](#string)
- levenshtein distance

###### [sal/algo/factorize.h --- factorization of integers](#factorize)
- prime factorize
- number of total factors
- sum of total factors

###### [sal/algo/utility.h --- utility and testing functions](#utility)
- random integer generation
- timer
- perturb a sequence (soft random shuffle)


Data structures
---
###### [sal/data/list.h --- basic linked list](#list)

###### [sal/data/matrix.h --- 2D matrix](#matrix)
- square identities
- random matrices
- efficient multiplication of a sequence of matrices
- resizable

###### sal/data/infint.h --- infinity precision integers (Sercan Tutar's)

###### [sal/data/tree.h --- red black tree and augmentations of it](#tree)
- easily extensible base RB tree
- node iterator
- order statistic tree augmentation
- treap
- order statistic tree

###### [sal/data/interval.h --- interval tree](#interval)

###### [sal/data/graph.h --- directed and undirected graphs](#graph)
- adjacency matrix (not really supported)
- adjacency list
- vertex iterator
- adjacency iterator

###### [sal/data/graph/search.h --- graph searches](#graph_search)
- breadth first search
- depth first search
- recursive depth first search
- visitors for custom actions at certain points of DFS

###### [sal/data/graph/utility.h --- important graph algorithms](#graph_utility)
- topological sort
- check for cycle
- transpose of graph
- strongly connected components

Example usage
===
```namespace sal``` will be used implicitely here  
functions that take iterator pairs are overloaded to take containers as well  
###### sal/algo/numeric.h --- <a name="numeric">numeric</a>
```C++
// 7^91 % 10
int a = modular_pow(7, 91, 10); 
// int 3


int_pow(5, 3);
// int 125


Infint c = fibonacci(1000);
// Infint 43466557686937456435688527675040625802564660517371780402481729089536555417949051890403879840079255169295922593080322634775209689623239873322471161642996440906533187938298969649928516003704476137795166849228875


// repeating part of 1/7 in base 10
make_cyclic(10, 7);
// Infint 142857


// length of repeating part of 1/7 in base 10
cycle_length(10, 7);
// int 6


// greatest common denominator
gcd(56, 91);
// unsigned int 7


// multiplies matrices using optimal parenthesization to minimize work
std::vector<Matrix<int>> mats;
size_t row {30}, col {35};
for (int i = 0; i != 100; ++i) {
	// generate sequence of random matrices
	mats.push_back(random_matrix<int>(row, col, 0, 50));
	// next matrix's row must be prev matrix's col
	row = col;
	col = rand() % 100 + 5;	// between 5 - 105
}
mul(mats);
// Matrix<int> 


totient(500);
phi(500);
// big_int 200 (200 numbers below 500 that are coprime with it)


// check for perfect square
is_square(21489798124);
// bool false
```
###### sal/algo/perm.h --- <a name="perm">permutation and combination</a>
```C++
std::string words {"Hello"};

// modifies the sequence without return
// algorithms work on indexable sequences
// k is from 0 to s.size()! - 1
perm(words, 1);
// string "oHell"


llperms(words);
// vector<string> (120 permutations of "Hello")

allperms_distinct(words);
// set<string> (60 distinct permutations of "Hello")



std::vector<int> ints {1,2,3,4,5,6};

std::set<int> int_add {combine(ints, 
	[](int a, int b){return a + b;})};
// 2 3 4 5 6 7 8 9 10 11 12 handshake by adding

std::set<int> int_add_odd {combine(ints,
	[](int a, int b){return a + b;},
	[](int a, int b){return (a & 1) && (b & 1);})};
// 2 4 6 8 10 handshake by adding odd elements



std::vector<int> coins {1, 2, 5, 10, 20, 50, 100, 200};
int combos = count_combos(coins, 200);
// size_t 73682 (ways to sum up to 200 using coins)
```
###### sal/algo/prime.h --- <a name="prime">prime generation and manipulation</a>
```C++
// keep a sieve object if continuous work with primes is needed
using big_int = size_t;
Sieve<big_int> sieve;


while (true) sieve.next_prime();
// generate infinite stream of primes (each of big_int)


sieve.nth_prime(1000);
// big_int 7919 (1000th prime)


sieve.is_prime(sieve.nth_prime(420000));
// bool true (the 4200000th prime is a prime!)


sieve.count(1000000);
// size_t 78498 primes below a million 


sieve.closest_prime(50000);
// big_int 49999
sieve.next_prime(50000);
// big_int 50021,


sieve.primes_upto(1000000);
// vector<big_int> (78498 primes that are under one million)


```
###### sal/algo/search.h --- <a name="search">basic searching, substring matching, and finding longest common features</a>
```C++
std::vector<int> seq {1,3,12,14,15,18,20};
// bin_search assumes sequence is in order and elements are comparable
bin_search(seq.begin(), seq.end(), 12);
// iterator to element 12

bin_search(seq.begin(), seq.end(), 17);
// iterator seq.end()


std::vector<int> seq2 {1,3,5,6,7,8,20,32};
std::vector<int> seq3 {2,3,6,9,20,32,45,55};
intersection(std::set<vector<int>>{seq, seq2, seq3});
// unordered_set {3, 20} (elements shared by all 3 sequences)


std::string a {"It was the best of times..."};
std::string b {"That's the best orange juice!"};
lc_substr(a, b);
// string "s the best o"


lc_subseq(a, b);
// string "as the best o ie"
lc_subseq_len(a, b);
// size_t 16


sub_match(a, std::string{"the best"});
// const_iterator to 't' in a


// find ith smallest element (1 is smallest)
std::vector<int> v {632, 32, 31, 50, 88, 77, 942, 5, 23};
select(v.begin(), v.end(), 4);
// iterator to 4th element (50)
```
###### sal/algo/sort.h --- <a name="sort">comparison, distributive, and hybrid sorts</a>
```C++
std::vector<int> u {632, 32, 31, 88, 77, 942, 5, 23};
partition(u.begin(), u.end());
// iterator to pivot 77
// 23 32 31 5 77 942 88 632 (all elements < 77 on left and > 77 on right)
 

std::vector<int> v {randgen(1048576, 100000)};	// 2^20

bub_sort(v.begin(), v.end());

// need to know maximum for counting sort, else wastes one pass finding maximum
cnt_sort(v.begin(), v.end(), 1048577);

// binary insertion sort
ins_sort(v.begin(), v.end());

heap_sort(v.begin(), v.end());

// linear insertion sort
lin_sort(v.begin(), v.end());

mer_sort(v.begin(), v.end());

// patience sort, really slow in practice
pat_sort(v.begin(), v.end());

qck_sort(v.begin(), v.end());

// need to know maximum for counting sort, else uses the maximum bit of size
rdx_sort(v.begin(), v.end(), 20);	// 20 bits needed for 2^20 max

tim_sort(v.begin(), v.end());
```

###### sal/algo/string.h --- <a name="string">edit distances</a>
```C++
// # edit actions including insertion, deletion, or substitution to turn source into new
levenshtein("Saturday", "Sunday");
// size_t 3 
```

###### sal/algo/factorize.h --- <a name="factorize">factorization of integers</a>
```C++
size_t num = 421412;

// prime factorize regular numbers (usually smooth) ----------
factorize(421412);
// vector<size_t> 2 2 137 769 (in order)


num_factors(num);
// size_t 12 (1 2 4 137 274 548 769 1538 3076 105353 210706 421412)

sum_factors(num);
// size_t 743820 (1 + 2 + 4 + 137 + ... + 421412)



// factorize primes or semiprimes -----------------------------
big_int semiprime = 32452843 * 32452867;	// 1053187797650881

// would still not be too slow
factorize(semiprime);

// would be faster
factorize_rough(semiprime);
// would be much faster for repeated uses (primes are kept static)

```

###### sal/algo/utility.h --- <a name="utility">utility and testing functions</a>
```C++
// precision timing, starts counting when constructed
Timer timing;


// generate random vector of integers for testing
int max_value = 50000;
int element_number = 4000;
randgen(max_value, element_number);
// vector<int> (4000 elements with values between 0 and 50000)


// restart timer
timing.restart();


std::vector<int> v;
v.reserve(element_number);
for (int i = 0; i < element_number; ++i) v.push_back(i);
// vector 1 2 3 4 5 ... 50000

// randomly displace elements of v probabilistically 5 elements away from sorted position
perturb(v.begin(), v.end(), 5);

// print a sequence so elements are separated by space
print(v);

timing.tonow();
// double in microseconds 
```
###### sal/data/list.h --- <a name="list">basic linked list</a>
```C++
// Construction ---------------------------
Basic_list<int> l {5, 7, 11, 13, 17, 19};
std::cout << l;
// 5 7 11 13 17 19



// Queries ---------------------------
l.kth_last(2);
// pointer to node 19 
std::cout << l.kth_last(2)->data << std::endl;
// 19



// Modification -------------------------
// insert at head
l.insert(3);
l.print();
// 3 5 7 11 13 17 19

// append to tail
l.append(23);
// 3 5 7 11 13 17 19 23


// insert after the 2nd last 
l.insert_after(19, l.kth_last(2));
// 3 5 7 11 13 17 19 19 23

l.erase(13);
// 3 5 7 11 17 19 19 23

// remove duplicates
l.remove_dup();
// 3 5 7 11 17 19 23
```
###### sal/data/matrix.h --- <a name="matrix">2D matrix</a>
```C++
// Construction ---------------------------
Matrix<int> A {{2, 5, 6},
	       {3, 4, -3},
	       {7, 8, 0}};
Matrix<int> B {{-1, 1},
	       {5, -2},
	       {4, 2}};


// Queries ---------------------------
// size query
A.row();
// size_t 3
B.col();
// size_t 2

// get element (indices from 0)
A.get(1, 2);
// int -3 (2nd row, 3rd col)




// Arithmetic Operations ---------------------------
Matrix<int> F {{1, 1},
			   {1, 0}};
// matrix exponentiation
F.pow(5);
// Matrix<int>
// 8  5
// 5  3

// matrix addition
F += Matrix<int> {{2, 5}, {5, 7}};
// 10 10
// 10 10
F + Matrix<int>{{1,1},{1,1},{1,1}};
// runtime error exception from adding different dimensions


// for least work in multiplying matrices, use mul(sequence) from numeric.h
Matrix<int> C = A * B;
// Matrix<int>
// 2 5  6   -1  1       47  4
// 3 4 -3    5 -2  -->  5  -11 
// 7 8  0    4  2       33 -9



// Modification -------------------------
// individual elements can be changed by get()
// size modification
Matrix<int> id3 {identity<int>(3)};
// 1 0 0
// 0 1 0
// 0 0 1

// default for new elements is 0
id3.resize(4,5,2);
// 1 0 0 2 2
// 0 1 0 2 2
// 0 0 1 2 2
// 2 2 2 2 2
id3.resize(2,2);
// 1 0
// 0 1

// clockwise rotation
C.rotate();
// 33   5   47
// -9  -11  4
```

###### sal/data/tree.h --- <a name="tree">red black tree and augmentations of it</a>
```C++
// Basic tree usage -------------------------
Basic_tree<int> t {5, 3, 7, 1, 9, 4, 2, 0, 10, 8, 6};
// insert element
t.insert(5);
// erase element
t.erase(1);
// find element
auto node = t.find(5);
// const iterator to node holding 5
t.find(11);
// const iterator to end == t.end()




// Iterations --------------------------------
// iteration over adjacent nodes (left to right)
for (auto adjacent = node.begin(); adjacent != node.end(); ++adjacent)
	std::cout << *adjacent << ' ';
// * operator returns key


// iteration over all nodes (in order)
using Node = Basic_node<int>;
std::for_each(t.begin(), t.end(), 
	[](const Node& node){std::cout << node.key << ' ';});




// Extending Tree for an augmented data structure ---------
// first step is creating a node
/* requires
	1. key element that can be compared using <
	2. static node pointer nil, sentinel
	3. parent, left, and right pointers initialized to nil
	4. Color attribute, BLACK for nil and RED for non-nil
*/
template <typename T>
struct Order_node {
	static Order_node* nil;

	using key_type = T;
	T key;
	Order_node *parent, *left, *right;
	// # of descendent nodes including itself = left->size + right->size + 1
	size_t size;	
	Color color;
	Order_node() : size{0}, color{Color::BLACK} {}	// sentinel construction
	Order_node(T val) : key{val}, parent{nil}, left{nil}, right{nil}, size{1}, color{Color::RED} {}
};

template <typename T>
Order_node<T>* Order_node<T>::nil {new Order_node{}};

// Augmented tree need to publically inherit from Tree<Node>
// it should use the following protected members from Tree<node>
template <typename Node>
class Order_augment : public Tree<Node> {

	using NP = Node*;	// optional
	using T = typename Node::key_type;	// optional
	using Tree<Node>::root;
	using Tree<Node>::rb_insert;
	using Tree<Node>::transplant;
	using Tree<Node>::rb_insert_fixup;
	using Tree<Node>::rb_delete_fixup;

// iterators can also be inherited from Tree
	using iterator  = typename Tree<Node>::iterator;
	using const_iterator = typename Tree<Node>::const_iterator;	

// override specific virtual methods to maintain augmented property
// tree_insert, rb_delete, rotate_left, and rotate_right
	virtual void tree_insert(NP start, NP node) override {
		NP parent {Node::nil};
		while (start != Node::nil) {
			// modification: simply increment size of each ancestor going down
			++start->size;	
			parent = start;
			if (node->key < start->key) start = start->left;
			else start = start->right;
		}
		node->parent = parent;
		if (parent == Node::nil) root = node;
		else if (node->key < parent->key) parent->left = node;
		else parent->right = node;
	}
};

// provide convenient user access
template <typename T>
using Order_tree = Order_augment<Order_node<T>>;




// Utilities ----------------------------
// non-member functions that work on nodes only
Order_tree<int> t {5, 3, 7, 1, 9, 4, 2, 0, 10, 8, 6};
using NP = Order_node<int>*;
auto node = t.find(5);
// const iterator

// check if t is a valid RB tree, 0 is invalid, else returns blackheight
// PRINTLINE macro is from utility.h
PRINTLINE(t.valid());

// get() on iterators return the raw node pointer
tree_min(node.get());
// node pointer to smallest node rooted at given node pointer
// goes left as far as possible

tree_max(node.get());
// node pointer to largest node, symmetrical to tree_min

tree_predecessor(node.get());
// node pointer to next smallest node
tree_successor(node.get());
// node pointer to next largest node

// traversals take NP root and an operation that acts on NP
inorder_walk(node.get(), [](const NP node){std::cout << node->key << ' ';});
preorder_walk(node.get(), [](const NP node){std::cout << node->key << ' ';});
postorder_walk(node.get(), [](const NP node){std::cout << node->key << ' ';});




// Order statistics tree usage (with t above)
// Queries ----------------------------
// selecting ith smallest
t.select(4);
t[4];
// iterator to node holding 4th smallest (3)
// select is overloaded on constness, operator[] is always non-const

// check the rank of a node pointer
auto node = t.find(5);
t.rank(node.get());
// size_t 6 (node holding 5 is 6th smallest node)

```
###### sal/data/interval.h --- <a name="interval">interval tree</a>
```C++
// for finding overlapping intervals, useful for scheduling and collision detection
// Construction ------------------------
Interval_set<int> t {{16,21},{8,9},{5,8},{15,23},{25,30},{0, 3},{6, 10},{17,19}, 
					{26,26}, {19,20}};




// Queries ----------------------------
// find any interval overlapping [22, 25]
// overlap by default includes edge, so [25, 26] overlaps [22, 25]
// easily adjustable by changing the strictness of ordering in the no_overlap method
t.find({22, 25});
t.find(22, 25);		// overloaded to accept Intervals as well as low, high pairs
// iterator to some overlapping interval, else end()

// find first (earliest) overlapping interval
t.find_first(20, 22);
// iterator to [15, 23] (note that [16, 21] is also overlapping, but starts later)

// find all intervals that match
for (auto interval : t.find_all(20,22)) PRINTLINE(*interval); 
// vector<node pointers> [15, 23], [16, 21]

// find exact interval match
t.find_exact(16, 21);
// iterator to [16, 21]
```
###### sal/data/graph.h --- <a name="graph">directed and undirected graphs</a>
```C++
// Construction ------------------------
// unweighted is equivalent to having uniform weight 1
// undirected and unweighted graph by default
graph<int> g {{5,1},{5,4},{5,10},{1,4},{4,10}};

// directed and and unweighted graph
digraph<int> g {{5,1},{5,4},{5,10},{1,4},{4,10}};


// undirected and weighted graph
graph<int> g {{5,1,10},{5,4,13},{5,10,17},{1,4,19},{4,10,23}};

// directed and weighted graph
digraph<int> g {{5,1,10},{5,4,13},{5,10,17},{1,4,19},{4,10,23}};




// Queries ----------------------------
// number of vertex
g.num_vertex();
// size_t 4

// number of edges
g.num_edge();
// size_t 5

// check if vertex exists
g.is_vertex(5);
// bool true

// check if edge exists
g.is_edge(5, 1);
// bool true

// weight of edge (1 for unweighted, 0 if not found)
g.weight(5, 1);
// int 10

// out degree of a vertex
g.degree(5);
// size_t 3




// Modification -------------------------
// adding vertex (names have to be unique)
g.add_vertex(7);

// adding edge (have to be unique)
// adding edge to non-existent vertices will create them
// weight is 1 by default
g.add_edge(6, 4);



// Iterations ---------------------------
// iterate over all vertices ordered by <
for (auto v = g.begin(); v != g.end(); ++v)
	std::cout << v;
// 1 4 5 10
// << overloaded for iterators

// adjacency iteration
auto edges = g.adjacent(5);
// alter the weight
for (auto v = edges.first; v != edges.second; ++v)
	v.weight() = 1;

// can also be done given a vertex iterator
// get vertex iterator
auto v = g.vertex(5);
// iterator to vertex named 5
// iterate over adjacent vertices u
for (auto u = v.begin(); u != v.end(); ++u)
	std::cout << u;
// 1 4 10 (5 has out edges to all of them)

```
###### sal/data/graph/search.h --- <a name="graph_search">graph searches</a>
```C++
/* algorithms expects Graph to have: 
	vertex iterators in begin() and end() (and reverse iterators in rbegin(), rend())
  		* gives vertex name of type V
  		begin() and end() to give adjacent iterators

  	adjacent iterator pair in adjacent(V)
  		* gives vertex name of type V, the destination vertex
  		dest() gives name of destination vertex (same as * operator)
  		weight() gives weight of edge to destination vertex
*/

// breadth first search ----------------
// usually used to find distances to a source node
// works with directed and undirected but have to be unweighted
graph<char> g {{'v','r'},{'r','s'},{'s','w'},{'w','t'},{'t','x'},{'w','x'},{'t','u'},
			   {'x','u'},{'x','y'},{'u','y'}};

// breadth first search starting from a vertex
auto property = bfs(g, 's');
// property map with distance and parent information
// tracing back parents of each vertex creates a breadth-first tree


for (char vertex = 'u'; ;vertex = property[vertex].parent) {
	std::cout << vertex;
	if (vertex == 's') break;
	std::cout << " <- ";
}
// u <- x <- w <- s


property['u'].distance;
// size_t 3 (number of edges to get to u from s)




// depth first search ------------------
// usually used as part of other algorithms

// can also function to find shortest number of edges from a source node
auto property = dfs(g, 's', 0);	// need dummy int variable for overload resolution
// property map with parent and time stamp information


for (char vertex = 'u'; ;vertex = property[vertex].parent) {
	std::cout << vertex;
	if (vertex == 's') break;
	std::cout << " <- ";
}
// u <- t <- w <- s different from bfs, but same length




digraph<char> e {{'u','v'},{'u','x'},{'x','v'},{'v','y'},{'y','x'},
				{'w','y'},{'w','z'},{'z','z'}};
// create depth-first forest
auto dfs_property = dfs(e);
// not particularly useful by itself, but very useful for other algorithms



// DFS visitor -----------------------
// actions taken at several key points of searching can be customized
struct Graph_visitor {

	// generate vector of vertices to visit
	// acts as a stack, visitation happens from back to front
	// by default visits every vertex
	template <typename Property_map, typename Graph>
	std::vector<typename Graph::vertex_type> initialize_vertex(Property_map& property, const Graph& g) {
		std::vector<typename Graph::vertex_type> exploring;
		for (auto v = g.rbegin(); v != g.rend(); ++v) {
			// mark unexplored
			property[*v] = {unsigned_infinity, 0, *v};
			// expore every vertex
			exploring.push_back(*v);
		}
		return std::move(exploring);
	}

	// root vertex of a new depth-first tree, when a new unconnected component is discovered
	template <typename Graph>
	void start_vertex(typename Graph::vertex_type, const Graph&) {}

	// when each vertex is first discovered
	template <typename Graph>
	void discover_vertex(typename Graph::vertex_type, const Graph&) {}

	// when a vertex's neighbours are all being explored
	template <typename Graph>
	void finish_vertex(typename Graph::vertex_type, const Graph&) {}

	// when an edge leads to an ancestor vertex of the current exploration path
	template <typename Graph>
	void back_edge(typename Graph::vertex_type, const Graph&) {}

	// when an edge leads to a vertex without a shared ancestor 
	template <typename Graph>
	void forward_or_cross_edge(typename Graph::vertex_type, const Graph&) {}
};

```
###### sal/data/graph/utility.h --- <a name="graph_utility">important graph algorithms</a>
```C++
// topological sort -----------------------
// for directed acyclic graph (dag)
// orders all vertices so that parent vertices are always before children
// if vertices are events, then sorting gives one possible sequence of events
// directed edge (u,v) means u has to happen before v
digraph<std::string> dress {{"undershorts", "pants"}, {"undershorts", "shoes"}, {"pants", "belt"},
			{"pants", "shoes"}, {"socks", "shoes"}, {"shirt", "belt"}, {"shirt", "tie"}, {"tie", "jacket"},
			{"belt", "jacket"}};
dress.add_vertex("watch");


std::list<string> dress_order;
// give a possible ordering of events
topological_sort(dress, std::front_inserter(dress_order));

for (const std::string& item : dress_order) std::cout << item << ' ';
// watch undershorts socks shirt tie pants shoes belt jacket
// perfectly logical sequence of dressing




// cycle testing -----------------------
// some algorithms need directed acyclic graphs (dag)
has_cycle(dress);
// bool true (if false, then the ordering given would be false)




// transpose -----------------------
// create graph with all the edges reversed
transpose(dress);
// graph (of taking off clothes)

// topological sort gives
// watch shoes socks jacket tie belt shirt pants undershorts




// strongly connected components --------------------
// a SCC is a part of a graph where any vertex can get to any other vertex inside it
// could model trading networks and the like
digraph<std::string> trade {{"China","USA"},{"USA","EU"},{"USA","Canada"},{"USA","Russia"},{"EU","Brazil"},{"EU","France"},
				{"Brazil","EU"},{"Brazil","Australia"},{"Canada","China"},{"Canada","Russia"},{"Russia","France"},
				{"France","Russia"},{"France","Australia"},{"Australia","Australia"}};

auto mutual_partners = strongly_connected(trade);
// vector of sets of strings


for (const auto& bloc: mutual_partners) {
	for (const std::string& country : bloc) std::cout << country << ' ';
	cout << '\t';
}
// USA China Canada      EU Brazil      Russia France   	Australia
```

