## 单元测试颗粒度规范

单测颗粒度根据需要使用到的资源来确定。
+ 小型测试禁止访问数据库、外部服务、网络服务、sleep语句、流程控制语句等。

+ 小型测试的时间<=60ms

+ 中型测试可以访问数据库、外部服务。

+ 中型测试的时间<=300ms

+ 大型测试的时间<=900ms

> 访问数据库，可能影响数据库的数据！回滚是比较难做到的，特别是在分布式场景下。


## 单元测试书写规范

### 项目结构规范

测试代码位于 `src/main/java`，资源放在`src/test/resources `，被测代码位于 `src/main/java`。
> 被测代码和测试代码都处于同一个package

### 命名规范

测试类：使用xxxTest/xxxUnitTest的格式

测试方法：使用命名驼峰方式，例如testMethodA{action}的格式
> action 代表对于 methodA 的某种行为的测试，用来标识不同的测试用例。可注释说明测试的目的。

### Case 规范

行为驱动测试中，每个Case只包含被测方法的其中一种行为，比面向方法的测试更清晰，e.g.,
```java
//面向方法的单测

@Test
public void transferFunds(Long accountFrom, Long accountTo,Long money){}


//面向行为的单测。该方法的行为包括：
//1.成功的把钱从A账户转到B账户  2.因某些原因没能把钱从A转到B（比如A余额不足）
@Test
public void testTransferFundsMoveMoneyBetweenAccounts(){}
@Test
public void testTransferFundsMoveMoneyFailed(){}

```

### Case 代码结构
可按照 "Given" "When" "Then" 三段式结构来进行编写：
> 单测必须通过断言验证测试结果，仅把结果输出到控制台是不规范的。


```
@Test
public void testTransferFundsMoveMoneyBetweenAccounts()
{
  //Given,初始化，分别给两个账户设定原始资金 ¥150 ¥20
  Account accoount1 = new Account(rmb,150);
  Account accoount2 = new Account(rmb,20);
  
  //When,测试行为，转100块从A账户到B账户
  bank.transferFunds(account1,account2,100);
  
  //Then,验证结果，新的账户余额应该反映出转账结果
  assertThat(account1.getBalance()).isEqualTo(50);
  assertThat(account2.getBalance()).isEqualTo(120);
}
```

### 单测 Mock 规范

在编写小型测试、中型测试时，外部依赖需要使用Mock。ref: [Mock](/Test/Mock.md)

### 单测分组规范

应该区分 UTCase 和 RD 开发过程中的自测 Case。避免非 UTCase 被当作 UTCase 运行，影响UT测试效率，或因自测Case无断言被误判从而影响UT组件通过率。

ref: [Junit, TestNg分组规则](/Test/UTGroup.md)


### 单测修改规范

当系统行为或功能进行更改时，需要更改存量的单测，因为这样的成本更低。

### 单测增量规范

以需求为维度，对应的future分支提交时，需满足增量覆盖率要求。

+ 增量行覆盖率需>=65% 

+ 增量分支覆盖率>=55%

> 60%为“可接受”，75%为“值得称赞”，90%为“模范”。团队可选择符合其业务需求的覆盖率指标。


### ref

`Software Engineering at Google`






