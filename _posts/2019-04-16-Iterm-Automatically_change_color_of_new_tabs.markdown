---
layout: post
title:  "iTerm2 - Automatically change color of new tabs"
date:   2019-04-16 07:04:16 -0300
tags: computers macos ssh terminal jot
---
Thousands of tabs opened at the same time and you would love some color hint?

[iTerm2](https://iterm2.com/) has some non-standard [escape codes](https://iterm2.com/documentation-escape-codes.html) that you can use to alter its configuration and trigger actions. These include changing the tab color.

The following use of the escape codes will make our tab red ([RGB](https://en.wikipedia.org/wiki/RGB_color_model) is used):
```bash
$ echo -e "\033]6;1;bg;red;brightness;255\a"
$ echo -e "\033]6;1;bg;green;brightness;0\a"
$ echo -e "\033]6;1;bg;blue;brightness;0\a"
```

We can integrate that with with "[jot](https://www.unix.com/man-page/FreeBSD/1/jot/)", to generate randon numbers between 0 and 255, and append it to our **.bashrc** file. Example:

```bash
function tcolor {
   # function to automatically change the color of tab
   echo -e "\033]6;1;bg;red;brightness;`jot -r 1 0 255`\a\033]6;1;bg;green;brightness;`jot -r 1 0 255`\a\033]6;1;bg;blue;brightness;`jot -r 1 0 255`\a"
}

# Calling the function
tcolor
```


That's it! Every time you create a new session (open a new tab), it will get a random color.



### Keep reading
* <https://iterm2.com/documentation-escape-codes.html>

