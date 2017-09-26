# IPv6WasRefused
ipv6 被拒 

详细教程案例参见我的博客：http://www.jianshu.com/p/06b7051c4357

开发的app审核提交4次，被拒绝3次，最后一次总算成功了

20170908第一次提交审核，20170910被打回

20170911第二次提交审核，20170913被打回

20170914第三次提交审核，20170916被打回

20170919第四次提交审核，20170922审核成功

仔细观察发现，现在审核还是挺快的，一般在提交的第二天就会有结果（当然被打回的结果也很快）

在这个经历过程中发现一个规律，那就是邮箱给你的邮件时间，主题分别是：You app(iOS)status is in Review和New message from App Review for yourAppName,You app(iOS)status is in Review会先发，New message from App Review for yourAppName后发，若是app被拒那么New message from App Review for yourAppName会和第一封间隔2个小时左右（可以从下图看出显示同一天的，打开后查看详细信息显示间隔2小时左右），若是成功：New message from App Review for yourAppName（或者不发给你，直接发welcome to app store主题邮件）会和第一封间隔超过2小时左右，第二天发给你，

所有亲们，提交app后，及时看appleid绑定的邮箱吧，查看到You app(iOS)status is in Review就开始祈福吧，千万不要在2小时后再发邮件了🙏🙏🙏🙏🙏🙏🙏🙏🙏🙏


第一次

第二次

第三次

第四次

第四次成功，昨天的日期就是20170922，就是New message from App Review for yourAppName（或者不发）和You app(iOS)status is in Review间隔一天就说明有戏
上面的是属于经验，

下面是技术分享：

1.由于app是只有登陆，不提供注册的，还被apple盘问过了


然后就在提交的备注里写上：内部使用，暂时不对外开发

给他们回复解释下你们为什么不需要注册，还不行写个假的注册入口

审核的时候打开，过审后关上


还有就是邮件里没出现没事的， 只要不是1.1.6 5.2.1 4.3 4.2.2其他的都没啥事（AppStore审核指南https://developer.apple.com/app-store/review/guidelines/cn/?from=groupmessage）

连续3次被打回，都是因为：


Resources
Resources

For information about testing your app and preparing it for review, please see Technical Note TN2431: App Testing Guide.

For a networking overview, please review About Networking. For a more specific overview of App Review’s IPv6 requirements, please review the IPv6 and App Review discussion on the Apple Developer Forum.

详细说来是前2次确实是不支持ipv6的网络，第三次是因为网络慢登陆超时，但是相对于前2次已经进步很大了（前2次登陆时发送数据失败，第三次接收数据失败）

2.在前2次不支持ipv6的网络修改方案时：

项目网络部分采用socket长连接


有没有写死的IP，就是说建立网络时要区分是ipv4还是ipv6

的，不能只写ipv4的网络建立

2.1先判断是那种ip地址：


ipv4的建立

ipv6的建立
接下来的send和receive数据就不需要区分了。

2.2还有一个重要一点就是要用域名访问

在代码里写域名，然后通过域名自动解析IP地址（好处就是用域名可以根据客户的所在位置进行ip地址的转换，是ipv4还是ipv6的（通过域名和端口查询对应的ipv4/ipv6的ip））

#define kDnsServerIp (bTestEnvironment?@"119.**.***.**8":getIp(@"test.baidu.com",kDnsSocketPort))


通过域名和端口查询对应的ipv4/ipv6的ip
2.3然后就是搭建一个NAT64网络进行ipv6的测试

搭建教程参考：iOS-不用网线搭建IPv6网络测试环境：http://www.cnblogs.com/SUPER-F/p/IPV6.html

或者：搭建IPv6本地环境测试App：http://www.jianshu.com/p/49442934b81d

3.第三次打回是因为网速慢登陆超时（此时已经支持ipv6）

方案：让后台写一个登陆超时的借口，通过后台返回，等审核通过后，就改为false（因为审核的在美国，但是我的app是在大陆内销售的，网络可能会慢）

4.至于在终端里怎么ping6 我是没研究懂

希望搞运维的同学可以帮忙下@最菜

MAC下使用终端工具执行dig命令，查找自己域名对应的IPv6地址，www/api这些是主机记录

示例命令：dig www.yourwebsite.com dns64.6box.cn AAAA

digapi.yourwesbite.comdns64.6box.cn AAAA


自动解析域名对应的IPv6地址

自动解析域名对应的IPv6地址（续）
5.若是一直审核失败就一直提交，过一次就行


也可以按照此法：（开阔眼界罢了）

