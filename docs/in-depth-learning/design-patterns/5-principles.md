# 设计原则

## 何为设计

* 即按照哪一种思路或者标准来实现功能
* 功能相同，可以有不同设计方案来实现
* 伴随着需求增加，设计的作用才能体现出来

## UNIX/LINUX 设计哲学

《UNIX/LINUX 设计哲学》这是一本书的名字，它里面讲了很多的设计准则。由于 UNIX/LINUX 是比较成功的操作系统，书的作者通过了解这两个操作系统，总结出了设计软件时应该遵循的 10 个设计准则。

* 准则 1: 小即是美
  * 一个系统尽量做得小而精，不要盲目追求大而全
* 准则 2: 让每个程序只做好一件事
  * 分割多个子程序，各自负责好自己的功能，让大程序成为多个子程序的集合
* 准则 3: 快速建立原型
  * 先搭建能满足用户最基本需求的原型，再根据用户或自己的需求去升级迭代
* 准则 4: 舍弃高效率而取可移植性
  * 代码的高效率相对于代码的可移植性和可复用性，优先级放低
  * 代码的效率问题会随着硬件产品的升级而被抹平
* 准则 5: 采用纯文本来存储数据
  * 纯文本比二进制可读性更高
  * 现在硬盘成本低，效率问题可以通过硬件的升级来解决
* 准则 6: 充分利用软件的杠杆效应（软件复用）
  * 写的东西尽可能抽象出来提高复用性，同准则 4
* 准则 7: 使用 shell 脚本来提高杠杆效应和可移植性
  * 同准则 4，这是一种具体的解决方案
* 准则 8: 避免强制性的用户界面
  * UNIX/LINUX 默认只有命令行，因为用户界面作为一个单独的软件而存在，不是一个操作系统的必备项
  * 对于操作系统而言，用户界面会占用很大的内存，用户界面的输入会带来安全问题，用户界面的操作没有命令行高效
  * 所以对于操作系统，应该避免强制绑定用户界面，把这个功能和系统本身分开
* 准则 9: 让每个程序都成为过滤器
  * 每个子程序都是独立的，只做好一件事：就像过滤器一样让数据进来，处理完再出去，结果进入另一个子程序
  * 举例：`ls | grep *.json`，管道符作为过滤器，ls 输出的结果通过过滤器过度到 grep 选择器的结果，实现了找到所有 .json 文件的功能

小准则：

* 允许用户定制环境
  * 环境不能限制死，要提供配置文件方便用户进行配置
* 尽量使操作系统内核小而轻量化
  * 内核与工具分离：内核只包含最核心的 API，其他的东西通过扩展或插件的形式来增加
* 使用小写字母并尽量简短
  * 这是 LINUX 命名规范（list -> ls）
* 沉默是金
  * 正例：如果只允许输出数字，遇到结果不是数字的场景，则输出 0 或什么都不输出
  * 举例：`ls | grep *.json | grep 'package'`，如果结果中没有关键词 package，就什么都不会输出
  * 反例：遇到结果不是数字的场景，输出 "这不是一个数字"，那么之后的统计规则就变了，会出现问题
  * 举例：`ls | grep *.json | grep 'package' | wc -l` 统计最终输出结果有几行，如果找不到关键词时输出了其它字符串，那么统计行数就会出错了
* 各部分之和大于整体
  * 以小的个体来组合成大的整体，遇到改变时就能解耦了
* 寻求 90% 的解决方案
  * 一个产品不可能满足所有客户的需求，遵循二八定律即可（花 20% 的成本解决 80% 的需求）

## 五大设计原则：SOLID

### S - 单一职责原则（single）

* 一个程序只做好一件事
* 如果功能过于复杂就拆分开，每个部分保持独立

### O - 开放封闭原则（open）

* 对扩展开放，对修改封闭
* 增加需求时，扩展新代码，而非修改已有代码
* 这是软件设计的终极目标

### L - 李氏置换原则

* 子类能覆盖父类
* 父类能出现的地方子类就能出现
* JS 中使用较少（弱类型 & 继承使用较少）

### I - 接口独立原则

* 保持接口的单一独立，避免出现「胖接口」
  * 胖接口：一个接口/函数做了所有事
  * 慎重使用，并非绝对不能使用，因为外观模式就违背了这一条原则
* JS 中没有接口（typescript 例外），使用较少
* 类似于单一职责原则，这里更关注接口

### D - 依赖导致原则

* 面向接口编程，依赖于抽象而不依赖于具体
* 使用方只关注接口而不关注具体类的实现
* JS 中使用较少（没有接口 & 弱类型）

在 JavaScript 编程中，受限于语言本身的特性，S O 体现较多；L I D 体现较少，但是要了解其用意。

（完）