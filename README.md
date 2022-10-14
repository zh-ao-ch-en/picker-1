# picker 

一个方便获取每日安全资讯的爬虫和推送程序。支持导入 opml 文件，因此也可以订阅其他任何 RSS 源。

基于github action 实现的自动化推送系统
## 使用
### 信息流
每日会在issue中生成昨日信息流

![img.png](img/信息流.png)

### 精选
如果认为某篇文章质量较好, 值得其他人阅读可以点击convert to issue 自动添加到精选文章列表. 

![img.png](img/精选.png)

或手动new issue, 也会自动添加到issue中

![img.png](img/手动添加.png)

每天下午13:30, 会将昨日的精选汇总进行一次推送.

### 标签

每日信息流会自动添加标签daily, 每日精选会自动添加标签dailypick. 精选文章会添加标签pick.

一些文章的细分领域可以通过手动添加不同的标签进行管理.

![img.png](img/标签.png)

已经给主要用户都添加了写权限, 可以自行创建标签.

### 评论

对精选文章的评论将会自动推送到钉钉群

### 订阅源

推荐订阅源：

- [CustomRSS](rss/CustomRSS.opml)

其他订阅源：

- [CyberSecurityRSS](https://github.com/zer0yu/CyberSecurityRSS)
- [Chinese-Security-RSS](https://github.com/zhengjim/Chinese-Security-RSS)
- [awesome-security-feed](https://github.com/mrtouch93/awesome-security-feed)
- [SecurityRSS](https://github.com/Han0nly/SecurityRSS)
- [安全技术公众号](https://github.com/ttttmr/wechat2rss)
- [SecWiki 安全聚合](https://www.sec-wiki.com/opml/index)
- [Hacking8 安全信息流](https://i.hacking8.com/)

非安全订阅源：

- [中文独立博客列表](https://github.com/timqian/chinese-independent-blogs)

**添加自定义订阅源**

1. 在 `config.json` 中添加本地或远程仓库：

```yaml
rss:
  CustomRSS:
    enabled: true
    filename: CustomRSS.opml
  CyberSecurityRSS:
    enabled: true
    url: >-
      https://raw.githubusercontent.com/zer0yu/CyberSecurityRSS/master/CyberSecurityRSS.opml
    filename: CyberSecurityRSS.opml
  CyberSecurityRSS-tiny:
    enabled: false
    url: 'https://raw.githubusercontent.com/zer0yu/CyberSecurityRSS/master/tiny.opml'
    filename: CyberSecurityRSS-tiny.opml
  Chinese-Security-RSS:
    enabled: true
    url: >-
      https://raw.githubusercontent.com/zhengjim/Chinese-Security-RSS/master/Chinese-Security-RSS.opml
    filename: Chinese-Security-RSS.opml
  awesome-security-feed:
    enabled: true
    url: >-
      https://raw.githubusercontent.com/mrtouch93/awesome-security-feed/main/security_feeds.opml
    filename: awesome-security-feed.opml
  SecurityRSS:
    enabled: true
    url: 'https://github.com/Han0nly/SecurityRSS/blob/master/SecureRss.opml'
    filename: SecureRss.opml
  wechatRSS:
    enabled: true
    url: 'https://wechat2rss.xlab.app/opml/sec.opml'
    filename: wechatRSS.opml
  chinese-independent-blogs:
    enabled: false
    url: >-
      https://raw.githubusercontent.com/timqian/chinese-independent-blogs/master/feed.opml
    filename: chinese-independent-blogs.opml
```

2.

自定义rss源位于`rss/CustomRSS.opml`中, 需要添加请提交pr, 次日自动加入到推送列表

非rss源可以使用rsshub转发
## 安装

```sh
$ git clone https://github.com/chainreactors/picker
$ cd picker && ./install.sh
```

## 运行

### 本地搭建

编辑配置文件 `config.json`，启用所需的订阅源和机器人（key 也可以通过环境变量传入），最好启用代理。

```sh
$ ./picker.py --help                            
usage: picker.py [-h] [--update] [--cron CRON] [--config CONFIG] [--test]
optional arguments:
  -h, --help       show this help message and exit
  --update         Update RSS config file
  --cron CRON      Execute scheduled tasks every day (eg:"11:00")
  --config CONFIG  Use specified config file
  --test           Test bot

# 单次任务
$ ./picker.py
```

### Github Actions

利用 Github Actions 提供的服务，你只需要 fork 本项目，在 Settings 中添加 secrets，即可完成部署。

目前支持的推送机器人及对应的 secrets：

- [钉钉群机器人](https://open.dingtalk.com/document/robots/custom-robot-access)：`DINGTALK_KEY`

目前有四个不同的推送信息类型

* 每日信息流,  默认为每天早上9点30分推送昨天新增文章列表.
* 每日精选, 默认为每天下午1点半推送昨日精选
* 精选推送, 在每天生成的issue中, 点击convert to issue生成新的issue并推送到钉钉群
* 精选文章的评论区推送, 当有人评论了改文章, 自动推送到钉钉

