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
