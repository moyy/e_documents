
# Cargo 私有库

## 1. 服务端

+ 参考文档：https://www.jianshu.com/p/4c8965a8b02e
+ 远程连接服务器 ssh root@192.168.35.16 vfr4bgt5
+ 关闭防火墙：systemctl stop firewalld
+ 启动服务：./target/debug/alexandrie -c alexandrie.toml

## 2. 客户端

### 2.1. 发布者

+ 访问 http://192.168.35.16:3000 注册用户
	- 注：密码 似乎只能 是 十六进制数
+ 进入 http://192.168.35.16:3000/account/manage ，点击「Create token」获得 token，记录下来供发布时使用；

### 2.2. 完整的发布命令

`注：` 一般不用这个，用的是最下面的 发布右键菜单；

``` toml
cargo publish --no-verify --manifest-path Cargo.toml --index=http://ser.yinengyun.com:10082/tech/crates-io.git –token 上面注册的token
```

### 2.2. 使用者

修改文件【没有则创建之】：C:\Users\用户名\.cargo\config.toml

``` toml
[registries]
yn = { index = "http://ser.yinengyun.com:10082/tech/crates-io.git" }
```

+ [可忽略] 登录 cargo login [TOKEN]
+ 项目中修改文件 Cargo.toml
	- 添加：test3 = { version = "0.1.0", registry = "yn" }
	- 注：yn 是上面config.toml 配置
+ 拉取 cargo b

### 2.3. 建立 发布的 右键菜单

+ 桌面建立文本文档命名格式：发布.bat 和 发布.reg 复制下面指令粘贴进入文档；
+ 运行"发布.reg"，将发布写入注册表
+ 之后，可以选中发布目录，右键 选择 “cargo发布” 即可自动发布（token 值自行更改）

1、"发布.bat"

``` bat

cd %1

cargo publish --no-verify --manifest-path Cargo.toml --

index=http://ser.yinengyun.com:10082/tech/crates-io.git --token 上面注册的token

@echo off

Pause
```

2、"发布.reg"

``` reg
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\Directory\shell\fastDel]
@="cargo 发布"
[HKEY_CLASSES_ROOT\Directory\shell\fastDel\command]
@="C:\\Users\\用户名\\Desktop\\发布.bat %1"
```