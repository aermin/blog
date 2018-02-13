---
layout: js
title: 各种时间转换总结
date: 2017-11-19 22:57:34
tags:
	- Js
	- 周总结
---
 这次项目写的的商业数据系统后台管理，用vue+饿了么的UI组件，其中我负责的模块有大量的时间选择器交互，写项目的过程中，很多时候都在封装一些时间格式转换的函数，在此总结一些，有一些借助以后的全局的轮子就不写了。
 
```
        //获取日期 yyyy-MM-dd 
        getDate: function(yourDate, i) {
            // 获取当前日期
            const date = new Date(yourDate);
            // 获取当前月份
            let nowMonth = date.getMonth() + 1;
            // 获取当前是几号或者距离今天i天
            let strDate = date.getDate() + i;
            // 添加分隔符“-”
            const seperator = "-";
            // 对月份进行处理，1-9月在前面添加一个“0”
            if (nowMonth >= 1 && nowMonth <= 9) {
                nowMonth = "0" + nowMonth;
            }
            // 对月份进行处理，1-9号在前面添加一个“0”
            if (strDate >= 0 && strDate <= 9) {
                strDate = "0" + strDate;
            }
            // 最后拼接字符串，得到一个格式为(yyyy-MM-dd)的日期
            const nowDate = date.getFullYear() + seperator + nowMonth + seperator + strDate;
            return nowDate;
        }
```

 <!-- more -->


```
    //当月某一个时间的date转换成当月1号的date
        getMonthfirstDayDate(yourDate, i) {
            const y = yourDate.getFullYear();
            const m = yourDate.getMonth();
            return new Date(y, m + i, 1);
        }
```

```
        //获取年月 yyyy-MM   i 为距离本月的月数
        getMonthTime: function(i) {
            // 获取当前日期
            const date = new Date();
            // 获取当前月份
            let nowMonth = date.getMonth() + 1 + i;
            // 对月份进行处理，1-9月在前面添加一个“0”
            if (nowMonth >= 1 && nowMonth <= 9) {
                nowMonth = "0" + nowMonth;
            }
            // 最后拼接字符串，得到一个格式为(yyyy-MM)的日期
            const nowDate = date.getFullYear() + "-" + nowMonth;
            return nowDate;
        },
```

```
        //获取当前日期 yyyy-MM-dd
        getSimpleDate: function() {
            // 获取当前日期
            const date = new Date();
            // 获取当前月份
            let nowMonth = date.getMonth() + 1;
            // 获取当前是几号
            let strDate = date.getDate();
            // 添加分隔符“-”
            const seperator = "-";
            // 对月份进行处理，1-9月在前面添加一个“0”
            if (nowMonth >= 1 && nowMonth <= 9) {
                nowMonth = "0" + nowMonth;
            }
            // 对月份进行处理，1-9号在前面添加一个“0”
            if (strDate >= 0 && strDate <= 9) {
                strDate = "0" + strDate;
            }
            // 最后拼接字符串，得到一个格式为(yyyy-MM-dd)的日期
            const nowDate = date.getFullYear() + seperator + nowMonth + seperator + strDate;
            return nowDate;
        },
```

```
        //获取当前星期几
        getWeekDay: function() {
            const day = new Date().getDay();
            const weekday = new Array(7);
            weekday[0] = "周日";
            weekday[1] = "周一";
            weekday[2] = "周二";
            weekday[3] = "周三";
            weekday[4] = "周四";
            weekday[5] = "周五";
            weekday[6] = "周六";
            return weekday[day];
        },
```

```
        //获取当前时间
        getTime: function() {
            const date = new Date();
            const hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
            const minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
            return hour + ":" + minute;
        }
```