# 关键数据结构设计
## 模型 ModelPRISM
### 属性
- semanticsType:Semantics
- modules:List<Module>
- publicModules:List<Module>
- initialStates:Expression
- globalVariables:Map<Expression, JANIType>
- globalInitValues:Map<Expression, Expression>
- publicGlobalInitValues:Map<Expression, Expression>
- system:SystemDefinition
- formulas:Formulas
- properties:PropertiesImpl
- unspecifiedConsts:Set<Expression>
- specifiedConsts:Map<Expression, Expression>
- rewards:List<RewardStructure>
- players:List<PlayerDefinition>
- rateIdentifier:Expression
- rateLabel
### 方法
- build(Builder) 根据builder初始化模型
    - 初始化 rateLabel=rateIdentifier=new ID...
    - 初始化 player=builder.palyer...
    - 初始化 formulas.constants by Options
    - 初始化 unspecifiedConsts, specifiedConsts
    - 初始化 rewards
    - 初始化 formulas, formulas.check(), formulas.expand
    - expandModules
    - 初始化 semanticsType
    - 初始化 globalVairables, globalInitValues, expandGlobalVariables 
    - 初始化 system, system.setModel, checkSystemDefinition
    - automataToCommands
    - flatten? 可配置
    - 初始化 multipleInit, initialStates 
    - replaceRewardsConstants(formulas, specifiedConsts) 
    - EngineDD fixUnchangedVariables ???
    - createProperties
-
