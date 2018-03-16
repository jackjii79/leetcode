# leetcode
solution

note: 
1: recursive call recursive(self,abc)
for i in range(x):
 self.recursive(abc+x)
has the equivalent effect as:
for i in range(x):
 abc += x
 self.recursive(abc)
 abc -= x
 
 problem categories:
 
 One: two pointers problem
 
 1 forward pointers(同向)
 
 1) windows pointers:
 Usually, used to find the range of subarray of range of subsum in either adjacent(连续) or disjoint(不连续) manner 
 purpose1: locate range of subarray
 sample code:
 j = 0;make sure you perform action with the case when j == 0
 for i in range(len(input)):
    while j+1 < len(input) and condition I:
      some actions with j+1
      j += 1
    if ! condition I: normally, this condition check is needed to be done when we looking for shortest substring/smallest sum,etc
       j - i + 1 is the range that we desire
    some actions to remove input[i] so that windows can keep moving from i+1
 Note:we need to make sure the postcondition of each while wihtin for loop is that between [i,j] there is a feasible solution when we exit from the while loop !
 
 purpose2: write/read pointer while read pointer read next element of input array and write pointer write/update current element with what read pointer reads, such is used to shift non-zero elements to left/right 
    
 2) fast/slow pointers:
 Usually, used to find/detect cycle of linked list or one repetition in array
 sample code:
 while True:
  slow_pointer = pointer[index]
  fast_pointer = pointer[pointer[index]]
  while slow_pointer == fast_pointer:
     break
 
 2 forward backward pointers(相向)
 Usually, used to find sum of subarray in a sorted manner since move either direction(left/right) of pointers will have different effects
 
 sample code:
 while left < right:
    if condition I:
       some actions(calculate expression with left and right:right-left,etc)
       left += 1
    if ! condition I:
       some actions
       right -= 1
       
 3 multiple arrays
 Usually, used to find kth smallest/biggest element in matrix/array
 if sort is allowed, then we sort all arrays and put all possible solutions into heap and traverse one by one until we reach the kth one;
 if sort is not allowed, then we maintain a min/max heap among k elements to find the smallest/biggest among k-max/min heap.
 
 
Two: Search problem(up/down/left/right) DFS algo
given a matrix, position can move in four directions. Such problem often can be solved in a recursive manner

Sample code:
def recursive_call(..,row,col): row/col indicates position information of the moving pointer in the matrix
 if condition I not satisfy:
   return
 if condition I satisfy:
   matrix[row][col] set something that can not satisfy condition I to prevent move back
   result = recursive_call(..,row+1,col) or recursive_call(..,row-1,col) or ...
   reset matrix[row][col] to be origin
   
Three: Hash table application problem
1 compute hashvalue for string match:
let base=10/163(constant number) given a string s where s[0],...,s[N-1] N is the length of string s
hashtable[0] = 0; hashtable[1] = s[0]; hashtable[2] = base * hashtable[1] + s[1]
in general, hashtable[i] = hashtable[i-1]*base + s[i], hashtable[N-1] indicate the hash value for the whole input string s !
hashtable[i] = s[1]*base^i + s[2]*base^(i-1) + ... + s[i]
compute the range of hashtable from l to r: hashtable[l,r] = hashtable[r] - hashtable[l-1]*x^(r-l+1)

2 use hashtable to find occurrence of certain element in the input array
if given input array has N element and each element is between [1,N] then we can use each element as key of the hashtable to check if certain element appears in the array. The assumption is that input array can be modified !

sample code:
for x in input_array:
   if input_array[abs(x)] < 0:
      perform action (abs(x) has been seen before thus munus)
   else:
      input_array[abs(x)] = 0 - abs(input_array[abs(x)]) (first time seen, thus set minus !)
      
      
 Three important advanced data structure !!!
 1 Segment Tree: master three kinds of operations (build a segment tree + query a segment tree + update a segment tree)
 build a segment tree:
 if start == end:
            return SegmentTreeNode(start,end,A[start])
        elif start > end:
            return None
        else:
            middle = int((start+end)/2)
            root = SegmentTreeNode(start,end,0)
            root.left = self.dfs_build(start,middle,A)
            root.right = self.dfs_build(middle+1,end,A)
            root.max = max(root.left.max,root.right.max)
            return root
            
 query a segment tree:
 if root == None or start > end:
            return 0
        elif root.start >= start and root.end <= end: # we do not use "==" since we need to consider the case when range [start,end] is bigger than node range(root.start,root.end)
            return root.count
        else:
            middle,leftcount,rightcount = int((root.start+root.end)/2),0,0
            if middle >= start: #if [start,end] resides on left sub tree
                if middle >= end: #if [start,end] completely resides on left sub tree
                    leftcount = self.query(root.left, start, end)
                else: #[start,end] partial resides on left sub tree
                    leftcount = self.query(root.left, start, middle)
            if middle < end: #if [start,end] resides on right sub tree
                if middle < start: #if [start,end] completely resides on right sub tree
                    rightcount = self.query(root.right, start, end)
                else: #[start,end] partial resides on right sub tree
                    rightcount = self.query(root.right, middle+1, end)
            return leftcount + rightcount
            alternatively,
            middle = int((root.start+root.end)/2)
            if root.left.end >= end: #if query range is completely resides on the left sub tree
               return self.query(root.left, start, end)
            if start >= root.right.start: if query range is completely resides on the right sub tree
               return self.query(root.right, start, end)
            return self.query(root.left, start, middle) + self.query(root.right, middle+1, end) #paritially redies on both sub trees
            
update a segment tree:
if index >= 0 and index == root.start and index == root.end:
            root.max = value
        elif index < root.start or index > root.enniod:
            return 
        else: #find the range of index treenode to see whether it resides on left sub tree or right sub tree
            middle = int((root.start+root.end)/2)
            if index <= middle and index >= root.start:
                self.modify(root.left,index,value)
            elif index <= root.end and index > middle:
                self.modify(root.right,index,value)
            root.max = max(root.left.max, root.right.max)
            
Summary: we can use segment to solve range sum proble; number of elements bigger/smaller than a certain given number in a list;
all problems related to find the min/max/sum of a consecutive range in a list
note: if we could change the scope of the range by deleting/updating the value of certain treenodes, then we can not use segment tree structure


2 Union set: master two kinds of operations (find the parent node id of a given node + union two separate nodes)

  find a parent node with path compression:
  union_find(self, union_set, index):
     while index != union_set[index]:
        union_set[index] = union_set[union_set[index]]
        index = union_set[index]
     return index
     
  merge two separate node into the same set(only change their father relation老大哥关系改变) with union size:
  union_merge(self, union_set, union_size, index_a, index_b):
     root_a, root_b = union_find(union_set, index_a), union_find(union_set, index_b)
     if root_a != root_b:
        if union_size[root_a] < union_size[root_b]:
           union_set[root_a] = root_b
           union_size[root_b] += union_size[root_a]
        else:
           union_set[root_b] = root_a
           union_size[root_a] += union_size[root_b]
  
  Summary: we can use union set data structure to solve connected graph problem(connected component -- cycle/tree -- not cycle), to decide if a graph is a tree or a connected component, if two nodes that already in the same set are merged again then a cycle will formed, otherwise it is only a tree;
  Hard problem: 1) surrounded region:we scan the input board using DFS in four directions and to decide if we change all "O" to "X", we need to check boarder, if any "O" is on the edge then we do nothing and put all "O" positions into scan_set to avoid rescanning, otherwise we change all "O" into "X" and put those into scan set as well.
  
  2) number of islands II:(I can be solved using DFS in four directions) we use union set structure to solve this problem, we first need to convert 2-dimension board position into one dimension index and vice versa; we doing this in a manner of sequential order as we read new island from input list, we consider four cases:1) total number of islands unchanged if all islands adjacent to the new island are belong to the same set 2) total number of islands increase by one if there is no island adjacent to the new island in any four directions 3) total number of islands decrease by one if there are two islands adjacent to the new island in two of four directions and they both not the same set 4) total number of islands decrease by two if there are three islands adjacent to the new island in three out of four directions and they all belong to different sets 5) total number of islands decrease by three if there are four islands adjacent to the new island in four directions and they all belong to different sets
  
 
 3 Trie Tree: master three operations (insert word + search word + search prefix)
   class TrieNode:
      self.children = collections.OrderedDict()
      self.is_endofword = False
   insert word:
      def insert_word(self, word):
         temp = self.root
         for ch in word:
            if ch not in temp.children.keys():
               temp.children[ch] = TrieNode()
            temp = temp.children[ch]
         temp.is_endofword = True
   search word:
      def search_word(self, word):
         temp = self.root
         for ch in word:
            if ch not in temp.children.keys():
               return False
            temp = temp.children[ch]
         return temp.is_endofword
   search prefix:
      def search_prefix(self, prefix):
         temp = self.root
         for ch in prefix:
            if ch not in temp.children.keys():
               return False
            temp = temp.children[ch]
         return True
  
  summary: Trie tree can be used to solve massive strings problem, like matching/searching/etc, we solve such problems by DFS Trie tree to explore all possible valid words in the tree, like following format:
  for key in tree.children.keys():
     self.dfs_explory(tree.children[key], word+key, xx+key..)
     
  1) K edit distance: DFS + Trie tree + DP
  The principle of DP is to help to determine if string founded so far as scanning trie tree has minimal edit distance less or equal to K, the relation of DP is as following:
  for str1 and str2 to determine if str1 can convert to str2, the minimal steps stored in array solution[len(str1)+1][len(str2)+1]:
for i in range(len(str1)+1):
for j in range(len(str2)+1):
if i == 0:
solution[i][j] = j
if j == 0:
solution[i][j] = i
if i >0 and j > 0:
if str1[i] == str2[j]:
solution[i][j] = solution[i-1][j-1]
else:
solution[i][j] = 1 + min(solution[i-1][j], solution[i][j-1], solution[i-1][j-1])

explain: solution[i-1][j] means delete ith char from str1;
solution[i][j-1] means insert jth char of str2 into str1;
solution[i-1][j-1] means replace ith char of str1 by jth char of str2

We use DFS+Trie tree to explore all possible branches words and we use DP to determine the minimal number of edit distance by far and as long as current node reaches endofword tag and its minimal edit distance is less than k, we put into our result list

notes: consider the case when target is "" and words is ""

  2) Palindrome Pairs: find all pairs of input string that could concatenate a whole Palindrome string(the order matters!!a+b!=b+a)
   this problem applys DFS+Trie+Stack
   we construct two trie trees, one for original input list of string and another one for reversed order of each string of input list.
   First of all, we use DFS to explore the first original input Trie tree to explore all valid string with endofword tag/flag and we put each character of such string into a temp_stack and then we call another DFS method to explore the second reversed Trie tree to find all possible Palindrome pairs of the string we found in the first tree;
   
   In the second DFS method, if temp_stack is empty then we use temp_reverse_stack to store all input character of reversed Trie tree as DFS ongoing, if temp_stack is not empty, we found all matched character and put each into word2 and we remove the first character of the temp_stack, until we reach the endofword flag, we examine: 1) if temp_stack is not empty and check if temp_stack is Palindrome; 2) or if temp_stack is empty and check if temp_reverse_stack is Palindrome; 3) we need to make sure that word1 != word2[::-1] to make sure that word1 and word2 are two different string !!!
At last, we append word1 and word2 into result_list and please note that we do not return but keep running to find all possible pairs in the following for loop !!!

We need to think through that origin Trie tree represents the head part of the whole concatenation Palindrome string and the reversed Trie tree represents the end part of the whole concatenation Palindrome string , what we doing is to remove heads and tails one by one, all middle part should also be Palindrome string ! that is why we check if stack is Palindrome !!!
   
