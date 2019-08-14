# ltl to PA
## 方案一: 根据Buchi自动机设计PA的表示，使用HOA parser解析得到
        
## 方案二: 理解epmc中有关PA的定义及使用方法，复用代码
### PropertySolverExplicitCoalition.buildGame 用来解决什么属性
### PA相关的类
#### Automaton:interface
- getInitState
- getNumState
- numberToState
- numberToLabel
- getExpressions
- queryState(Value[] modelState, int automatonState)
- getIdentifier
- getBuechi
- getSuccessorState
- getSuccessorLabel
- getSuccessorState(int successorNumber)
- getSuccessorLabel(int successorNumber)
- isDeterministic
- close
- Automaton.Builder: interface
    - getIdentifier:String
    - setBuechi(Buechi buechi): Builder
    - setExpression(Expression expression, Expression[] expressions): Builder
    - setExpression(Expression expression):Builder
    - build():Automaton
#### AutomatonParity:interface
- getNumPriorities: int
- AutomatonParity.Builder: interface
    - build(): AutomatonParity
#### AutomatonSchewe implements AutomatonRabin, AutomatonParity, AutomatonSafra
- properties
    - useAutomatonMapsCache:boolean
    - automatonMaps:AutomatonMaps<AutomatonScheweState,AutomatonScheweLabeling> 
    - succState:AutomatonScheweState
    - succStateNumber:int
    - succLabel:AutomatonScheweLabeling
    - succLabelNumber:int
    - numLabel:int
    - buechiGraph:GraphExplicity
    - initState:AutomatonScheweState
    - nodeNumbers:Object2IntOpenCustomHashMap<int[]>
    - buechi:Buechi
    - expressions:Expression[]
    - parity:boolean
    - prioritiesSeen:BitSet
    - cache:BuechiSubsetCache<AutomatonScheweState,AutomatonScheweLabeling> 
- methods
    - AutomatonSchewe(Builder builder)
    - numberToState(int):AutomatonStateUtil
    - getInitState
    - queryState(Value[], int)
    - getSuccessorState()
    - getSuccessorLabel()
    - getNumPairs()
    - getBuechi():Buechi
    - getExpressions():Expression[]
    - numberToLabel(int):AutomatonLabelUtil
    - isParity()
    - getNumPriorities()
    - toString()
- AutomatonSchewe.Builder implements AutomatonParity.Builder
    - properties
        - buechi: Buechi
        - init: BitSet
        - parity: boolean
    - methods
        - setBuechi(Buechi buechi)
        - setInit(BitSet init)
        - setParity(boolean parity)
        - build(): AutomationSchewe
- AutomatonScheweParity implements AutomatonPairty, AutomatonSafra
    - IDENTIFIER="schewe-parity"
    - inner:AutomatonSchewe
    - AutomatonScheweParity(Builder builder)
        - new AutomatonSchewe.Builder
        - setParity(true).setBuechi().setInit()
        - inner = build()
