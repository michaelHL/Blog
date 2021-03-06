---
title: Linux 中日期 date 命令格式控制符
date: 2018-01-08 11:14:46
tags:
    - Date
    - Linux
categories: Operation and Maintenance
---

Linux 中的 `date` 命令格式控制符实在令人眼花缭乱，整理如下。

<!-- more -->

| 格式控制符 | 说明                                        | 举例                              |
| :---:      | :---                                        | :---                              |
| `%%`       | 百分号                                      | `%`                               |
| `%a`       | 短星期（区域相关）                          | `Sat`                             |
| `%A`       | 长星期（区域相关）                          | `Saturday`                        |
| `%b`       | 短月份                                      | `Dec`                             |
| `%B`       | 长月份                                      | `December`                        |
| `%c`       | 日期时间                                    | `Sat 09 Dec 2017 08:45:40 PM CST` |
| `%C`       | 年份前两位                                  | `20`                              |
| `%d`       | 天数（前置补零）                            | `09`                              |
| `%D`       | 日期，格式为：`%m/%d/%y`                    | `12/09/17`                        |
| `%e`       | 天数（前置空格）                            | `9`                               |
| `%F`       | 日期，格式为：`%Y-%m-%d`                    | `2017-12-09`                      |
| `%g`       |                                             | `17`                              |
| `%G`       |                                             | `2017`                            |
| `%h`       | 同 `%b`                                     | `Dec`                             |
| `%H`       | `24` 小时制小时（前置补零）                 | `20`                              |
| `%I`       | `12` 小时制小时（前置补零）                 | `08`                              |
| `%j`       | 一年中第几天（前置补零）                    | `343`                             |
| `%k`       | `24` 小时制小时（前置空格）                 | `20`                              |
| `%l`       | `12` 小时制小时（前置空格）                 | `8`                               |
| `%m`       | 数字月份（前置补零）                        | `12`                              |
| `%M`       | 分钟（前置补零）                            | `45`                              |
| `%n`       | 换行                                        |                                   |
| `%N`       | 纳秒                                        | `893369643`                       |
| `%p`       | `PM` 或 `AM`                                | `PM`                              |
| `%P`       | 小写的 `pm` 或 `am`                         | `pm`                              |
| `%r`       |                                             | `08:45:40 PM`                     |
| `%R`       | 24 小时制小时分钟: `%H:%M`                  | `20:45`                           |
| `%s`       | 从 `1970-01-01 00:00:00 UTC` 开始计算的秒数 | `1512823540`                      |
| `%S`       | 秒数（`00..60`，前置补零）                  | `40`                              |
| `%t`       | 制表符号                                    |                                   |
| `%T`       | 时间：`%H:%M:%S`                            | `20:45:40`                        |
| `%u`       | 数字星期（`1-7`，`1` 表示周一）             | `6`                               |
| `%U`       | 周数（周日为星期第一天，前置补零）          | `49`                              |
| `%V`       |                                             | `49`                              |
| `%w`       | 数字星期（`0-6`，`0` 表示周日）             | `6`                               |
| `%W`       | 周数（周一为星期第一天，前置补零）          | `49`                              |
| `%x`       | 区域日期                                    | `12/09/2017`                      |
| `%X`       | 区域时间                                    | `08:45:40 PM`                     |
| `%y`       | 年份后两位（短年份）                        | `17`                              |
| `%Y`       | 长年份                                      | `2017`                            |
| `%z`       |                                             | `+0800`                           |
| `%:z`      |                                             | `+08:00`                          |
| `%::`      |                                             | `+08:00:00`                       |
| `%::`      |                                             | `+08`                             |
| `%Z`       | 字母时区简写                                | `CST`                             |
