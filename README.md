# laravel-soar

> SQL optimizer and rewriter extension package for laravel framework. - 适配于 laravel 的 SQL 优化器和重写器扩展包。

[简体中文](README-CN.md) | [ENGLISH](README.md)

[![Tests](https://github.com/guanguans/laravel-soar/workflows/Tests/badge.svg)](https://github.com/guanguans/laravel-soar/actions)
[![Check & fix styling](https://github.com/guanguans/laravel-soar/workflows/Check%20&%20fix%20styling/badge.svg)](https://github.com/guanguans/laravel-soar/actions)
[![codecov](https://codecov.io/gh/guanguans/laravel-soar/branch/main/graph/badge.svg?token=URGFAWS6S4)](https://codecov.io/gh/guanguans/laravel-soar)
[![Latest Stable Version](https://poser.pugx.org/guanguans/laravel-soar/v)](//packagist.org/packages/guanguans/laravel-soar)
[![Total Downloads](https://poser.pugx.org/guanguans/laravel-soar/downloads)](//packagist.org/packages/guanguans/laravel-soar)
[![License](https://poser.pugx.org/guanguans/laravel-soar/license)](//packagist.org/packages/guanguans/laravel-soar)

## 环境要求

* laravel >= 5.5

## 安装

``` shell
$ composer require guanguans/laravel-soar --dev -vvv
```

### 发布配置

```php
$ php artisan vendor:publish --provider="Guanguans\\LaravelSoar\\SoarServiceProvider"
```

## 使用

### 生成 sql 评分报告示例

``` php
// 查询构建器示例
DB::table('yb_member')
    ->select('*')
    ->join('yb_member_account as yb_member_account', 'yb_member_account.member_id', '=', 'yb_member.id')
    ->whereRaw('1 <> 1')
    ->where('yb_member.nickname', 'like', 'admin')
    ->where('yb_member.username', 'like', '%admin%')
    ->whereRaw("substring(yb_member.username, 1, 5) = 'admin'")
    ->whereIn('yb_member.id', [110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120])
    ->orWhereNotNull('yb_member.realname')
    ->groupByRaw("yb_member.status, '100'")
    ->having('yb_member.id', '>', '100')
    ->inRandomOrder()
    // ->toSoarScore()   // 生成 sql 评分报告
    // ->dumpSoarScore() // 打印 sql 评分报告
    ->ddSoarScore()      // 打印 sql 评分报告，并且退出应用程序。
;

// 门面和 app 函数示例
echo \Soar::score($sql);
echo app('soar')->score($sql);
```

![score](./docs/score.png)

### 生成 Explain 信息解读报告示例

``` php
// 查询构建器示例
DB::table('yb_member')
    ->select('*')
    ->join('yb_member_account as yb_member_account', 'yb_member_account.member_id', '=', 'yb_member.id')
    ->whereRaw('1 <> 1')
    ->where('yb_member.nickname', 'like', 'admin')
    ->where('yb_member.username', 'like', '%admin%')
    ->whereRaw("substring(yb_member.username, 1, 5) = 'admin'")
    ->whereIn('yb_member.id', [110, 120])
    ->orWhereNotNull('yb_member.realname')
    ->groupByRaw("yb_member.status, '100'")
    ->having('yb_member.id', '>', '100')
    ->inRandomOrder()
    // ->toSoarHtmlExplain()   // 生成 Explain 信息解读报告
    // ->dumpSoarHtmlExplain() // 打印 Explain 信息解读报告
    ->ddSoarHtmlExplain()      // 打印 Explain 信息解读报告，并且退出应用程序。
;

// 门面和 app 函数示例
echo \Soar::htmlExplain($sql);
echo app('soar')->htmlExplain($sql);
```

![explain](./docs/explain.png)

## 测试

``` bash
$ composer test
```

## 变更日志

请参阅 [CHANGELOG](CHANGELOG.md) 获取最近有关更改的更多信息。

## 贡献指南

请参阅 [CONTRIBUTING](.github/CONTRIBUTING.md) 有关详细信息。

## 安全漏洞

请查看[我们的安全政策](../../security/policy)了解如何报告安全漏洞。

## 贡献者

* [guanguans](https://github.com/guanguans)
* [所有贡献者](../../contributors)

## 协议

MIT 许可证（MIT）。有关更多信息，请参见[协议文件](LICENSE)。