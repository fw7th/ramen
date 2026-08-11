## Reduce Max
When given a tensor, reduceMax collapses the specified axis by taking it's maximum. If `keepdims` = 1, the output tensor has the same rank as the input tensor. If `keepdims` = 0. Then it has the dimension pruned, lemme show you with a 1d and a 2d example:

#### 1D
```
T = [1, 2, 3, 4]

if keepdims = 0, and axis = 0
ReduceMax(T) = 4 

Observe when keepdims is false, we don't preserve the dimension. If keepdims were = 1, 
ReduceMax(T) = [4]
```


#### 2D
T = [[1, 2],
    [3, 4]]

with keepdims = 1 and axis = 0:
ReduceMax(T) = [[3, 4]]

Notice how the rank of the tensor is maintained.

if keepdims = 0 and axis = 1:
ReduceMax(T) = [2, 4]. 

If keepdims were 1 though, F(T) = [[2], [4]] where F = ReduceMax.

- IT IS NOT A SUM OPERATOR, YOU CHOOSE THE BIGGEST NUMBER ALONG THAT AXIS.
