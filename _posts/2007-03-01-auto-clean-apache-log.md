---
title: 自动删除7天前的apache日志
tags:
- Linux
- Apache
- Log
---


```bash
#!/usr/local/bin/bash
# del front 7 day apache log
/usr/bin/find /usr/website/apache2/logs -name "*.log" -ctime +7 -exec rm {} \;

{% endhighlight %}
```