# IdValidator.php

**中华人民共和国居民身份证**、**中华人民共和国港澳居民居住证**以及**中华人民共和国台湾居民居住证**号码验证工具（PHP Composer 版）支持 15 位与 18 位号码。基于 [JavaScript 版本](https://github.com/mc-zone/IDValidator)。


[![Build Status](https://travis-ci.org/jxlwqq/id-validator.svg?branch=master)](https://travis-ci.org/jxlwqq/id-validator)
[![StyleCI](https://github.styleci.io/repos/147758862/shield?branch=master)](https://github.styleci.io/repos/147758862)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/jxlwqq/id-validator/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/jxlwqq/id-validator/?branch=master)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fjxlwqq%2Fid-validator.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fjxlwqq%2Fid-validator?ref=badge_shield)

## 安装

```bash
composer require "jxlwqq/id-validator"
```

## 使用

> `440308199901101512` 和 `610104620927690` 示例大陆居民身份证均为随机生成的假数据，如撞车，请联系删除。
> `810000199408230021` 和 `830000199201300022` 示例港澳台居民居住证为北京市公安局公布的居住证样式号码。

### 验证身份证号合法性

验证身份证号是否合法，合法返回 `true`，不合法返回 `false`：

```php
use Jxlwqq\IdValidator\IdValidator;

$idValidator = new IdValidator();
$idValidator->isValid('440308199901101512'); // 大陆居民身份证 18 位
$idValidator->isValid('610104620927690');    // 大陆居民身份证 15 位
$idValidator->isValid('810000199408230021'); // 港澳居民居住证 18 位
$idValidator->isValid('830000199201300022'); // 台湾居民居住证 18 位
```

### 获取身份证号信息

当身份证号合法时，返回分析信息（地区、出生日期、星座、生肖、性别、校验位），不合法返回 `false`：
```php
use Jxlwqq\IdValidator\IdValidator;

$idValidator = new IdValidator();
$idValidator->getInfo('440308199901101512'); // 18 位
$idValidator->getInfo('610104620927690');    // 15 位
```
返回信息格式如下：

```php
[
'addressCode'   => '440308',          // 地址码   
'abandoned'     => 0,                 // 地址码是否废弃，1 为废弃的，0 为正在使用的
'address'       => '广东省深圳市盐田区', // 地址
'birthdayCode'  => '1999-01-10',      // 出生日期
'constellation' => '水瓶座',           // 星座
'chineseZodiac' => '卯兔',             // 生肖
'sex'           => 1,                 // 性别，1 为男性，0 为女性
'length'        => 18,                // 号码长度
'checkBit'      => '2',               // 校验码
]
```

### 生成可通过校验的假数据
伪造符合校验的身份证：

`fakeId()` 方法有 4 个可选参数：
* `$eighteen` 是否生成 18 位号码，默认为 `true`；
* `$address` 地址，即省市县三级地区官方全称，如`北京市`、`台湾省`、`香港特别行政区`、`深圳市`、`黄浦区`等，默认或参数非法，则生成合法的随机地址；
* `$birthday` 出生日期，如 `2000`、`198801`、`19990101` 等，默认或参数非法，则生成合法的随机出生日期；
* `$sex` 性别，1 为男性，0 为女性，默认或参数非法，则生成合法的随机性别；

```php
use Jxlwqq\IdValidator\IdValidator;

$idValidator = new IdValidator();
$idValidator->fakeId();                                    // 18 位
$idValidator->fakeId(false);                               // 15 位
$idValidator->fakeId(true, '上海市', '2000', 1);            // 生成出生于 2000 年上海市的男性居民身份证
$idValidator->fakeId(true, '南山区', '1999', 0);            // 生成出生于 1999 年广东省深圳市南山区的女性居民身份证
$idValidator->fakeId(true, '江苏省', '200001', 1));         // 生成出生于 2000 年 1 月江苏省的男性居民身份证
$idValidator->fakeId(true, '厦门市', '199701', 0));         // 生成出生于 2000 年 1 月福建省厦门市的女性居民身份证
$idValidator->fakeId(true, '台湾省', '20131010', 0);        // 生成出生于 2013 年 10 月 10 日台湾省的女性居民居住证
$idValidator->fakeId(true, '香港特别行政区', '19970701', 0); // 生成出生于 1997 年 7 月 1 日香港特别行政区的女性居民居住证
```

## 参考资料
* ~~GB 11643-1999 公民身份证号码~~

* ~~GB 2260-1995 中华人民共和国行政区划代码~~

* [中华人民共和国公民身份号码](https://zh.wikipedia.org/wiki/中华人民共和国公民身份号码)

* [中华人民共和国民政部：行政区划代码](http://www.mca.gov.cn/article/sj/xzqh/)

* [中华人民共和国行政区划代码历史数据集](https://github.com/jxlwqq/address-code-of-china)

* [国务院办公厅关于印发《港澳台居民居住证申领发放办法》的通知](http://www.gov.cn/zhengce/content/2018-08/19/content_5314865.htm)

* [港澳台居民居住证](https://zh.wikipedia.org/wiki/港澳台居民居住证)

## Change Log
* 1.1.0 身份证号返回信息新增生肖和星座内容；

* 1.2.0 支持港澳台居民居住证；

* 1.3.0 行政区划代码（地址码）数据改由从中华人民共和国民政部官方网站获取；

* 1.4.0 支持查询因行政区变更而废弃的地址码；

* 1.4.2 `fakeId()` 方法增加可选参数；

## License
MIT


