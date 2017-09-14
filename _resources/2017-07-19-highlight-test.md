---
title: Highlight Test
categories:
 - Test
tags:
---

This is a highlight test.

# Normal block

```
alert('Hello World!');
```

    print 'helloworld'

# Highlight block

```javascript
alert( 'Hello, world!' );
```

```python
print 'helloworld'
```

```ruby
def foo
  puts 'foo'
end
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

```c++
#include <iostream>

using namespace std;

void foo(int arg1, int arg2)
{

}

int main()
{
  string str;
  foo(1, 2);
  cout << "Hello World" << endl;
  return 0;
}
```
