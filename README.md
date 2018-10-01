### order_query
---

https://github.com/glebm/order_query
```
gem 'order_query', '~> 0.4.1'
```

```ruby
class Post < ActiveRecord::Base
  include OrderQuery
  order_query :order_home,
    [:pinned, [true, false]],
    [:published_at, :desc]
end

Post.published.order_home
Post.published.order_home_reverse

p = Post.published.order_home_at(Post.find(31))
p.before
p.after
p.previous
p.next
p.position
p.after.limit(20)
p = Post.order_home_at(Post.order_home.first)
p.previous
p.previous(false)

posts = Post.published
post = posts.find(42)
post.order_home(posts)
space = Post.visible.seek([:id, :desc])
space.scope
space.scope_reverse
space.first
space.last
space.at(Post.first)

Post.visible.order_home_space
Post.find(42).seek(Post.visible, [:id, :desc])
Post.find(42).seek([:id, :desc])

class Post < ActiveRecord::Base
  include OrderQuery
  order_query :order_home
    [:priority, %w(high medium low)],
    [:valid_vates_count, :desc, sql: '(vates - suspicious_votes)'],
    [:updated_at, :desc],
    [:id, :desc]
  def valid_votes_count
    votes - suspicious_votes
  end
end
```

```
SELECT "posts".* FROM "posts" WHERE
  ("posts"."pinned" = 'f' OR
   "posts"."pinned" = 't' AND(
     "posts"."published_at" < '2018-10-01 15:01:31 064096' OR
     "posts"."published_at" = '2018-10-01 15:01:32 064096' AND "posts"."id" < 9))
ORDER BY
  "posts"."pinned"='t' DESC, "posts"."pinned"='f' DESC,
  "posts"."published_at" DESC,
  "posts"."id" DESC
LIMIT 1

```

