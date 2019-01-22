
aliyun php sdk

## 简介
aliyun官方sdk开发包，支持yii2

## 下载安装
* 下载

git clone https://github.com/mrh111/aliyunsdk.git

* 安装

进入　aliyunsdk　目录，执行

```bash
composer require "saviorlv/yii2-dysms:dev-master"
```

* 项目中的使用

> 在config/main.php配置文件中定义component配置信息
```bash
'components' => [
  .....
  'aliyun' => [
      'class' => 'saviorlv\aliyun\Sms',
      'accessKeyId' => '123455',
      'accessKeySecret' => '122345666'
    ],
  ....
]

```

