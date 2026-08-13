A .pt file stores the internal state of a pytorch model, it's essentially a pickled python dictionary. It will typically contain:
- A state dict; which maps a layer name to a tensor of learned params
- Training metadata

*pickle can execute arbitrary code during deserialization*
