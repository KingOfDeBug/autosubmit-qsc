# autoSubmit
![Go](https://github.com/yzs981130/autoSubmit/workflows/Go/badge.svg?branch=master)

本项目通过读取环境变量中的学号和密码，依次执行:
- 登录portal获取`portalToken`
- 通过token登录portal获取cookie
- 带着cookie访问出入校报备，获取`simsoToken`
- 通过`simsoToken`登录simso系统，获取`sid`
- 带着`sid`访问出入校报备小程序并填报

## usage

**强烈建议使用自动执行**


### github actions 自动执行

配置文件在[go.yml](.github/workflows/go.yml)，目前配置为每天北京时间8点执行一次。

#### github actions 配置

- fork本项目
- 在自己的repo下Settings/Secrets中设置USERNAME和PASSWORD，分别为学号和密码
- 由于出入校报备变严，还需要设置REASON和TRACK，分别为出入校事由详细描述和行动路径
- 【可选】如果需要微信通知，可以配置FT_SCKEY，为[ftqq微信推送服务](http://sc.ftqq.com/?c=code)中的SCKEY
- fork的项目会默认关闭actions，需手动点击repo页的actions以enable


### local run

#### build

建议使用`go >= 1.13`

```shell script
git clone https://github.com/yzs981130/autoSubmit.git
cd autoSubmit
go build
```

#### run

##### 环境变量方法设置参数
- 环境变量`USERNAME`：学号
- 环境变量`PASSWORD`：密码
- 环境变量`REASON`：  出入校事由详细描述
- 环境变量`TRACK`：   行动路径
- 环境变量`FT_SCKEY`：SCKEY

```shell script
USERNAME=xxx PASSWORD=xxx REASON=xxx TRACK=xxx FT_SCKEY=xxxxxxxxx ./autosubmit
```
其中`FT_SCKEY`配置可选，配置会有

##### 命令行传参设置
```shell script
$ ./autosubmit -h
Usage of ./autosubmit:
  -password string
    	portal密码
  -reason string
    	出入校事由详细描述
  -reasonxx string
      出入校事由 (default "科研")
  -track string
    	出校行动轨迹
  -username string
    	学号
```

```shell script
./autosubmit -username=1900012345 -password=dashabi -reason "玩" -track "康博斯-CBD-公主楼"
```

注意环境变量只能传`USERNAME`和`PASSWORD`，其他参数需要用命令行参数传，如果同时设置`USERNAME`和`PASSWORD`的环境变量和参数，**命令行参数优先**

如果成功，会显示如下log：
```shell script
portal登录成功
simso登录成功
出校备案成功
微信通知发送成功
入校备案成功
微信通知发送成功
```
如果未配置`FT_SCKEY`则无`微信通知发送成功`字样，与上述log不同则可能失败，请在issue中反馈；后续也可能会添加debug信息和错误处理

## TODO

热烈欢迎pr！！


- [ ] error handling
- [ ] code structure refactor
- [ ] unit test