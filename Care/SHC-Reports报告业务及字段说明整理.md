[TOC]
## 报告业务逻辑

### 统一业务逻辑

1. 数据展示
   - 头部
     - 表头（医院+报告名称）
     - 记录时间
     - 设备编号
     - 床号
     - 用户类型
   - 用户信息表
   - 数据摘要表
     - AHI+示例图
   - 睡眠时间统计表
     - 记录开始结束时刻，睡眠时长，睡眠效率
   - 呼吸事件表（9个）
   - 睡眠分期统计
     - 饼图
     - 统计表
   - 血氧表
   - 脉率表
   - 筛查或诊断（医院）意见
   - 血氧图
   - 脉率图
   - 呼吸事件图
   - 睡眠分期图
2. 数据编辑
   - 补充+编辑用户信息
3. 事件
   - 初始化
     - 获取报告id（路由），设备号（session）
     - LC获取报告数据
     - 设置表头信息
     - 获取ModifiedReport表已修改的报告信息（编辑过的表头，血氧表数据等）
     - 处理表格数据
     - 渲染睡眠分期数据饼图，分期图，呼吸事件图
     - 判断是否有血氧数据，及血氧数据版本
     - 拉取呼吸波文件，渲染呼吸波
   - 打印
     - 第二页后只显示用户名和报告编号
   - 血氧裁剪（v>247534）
     - 先编辑，调整滚动条，裁剪，如需重置则点击恢复
     - 裁剪后，根据截取时间点显示数据
     - 重新计算（调用parseSpoAndSleep.js）
   - 保存用户信息（patientInfo字段）
   - 选择呼吸波节点，展示改节点详情，翻页，涉及到数据拼接，抽稀等方法（血氧心率呼吸事件呼吸波详情图）
   - 保存日志

### 照护版业务逻辑

1. 数据展示
   - 数据摘要（离床次数、最高最低呼吸心率）
   - 24H生命体征图
   - 体动占比图
   - 体动幅度图
2. 医师签名、配合群组实现二级审核功能
3. 事件
   - 报告带水印打印，调整样式
   - 数据编辑、保存
   - 建议修改、保存
   - 科室床号等修改、保存
   - 修改最低血氧、最小脉率，重新画图
   - 修改AHI，生成新的建议模板

### 医疗版业务逻辑

1. 数据展示
   - 呼吸波趋势详情图
   - 呼吸波趋势图
   - 鼾声数据表
   - 鼾声趋势图

## 报告字段说明

### 报告数据JSON对象示例

``` json
{
  'remSleepScore': 10,
  'boundDevices': [
    {
      'hwVersion': '3.0',
      'btVersion': '2.9',
      'swVersion': '2.0.7964',
      'powerStatus': -1,
      'type': 0,
      'battery': -1,
      'connectStatus': 0,
      'monitor': -1,
      'deviceType': 'MegaRing',
      'sn': 'P11D71903000047',
      'mac': 'BC:E5:9F:48:9B:92'
    }
  ],
  'lastOffBedTimeMinute': 438,
  'startSleepTime': 1560264129756,
  'algorithmStartTime': 1560266919000,
  'ACL': {
    '*': {
      'read': true,
      'write': true
    }
  },
  'ODNum': 0,
  'hasEdited': true,
  'endStatusTimeMinute': 438,
  'startStatusTimeMinute': 47,
  'meanSAO2': 0,
  'totalSleepScore': 35,
  'failSleepScore': 10,
  'editedData': {
    'spo2Min': 93.48,
    'maxDuration': '03:23',
    'spo2Less90TimePercent': '0.00',
    'prAvg': 57,
    'validInTotal': 0,
    'sleepEfficiency': '90.3',
    'mixBreathNum': 0,
    'diffThdLge3Pr': '0.4',
    'totalBreathNum': 2,
    'totalDuration': 0,
    'breathMax': 18,
    'ahi': '0.3',
    'spo2Less80TimePercent': '0.00',
    'reviewDate': '2019-06-12',
    'spo2Less85TimePercent': '0.00',
    'breathMin': 10,
    'spo2Less80Time': 0,
    'secEnd': '06:00',
    'heartMin': 51,
    'recordDay': '2019-06-12',
    'reviewDoctor': '古春伟',
    'getUp': '06:00',
    'spo2Avg': 97.42,
    'totalRecordTime': '6小时16分钟',
    'spo2Less95TimePercent': '0.00',
    'eventCnt': 2,
    'leaveTimes': 1,
    'prMax': 92,
    'reportDoctor': '李欣成',
    'max': 34,
    'heartMax': 101,
    'spo2Less95Time': 1,
    'spo2Less90Time': 0,
    'fallAsleep': '23:44',
    'average': 23,
    'secStart': '22:42',
    'diffThdLge3Cnts': 2,
    'centralBreathNum': 0,
    'ahiAdvice': '1.保持良好的生活习惯；\n2.适当运动，增强体质；\n3.定期进行睡眠监测；',
    'spo2Less85Time': 0,
    'prMin': 44
  },
  'start': 190611232839,
  'BMPRList': [
    [
      0,
      0
    ],
    [
      0,
      0
    ]
  ],
  'tempSleepId': 'temp201906112242icb62305c49e74a7da3104d6fab980fb0',
  'idReport': 'ZZ1906_0733',
  'ringDataVer': '6',
  'remoteDevice': {
    'deviceSN': 'J01A181100014E',
    'modeType': 0,
    'romVersion': '3.1.8',
    'setTimezone': 800,
    'versionNO': '2.4.8098'
  },
  'TS80': 0,
  'lastInBedTimeMinute': 31,
  'customInfo': {
    'name': '公培艳',
    'gender': '女',
    'age': '30',
    'height': '166',
    'bmi': '20.69',
    'weight': '57',
    'roomNum1': '山亭区人民医院'
  },
  'ahiScore': 10,
  'wakeSleepScore': 9,
  'TS85': 0,
  'ringOriginalData': '',
  'eventCnt': 2,
  'hasReview': true,
  'ODI': 0,
  'deepSleepScore': 10,
  'status': '7',
  'roomNumber': '12',
  'DlowestSAO2': 0,
  'ringData': '',
  'extraCheckTimeMinute': 438,
  'validEndTimeMinute': 0,
  'bodyMoveList': [
    [
      19.022005,
      2.2870653,
      0
    ],
    [
      7.488226,
      1.5805852,
      0
    ]
  ],
  'TS90': 0,
  'order': 29212,
  'idPatient': {
    '__type': 'Pointer',
    'className': 'Patients',
    'objectId': '5cee3164eaa3750068a45df3'
  },
  'lowestSAO2': 0,
  'abnormalStatistics': {
    'brMax': [],
    'brMin': [],
    'hrMax': [],
    'hrMin': [],
    'laMax': [
      {
        'occurrenceTime': '2019-06-11T18:58:09.756Z',
        'occurrenceData': 2,
        'occurrenceType': 'LAMax'
      }
    ]
  },
  'end': 23520,
  'breathList': [
    [
      4825,
      31,
      101651,
      102309,
      2
    ],
    [
      5015,
      12,
      104228,
      104501,
      2
    ],
  ],
  'visible': '1',
  'idDevice': {
    '__type': 'Pointer',
    'className': 'Device',
    'objectId': '5c92fef10237d700686c7933'
  },
  'endSleepTime': 1560291270727,
  'sleepData': [
    0,
    0,
    0,
    0
  ],
  'startSleepMode': 'Auto',
  'leaveBedTimes': 1,
  'AHI': 0.3409091,
  'lightSleepScore': 3,
  'idBaseOrg': {
    '__type': 'Pointer',
    'className': 'BaseOrganizations',
    'objectId': '5bf61ec79f545400675b205f'
  },
  'fallSleepTimeMinute': 62,
  'objectId': '5d002473c8959c0069b66cbf',
  'createdAt': '2019-06-11T22:00:19.602Z',
  'updatedAt': '2019-06-12T03:11:13.039Z'
}
```

 ### 报告字段说明

#### 1.主要字段 

| 参数名                | 类型    | 说明                             |
| :-------------------- | :------ | -------------------------------- |
| objectId              | string  | 报告ID                           |
| idReport              | string  | 报告机构ID                       |
| idDevice              | pointer | 指向设备信息表                   |
| idBaseOrg             | object  | 指向机构信息表                   |
| AHI                   | number  | 呼吸暂停和低通气指数，1位小数    |
| eventCnt              | number  | 阻塞及低通气次数                 |
| startSleepTime        | number  | 睡眠相关时间（时间戳），参考备注 |
| lastInBedTimeMinute   | number  | 睡眠相关时间（分钟），参考备注   |
| startStatusTimeMinute | number  | 睡眠相关时间（分钟），参考备注   |
| fallSleepTimeMinute   | number  | 睡眠相关时间（分钟），参考备注   |
| endStatusTimeMinute   | number  | 睡眠相关时间（分钟），参考备注   |
| lastOffBedTimeMinute  | number  | 睡眠相关时间（分钟），参考备注   |
| extraCheckTimeMinute  | number  | 睡眠相关时间（分钟），参考备注   |
| breathList            | array   | 呼吸事件数据，参考备注           |
| sleepData             | array   | 睡眠分期数据，参考备注           |
| boundDevices          | array   | 戒指信息                         |
| ringDatData           | string  | 之前的戒指数据，废弃             |
| ringDataVer           | string  | 戒指数据版本                     |
| ringDataStatus        | number  |                                  |
| 【ringData】          | string  | 戒指数据                         |
| remoteDevice          | object  | 设备信息                         |
| 【ringOriginalData】  | string  | 旧版戒指数据                     |
| fileId                | pointer | 算法bin                          |
| createdAt             | date    | 报告生成时间                     |
| updatedAt             | data    | 报告更新时间                     |

 #### 2. 照护版使用字段 

| 参数名             | 类型    | 说明                     |
| :----------------- | :------ | ------------------------ |
| idPatient          | pointer | 指向用户信息表           |
| hasEdited          | boolean | 是否编辑                 |
| hasReview          | boolean | 是否审核                 |
| roomNumber         | string  | 房间号                   |
| editedData         | object  | 已修改内容               |
| abnormalStatistics | object  | 统计报告离床等告警       |
| leaveBedTimes      | number  | 离床次数                 |
| customInfo         | object  | 已编辑用户信息           |
| bodyMoveList       | array   | 体动数据，参考备注       |
| leaveBedTimes      | number  | 离床次数                 |
| BMPRList           | array   | 呼吸率心率数据，参考备注 |

#### 3. 医疗版使用字段 

| 参数名            | 类型    | 说明           |
| :---------------- | :------ | -------------- |
| idModifiledReport | pointer | 已修改字段     |
| waveFiled         | pointer | 呼吸波文件     |
| startSnoreTime    | number  | 鼾声开始时间             |
| endSnoreDuration | number | 鼾声总时间，秒 |
| snoreList         | array   | 鼾声数组，参考备注  |
| somniloquyList | array | 梦话记录，参考备注 |
| noiseList | array | 噪声记录，参考备注 |
| patientInfo       | array   | 已修改用户信息 |
| advice            | string  | 已修改建议     |

```json
{
    objectId: "",
    // 设备信息
    remoteDevice: {
        deviceSN: "", 
        modeType: 0, 
        romVersion: "", 
        setTimezone: 800, 
        versionNO: ""
    },
    boundDevices: [{}],
    // 保存编辑信息
    patientInfo: null,
    idModifiledReport: null,
    advice: null,
    // 关键数据
    AHI: 0,
    eventCnt: 0,
    sleepData: [],
    breathList: [],
    // 睡眠相关时间
    startSleepTime: 0,
    startStatusTimeMinute: 0,
    endStatusTimeMinute: 0,
    extraCheckTimeMinute: 0,
    // 血氧等戒指数据
    ringData: "",
    ringDataVer: "",
    ringOriginalData: "",
    // 鼾声数据
    snoreList: null,
    noiseList: null,
    somniloquyList: null,
    startSnoreTime: null,
    endSnoreDuration: null,
    // 呼吸波数据
    waveFiled: null,
    // 报告生成时间
    createdAt: "",
    updatedAt: "",
}
```



#### 4. 其他字段

| 参数名             | 类型    | 说明               |
| :----------------- | :------ | ------------------ |
| ACL                | object  | LC读写权限         |
| startSleepMode     | string  | 开启方式（手动，自动）                |
| tempSleepId        | string  | 本地生成的报告id                 |
| start              | number  | 开始时间（已废弃） |
| end                | number  | 结束时间（已废弃） |
| endSleepTime       | number  | 结束时间（已废弃） |
| algorithmStartTime | number  | 开始时间（已废弃） |
| ODNum              | number  | 氧减时间个数                   |
| ODI                | number  | 氧减个数/时                   |
| meanSAO2           | number  | 平均血氧                   |
| DlowestSAO2        | number  |                    |
| lowestSAO2         | number  | 最低血氧                   |
| TS80               | number  | 血氧小于80的时间百分比                   |
| TS85               | number  | 血氧小于85的时间百分比                   |
| TS90               | number  | 血氧小于90的时间百分比                    |
| ahiScore           | number  | 废弃 |
| wakeSleepScore     | number  | 废弃 |
| wakeSleepTime      | number  |  老的睡眠分期              |
| lightSleepScore    | number  | 废弃 |
| lightSleepTime     | number  |  老的睡眠分期                    |
| remSleepScore      | number  | 废弃 |
| remSleepTime       | number  |  老的睡眠分期                  |
| deepSleepScore     | number  | 废弃 |
| deepSleepTime      | number  |  老的睡眠分期                  |
| totalSleepScore    | number  | 废弃 |
| status             | string  |  报告状态                  |
| validEndTimeMinute | number  | 有效结束时间到实际结束时间偏移（BOE、梦加使用） |
| order              | number  | 废弃 |
| visible            | string  |   废弃                 |
| num                | number  | 废弃 |
| time               | array   | 废弃 |
| modifiedStart      | number  |                    |
| score              | number  | 废弃 |
| medicalNo | number |  |

 #### 5. ringData解析字段

| 参数名                | 类型   | 说明                             |
| :-------------------- | :----- | -------------------------------- |
| ringData              | object | 戒指数据                         |
| BEMeanlen             | number | 平均暂停和低通气时间(秒)         |
| BEMaxlen              | number | 最长暂停和低通气时间(秒)         |
| BEMaxlentime          | number | 最长暂停和低通气时间(秒)发生时间 |
| BECnt                 | number | 总呼吸事件数(次)                 |
| BETotalTime           | number | 总呼吸事件时间(分钟)             |
| BETotalrate           | number | 占总记录时间(%)，2位小数         |
| BEOHCnt               | number | 阻塞及低通气事件次数             |
| BECCnt                | number | 中枢性呼吸事件次数               |
| BEMCnt                | number | 混合性呼吸事件次数               |
| Waketime              | number | 清醒期时间(分钟)                 |
| REMtime               | number | 眼动期时间(分钟)                 |
| LightSleeptime        | number | 浅睡期时间(分钟)                 |
| DeepSleeptime         | number | 深睡期时间(分钟)                 |
| Wakerate              | number | 清醒期占比(%)，2位小数           |
| REMrate               | number | 眼动期占比(%)，2位小数           |
| LightSleeprate        | number | 浅睡期占比(%)，2位小数           |
| DeepSleeprate         | number | 深睡期占比(%)，2位小数           |
| duration              | number | 总时长                           |
| startpos              | number | 截取开始，未使用字段             |
| endpos                | number | 截取结束，未使用字段             |
| handonTotalTime       | number | 佩戴时长                         |
| timeStart             | number | 开始时间                         |
| Spo2Avg               | number | 平均血氧                         |
| Spo2Min               | number | 最低血氧                         |
| maxSpo2DownTime       | number | 最长氧减时间                     |
| prAvg                 | number | 平均脉率                         |
| prMax                 | number | 最大脉率                         |
| prMin                 | number | 最小脉率                         |
| diffThdLge3Cnts       | number | 氧减次数                         |
| diffThdLge3Pr         | number | 氧减指数，1位小数                |
| spo2Less95Time        | number | 血氧值低于95%的时间              |
| spo2Less90Time        | number | 血氧值低于90%的时间              |
| spo2Less85Time        | number | 血氧值低于85%的时间              |
| spo2Less80Time        | number | 血氧值低于80%的时间              |
| spo2Less70Time        | number | 血氧值低于70%的时间              |
| spo2Less60Time        | number | 血氧值低于60%的时间              |
| spo2Less95TimePercent | number | 血氧值低于95%的比例，2位小数     |
| spo2Less90TimePercent | number | 血氧值低于90%的比例，2位小数     |
| spo2Less85TimePercent | number | 血氧值低于85%的比例，2位小数     |
| spo2Less80TimePercent | number | 血氧值低于80%的比例，2位小数     |
| spo2Less70TimePercent | number | 血氧值低于70%的比例，2位小数     |
| spo2Less60TimePercent | number | 血氧值低于60%的比例，2位小数     |
| handOffArrlen         | number | 离手时长                         |
| Spo2Arr               | array  | 血氧数据                         |
| prArr                 | array  | 脉率数据                         |
| handOffArr            | array  | 离手数据                         |
| SAO2EventVect         | number | 未使用字段                       |

#### 6. ringData / ringOriginalData解析结果

```json
AHI: 4.29

BECCnt: 1
BECnt: 31
BEMCnt: 0
BEMaxlen: 53
BEMaxlentime: 1560806918
BEMeanlen: 20
BEOHCnt: 30
BETotalTime: 10
BETotalrate: 2.01
BreathEventVect: [Array(5),...]
                  
Wakerate: 12.68
Waketime: 63
DeepSleeprate: 31.39
DeepSleeptime: 156
LightSleeprate: 29.38
LightSleeptime: 146
REMrate: 26.56
REMtime: 132
                  
SAO2EventVect: [Array(2), ...] // 未使用

Spo2Arr: [98, 98, …]/// spoArr
Spo2Avg: 96.9///
Spo2Min: 89///

diffThdLge3Cnts: 46 /// 氧减次数
diffThdLge3Pr: 6.36 /// 氧减指数
duration: 38785 /// 总时长
startpos: 0
endpos: 38785 // 截取结束
handOffArr: (4) [36472, 36845, 38222, 38573]///
handOffArrlen: 4 // 离手时长
handonTotalTime: 38061 
maxSpo2DownTime: 48 /// 最长氧减时间

prArr: [68, 68, …]///
prAvg: 62///
prMax: 91///
prMin: 52///

spo2Less60Time: 0///
spo2Less60TimePercent: 0///
spo2Less70Time: 0///
spo2Less70TimePercent: 0///
spo2Less80Time: 0///
spo2Less80TimePercent: 0///
spo2Less85Time: 0///
spo2Less85TimePercent: 0///
spo2Less90Time: 11///
spo2Less90TimePercent: 0.04///
spo2Less95Time: 4643///
spo2Less95TimePercent: 17.83///

timeStart: 1560784992///
```

```json
diffThdLge3Cnts: 55
diffThdLge3Pr: 6.38236141204834
duration: 38786
handOffArr: (6) [29759, 34134, 34406, 34657, 35017, 35459]
maxSpo2DownTime: 0
prArr: [73, 73, …]
prAvg: 63
prMax: 106
prMin: 46
spo2Avg: 9692
spo2Less60Time: 0
spo2Less60TimePercent: 0
spo2Less70Time: 0
spo2Less70TimePercent: 0
spo2Less80Time: 0
spo2Less80TimePercent: 0
spo2Less85Time: 0
spo2Less85TimePercent: 0
spo2Less90Time: 2
spo2Less90TimePercent: 0.00006595435843337327
spo2Less95Time: 5226
spo2Less95TimePercent: 0.17233873903751373
spo2Min: 8993
spoArr: [9800,  …]
stageArr: [0, 0,  …]//---
timeStart: 1560784992
timeTotal: 0//---
```

| ringData | ringOriginalData |
| -------- | ---------------- |
| AHI: 4.29||
| BECCnt: 1||
| BECnt: 31||
| BEMCnt: 0||
| BEMaxlen: 53||
| BEMaxlentime: 1560806918||
| BEMeanlen: 20||
| BEOHCnt: 30||
| BETotalTime: 10||
| BETotalrate: 2.01||
| BreathEventVect: [Array(5),...]||
| Wakerate: 12.68||
| Waketime: 63||
| DeepSleeprate: 31.39||
| DeepSleeptime: 156||
| LightSleeprate: 29.38||
| LightSleeptime: 146||
| REMrate: 26.56||
|REMtime: 132||
| SAO2EventVect: [Array(2), ...] ||
| Spo2Arr: [98, 98, …] |✅ spoArr|
| Spo2Avg: 96.9 |✅|
| Spo2Min: 89 |✅|
| diffThdLge3Cnts: 46 | ✅ |
| diffThdLge3Pr: 6.36 | ✅ |
| duration: 38785 | ✅ |
| startpos: 0||
| endpos: 38785 ||
| handOffArr: (4) [36472, 36845, 38222, 38573] |✅|
| handOffArrlen: 4 ||
| handonTotalTime: 38061 ||
| maxSpo2DownTime: 48 |✅|
| prArr: [68, 68, …] |✅|
| prAvg: 62 |✅|
| prMax: 91 |✅|
| prMin: 52 |✅|
| spo2Less60Time: 0 |✅|
| spo2Less60TimePercent: 0 |✅|
| spo2Less70Time: 0 |✅|
| spo2Less70TimePercent: 0 |✅|
| spo2Less80Time: 0 |✅|
| spo2Less80TimePercent: 0 |✅|
| spo2Less85Time: 0 |✅|
| spo2Less85TimePercent: 0 |✅|
| spo2Less90Time: 11 |✅|
| spo2Less90TimePercent: 0.04 |✅|
| spo2Less95Time: 4643 |✅|
| spo2Less95TimePercent: 17.83 |✅|
| timeStart: 1560784992 |✅|
|          | timeTotal |
|          | stageArr |

### 数据使用说明 

#### 1. AHI分级标准

- =-1：0，无效
- <5：1，正常
- \>=5 && <15：2，轻度
- \>=15 && <30：3，中度
- \>=30：4，重度

* * *

#### 2. 监测各阶段起止时间定义

| 实际开启时间        | 初次到床时间            | 睡眠分期开始时间          | 入睡时间                | 睡眠分期结束时间        | 最后在床时间             | 实际结束时间             |
| ------------------- | ----------------------- | ------------------------- | ----------------------- | ----------------------- | ------------------------ | ------------------------ |
| startSleepTime(sst) | sst+lastInBedTimeMinute | sst+startStatusTimeMinute | sst+fallSleepTimeMinute | sst+endStatusTimeMinute | sst+lastOffBedTimeMinute | sst+extraCheckTimeMinute |

- 入睡时间：(fallSleepTimeMinute == -1) ? 实际开启时间 : 入睡时间
- 起床时间：(lastOffBedTimeMinute == -1) ? 实际结束时间 : 最后在床时间
- 入睡时长：入睡时间 - 分期开始
- 睡眠时长：起床 - 入睡
- 分期时长：分期结束 - 分期开始

* * *

#### 3. 睡眠分期数据

- sleepData：睡眠分期数组
    - value：0，清醒期
    - value：2，眼动期
    - value：3，浅睡期
    - value：4，深睡期
    - value：6，离床
- 开始时间：睡眠分期开始时间
- 间隔：1分钟

* * *

#### 4. 呼吸事件数据

- breathList：呼吸事件二维数组
    - breathList [i][0]：呼吸事件发生时间，相对于睡眠分期开始时间的秒偏移，单位：秒（s）
    - breathList [i][1]：呼吸事件持续时长，单位：秒（s）
    - breathList [i][2]：采样开始帧
    - breathList [i][3]：采样结束帧
- 开始时间：睡眠分期开始时间
- 间隔：不定

* * *

#### 5. 体动占比/体动幅度

- bodyMoveList：体动数据
    - bodyMoveList [i] [0]：体动时长，单位：秒（s）
    - bodyMoveList [i] [1]：体动幅度（1-3）
    - bodyMoveList [i] [2]：离床
- 开始时间：设备实际开启时间
- 间隔：1分钟

* * *

#### 6. 呼吸率心率趋势

- BMPRList：呼吸率心率数据
    - BMPRList [i] [0]：呼吸率
    - BMPRList [i] [1]：心率
- 开始时间：根据设备实际结束时间和数据长度进行倒推
- 间隔：10秒

* * *

#### 7. 血氧脉率趋势
- spoArr：血氧数组
- prArr：脉率数组 
- 开始时间：timeStart
- 数据间隔：1秒

#### 8. 呼吸波数据

- waveFiled：呼吸波文件 zip =>txt => JSON
- 开始时间--结束时间 与 睡眠分期 对齐：
  - 开始时间 == **startStatusTimeMinute**；
  - 结束时间 == **endStatusTimeMinute**；
- 一分钟的呼吸波数据为一个float数组，数组长度在 1200 左右，长度不定；
- 相邻呼吸波数组间的时间偏移为： 1 分钟 。

```javascript
/* 将呼吸波文件转化为JSON对象
*@method turnWaveFileToJson
*@param {waveFileId} 呼吸波文件
*@return {waveObj} 呼吸波数据对象
*/
function turnWaveFileToJson({
    waveFileId: waveFileId
}) {
    var waveFileUrl = waveFileId? waveFileId.url: null;
    var promise = new JSZip.external.Promise(function (resolve, reject) {
        JSZipUtils.getBinaryContent(waveFileUrl, function(err, data) {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
    promise.then(JSZip.loadAsync)                    
    .then( zip=> {
        var obj = zip.files;
        var name = '';
        for (var prop in obj) {
            if (obj.hasOwnProperty(prop)) {
                name = prop;
            }
        }
        return zip.file(name).async("string");
    })
    .then( text =>{                   
        var waveObj = JSON.parse(text);
        return waveObj;
    }, err => {
        console.log(err);
    });
}
```


#### 9. 鼾声梦话噪声数据

- snoreList：鼾声列表
  - Array [[1270,34,19],[1366,19,17]] 
  - [[开始时间偏移(秒),持续时间,分贝]]
- somniloquyList 梦话列表 
  - Array [[1270,34,19],[1366,19,17]] 
  - [[开始时间偏移(秒),持续时间,分贝]]
- noiseList 噪声列表 
  - Array [[1270,34,19],[1366,19,17]]
  - [[开始时间偏移(秒),持续时间,分贝]]
- startSnoreTime 鼾声开始时间 Number 1559273867 单位秒
- endSnoreDuration 鼾声总时间 Number 30000 单位秒

* * *

#### 10. ringData对象解析方法

```javascript
/* 解码ringData数据
*@method decodeRingData
*@param {ringData} 戒指数据
*@return {decodeRingData} 已解码戒指数据
*/
function decodeRingData({
    ringData: ringData
}) {
    return JSON.parse(pako.inflate(window.atob(ringData), { to: 'string' }));
}
```
window.atob() 方法的作用是对Base64加密字符进行解码
pako.inflate() 方法需要引入外部依赖pako.js

* * *

#### 11. 无血氧时呼吸事件计算方法
```javascript
/*计算呼吸事件
*@method getBreathEvent
*@param {array} array breathList数组,0:发声时间,1:持续时长,2:开始采样帧,3:结束采样帧,4:呼吸事件类型
*@param {number} totalSleepMinutes 总睡眠时长，分钟
*@return {object} 包含9个呼吸事件相关数据的对象
*/
function getBreathEvent(array,totalSleepMinutes) {
    var hasBreathType = (array.length>0&&array[0].length > 4)?true:false;
    var max = 0;
    var maxDuration = '';
    var total = 0;
    //t0:混合性 t1：中枢性 t2：阻塞性 t3：低通气
    var breathTypeEnt = {
        t0: 0,
        t1: 0,
        t2: 0,
        t3: 0
    };
    if (array.length !== 0) {
        for (var i = 0; i < array.length; i++) {
            var thisValue = array[i][1];
            if(max<thisValue) {
                max = thisValue;
                maxDuration = array[i][0];
            }
            total += thisValue;
            if(hasBreathType) {
                switch (array[i][4]) {
                    case 0:
                        breathTypeEnt.t0++;
                        break;
                    case 1:
                        breathTypeEnt.t1++;
                        break;
                    case 2:
                        breathTypeEnt.t2++;
                        break;
                    case 3:
                        breathTypeEnt.t3++;
                        break;
                }
            }
        }
    }

    return {
        BEMeanlen: (total/array.length).toFixed(0),
        BEMaxlen: max,
        BEMaxlentime: maxDuration,
        BEOHCnt: breathTypeEnt.t2 + breathTypeEnt.t3,
        BECCnt: breathTypeEnt.t1,
        BEMCnt: breathTypeEnt.t0,
        BECnt: array.length,
        BETotalTime: (total/60).toFixed(0),
        BETotalrate: (total/60/totalSleepMinutes).toFixed(1),
    };
}
```

#### 12. 截取血氧重新计算血氧和呼吸事件方法

```javascript
/**
 * 
 * @param {Array} breathEventArr 呼吸事件数组 [[starttime, durtime, startPos, endPos, type]...]
 * @param {Array} statusArr 睡眠分期数组
 * @param {Number} btimeStart 睡眠分期的开始时间
 * @param {Number} startSpoTimeOffset real spo start time - timestart
 * @param {Number} endSpoTimeOffset real spo end time - timestart
 * @param {*} originSpoObj
 */
function parseSpoAndSleep(breathEventArr, statusArr, btimeStart, startSpoTimeOffset, endSpoTimeOffset, originSpoObj)  // originSpoObj
{
    // 1. 处理血氧数据
    var retSpo = makeSpoResult();

    var vldsp = startSpoTimeOffset;
    var vldep = endSpoTimeOffset;
    if (vldep <= vldsp) return null;
    var o2Arr = originSpoObj.Spo2Arr;
    var prArr = originSpoObj.prArr;

    var prmax = 0;
    var prmin = 255;
    var prmean = 0;
    var vldcnt = 0;

    for (var i = vldsp; i < vldep; i++) {
        if (prArr[i] == undefined) break;
        if (prArr[i] === 0) continue;
        if (prArr[i] > prmax) prmax = prArr[i];
        if (prArr[i] < prmin) prmin = prArr[i];
        prmean += prArr[i];
        vldcnt++;
    }
    retSpo.prMax = prmax;
    retSpo.prMin = prmin;
    retSpo.prAvg = Math.floor(prmean / vldcnt);

    retSpo.spo2Less95Time = 0;
    retSpo.spo2Less90Time = 0;
    retSpo.spo2Less85Time = 0;
    retSpo.spo2Less80Time = 0;
    retSpo.spo2Less70Time = 0;
    retSpo.spo2Less60Time = 0;

    var spoMin = 100;
    var spoMean = 0;
    vldcnt = 0;
    for (var i = vldsp; i < vldep; i++) {
        var e = o2Arr[i];
        if (e == undefined) break;
        if (e < originSpoObj.Spo2Min) continue;
        if (e < 95) retSpo.spo2Less95Time++;
        if (e < 90) retSpo.spo2Less90Time++;
        if (e < 85) retSpo.spo2Less85Time++;
        if (e < 80) retSpo.spo2Less80Time++;
        if (e < 70) retSpo.spo2Less70Time++;
        if (e < 60) retSpo.spo2Less60Time++;

        if (e < spoMin) spoMin = e;
        spoMean += e;
        vldcnt++;
    }

    retSpo.Spo2Min = spoMin;
    retSpo.Spo2Avg = Math.floor(spoMean / vldcnt);
    retSpo.spo2Less95TimePercent = retSpo.spo2Less95Time * 100 / vldcnt;
    retSpo.spo2Less90TimePercent = retSpo.spo2Less90Time * 100 / vldcnt;
    retSpo.spo2Less85TimePercent = retSpo.spo2Less85Time * 100 / vldcnt;
    retSpo.spo2Less80TimePercent = retSpo.spo2Less80Time * 100 / vldcnt;
    retSpo.spo2Less70TimePercent = retSpo.spo2Less70Time * 100 / vldcnt;
    retSpo.spo2Less60TimePercent = retSpo.spo2Less60Time * 100 / vldcnt;

    var spoEventCnt = originSpoObj.diffThdLge3Cnts;
    retSpo.diffThdLge3Cnts = 0;
    retSpo.diffThdLge3Pr = 0;
    retSpo.maxSpo2DownTime = 0;
    var spoEventArr = originSpoObj.SAO2EventVect;

    for (var i = 0; i < spoEventCnt; i++) {
        var e = spoEventArr[i];
        // [[start, len], [start, len]...]
        if (e[0] > vldsp && e[0] + e[1] < vldep) {
            retSpo.diffThdLge3Cnts++;
            if (e[1] > retSpo.maxSpo2DownTime) retSpo.maxSpo2DownTime = e[1];
        }
    }
    retSpo.diffThdLge3Pr = retSpo.diffThdLge3Cnts * 3600 / vldcnt;

    var vldtimestart = originSpoObj.timeStart + vldsp;
    var vldtimeend = originSpoObj.timeStart + vldep;
    var vldtimecnt = 0;

    for (var i = 0; i < startSpoTimeOffset; i++) {
        if (originSpoObj.Spo2Arr[i]) originSpoObj.Spo2Arr[i] = 0;
        if (originSpoObj.prArr[i]) originSpoObj.prArr[i] = 0;
    }
    for (var i = endSpoTimeOffset; i < originSpoObj.Spo2Arr.length; i++) {
        originSpoObj.Spo2Arr[i] = 0;
        originSpoObj.prArr[i] = 0;
    }
    retSpo.Spo2Arr = originSpoObj.Spo2Arr
    retSpo.prArr = originSpoObj.prArr

    // 2. 处理 breath event 数据
    var retBE = makeBEResult();
    retBE.Waketime = 0;
    retBE.REMtime = 0;
    retBE.LightSleeptime = 0;
    retBE.DeepSleeptime = 0;

    retBE.Wakerate = 0;
    retBE.REMrate = 0;
    retBE.LightSleeprate = 0;
    retBE.DeepSleeprate = 0;

    for (var i = 0; i < statusArr.length; i++) {
        var t = btimeStart + i * 60; // i 的间隔为 min，需要转换为 s
        if (t > vldtimestart && t < vldtimeend) {
            vldtimecnt++;
            switch (statusArr[i]) {
                case 0: retBE.Waketime++; break;
                case 2: retBE.REMtime++; break;
                case 3: retBE.LightSleeptime++; break;
                case 4: retBE.DeepSleeptime++; break;
                case 6: retBE.Waketime++;
            }
        }
    }

    if (vldtimecnt > 0) {
        retBE.Wakerate = retBE.Waketime * 100 / vldtimecnt;
        retBE.REMrate = retBE.REMtime * 100 / vldtimecnt;
        retBE.LightSleeprate = retBE.LightSleeptime * 100 / vldtimecnt;
        retBE.DeepSleeprate = retBE.DeepSleeptime * 100 / vldtimecnt;
    }

    retBE.AHI = 0;
    retBE.BECnt = 0;
    retBE.BEMeanlen = 0;
    retBE.BEMaxlen = 0;
    retBE.BEMaxlentime = 0;
    retBE.BETotalTime = 0;
    retBE.BETotalrate = 0;
    retBE.BEOHCnt = 0;
    retBE.BECCnt = 0;
    retBE.BEMCnt = 0;

    for (var i = 0; i < breathEventArr.length; i++) {
        // 0: starttime; 1: lasttime/durtime; 2: startpos; 3: endpos; 4: type;
        var e = breathEventArr[i];
        if (e[0]+btimeStart > vldtimestart && e[0]+btimeStart < vldtimeend) {
            retBE.BECnt++;
            retBE.BEMeanlen += e[1];
            if (retBE.BEMaxlen < e[1]) {
                retBE.BEMaxlen = e[1];
                retBE.BEMaxlentime = e[0]+btimeStart;
            }
            switch (e[4]) {
                case 0:
                    retBE.BEMCnt++;
                    break;
                case 1:
                    retBE.BECCnt++;
                    break;
                case 2:
                case 3:
                    retBE.BEOHCnt++;
            }
        }
    }
    if (vldtimecnt > 0) {
        retBE.AHI = retBE.BECnt * 60 / vldtimecnt;
        retBE.BETotalrate = retBE.BETotalTime / vldtimecnt;
    }

    if (retBE.BECnt) {
        retBE.BEMeanlen = retBE.BEMeanlen / retBE.BECnt;
    }

    // 2019年4月1日 11点22分 增加离手处理
    if (originSpoObj.handOffArr) {
        for (var i = 0, len = originSpoObj.handOffArr.length; i < len; i++) {
            var t = originSpoObj.handOffArr[i] + btimeStart
            if (t <= vldtimestart) {
                originSpoObj.handOffArr[i] = 0
                 continue
            }
            if (t >= vldtimeend) {
                originSpoObj.handOffArr[i] = 0
                continue
            }
        }
    }
    retSpo.handOffArr = originSpoObj.handOffArr;
    
    retBE.BETotalTime = retBE.BETotalTime / 60;
    return [retSpo, retBE];
}

function makeSpoResult() {
    return {
        'duration': -1,
        'startpos': -1,
        'endpos': -1,
        'handonTotalTime': -1,
        'timeStart': -1,
        'Spo2Avg': -1,
        'Spo2Min': -1,
        'maxSpo2DownTime': -1,
        'prAvg': -1,
        'prMax': -1,
        'prMin': -1,
        'diffThdLge3Cnts': -1,
        'diffThdLge3Pr': -1,
        'spo2Less95Time': -1,
        'spo2Less90Time': -1,
        'spo2Less85Time': -1,
        'spo2Less80Time': -1,
        'spo2Less70Time': -1,
        'spo2Less60Time': -1,
        'spo2Less95TimePercent': -1,
        'spo2Less90TimePercent': -1,
        'spo2Less85TimePercent': -1,
        'spo2Less80TimePercent': -1,
        'spo2Less70TimePercent': -1,
        'spo2Less60TimePercent': -1,
        // 'handOffArrlen': -1,
        'handOffArr': [],
        'Spo2Arr':[],
        'prArr':[]
    }
}

function makeBEResult() {
    return {
        'AHI': -1,
        'BEMeanlen': -1,
        'BEMaxlen': -1,
        'BEMaxlentime': -1,
        'BECnt': -1,
        'BETotalTime': -1,
        'BETotalrate': -1,
        'BEOHCnt': -1,
        'BECCnt': -1,
        'BEMCnt': -1,
        'Waketime': -1,
        'REMtime': -1,
        'LightSleeptime': -1,
        'DeepSleeptime': -1,
        'Wakerate': -1,
        'REMrate': -1,
        'LightSleeprate': -1,
        'DeepSleeprate': -1,
    }
}
```

