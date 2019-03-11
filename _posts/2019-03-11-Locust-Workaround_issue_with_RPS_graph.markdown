---
layout: post
title:  "Locust - Workaround issue with RPS graph"
date:   2019-03-11 20:44:16 -0300
tags: computers python locust benchmarking javascript stress workaround
---
Yes, we love you [Locust](https://locust.io/) - but why don't show us the right RPS in the stats!? Here is a workaround.

As many users have found (**hoangphucITJP** [reported](https://github.com/locustio/locust/issues/889)), there is a strange behaviour in current version of Locust, where "Total Requests per Second" graph RPS drops to cero after some time after the test started, despite the RPS number on the header keeps rising (pic below):


![](https://user-images.githubusercontent.com/14938527/45725860-f873a200-bbe6-11e8-8fcf-3487d0afe8cc.png)


User **klloveal** realized that "_the /stats/request endpoint stops returning the 'Total' stat block at the end of the stats list_". With that in mind, I had a look at /stats/request response and realized that the header RPS was being filled by a different variable called 'report.total_rps' - ipso facto, the following super simple change: <https://github.com/locustio/locust/pull/977/files>

That's it!

> Coward disclaimer: I don't know if this change could have other consequences and it is not addressing the root cause.

### Keep reading
* [Locust](https://locust.io/)
* [kStress](https://github.com/supermasita/kstress): An implementation of Locust to stress Kaltura's eCDN 
