---
layout: post
title: Use hashes ActiveRecord:Enum
category: posts
---

Did you know you can use **Hash** instead of **Array** to define enum's in ActiveRecord?
Well I recently did and it's pretty rad.

```ruby
# So instead of
class Post < ApplicationRecord
  enum status %w(draft published)
end

# You can use
class Post < ApplicationRecord
  enum status { draft: "draft", published: "published" }
end
```

You might ask "why? that looks worse". But when you look into the database

```
# using array
   id   | status
--------+----------
    1   |        1
    2   |        0
    3   |        1

# using hash
   id   | status
--------+-----------
    1   | published
    2   | draft
    3   | published
```

Much nicer yeah?

There is some performance overhead as `integer` comparison is faster then `varchar` in most databases but with well selected indexes it shouldn't give you too much grief.

Finally, when your model needs a new state adding a new value to the enum is more explicit.

```ruby
class Post < ApplicationRecord
  enum status { draft: "draft", reviewing: "reviewing", published: "published" }
end
```

Removing the potential bug where changing the order of an enum changes the meaning of the data backing it.
