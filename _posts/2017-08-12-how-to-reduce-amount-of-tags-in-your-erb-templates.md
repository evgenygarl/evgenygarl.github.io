---
layout: post
title:  "How to reduce amount of tags in your ERB templates"
---

Embedded Ruby (or ERB) template syntax is well-know for any Ruby on Rails developer. Someone likes it, someone does not. Many developers prefer ERB alternatives (like [Slim](http://slim-lang.com/) or [Haml](http://haml.info/)). I worked with Slim on several projects, and I really like it!  
But sometimes you have to deal with ERB despite of your preferences. And the other dat I've found a way how to reduce amount of tags in such templates. Let me share it with you.

<!--more-->

ERB has two kinds of tags: non-printing `<% %>` tag and expression-printing `<%=  %>` tag. Also, ERB has a comment tag (`<%# %>`), but it is not necessary for this topic.

Here is an example of simple ERB template from [Ruby on Rails Guides](http://guides.rubyonrails.org/layouts_and_rendering.html):

```erb
<h2><%= article.title %></h2>

<% if local_assigns[:full] %>
  <%= simple_format article.body %>
<% else %>
  <%= truncate article.body %>
<% end %>
```

I guess, everything is clear here. We render article's title in `h2` tag. And bellow the title we render article's body: if `local_assigns[:full]` is truthy, full body is rendered, otherwise we render truncated body. As I already said, everything is clear!

Several weeks ago when I was learning [Ruby web framework Hanami](http://hanamirb.org/) I've found the other way to write this template in Hanami's guides. Here it is:

```erb
<h2><%= article.title %></h2>

<%=
  if local_assigns[:full]
    simple_format article.body
  else
    truncate article.body
  end
%>
```

As you can see, we can replace all ERB tags used in `if/else` statement with just one expression-printing `<%=  %>` tag. And it returns us the same result.  
I think it's really cool! We have the same result, but with less code. And I've already started using this approach on my current project.

I hope, this little trick will help you to decrease number of tags in your ERB templates. 😊
