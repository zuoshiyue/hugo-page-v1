+++
title = '获取网站token脚本代码'
date = 2025-03-05T10:12:44+08:00
menu = ['posts']
tags = ['游戏', '鸣潮','脚本']
toc = true
+++


以库街区为例子，除了F12获取token信息外，如何快速获取登陆的token或者cookie的js脚本示例：
```
javascript: (function () {

    const token = localStorage.getItem('auth_token');
    if (token) {
        navigator.clipboard.writeText(token).then(() => {
            alert('token 已复制到剪贴板: ' + token);
        }).catch((err) => {
            alert('复制 token 到剪贴板时出错: ' + err);
        });
    } else {
        alert('未找到 token');
    }
})();

```