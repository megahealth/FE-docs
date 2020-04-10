# 云引擎应用说明

[TOC]

## 目录结构

- [build]
- [public]
- [routes]
- [views]
- [ctrls] 血氧接口 => 周靖维
- [cpp]
- [api]
  - MSservice 开放平台
  - webApi 应用接口
  - [wave] 呼吸波文件
- [cloud] 云函数预留目录
- [hock] 钩子函数目录
  - beforeUpdateDevice.js 实时告警函数 => 臧腾飞
- app.js
- cloud.js 云函数当前文件 => 胡松
- package.json
- server.js
- README.md

## 云函数

### resetRepNum

用于定时任务，每月刷新hotel的reportNum字段为0

### newDevice

新建设备，给前端调用

### updateDeviceForWeb

更新设备信息，给前端调用

### addDailyReport

### updateDevice

### updateADevices

### updateBoundDevices

### newUpdateADevices

### updateDeviceInfo

### addOQC

### sendSMSToHotel

### boundBluetoothDevice

### getDevices

### unboundBluetoothDevice

### monitorDevice

*开启状态下的设备，其updatedAt时间设备端会每隔1min刷新一次，加上网络延迟什么的，至少会和当前网络时间不同，*

 理论上来讲当前时间若大于updatedAt时间1min，则设备肯定是断网了。

 而实际上，其实是最大要每隔2min才能判断设备是否真正的断网。（定时任务每隔1min监测一次）

 16/08/17 16:29:00  改为每隔5min监测一次是否网络异常。下线仍旧是1min监测一次。

### checkBoundDeviceStatus

戒指状态

### resetValidatePateintName

*云函数，用于定时任务，每日晚上00:00执行。将今日报告中对应的设备的今日用户信息替换.*

与resetIdpatient 合用，用于在今日报告中显示昨晚的用户信息。 在设备信息中显示实际登记的信息，且在完成过一次监测且上传了报告后，将登记用户信息清空

### resetIdpatient

云函数，用于定时任务。每分钟执行一次，监测是否有设备完成了监测，并收集了报告。若有，则将对应的idpatient清空，并更新validatePatientTime。

与resetValidatePateintName 合用，用于在今日报告中显示昨晚的用户信息。 在设备信息中显示实际登记的信息，且在完成过一次监测且上传了报告后，将登记用户信息清空

### ota

### otaShcDevice

### updateName_User

管理员身份修改其他用户昵称

### getUploadToken

### uptoken

返回上传uptoken

### downloadUrl

返回七牛图片的url地址

## 钩子函数

### afterSave('Reports')

*报告上传完成后处理(1挂载到patients表的idReport字段下，2生成报告编号idReport，3发送短信给对应教授/体检中心，4给hotel的reportNum字段自增1)*

无效报告或者ahi为-1的报告均不会发送通知短信。 如果baseorganizations的roletype为1也不发送短信。

### beforeSave('Reports')

仅仅作为hook函数唤醒用

### beforeUpdate('Device')

**功能**：Device表更新时进行拦截，get到更新内容，并查询相关数据判断是否达到告警阈值，若达到，则告警并记录。

**更新频率**：与Device表更新频率统一

**API请求**：GET  FIND CREATE

**功能分析**：

1. 拿到（GET）Device更新内容
2. 判断是否属于特定机构
3. 判断是否处于工作状态
4. 获取（FIND）完整设备信息、用户信息、告警配置
5. 判断是否开启特定告警（离床、体征等）
6. 判断是否满足触发特定告警的条件（离床、体征等）
7. 记录（CREATE）告警、短信告警、语音告警、更新告警配置（UPDATE）

**平安数据推送接口**

POST: http://47.107.189.213:8080/zg/deviceDetail

