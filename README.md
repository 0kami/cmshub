# cmshub - A Collection Of Open Source CMS Vulnerability
  cmshub是一个收集各类开源CMS的漏洞库，以及自己的一些感悟总结。

# 如何进行代码审计
  基础：php基础，常规漏洞基础，深入后的框架基础
  以下为我自己的一些总结，不保证都准确。

## 常规流程
  1. 阅读项目的执行流程，差不多到初始化结束即可
     了解什么位置做了安全处理，怎么做的安全处理
     了解common函数的位置
     这一步要做到熟悉整个运行流程，目录结构，以便后期快速定位
  2. 如果可以的话，在网上查找待审计项目的历史漏洞。看看表哥们是怎么审计的，以及厂商是怎么修补的。
     了解表哥们的审计过程，以及该漏洞的出现原因，可以在接下来的审计中多注意一下。
  3. 变量跟踪+危险函数查找
     从危险函数的角度，要先明确要审计的漏洞类型，根据这个漏洞的原理，去归纳整理规则，不过是用grep还是其他的方式，找到危险函数的位置，并向上回溯，观察是否可控
  4. 如果可以，制定一下审计整个项目的计划（好吧，其实我自己也没做到）

## 常见漏洞

### 重装漏洞
  1. 查看项目是如何判断是否已经安装（注意判断完了有没有die、exit）
  2. 如果存在该漏洞，注意配置文件写入的地方，写入一句话
无敏感函数，看逻辑

### SQL注入漏洞

起因：SQL语句拼接
  1. 查找SQL语句 或 mysql_query mysqli_query pdo (针对做预处理的情况，只查找预处理前拼接的情况)，一般项目里会有专门做数据库查询的类，可直接从这部分开始回溯
  2. 回溯可疑点拼接前是否经过安全处理，查看是否可绕过
    a. addslash 对数字型不起作用
    b. 转义单引号 看编码 宽字节注入
    c. 看处理逻辑是否可绕过，如果是软waf类似360wenscan，这种先找找网上绕过的方法。如果是自己写的处理，那就好好看代码吧
关注SQL语句，看看是否是拼接(这边回溯可能要很多，但是有可能回溯到的位置会很有意思，比如解析xml出来的注入)
mysql_query,mysqli_query,pdo

### 文件包含

起因：包含文件字段可控

  1. 查找include|require|include_once|require_once，查看包含文件字段是否可控




