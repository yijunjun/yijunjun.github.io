# 上传erp数据格式

## 金币部分

1. 用户平台购买

   1.1. 上传数据

    | 字段 | 类型 | 说明 |
    |:-------------: |:---------------:| :-------------:|
    | code | int | 业务代码,此处为1 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | uid | int | 用户的id |
    | phone | string | 用户的手机号 |
    | mid | int | 平台管理员的id |
    | manger | string | 平台管理员的名字 |
    | number | int | 购买金币数,单位是分 |
    | account | int | 购买后帐户总金币数,单位是分 |
    | time | int | 购买时间,Unix时间戳 |

   1.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为1 |
    | id | int | 此记录在本系统的id |
    | rid | int | 此记录在erp系统的id,业务范围内唯一 |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |

2. 用户玩游戏赚金币

   2.1. 上传数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为2 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | uid | int | 用户的id |
    | phone | string | 用户的手机号 |
    | game | string | 游戏名称 |
    | grade | string | 游戏场次 |
    | number | int | 赚金币数,单位是分 |
    | account | int | 赚后帐户总金币数,单位是分 |
    | time | int | 发生时间,Unix时间戳 |

   2.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为2 |
    | id | int | 此记录在本系统的id |
    | rid | int | 此记录在erp系统的id,业务范围内唯一 |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |

3. 用户玩游戏输金币

   3.1. 上传数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为3 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | uid | int | 用户的id |
    | phone | string | 用户的手机号 |
    | game | string | 游戏名称 |
    | grade | string | 游戏场次 |
    | number | int | 输金币数,单位是分 |
    | account | int | 输后帐户总金币数,单位是分 |
    | time | int | 发生时间,Unix时间戳 |

   3.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为3 |
    | id | int | 此记录在本系统的id |
    | rid | int | 此记录在erp系统的id,业务范围内唯一 |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |

4. 平台收游戏服务费

   4.1. 上传数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为4 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | game | string | 游戏名称 |
    | grade | string | 游戏场次 |
    | number | int | 金币数,单位是分 |
    | time | int | 发生时间,Unix时间戳 |

   4.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为4 |
    | id | int | 此记录在本系统的id |
    | rid | int | 此记录在erp系统的id,业务范围内唯一 |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |

5. 用户之间赠送金币

   5.1. 上传数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为5 |
    | id | int | 此记录在系统的id,业务范围内唯一 |
    | srcId | int | 赠送者的id |
    | srcPhone | string | 赠送者的手机号 |
    | srcAccount | int | 赠送者赠送后的金币数,单位是分 |
    | dstId | int | 接受者的id |
    | dstPhone | string | 接受者的手机号 |
    | dstAccount | int | 接受者接受后的金币数,单位是分 |
    | number | int | 赠送金币数,单位是分 |
    | time | int | 发生时间,Unix时间戳 |

   5.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为5 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | rid | int | 此记录在erp系统的id |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |

6. 平台回收金币

   6.1. 上传数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为6 |
    | id | int | 此记录在本系统的id,业务范围内唯一 |
    | uid | int | 用户的id |
    | phone | string | 用户的手机号 |
    | mid | int | 平台管理员的id |
    | manger | string | 平台管理员的名字 |
    | number | int | 回收金币数,单位是分 |
    | account | int | 回收后帐户总金币数,单位是分 |
    | time | int | 回收时间,Unix时间戳 |

   6.2. 期望回包数据

    | 字段 | 类型 | 说明 |
    | - | - | - |
    | code | int | 业务代码,此处为6 |
    | id | int | 此记录在本系统的id |
    | rid | int | 此记录在erp系统的id,业务范围内唯一 |
    | success | bool | erp是否处理成功 |
    | errMsg | string | erp处理失败原因 |
