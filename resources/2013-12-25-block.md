---
title: Block
date: 2013-12-25 00:14:39
categories:
- Foo
tags:
---

This post is used for testing tag plugins. See [docs](http://zespia.tw/hexo/docs/tag-plugins.html) for more info.

## Block Quote

### Normal blockquote

> Praesent diam elit, interdum ut pulvinar placerat, imperdiet at magna.

## Code Block

### Inline code block

This is a inline code block: `python`, `print 'helloworld'`.

### Normal code block

```
alert('Hello World!');
```

    print "Hello world"

### Highlight code block

```python
print "Hello world"
```

{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}

### Gist

{% gist 996818 %}
