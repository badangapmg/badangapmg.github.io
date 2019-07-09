---
layout: post
title:  "Welcome to Jekdddyll!"
date:   2015-04-18 08:43:59
author: Ben Centra
categories: Jekyll
tags:	jekyll welcome
cover:  "/assets/instacode.png"
---

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

## Adding New Posts

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

### Tags and Categories

If you list one or more categories or tags in the front matter of your post, they will be included with the post on the page as links. Clicking the link will bring you to an auto-generated archive page for the category or tag, created using the [jekyll-archive][jekyll-archive] gem.

### Cover Images

To add a cover image to your post, set the "cover" property in the front matter with the relative URL of the image (i.e. <code>cover: "/assets/cover_image.jpg"</code>).

### Code Snippets

You can use [highlight.js][highlight] to add syntax highlight code snippets:

Use the [Liquid][liquid] `{% raw %}{% highlight <language> %}{% endraw %}` tag to add syntax highlighting to code snippets.

For instance, this template...
{% highlight html %}
{% raw %}{% highlight javascript %}    
function demo(string, times) {    
  for (var i = 0; i < times; i++) {    
    console.log(string);    
  }    
}    
demo("hello, world!", 10);
{% endhighlight %}{% endraw %}
{% endhighlight %}

...will come out looking like this:

{% highlight javascript %}
/* 자바 */
int strLen(String s) {
  return s.length()
}
{% endhighlight %}

위의 함수에 null 을 넘기면 NullPointerException이 발생 한다.

이 함수를 코틀린으로 다시 작성 해보자.

{% highlight javascript %}
/* 자바 */
fun strLen(s:String) = s.length
{% endhighlight %}

코틀린에서 이런 함수를 작성 할 때 가장 먼저 답을 알아야 할 질문은 `이 함수가 널을 인자로 받을 수 있는가?`이다.
null이 인자로 들어올 수 없다면 위와 같이 작성 할 수 있으며, 혹시 null 값을 넘기면 컴파일 시 오류가 발생한다.
{% highlight javascript %}
ERROR: Null can not be a value of a non-null type String
{% endhighlight %}

위의 함수가 널과 문자열을 인자로 받을 수 있게 하려면 타입 이름 뒤에 물음표(?)를 명시해야 한다.

{% highlight javascript %}
fun strLen(s:String?) = s.length
{% endhighlight %}


Syntax highlighting is done using [highlight.js][highlight]. You can change the active theme in [head.html](https://github.com/bencentra/centrarium/blob/2dcd73d09e104c3798202b0e14c1db9fa6e77bc7/_includes/head.html#L15).

### Images

Lightbox has been enabled for images. To create the link that'll launch the lightbox, add <code>data-lightbox</code> and <code>data-title</code> attributes to an <code>&lt;a&gt;</code> tag around your <code>&lt;img&gt;</code> tag. The result is:

<a href="//bencentra.com/assets/images/falcon9_large.jpg" data-lightbox="falcon9-large" data-title="Check out the Falcon 9 from SpaceX">
  <img src="//bencentra.com/assets/images/falcon9_small.jpg" title="Check out the Falcon 9 from SpaceX">
</a>

For more information, check out the [Lightbox][lightbox] website.

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
[highlight]:   https://highlightjs.org/
[lightbox]:    http://lokeshdhakar.com/projects/lightbox2/
[jekyll-archive]: https://github.com/jekyll/jekyll-archives
[liquid]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers
