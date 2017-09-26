---
comments: true
layout: post
date:   2017-09-26 18:00:00 +0300
title:  "How To Recognize Path Direction In Your Rails App"
description: "Working on your Rails application, sometimes you need to recognize which controller and action respond to request with some path and method. The simplest way to do it is to send a particular request to your application and see what does application log show you. But this isn’t the optimal way. Rails provides you a much better way to check it. Let me acquaint you with it."
---

Working on your Rails application, sometimes you need to recognize which controller and action respond to request with some path and method. The simplest way to do it is to send a particular request to your application and see what does application log show you 😉 But this isn’t the optimal way. Rails provides you a much better way to check it. Let me acquaint you with it 😉

<!--more-->

Imagine you want to recognize where `PUT /users/5948` request is directed in your application. Rails provides you `Rails.application.routes.recognize_path` method to work it out!

So, let’s try:

```ruby
Rails.application.routes.recognize_path('/users/5948')
#=> {:controller=>"users", :action=>"show", :id=>"5948"}
```

Wait, but it seems to display direction for `GET /users/5948` request. That’s correct! `GET` request method is used by default there. To specify the request method you should pass it with (surprisingly) `method` key as the second argument:

```ruby
Rails.application.routes.recognize_path('/users/5948', method: :put)
#=> {:controller=>"users", :action=>"update", :id=>"5948"}
```

Now it displays the correct direction. And we no longer need to check logs for such things. That’s cool, isn’t it? 😊  

Also, this method handles unknown request. If you try to pass unknown path/method combination the method will raise `ActionController::RoutingError` error:

```ruby
Rails.application.routes.recognize_path('/some_invalid_path')
ActionController::RoutingError: No route matches "/some_invalid_path"
```

I hope `Rails.application.routes.recognize_path` will help you out by saving your time for other more interesting stuff 😊 
