# MadStar 现场投票系统

## Requirements for Madstar Voting System: 
* 活动大概有500人到现场
* 比赛分很多轮进行 所以每次Matchup需要输入新的选手信息到Voting Program里
* 限制每个人只能投一票: 
* 投票的网页通过门票二维码交给观众
* 每个人的二维码不一样来确保只能投票一次

## 教学
### HTTP Request 参数获取
https://blog.csdn.net/m0_37068028/article/details/74612765

## 系统状态
1. 待机 
2. 采集投票
3. 显示投票结果

## 接口
### POST: 创建一个投票 **/createPoll**
将系统状态从待机切换到采集投票
参数:
``` javascript
{
 "candidates": ["选手名单"]
 "duration": Integer // 投票的采集阶段时长 (按秒计算)
 "maxChoice": Integer // 观众可以最多选择几个选项
}
```
可以在采集阶段重新开始投票，这样就会终止之前的投票

### GET: 开始刚刚创建的投票 **/startPoll**
有三种情况:
* 正常开启投票
* 重复开启投票 (还不清楚需不需要这个功能)
* 无法开始投票，因为还没有创建任何投票

### GET: 获取后端状态 **/getStatus** 

* 如果 currVote = undefined, 提示 "暂时没有投票" 
* 如果 currVote.status = ACTIVE_STATUS, 提示"投票还有多久结束以及当前投票"
* 如果 currVote.status = SLEEP_STATUS, 显示当前票数 (投票刚刚创建的时候，大家都是0票)

### POST: 给选手投票 **/vote**
投票条件:
* 当前投票为active
* 选手存在于当前Vote
* 观众通过了验证 (暂时先不做，有可能可以直接在前端验证)


工作清单:
- [ ] ```Vote Class``` @完成人名字
- [ ] ```/vote```
- [ ] ```/getStatus```
- [ ] ```/startPoll```
- [ ] ```/createPoll```

---
# 讨论草稿分割线




## 投票有哪些选项 可以一次投几个选手

启动接口  (只面对技术部成员 观众无法看到)
管理界面
创建一个投票组 
告诉程序哪些选手在对抗  (传数组?)
投票启动
Group里面有不同的Choice

投票的Life Cycle
关闭 aka.睡觉/睡眠
投票收集 aka.采集
开始新一轮投票
屏幕上显示采集时长是xxx秒 (倒计时)
允许在采集时开始新一轮投票 覆盖上一轮结果
结果 aka.结果
倒计时结束后状态转移到
重新回到睡眠状态(?)
再关闭 aka.

如何和用户交互
睡眠Phase: 欢迎界面
先登陆用户自己的Code
然后进入系统对应的Phase

采集Phase: 选项/倒计时
Optional: 实时显示投票结果 (长连接)
点击选项时触发Vote接口
选项名(选手)
除了采集Phase都会被禁用
每Call一次留下cookie做标记
显示门票上的Code
后端服务器每次收到投票时检查设备上有没有之前的Code
投票后按键会被禁用
结果Phase: 显示结果
投完票回到什么界面?
获取状态接口
根据目前状态前端决定显示什么界面
拿到投票选项和结果后分别显示选项和结果?
决定是在大屏幕还是在用户设备界面上显示

管理员界面看起来是什么样?

多个设备同时访问服务器---> @Galvin

## Plan B: 微信投票/Google Form
