When Onnxscript says "a subset of python", they mean some python constructs like:
```python
def my_func(x, y):
    z = x + y
    return z
``` 
can be interpreted as an ONNX graph, but not arbitrary python behaviour. Something like `print(x)`, can't be.

**ONNX** = representation/specification of the computation
**ONNXScript** = Python-based way to author/transform that representation
**ONNX Runtime** = engine that executes it

- Onnxscript aims to have to capability to:
1. Optimize an Onnx model
2. Rewrite patterns in an onnx graph based on user-defined rules
3. A converter which rewrites an onnxscript function into an onnx graph
4. An inverse converter that translates onnx models into onnx script 

> 3 & 4 then allow you to convert Onnxscript ↔ Onnxgraph


- The @script() decorator shows that the function should be converted to ONNX
- Intermediate representation of a function can be converted to an ONNX graph structure of type `FunctionProto`
- Eager mode is used for evaluation and debugging
