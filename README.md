# Prometheus 实战

v0.1.0

在过去一年左右时间里，我们使用 Prometheus 完成了对几个机房的基础和业务监控，大大提高了服务质量以及 oncall 水平，在此特别感谢 Promethues 这样优秀的开源软件。

当初选择 Prometheus 并不是偶然，因为：

* Prometheus 是按照 Google SRE 运维之道的理念构建的，具有实用性和前瞻性。

* Prometheus 社区非常活跃，基本稳定在 1个月1个版本的迭代速度，从 2016 年 v1.01 开始接触使用以来，到目前发布的 v1.8.2 以及最新最新的 v2.1 ，你会发现 Prometheus 一直在进步、在优化。

* Go 语言开发，性能不错，安装部署简单，多平台部署兼容性好。

* 丰富的数据收集客户端，官方提供了各种常用 exporter。

* 丰富强大的查询能力。

Prometheus 作为监控后起之秀，虽然还有做的不够好的地方，但是不妨碍我们使用和喜爱它。根据我们长期的使用经验来看，它足以满足大多数场景需求，只不过对于新东西，往往需要花费更多力气才能发挥它的最大能力而已。

本书主要根据个人过去一年多的使用经验总结而成，内容主要包括 Prometheus 基本知识、进阶、实战以及常见问题列表等方面，希望对大家有所帮助。

本开源书籍既适用于具备基础 Linux 知识的运维初学者，也可供渴望理解 Prometheus 原理和实现细节的高级用户参考，同时也希望书中给出的实践案例在实际部署监控中对大家有所帮助。

你准备好了吗？接下来就让我们一起开始这段神奇旅行吧！

## 技术交流

欢迎加入 Prometheus 技术交流 QQ 群，分享 Prometheus 资源，交流 Prometheus 技术。

* QQ 群：465362780 (已满) 申请加入请备注：prometheus 实战
* 加入我们的知识星球，在这里你可以和国内最专业的 Prometheus 同行一起交流和学习：

  ![image_228845211141_3](https://user-images.githubusercontent.com/1459834/41642276-7101ce02-749a-11e8-8da0-6cf702af0870.JPG)

## 关于作者

* small_fish__

  * [微博](https://weibo.com/songjiayang1)
  * [github](https://github.com/songjiayang)
  * 个人公众号
  
  ![第二范式](https://git.io/vAQvJ)
 
- 薛锦

  * [微博](https://weibo.com/1660913012/profile?topnav=1&wvr=6)
  * [github](https://github.com/csxuejin)
  * 个人公众号

  ![GitHub Logo](https://songjiayang.gitbooks.io/go-basic-courses/content/pics/easy-hacking.jpg)
  
## GitHub 关注曲线

[![Stargazers over time](https://starcharts.herokuapp.com/songjiayang/prometheus_practice.svg)](https://starcharts.herokuapp.com/songjiayang/prometheus_practice)
  
