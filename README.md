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

 
 
