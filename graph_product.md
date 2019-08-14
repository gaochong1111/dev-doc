# LTL 使用GrapProduct的示例
## Test 中示例代码
![示例](./pics/demo1.png)
## 使用Solver:PropertySolverExplicitLTLLazy
![dosolver](./pics/dosolve.png)
## 调用PropertySolverExplicitLTLLazy.checkTemporalLTLNonIncremental
![docheck](./pics/docheck.png)
## 调用 PropertySolverExplicitLTLLazy.computeNonIncremental
![doproduct](./pics/doproduct.png)
```
    1. 红框 switch case 可以扩展
    2. build 得到ProductGraph对象
```
## 构造用于计算的Graph
### Product
```
    根据ProductGraph构造结果GraphExpliciteWrapper
        这里可能包括Product的过程
```
![res-graph](./pics/res-graph.png)
### ResultGraph
```
    1. 对Graph 的空间进行 explore [比较耗时]
    2. 计算Graph的NumStates [比较耗时]
```
![compute-graph](./pics/compute-graph.png)
### Decide?
```
    好像是在做可判定性的判断
```
![decide-graph](./pics/decide-graph.png)
### IterateGraph
```
    迭代得到结果 (一个值)
```
![iterate-graph](./pics/iterate-graph.png)



