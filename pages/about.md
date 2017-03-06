---
layout: page
title: About
description: 李泽坤的个人主页
keywords: Leezekun，李泽坤
comments: true
menu: 关于
permalink: /about/
---
李泽坤
山东大学计算机科学与技术学院
14级基地班

## 联系
* GitHub：[@leezekun](https://github.com/leezekun)
* 微博: [@kuhnzzang](http://weibo.com/kuhnzzang)

## Skill Keywords

#### Software Engineer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_software_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Mobile Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_mobile_app_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Windows Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_windows_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>
