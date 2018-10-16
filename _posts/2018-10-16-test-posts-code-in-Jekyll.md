---
layout: post
title: "test posts code in Jekyll"
category: jekyll update
tags: [Python]
---
Testing code show in jekyll blog:

<head>
    <title>Rouge</title>
    <link media="all" rel="stylesheet" type="text/css" href="../assets/rouge/rouge.css" />
</head>

<body>
	{% highlight python %}
import pandas as pd
for i in range(100):
	print(i)
f= lambda s : int(s)
	{% endhighlight %}
</body>

Let's see another code :

<body>
print('Hello colorful world!')
</body>

So I know that for code highlight we must contain \<% highlight python %>and\<% endhighlight %>