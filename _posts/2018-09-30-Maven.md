[toc]
### 一、Maven概述
> Maven是基于项目对象模型(POM project object model)，可以通过一小段描述信息（配置）来管理项目的构建，报告和文档的软件项目管理工具。  

Maven的核心功能便是合理叙述项目间的依赖关系，通俗点讲，就是通过pom.xml文件的配置获取jar包，而不用手动去添加jar包。所有用Maven管理的真实的项目都应该是分模块的，每个模块都对应着一个pom.xml。它们之间通过继承和聚合（也称作多模块，multi-module）相互关联。
### 二、Maven生命周期
> Maven基于构建生命周期的核心概念，这意味着构建和分发特定工件(项目)的过程是明确定义的。对于构建项目的人来说，只需要学习一小组命令来构建任何Maven项目，而POM将确保他们得到他们想要的结果。

有三种内置的构建生命周期:default、clean和site。default的生命周期处理您的项目部署，clean的生命周期处理项目清理工作，而site的生命周期处理你的项目所生成的文档。  
默认（default）的生命周期包括以下阶段（注意：这里是简化的阶段，用于生命周期阶段的完整列表，请参阅下方生命周期参考）：
- 验证（validate） - 验证项目是否正确，所有必要的信息可用
- 编译（compile） - 编译项目的源代码
- 测试（test） - 使用合适的单元测试框架测试编译的源代码。这些测试不应该要求代码被打包或部署
- 打包（package） - 采用编译的代码，并以其可分配格式（如JAR）进行打包
- 验证（verify） - 对集成测试的结果执行任何检查，以确保满足质量标准
- 安装（install） - 将软件包安装到本地存储库中，用作本地其他项目的依赖项
- 部署（deploy） - 在构建环境中完成，将最终的包复制到远程存储库以与其他开发人员和项目共享

这些生命周期阶段（以及此处未显示的其他生命周期阶段）依次执行，以完成默认生命周期。给定上述生命周期阶段，这意味着当使用默认生命周期时，Maven将首先验证项目，然后尝试编译源代码，运行这些源代码，打包二进制文件（例如jar），运行集成测试软件包，验证集成测试，将验证的软件包安装到本地存储库，然后将安装的软件包部署到远程存储库。
### 三、Maven标准工程结构
Maven的标准工程结构如下：
- -- pom.xml(maven的核心配置文件)
- -- src
- -- main
  - -- java(java源代码目录)
  - -- resources(资源文件目录)
- -- test
  - -- java(单元测试代码目录)
- -- target(输出目录，所有的输出物都存放在这个目录下)
- --classes(编译后的class文件存放处)
### 四、POM
> Maven最核心的就是对依赖jar包的管理，Maven要求每一个jar包都必须明确定义自己的坐标，Maven就是通过这个坐标来查找管理这些jar包的。

在Maven中，一个jar包的坐标是由它的groupId、artifactId、version这些元素来定义的。
- groupId：表明其所属组织或公司及其所属项目，命名规则为组织或公司域名反转加项目名称。
- artifactId：项目的模块名，通常与实际项目名称一致。模块的命名通常为项目名前缀加模块名。
- version：当前项目的版本号。
- packaging：定义项目的打包方式，可选值有jar、war、pom，默认为jar。

POM中dependencies元素包含了所有依赖的jar包，每一个jar包依赖使用dependency元素定义。
在声明一个jar包依赖时，除了指定groupId、artifactId、version这三项基本坐标外，还可以使用使用以下元素进行配置：
- scope元素：指定依赖的范围。
- exclusions元素：排除传递性依赖。

在maven多模块项目中，做统一版本管理的时候通常在父类pom文件中定义<dependencyManagement>节点，该节点类申明的<dependencies>节点内容都是关于统一定义的资源版本，子模块引用依赖的<dependencies>节点里面的资源无需再申明版本号
### 五、Maven常用命令
1. 编译源代码：mvn compile
2. 打包：mvn package
3. 在本地Repository中安装jar：mvn install
4. 清除产生的项目：mvn clean
5. 清除之前旧项目安装新项目并跳过单元测试：mvn clean install -DskipTests
6. 部署上传到服务器：mvn deploy
   1. mvn compile与mvn install、mvn deploy的区别：mvn compile，编译类文件；mvn install，包含mvn compile，mvn package，然后上传到本地仓库；mvn deploy,包含mvn install,然后，上传到私服