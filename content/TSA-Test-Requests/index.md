---
title: "时间戳服务器检测要求"
date: 2023-08-29T16:21:14+08:00
draft: false
categories: [商用密码产品检测]
tags: []
card: false
weight: 0
---

> 标准依据：GM/T 0123 《时间戳服务器密码检测规范》

# 外观和结构的检查
+ 指示灯：电源指示灯、状态指示灯、故障指示灯或其他故障指示方式
+ 网络接口：至少两个网络接口（RJ45或光口）
+ 密码部件：经商用密码检测认证的密码部件或模块，如加密卡、安全芯片等


# 功能检测

## 初始化功能
+ 初始化操作主要包括：
  + 系统初始配置
  + 初始化管理员或操作员
  + 初始密钥生成（或恢复）与安装
+ 只有在初始化操作完成后才能提供密码服务

## 设备自检
+ 自检条件：上电/复位自检、周期自检、接收指令后的自检
+ 自检内容：算法正确性、随机数发生器、存储密钥和数据的完整性、关键部件的正确性
~~~ 
关键部件是否应包含时间同步部件？
~~~
+ 自检结束后应报告自检结果，自检失败应停止对外提供密码服务

## 密码运算
+ 至少支持SM2、SM3算法

## 密钥管理
+ 时间戳签名密钥对的生成应采用经商用密码检测认证的密码部件或模块
+ 时间戳签名私钥应安全存储在密码部件或模块内

## 随机数检测
+ 采用经检测认证的具有物理噪声源功能的两个及以上独立芯片
+ 自检符合GM/T 0062中E类产品要求

## 证书管理
+ 检测范围：对应用实体证书、根证书或证书链的导入、存储、验证、使用、删除以及备份和恢复等操作

## 时间戳服务
+ 通信方式至少支持电子邮件、文件、Socket、HTTP、SOAP中的一种
+ 反馈的时间格式应是UTC格式
> 根据 ISO 8601《数据存储和交换形式·信息交换·日期和时间的表示方法》，UTC时间，也就是国际统一时间/国际协调时，表示方法：
> YYYYMMDD T HHMMSS Z(或者时区标识)。
> 
> 例如，20100607T152000Z，表示2010年6月7号15点20分0秒，Z表示是标准时间。如果表示北京时间，那么就是：20100607T152000+08，其中 “+08” 表示东八区。

## 可信时间源
+ 源头：国家权威时间部门（如国家授时中心）或国家权威时间部门认可的硬件和方法
+ 方式：
  + 使用无线接收装置，如长波信号、卫星信号灯
  + 使用时间同步协议
  + 使用硬件获取时间，如原子钟
+ 时间戳服务器应能够自动同步时间，满足GB/T 20520-2006中6.3的要求
~~~
定期从可信时间源获取可信时间，再根据可信时间检查自身时间，与可信时间源保持时间同步

同步时间间隔不应长于30min

所有部件统一行动检查并同步时间

可信时间源第一个启动，确保开始颁发时间戳时，已经进行过时间同步

时间同步失败，停止接收时间戳申请和时间同步，向管理者发出警报并写入审计日志
~~~


# 管理安全

## 配置管理
+ 主要管理功能
  + 网络地址配置，至少包含：IP地址、子网掩码、网关地址
  + 状态管理：至少包含：部件状态、软件状态、版本状态、当前状态
  + 配置管理：至少包含：权限配置、访问控制配置
+ 权限配置
  + 不少于管理员、审计员两类角色
  + 管理员负责设备的证书管理、访问控制、可信时间源配置
  + 审计员负责设备的日志管理

## 管理员管理
+ 采用智能密码钥匙、智能IC卡能硬件装置与登录口令相结合，并使用证书进行身份验证

## 设备访问控制
+ 存储在设备内部的私钥，应持有正确的私钥授权码才能使用

## 设备日志记录
+ 提供日志记录、查看和导出功能
+ 对日志信息进行审计，防止日志内容被非法修改
+ 日志内容
  + 管理员操作行为：登录认证、系统配置、密钥管理
  + 异常事件：认证失败、非法访问
  + 应用接口调用