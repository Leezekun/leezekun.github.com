---
layout: page
title: About
description: 李泽坤的个人主页
keywords: Leezekun，李泽坤
comments: true
menu: 关于
permalink: /about/
---

空 风 波 梦

## 联系

* GitHub：[@leezekun](https://github.com/leezekun)
* 微博: [@mzlogin](http://weibo.com/kuhnzzang)

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
