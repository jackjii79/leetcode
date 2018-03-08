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
 
 
Two: Search problem(up/down/left/right)
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
