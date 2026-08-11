### Gather
Gather is a "tensor selection operation". This is how it works:

If you have a tensor A, another B, and an axis: int.
A.shape = [2, 2]
B.shape = [4]
axis = 0

output tensor = [4, 2]. The shape will be rearranged like this everytime. Just place the shape in the required axis. Holds with N dims. Let me show you why in an explicit example.

If 
A =
[
    [10, 20, 30],
    [40, 50, 60],
]

B =
[
    [1, 0],
    [0, 1],
]

axis = 0

Output = 
[
  [[40, 50, 60],
   [10, 20, 30]],

  [[10, 20, 30],
   [40, 50, 60]]
]

output.shape = [2, 2, 3]

You pick the tensor in the dim you want. Same with columns

In this example, you pick tensor 1 of axis 0 -> that's what B asked for, you place it in a tensor Output, you pick the tensor 0 of same axis, you keep that working axis throughout. You finish the row B is demanding? Then you make a new dim. Here's a column based example:

A =
[
    [1,  2,  3,  4],
    [5,  6,  7,  8],
    [9, 10, 11, 12],
]

B = [2, 0]

axis = 1


Output = 
[
 [3, 1],
 [7, 5],
 [11, 9]
]

Output.shape = [3, 2]
