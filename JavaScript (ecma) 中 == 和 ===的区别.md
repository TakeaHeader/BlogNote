### JavaScript (ecma) 中 == 和 ===的区别
> 相关链接:
1. [11.9.1 The Equals Operator ( == )](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.1)
2. [11.9.3 The Abstract Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3)
3. [11.9.4 The Strict Equals Operator ( === )](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.4)
4. [11.9.6 The Strict Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.6)
5. [JavaScript 中应该用 \"==\" 还是 \"===\"？](https://www.zhihu.com/question/20348948/answer/14867031)
> 查看ECMA-262标准: 
1. === 被称为 Strict Equals Operator（严格相等运算符），如果存在 **a === b** 的表达式 ，ECMA-262解释的执行过程是:
 - 计算出表达式 a 的结果，
  - 并存入 lref 变量将 GetValue(lref) 的结果存入 lval 变量
   - 计算出表达式 b 的结果， 
   - 并存入 rref 变量将 GetValue(rref) 的结果存入 rval 变量 
   - 执行 Strict Equality Comparison 算法判断 rval === lval 并将结果直接返回  
   **具体执行过程在这里[11.9.6 The Strict Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.6)**
2. == 被称为 Abstract Equality Operator (抽象相等运算符)，如果存在 **a == b** 的表达式， ECMA-262解释的执行过程是: 
- 计算出表达式 a 的结果， 
- 并存入 lref 变量将 GetValue(lref) 的结果存入 lval 变量 
- 计算出表达式 b 的结果， 
- 并存入 rref 变量将 GetValue(rref) 的结果存入 rval 变量 
- 执行 Abstarct Equality Comparison 算法判断 rval === lval 并将结果直接返回  
**具体执行过程在这里[11.9.3 The Abstract Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3)**
> 例如:
```var a = null;var b = undefined;a == b```
这段代码执行结果为**true**,但是代码是如下:
```var a = null;var b = undefined;a === b```
执行结果却是false.再如这个例子:
```var a = \"23\";var b = 23;a == b```
结果同样是**true**
```var a = \"23\";var b = 23;a === b```
结果是**false**我的理解是： **==** 符号在运算的时候，会存在隐式转换，当两个类型不一致时,JS会尝试调整a和b的类型去比较，例如a为字符串，会尝试调整为数字去比较,返回结果true。但是**===**在类型不同的时候，会直接返回false.
