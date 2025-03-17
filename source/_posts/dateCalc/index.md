---
title: 日期计算器
date: 2025-03-17 18:59:44
categories: 工具
tags: ["前端小工具"]
cover: date
author: 云深不知雀
banner:
    type: video
    bgurl: /img/video/cloud.mp4
    bannerText: 
---
## 一、计算两个日期之间差多少天
<table>
    <tbody>
        <tr>
            <td></td>
            <td>
                <input type="number" class="y" id="SY2">年
                <input type="number" class="d" id="SM2">月
                <input type="number" class="d" id="SD2">日（默认今天）
            </td>
        </tr>
        <tr>
            <td style="width:1rem" valign="top">距</td>
            <td>
                <input type="number" class="y" id="SY3">年
                <input type="number" class="d" id="SM3">月
                <input type="number" class="d" id="SD3">日
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <input type="button" value=" 相 差 " onclick="daydiff()">
                <span class="res" id="result2"></span>
            </td>
        </tr>
    </tbody>
</table>

-----
## 二、推算几天后的日期
<table>
    <tbody>
        <tr>
            <td></td>
            <td>
                <input type="number" class="y" id="SY" name="SY">年
                <input type="number" class="d" id="SM" name="SM">月
                <input type="number" class="d" id="SD" name="SD">日（默认今天）
            </td>
        </tr>
        <tr>
            <td style="width:1rem">&nbsp;</td>
            <td>
                <select id="pom">
                    <option value="1">往后</option> 
                    <option value="-1">往前</option>   
                </select>
                <input type="number" class="y" id="dnum">
                <select id="dtype" onchange="sayHello()">
                    <option value="days">日</option>
                    <option value="weeks">星期</option>
                    <option value="months">月</option>  
                </select>   
                <span id="hint"></span>
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <input type="button" value="   是   " onclick="dayadd()">     
                <span id="hint0"></span>
                <span class="res" id="result1"></span>
            </td>
        </tr>
    </tbody>
</table>

<style>
.y {
    width: 7rem;
}
.d {
    width: 4rem;
}
.m {

}
input, select {
    height: 2.4rem;
    font-size: 1.2rem;
}
</style>

<script>
// 页面加载后执行计算
document.addEventListener('DOMContentLoaded', () => {
    const date = new Date();
    document.getElementById('SY2').value = date.getFullYear();
    document.getElementById('SM2').value = date.getMonth() + 1;
    document.getElementById('SD2').value = date.getDate();

    document.getElementById('SY').value = date.getFullYear();
    document.getElementById('SM').value = date.getMonth() + 1;
    document.getElementById('SD').value = date.getDate();
});

function daydiff() {
    const date2 = new Date(document.getElementById('SY2').value, document.getElementById('SM2').value - 1, document.getElementById('SD2').value);
    const date3 = new Date(document.getElementById('SY3').value, document.getElementById('SM3').value - 1, document.getElementById('SD3').value);
    const diff = Math.abs(date2 - date3);
    const calcDays = Math.floor(diff / (1000 * 60 * 60 * 24));
    const calcWeeks = Math.floor(diff / (1000 * 60 * 60 * 24 * 7));
    // 取余
    const calcWeeksRemain = calcDays % 7;
    const calcMonths = Math.floor(diff / (1000 * 60 * 60 * 24 * 30));
    const calcMonthsRemain = calcDays % 30;
    document.getElementById('result2').innerHTML = calcDays + ' 天 = ' + calcWeeks + ' 星期 '+ calcWeeksRemain + ' 天 = ' + calcMonths + ' 月 '+ calcMonthsRemain + ' 天';
}

function dayadd() {
    const date = new Date(document.getElementById('SY').value, document.getElementById('SM').value - 1, document.getElementById('SD').value);
    const pom = document.getElementById('pom').value;
    const dtype = document.getElementById('dtype').value;
    switch (dtype) {
        case 'days':
            // 日期加减
            date.setDate(date.getDate() + document.getElementById('dnum').value * pom);
            break;
        case 'weeks':
            // 周加减
            date.setDate(date.getDate() + document.getElementById('dnum').value * 7 * pom);
            break;
        case 'months':
            // 月份加减
            date.setDate(date.getMonth() + document.getElementById('dnum').value * 30 * pom);
            break;
        default:
            break;
    }
    const week = date.getDay();
    const weekStr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
    const result = date.getFullYear() + ' 年 ' + (date.getMonth() + 1) + ' 月 ' + date.getDate() + ' 日 ' + weekStr[week];
    document.getElementById('result1').innerHTML = result;
}
</script>

<script src="/js/dateCalc/func.js"></script>
