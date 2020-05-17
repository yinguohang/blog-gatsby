---
title: "Tampermonkey Fanatical Bundle Key Extractor | 油猴Fanatical包站Key提取"  
date: "2018-12-20 10:37:00 +0800"  
template: "post"
draft: false
slug: "tampermonkey-fanatical"
category: "other"
description: "Tampermonkey Fanatical Bundle Key Extractor | 油猴Fanatical包站Key提取" 
tags:
  - "javascript"
---

### 脚本内容

自动点击界面内“Reveal Key”，然后复制所有的激活码并输出到Console。

```javascript
// ==UserScript==
// @name         Fanatical Bundle Key Collector
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       Guohang Yin
// @match        https://www.fanatical.com/en/orders/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    var revealAll = function() {
        var buttons = document.getElementsByClassName("btn-block")
        for (var i = 0; i < buttons.length; i++) {
            buttons[i].click()
        }
        setTimeout(function() {
            var codes = document.getElementsByClassName("text-center")
            var text = ""
            for (var i = 0; i < codes.length; i++) {
                text += codes[i].value + "\n";
            }
            console.log(text)
        }, 3000)
    }
    window.addEventListener('load', function() {
        console.log(document.getElementsByClassName("mt-4"))
        var ui = document.getElementsByClassName("mt-4")[0]
        var holder = document.createElement('div');
        holder.innerHTML = "<button id='revealAll' class='btn'>Reveal All</button><!--<button class='btn'>Reveal Left</button>-->"
        ui.appendChild(holder)
        document.getElementById("revealAll").onclick = revealAll
    }, false);
})();
```
