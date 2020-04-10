[TOC]

# BOE License

## 设备激活方案概述

为避免设备软硬件被破解，对设备增加激活启用策略，只有输入正确激活码的设备才能正常开启监测。

### 网络图

![page1image574064.png](/Users/zangtengfei/Library/Application Support/typora-user-images/page1image574064.png) 

### 流程图

![page2image528864.png](/Users/zangtengfei/Library/Application Support/typora-user-images/page2image528864.png) 

## 加密方法及秘钥

生成激活码的思路为，13位SN明文通过AES加密生成密文，将密文POST到服务器生成激活码。

服务器收到密文先解析为明文，再使用RSA加密生成激活码返回。

验证激活码的思路相同，先将13位SN明文通过AES加密生成密文，将密文POST到服务器验证设备是否激活。

服务器收到密文先解析为明文，再从数据库查找激活码返回。

同一SN号只能调用十次生成激活码和验证激活码的接口。

### AES加解密

服务器：密钥 加密

设备：密钥 解密



模式：CBC

填充模式：pkcs7padding

数据块位数：128

编码：utf8

编码格式：base64



密钥：926aed88cad82b52

IV：926aed88cad82b52

### RSA加解密

- **PublicKey**

-----BEGIN PUBLIC KEY-----
MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAMIwTqj4e16YF1wmBtesVda/2qW1CT4C
KW5LQBVYtRBtcnhBy8eFfmSzpw5vaPvoyecNKgkTdHEC4+09uBngC0ECAwEAAQ==
-----END PUBLIC KEY-----

- **Private Key**

-----BEGIN PRIVATE KEY-----
MIIBUwIBADANBgkqhkiG9w0BAQEFAASCAT0wggE5AgEAAkEAwjBOqPh7XpgXXCYG
16xV1r/apbUJPgIpbktAFVi1EG1yeEHLx4V+ZLOnDm9o++jJ5w0qCRN0cQLj7T24
GeALQQIDAQABAkAbJPeJ5RoRsf7p8aLZOQzStQTSTwkYsuKpuTkfAsRkpDlf7jva
uPu/ywdwsqaABZwcm02oEYnDdgXomtDPP7exAiEA8xg2QVYweoqTVGTU2xGNQ4gZ
/T2nvcnF4/PV76OLPU0CIQDMf3AQNueSLgzIe/7lz2p4szDs0bPtSO5v/IPT6u3b
xQIgcoUjeiA6cmA6C/X8eL+KBxhk9fJHxZb6jOrgDCbFf7kCIED/KEEfEk81774x
Gv00BaVDXwOGS2fZzF8vpT7P5rX5AiBjWfc58+QV2zyaPdDV2zgihmdKN07NratP
NmtW4TMggA==
-----END PRIVATE KEY-----

## 接口详情
### 生成激活码

**简要描述：**

- 激活码生成接口

**请求URL：**

- `http://raw.megahealth.cn/rsa/generateActivationCode`

**请求方式：**

- POST
- HTTP-HEADER  contentType: application/json

**参数：**

| 参数名 | 必选 | 类型   | 说明                      |
| :----- | :--- | :----- | ------------------------- |
| data   | 是   | string | 11位设备序列号AES加密密文 |

AES加密密文示例：S01A181100000 => kb0YvLRY/mLjoAFf0xgb8A==

**正确返回示例：**

``` json
{
    "code": 100,
  	"data": "QtsWtFBJkvXOQSeGjTV9zIk1hPPlGpcJWSxeUZ3s0wtFUFE1lD5JqmDnfZqGIoKPPT+yyBse4ojr7LjSDVdstw=="
}
```

**错误信息列表：** 

- 参数sn不能为空

```json
{
    "code": 101,
    "message": "No sn specified!"
}
```

- 参数格式错误

```json
{
    "code": 102,
    "message": "Incorrect data format!"
}
```

- 此sn生成激活码超过十次，已失效，请联系服务商解决。

```json
{
    "code": 103,
    "message": "Invalid device!"
}
```
- 激活未授权，请联系服务商解决。（激活设备数量超过已授权激活码数量）

```json
{
    "code": 104,
    "message": "Unauthorized activation!"
}
```
### 查询是否激活

**简要描述：**

- 查询设备是否激活

**请求URL：**

- `http://raw.megahealth.cn/rsa/checkActivation`

**请求方式：**

- POST
- HTTP-HEADER  contentType: application/json

**参数：**

| 参数名 | 必选 | 类型   | 说明                      |
| :----- | :--- | :----- | ------------------------- |
| data   | 是   | string | 11位设备序列号AES加密密文 |

AES加密密文示例：S01A181100000 => kb0YvLRY/mLjoAFf0xgb8A==

**正确返回示例：**

``` json
{
    "code": 100,
    "data": "QtsWtFBJkvXOQSeGjTV9zIk1hPPlGpcJWSxeUZ3s0wtFUFE1lD5JqmDnfZqGIoKPPT+yyBse4ojr7LjSDVdstw=="
}
```

**错误信息列表：** 

- 激活码不能为空

```json
{
    "code": 101,
  	"message": "No data specified!"
}
```

- 激活码格式错误

```json
{
    "code": 102,
  	"message": "Incorrect data format!"
}
```

- ~~设备激活验证超过十次，已锁定，请联系服务商解决~~

```json
{
    "code": 103,
    "message": "Invalid device!"
}
```

- 设备未激活

```json
{
    "code": 104,
  	"message": "Device not activated!"
}
```