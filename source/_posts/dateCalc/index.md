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
                <select id="dtype">
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

<script src="/js/dateCalc/func.js"></script>
