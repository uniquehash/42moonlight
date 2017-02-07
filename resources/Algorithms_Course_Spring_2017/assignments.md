Assignments
-----------
-----------

This is a list of projects to do and things to build in order to completely understand [Algorithms](http://www.cse.iitd.ernet.in/~naveen/courses/CSL630/all.pdf) by Sanjoy Dasgupta, Christos Papadimitriou, and Umesh Vazirani.

Feel free to add to it should you think of any useful exercises or find any cool resources.

You may build these in any langauge. I suggest a dynamic language like Python or JavaScript. These are good for interviews because they're generally more permissive, so things can be written in them with fewer lines of code (better for cramming stuff onto a whiteboard).

For any algorithm you write, you MUST include a comment with it's runtime before the function decalaration.

### These are not made to be easy.

TODO: Think up more fun projects so this isn't QUITE so awful :D

Day 00
------

* for whatever reason, this day is probably the hardest :)

* mini-calc: write a function that takes two numbers of ANY SIZE with an operator in between them and outputs the result of the operation.

 * only need to handle +, -, *, and /

 * examples: ./mini-calc 123456789000 + 987654321000

 * 1111111110000

 * ./mini-calc 217340927348 * 982734927340

 * 213588520445344998894320

 * ./mini-calc 2 / 2

 * 1

 * You need not implement full error-handling (do watcha want, Moulinette ain't gunna get yat)

* implement a hash table/map

 * [wikipedia](https://en.wikipedia.org/wiki/Hash_table)

 * would be wise to implement the hash function used in the hash literal for whichever language your using (assuming you aren't using C... if you are then use whichever pleases you!)

 * implement the following operations (make methods of the hash table object if OOP)

  * you DO NOT need to name them like this - feel free to use whatever convention your langauge suggests.

  * hash_map_name.len() returns the total number of elements in the map

  * hash_map_name.get_val(key_name) returns the value associated with key_name or NULL if key_name doesn't exist in hash map

  * hash_map_name.set_val(key_name, val_name) sets the value associated with key_name to val_name

  * hash_map_name.pop(key_name) removes the key_name from the map and returns the value associated with key_name

  * hash_map_name.keys() returns an array of all the keys in hash map

  * hash_map_name.values() returns an array of all the values in hash map

* find_sum: given an unsorted array of integers, find a pair that sum to a given number and return their locations in the array

 * use your hash table for fastest time! (To be complete, this must be done :) )

 * the last number passed via commandline will be the desired sum

 * examples: ./find_sum 1 2 3 4 5 8

 * (2, 4)

 * ./find_sum 1 2 3 4 5 6

 * (0, 4) OR (1, 3) - you don't need to write 'OR', you can just give either answer and still be considered correct

 * ./find_sum 1 2 3 4 5 10

 * NO SOLUTION

* (bonus) implement RSA encryption

 * [wikipedia](https://simple.wikipedia.org/wiki/RSA_(algorithm))

 * Good luck ;)

Day 01
------

* implement a binary search tree

 * [wikipedia](https://en.wikipedia.org/wiki/Binary_search_tree)
 
 * each node should have a key and a value, the key being the numeric, sortable element and the value being any associated content (think of the piscine)

 * implement the following operations:

  * bst_name.add(node) adds a node with an associated key and val to the bst using key to place the node
  
  * bst_name.len() returns the number of nodes in the bst
  
  * bst_name.pop(key) removes the node from the bst and returns the value associated with the key
  
  * bst_name.search(key) returns the value associated with the key

* implement mergesort and quicksort

 * [mergesort](https://en.wikipedia.org/wiki/Merge_sort) and [quicksort](https://en.wikipedia.org/wiki/Quicksort) wikipedias

 * examples: ./mergesort 3 8 7 4 2 1
 
 * 1 2 3 4 7 8
 
 * ./quicksort 12 7 6 1 1 0 8 4 2
 
 * 0 1 1 2 4 6 7 8 12
 
* (bonus) implement Fast Fourier Transform

 * [wikipedia](https://en.wikipedia.org/wiki/Fast_Fourier_transform) and [cool article](https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/)
 
 * Have fun!!! :)
 
Day 02
------

* implement an adjacency list

 * [wikipedia](https://en.wikipedia.org/wiki/Adjacency_list)
 
 * can be done multiple ways - hash table, an array and a linked list, or via objects (see wikipedia)
 
 * implement the following operations:
 
  * adj_lst_name.add_node(node_name) initializes a new node in the adjacency list
  
  * adj_lst_name.add_dir_edge(node_1, node_2, weight) define an edge from node_1 to node_2 with value of weight
  
  * adj_lst_name.add_undir_edge(node_1, node_2, weight) define edges from node_1 to node_2 and from node_2 to node_1 with values of weight
  
  * adj_lst_name.vertex_count() returns the number of vertices
  
  * adj_lst_name.vertices() returns an array with the memory addresses of the vertices
  
  * adj_lst_name.edge_count() returns the number of edges
  
  * adj_lst_name.edges() returns an array with the memory addresses of the vertices

* implement an adjacency matrix (not as important as adjacency list IMHO)

 * [wikipedia](https://en.wikipedia.org/wiki/Adjacency_matrix)
 
 * implement the following operations:
 
  * adj_mat_name.add_node(node_name) initializes a new node in the adjacency matrix
  
  * adj_mat_name.add_dir_edge(node_1, node_2, weight) define an edge from node_1 to node_2 with value of weight
  
  * adj_mat_name.add_undir_edge(node_1, node_2, weight) define edges from node_1 to node_2 and from node_2 to node_1 with values of weight
  
  * adj_mat_name.vertex_count() returns the number of vertices
  
  * adj_mat_name.vertices() returns an array with the memory addresses of the vertices
  
  * adj_mat_name.edge_count() returns the number of edges
  
  * adj_mat_name.edges() returns an array with the memory addresses of the vertices

* implement depth-first search

 * [wikipedia](https://en.wikipedia.org/wiki/Depth-first_search)

 * I would recommend using an adjacency list :)
 
 * Need not (nor can in any reasonable fashion) have a commandline interface
 
 * Should return the order in which the nodes where first visited
 
* implement algorithm to find strongly connected components in a directed graph

 * [wikipedia](https://en.wikipedia.org/wiki/Strongly_connected_component)
 
 * Should return an array of lists of nodes, with each list being a single component
 
  * See the end of chapter 3 as an example of the formatting of one such resulting sequence
  
Day 03
------

* This day will be much more fun if you've built (at least) the adjacency list from day 02 :)

* Implement a heap

 * [wikipedia](https://en.wikipedia.org/wiki/Heap_(data_structure))
 
 * Can use an array, or a binary heap, or something even fancier (Fibonacci heap?!?)
 
 * implement the following operations:
 
  * heap_name.peek() return the root of the heap
  
  * heap_name.push(node) insert node with an associated key and val into the heap
  
  * heap_name.pop() remove the root of the heap and re-organize (sift) the heap and return the recently removed node
  
  * heap_name.replace(node) pop the root and push the node, requiring only 1 sift instead of 2 if pop and push happen independently
  
* Implement heapify

 * Given an array of nodes (with values and keys), heapify takes that nodes and organizes them into a heap!
 
* Implement Dijkstra's algorithm

 * [wikipedia](https://en.wikipedia.org/wiki/Dijkstra's_algorithm)
 
* Implement Bellman-Ford algorithm

 * [wikipedia](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm)
