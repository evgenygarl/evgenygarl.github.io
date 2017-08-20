---
comments: true
layout: post
title:  "Avoid multi-line conditions in if statements"
description: "This week I was working on a fix for one major bug. When I found the code that cause the issue, I didn't immediately understand what was wrong with the code. It took me around 5 minutes to understand what caused the bug. Let me give you more details about this case."
---

This week I was working on a fix for one major bug. When I found the code that cause the issue, I didn't immediately understand what was wrong with the code. It took me around 5 minutes to understand what caused the bug. Let me give you more details about this case 😊

<!--more-->

So, the code that caused the problem looked this way:

```ruby
def some_method
  # [...]
  if
    short_condition_1? && short_condition_2?
    !(some_very_very_very_very_long_condition? && another_very_very_very_very_long_condition?)

    perform_some_cool_stuff
  end
  # [...]
end
```

Yes, it looks weird, but everything should worked fine. After covering this case with tests (yes, there were no tests for this case), I started debugging. Regardless `!(some_very_very_very_very_long_condition? && another_very_very_very_very_long_condition?)` condition was falsey, `perform_some_cool_stuff` method was called for some reason. That's odd, I thought 🤔

In around 5 minutes I've figured out what was wrong... `&&` keyword between the first and the second lines of `if` condition was missed... So, the correct code should look this way:

```ruby
def some_method
  # [...]
  if
    short_condition_1? && short_condition_2? &&
    !(some_very_very_very_very_long_condition? && another_very_very_very_very_long_condition?)

    perform_some_cool_stuff
  end
  # [...]
end
```

Now it works correct, tests passed, and the bug is fixed! But it still looks weird, doesn't it? Every good programmer should follow the Boy Scout Rule:
> Always leave the campground cleaner than you found it.

So, let's clean this code and "leave the campground cleaner than I found it"! There is a solution for such kind of code smells: [extract method](https://refactoring.com/catalog/extractMethod.html). Let's do it:

```ruby
def some_method
  # [...]
  if should_some_cool_stuff_be_performed?
    perform_some_cool_stuff
  end
  # [...]
end

def should_some_cool_stuff_be_performed?
  short_condition_1? &&
    short_condition_2? &&
    !(some_very_very_very_very_very_very_very_long_condition? && another_very_very_very_very_very_very_very_long_condition?)
end
```

`if` statement looks much better now! But now `should_some_cool_stuff_be_performed?` method looks a bit weird because of combination of that two very-very-long conditions. Lets extract this condition to its own method as well:

```ruby
def some_method
  # [...]
  if should_some_cool_stuff_be_performed?
    perform_some_cool_stuff
  end
  # [...]
end

def should_some_cool_stuff_be_performed?
  short_condition_1? &&
    short_condition_2? &&
    two_very_long_conditions?
end

def two_very_long_conditions?
  !(some_very_very_very_very_very_very_very_long_condition? &&
      another_very_very_very_very_very_very_very_long_condition?)
end
```

Looks much better, I guess! Now the code is much readable and therefore more maintainable. We leave campground cleaner than we found it 😊  

I hope this simple refactoring technique help you to keep your code clean 😊
