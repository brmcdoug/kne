# xrd2
Updated the code to support both pre/post 7.7 xrd devices in labs.

If the model knob is __"xrd2"__ the pod will be provisioned with post 7.7 variables

The patch can be applied to _cisco.go_ file by:
```
patch cisco.go < cisco.go.patch
```