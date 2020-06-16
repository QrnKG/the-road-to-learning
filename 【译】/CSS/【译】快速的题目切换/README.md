> 【译】 https://uglyduck.ca/quick-dirty-theme-switcher/

> 一种快速的主题切换


最近，我在我的网站上添加了一个非常简单的配色方案(主题)切换器。你可以在网站的页脚中切换这个简单的颜色切换器，来查看实际效果。我想，有人也想要将次功能添加到网站/项目中。于是我写下这篇短文来解释如何实现。让我们开始吧。

![](https://uglyduck.ca/wp-content/uploads/2020/06/site-color-schemes.gif)
_我的网站正在运行中的主题切换器_

# The HTML
First we need to include the “buttons” that will trigger the theme to switch based on which one is selected. (Note: you could always render these as `options` in a `select` element if you preferred that method)

```
<div class="color-select">
    <button onclick="toggleDefaultTheme()"></button>
    <button onclick="toggleSecondTheme()"></button>
    <button onclick="toggleThirdTheme()"></button>
</div>
```
That’s it! Don’t worry too much about the `onclick` parameter right now, we’ll come back to that when adding our JavaScript. The only remaining item is adding a default theme class to our `html` element, like so:
```
<html class="theme-default">
```

# The CSS

Next we need to style both the `color-select` buttons, along with the custom color schemes that will alter the entire website. We will start with the color schemes.

For these themes to swap seamlessly between each other, we will be setting our altering color sets as CSS variables:

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
Finally, we style the user-facing color swatches:

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

# The JavaScript

We need to have each color swatch button trigger it’s corresponding theme and swap out the `theme-default` class that we have originally attached to the main `html` element. We also need to store what the user has selected into `localStorage`, so their choice persists when reloading or navigating to other pages.

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

And that’s it! Now it just depends on how custom you want each individual theme style to be. The possibilities are endless!

# Extra Improvements
You could improve this concept even further hiding the color-select item if the user has JavaScript disabled. For my needs, I felt it was a fine trade-off to keep the non-functioning color swatch pickers if JavaScript was disabled. However, your project/site might need better fallbacks.