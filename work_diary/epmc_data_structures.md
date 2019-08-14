# 关键数据结构设计
## 模型 ModelPRISM
- Q1: ModelPRISMQMC 定义并没有在QMC求解过程中使用?
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
### 关键方法
- build(Builder) 根据builder初始化模型
    - 初始化 rateLabel=rateIdentifier=new ID... 
    - 初始化 player=builder.palyer... 
    - 初始化 formulas.constants by Options [TODO]
    - 初始化 unspecifiedConsts, specifiedConsts
    - 初始化 rewards [TODO]
    - 初始化 formulas 
    - formulas.check() 进行语义检查比如常量的循环依赖、常量是否被定义为常量
    - formulas.expand() 对formulas进行语义值替换
    - expandModules
        - ModuleCommands:module.asCommands().replaceFormulas(formulas)
        - MomduleRename:base.asCommands().rename...
        - formulas.expandConstants() 扩展formulas中的常量
        - collectConstants() 具体化unspecifiedConsts, specifiedConsts
        - module.replaceFormulas(specifiedConstants) 替换常量值
    - 初始化 semanticsType
    - 初始化 globalVairables, globalInitValues 
    - expandGlobalVariables() 根据formulas, specifiedConsts展开globalVariables
    - 初始化 system, system.setModel, checkSystemDefinition [TODO]
    - automataToCommands() check Modules是否全是ModuleCommands
    - flatten? 可配置 flatten SystemDefinition
    - 初始化 multipleInit, initialStates:Expression 
    - replaceRewardsConstants(formulas, specifiedConsts) 
    - EngineDD fixUnchangedVariables ???
    - createProperties [DOING]
        - properties 添加constant, formulas, label的定义
        - properties.expandAndCheckWithDefinedCheck()
            - checkCyclic()
            - expand()
            - checkNonConstantConst()
            - checkUndefinedConst()
- read(Object part, InputStream inputs...)
    - Questions
        - q1: what is the part?
        - q2: inputs只使用了第一个，其意义是什么?
    - parser = new PrismParser()
    - parser.parseModel()

## PrismParser
### 属性
- sdcount, modelcount initcount: int
- moduleNames, playerNames, rewardsNames, otherNames: List<String>
- expressionToken: Token
- model: ModelPRISM
- part: Object
    what is it?
### 关键方法
- parseModel
    - privParseModel
        - 定义构建模型所需要的变量
        - actualParser
            - model[0]=parseModelType: CTMC, CTMDP, DTMC, IMC, MA, MDP, NONDETERMINISTIC, PROBABILISTIC, PTA, STOCHASTIC, SMG
                return type:Semantics;
            - parseConstant(formulae): constant, prob, rate
                - prob or rate is equivalent to constant double
                - id is identifier, value is the Expression, type is the TYPE.
                - othersName.add(id.toString())
                - formulae.addConstant(id.toString(), value, type)
            - parseLabel(formulas): label
                - id is identifier, value is the Eepression
                - othersName.add(id.toString())
                - formulae.addLabel(id.toString(), value)
            - parseGlobal(globalVariables:Map<Expression, JANIType>, globalInitValues:Map<Expression, Expression>): global
                - parseVariableDeclaration(globalVariables, globalInitValues)
                    - id is the IdentifierExpression, initValue is the Expression, type is the TYPE.
                    - othersName.add(id.name)
                    - globalVariables.put(id, type)
                    - globalInitValues.put(id, initValue)
            - parseFormula(formulae): formula
                - id is the IdentifierExpression, value is the Expression
                - othersName.add(id.name)
                - formulae.addFormula(id.toString(), value)
            - parseModule(modules): module ID ...
            - parseRewards(rewards): rewards ID ...
            - parsePlayers(player): player ID ...
            - parseInit(): init 状态转移？
            - parseSystem(): system ID ...

## Formula关键
### 属性
    - formulas:Map<Expression,Expression>
    - constants:Map<Expression,Expression>
    - constantTypes:Map<Expression,JANIType>
    - labels:Map<Expression,Expression>
### 关键方法
    - check() 
        - checkCyclic() 常量定义是否有常量环依赖？？？
        - checkNonConstantConst() 检查常量定义是否可计算常量值
    - expandFormulas() 根据formulas的定义进行展开
        - expand(formulas)
            - expand(value, seen, formulas)
                对formulas, constants, labels的值进行计算并展开
    - expandConstants() 根据constants的非空定义进行展开
## PropertiesImpl
### 属性
    - properties:Map<RawProperty, Expression>
    - names:Set<String>
    - constants:Map<String, Expression>
    - constantTypes:Map<String, Type>
    - formulas:Map<String, Expression>
    - labels:Map<String, Expression>
    - model:ModelPRISM
### 关键方法
    - parseProperties(Object part, InputStream input)
        - property = UtilOptions.getInstance(OptionModelchecker.PROPERTY_INPUT_TYPE): create property by options
        - property.readProperties: get RawProperties
            - propertyPRISM.readProperties()
        - parseProperties(rawProperties)
            - UtilModelChecker.parseExpression(definition)
                - property = UtilOptions.getInstance(OptionModelchecker.PROPERTY_INPUT_TYPE): create property by options
                - property.parseExpression(stream)
                -
        - expand
    - parseProperties(Object part, RawProperties rawProperties)
## Semantics enum
- SemanticsQMC implements SemanticsDTMC, SemanticsDiscreteTime, SemanticsMarkovChain
