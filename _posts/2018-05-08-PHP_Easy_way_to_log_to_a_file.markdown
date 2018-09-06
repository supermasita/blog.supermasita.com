---
layout: post
title:  "PHP - Easy way to log to a file"
date:   2018-05-08 07:04:16 -0300
tags: php debug
---
Not fancy, but it works for me every time.

If you are trying to find a error, or understand the workflow of a PHP code, you can add a simple line with "**file_put_contents**" to log variables or text to a file.

Example:
{% highlight php %}
// Using the value of a variable
$targetUrl = "http://$host/$appId/$version/";
file_put_contents("/tmp/test.log", $targetUrl . PHP_EOL, FILE_APPEND);
 
// Using simple hardcoded text
if($this->isLive)
{
        $targetUrl .= "getLiveURL";
        if(is_null($url) || $url == "")
                $url = $urlPrefix;
        file_put_contents("/tmp/test.log", "is live" . PHP_EOL, FILE_APPEND);
}
 
else
        $targetUrl .= "getVodURL";
        file_put_contents("/tmp/test.log", "is vod" . PHP_EOL, FILE_APPEND);
 
// Using PRINT_R
$parts = parse_url($this->http_url);
file_put_contents("/tmp/test.log", print_r($parts, true), FILE_APPEND);
{% endhighlight %}
