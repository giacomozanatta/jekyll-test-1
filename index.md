---
title: Main Page
---
To do

Code snippet example

{% highlight java %}
public class Main {

    public static void main(String[] args) {
        int x = 10;
        int y = 50;
        int z = x + y;
        int a = 3;
        System.out.println("Hello World");
    }
}
{% endhighlight %}


<h2>Content</h2>
<ul>
    {% assign grouped = site.docs | group_by: 'category' | sort: 'name' %}
    {% for group in grouped %}
        <li class="">
            {% assign items = group.items | sort: 'order' %}
            <a class="content-link" href="{{ site.baseurl }}{{ items.first.url }}">{{ group.name }}</a>
            <ul>
                {% for item in items %}
                    <li class=""><a class="content-link" href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a></li>
                {% endfor %}
            </ul>
        </li>
    {% endfor %}
</ul>