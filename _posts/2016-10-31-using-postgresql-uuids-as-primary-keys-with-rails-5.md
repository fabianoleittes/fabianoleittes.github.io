---
layout: post
title: Using PostgreSQL UUIDs as primary keys with Rails 5
date: 2016-10-31 20:35
categories: rails5 PostgreSQL UUIDs
summary: You can use UUIDs as the primary key on Rails 5 tables in a few simple steps.
---
## Using PostgreSQL UUIDs as primary keys with Rails 5.

PostgreSQL has a [native datatype for UUIDs](https://www.postgresql.org/docs/9.5/static/datatype-uuid.html) and [Rails 4.0+](https://github.com/rails/rails/commit/bc8ebefe9825dbff2cffa29ff015a1e7a31f9812) has [built-in support](http://guides.rubyonrails.org/active_record_postgresql.html#uuid-primary-keys) for using them as the primary key.

You can use UUIDs as the primary key on Rails 5 tables in a few simple steps.

### Enable the extension.
Before using UUIDs in PostgreSQL, you must first enable the [uuid-ossp](https://www.postgresql.org/docs/9.5/static/uuid-ossp.html) extension.

If you're already using UUIDs, you've already done this step. To enable the extension, generate a migration.

```
$ bin/rails g migration enable_uuid_extension
```
That should generate a migration that looks like this.

```ruby
class EnableUuidExtension < ActiveRecord::Migration[5.0]
  def change
    enable_extension 'uuid-ossp'
  end
end
```

### Add ability to default to [uuid](https://github.com/rails/rails/compare/f94e328cf801...5a47639aac45#diff-b3e4617f47cb06d8e22542a2c2e40d97) as primary key when generating database migrations.

Put this in your config/application.rb file.

```ruby
config.generators do |g|
  g.orm :active_record, primary_key_type: :uuid
end
```
Or create a new file at config/initializers/generators.rb. Best practices.

```ruby
Rails.application.config.generators do |g|
  g.orm :active_record, primary_key_type: :uuid
end
```

### Now, just use rails generate.

```
$ bin/rails g model User
```

Now, if you don't want it as the default for all migrations.
Does not ability default UUID.

You can use so for single migration.

```
bin/rails g model User --primary_key_type=uuid
```

Do you use UUID as your primary keys? Do you have anything to add? Leave your comment!

That's all folks, until next time!!!
