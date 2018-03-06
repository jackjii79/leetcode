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
 One. two pointers problem:
 1 forward pointers(同向)
 
 1) windows pointers:
 Usually, used to find the range of subarray of range of subsum in either adjacent(连续) or disjoint(不连续) manner 
 
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
 
