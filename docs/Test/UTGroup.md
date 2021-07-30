## 为什么单测要分组
UT 和 RD日常自测 基于Testng框架或Junit框架，所有的用例均适用@Test进行标记。

1. 根据UT书写规范，UT组件会对UT case会进行一系列的研判，评估case质量。而RD在开发时的自测case不需要该遵守UT书写规范。

2. UTcase的思路和自测case的思路不一致，为避免自测case被误认为是UTcase，影响UT组件执行效率*，需要对UTcase进行区分，UT组件只跑UT case，RD的自测用例只会在本地运行。


## 执行规范

RD 自测case 标记为“LocalCase”组

Junit/TestNg 中设置case分组步骤如下

### Junit
分版本 Junit4 和 Junit5

Junit4:
使用 Category 注解
1. 在test目录下，新增一个接口，比如命名为“LocalCaseTest”

2. 在 pom.xml 文件中更改插件配置

	(1) 在`maven-surefire-plugin`插件中，新增excludedGroups设置

	(2) 在设置中填入LocalCaseTest.class的路径
3. 自测 case 前加 @Category(LocalCaseTest.class)


Junit5：

使用 Tag 注解。

### TestNg



