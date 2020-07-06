> 【译】 https://uglyduck.ca/quick-dirty-theme-switcher/

> 一种快速的主题切换


最近，我在我的网站上添加了一个非常简单的配色方案(主题)切换器。你可以在网站的页脚中切换这个简单的颜色切换器，来查看实际效果。我想，有人也想要将次功能添加到网站/项目中。于是我写下这篇短文来解释如何实现。让我们开始吧。

![](https://uglyduck.ca/wp-content/uploads/2020/06/site-color-schemes.gif)
_我的网站正在运行中的主题切换器_

# HTML页面
首先我们需要写几个"按钮"用来切换主题。(注意：同样也可以使用 `<select>`和`<option>`标签来完成这个切换)
```
<div class="color-select">
    <button onclick="toggleDefaultTheme()"></button>
    <button onclick="toggleSecondTheme()"></button>
    <button onclick="toggleThirdTheme()"></button>
</div>
```
如上所示。先不用担心`onclick`中的具体实现，再添加js代码时，我们再来看这里。现在，在Html页面我们只剩下添加一个默认的class在<html>上面，如下所示： 
```
<html class="theme-default">
```

# CSS
接下来，我们需要编写 `颜色选择`按钮样式和 网站自定义主题样式。我们先从主题样式开始。

为了使主题可以无缝切换，我们将使用**css变量**来设置颜色集合。 

```
.theme-default {
   --accent-color: #72f1b8;
   --font-color: #34294f;
}

.theme-second {
    --accent-color: #FFBF00;
    --font-color: #59316B;
}

.theme-third {
    --accent-color: #d9455f;
    --font-color: #303960;
}

body {
    background-color: var(--accent-color);
    color: var(--font-color);
}
```
之后，我们设置面向用户的，按钮颜色:

```
.color-select button {
    -moz-appearance: none;
    appearance: none;
    border: 2px solid;
    border-radius: 9999px;
    cursor: pointer;
    height: 20px;
    margin: 0 0.8rem 0.8rem 0;
    outline: 0;
    width: 20px;
}

/* Style each swatch to match the corresponding theme */
.color-select button:nth-child(1) { background: #72f1b8; border-color: #34294f; }
.color-select button:nth-child(2) { background: #FFBF00; border-color: #59316B; }
.color-select button:nth-child(3) { background: #d9455f; border-color: #303960; }
```

# JS

我们需要让每个控制按钮触发相应的主题，并且改变我们之前写在`<html>`上的`theme-default`类
 
 我们还需要将用户选择存储到`localStorage`，以便当用户再次加载或进入其他界面时所选样式依然存在。 
```
// Set a given theme/color-scheme
function setTheme(themeName) {
    localStorage.setItem('theme', themeName);
    document.documentElement.className = themeName;
}

// Toggle between color themes
function toggleDefaultTheme() {
    if (localStorage.getItem('theme') !== 'theme-default'){
        setTheme('theme-default');
    }
}
function toggleSecondTheme() {
    if (localStorage.getItem('theme') !== 'theme-second'){
        setTheme('theme-second');
    }
}
function toggleThirdTheme() {
    if (localStorage.getItem('theme') !== 'theme-third'){
        setTheme('theme-third');
    }
}

// Immediately set the theme on initial load
(function () {
    if (localStorage.getItem('theme') === 'theme-default') {
        setTheme('theme-default');
    }
    if (localStorage.getItem('theme') === 'theme-second') {
        setTheme('theme-second');
    }
    if (localStorage.getItem('theme') === 'theme-third') {
        setTheme('theme-third');
    }
})();
```
如上所示。通过这些代码让你可以定义任何你想要呈现的主题----让你拥有无限可能!

# 额外改进

如果用户禁用了JavaScript，你改进这个概念，甚至进一步隐藏颜色选择项。出于我的需要，我觉得如果JavaScript被禁用，保留不起作用的色样选择器是一个很好的权衡。但是，您的项目/站点可能需要更好的备用计划。
