# Notes on Ruby on Rails

## SELECT DISTINCT field from Model

```ruby
Model.distinct.pluck(:field)
```

## Count records by field

```ruby
Model.group(:field).count(:field)
```

## Count associated records

https://stackoverflow.com/a/47566622

```ruby
Comment.joins(:post).group(:post_id).count(:post_id)
```

## Find records with duplicate fields

```ruby
duplicate_values = Model.group(:field).having(Model.arel_table[:field].count.gt(1)).count.keys
Model.where(field: duplicate_values).group_by(&:field).each do |value, records|
  puts "The records with ids #{records.map(&:id).to_sentence} have field set to #{value}"
end
```

## Records with or without associations

http://stackoverflow.com/a/19080147

```ruby
Person.includes(:friends).where('friends.person_id IS NOT NULL')
Person.includes(:friends).where(:friends => { :person_id => nil })
```

## Change hash to query string

```ruby
{ :a => 1 }.to_query
```

## Rollback a migration

```bash
rake db:rollback STEP=2
```

## Include vs Joins

> When you join comments, you are asking for posts that have comments- an inner join by default. When you include comments, you are asking for all posts- an outer join.

http://stackoverflow.com/a/4315729/167369

## SQL logging in the console

https://stackoverflow.com/a/2936016/167369

```ruby
ActiveRecord::Base.logger = Logger.new(STDOUT)
```

## Execute an arbitrary SQL query

```ruby
ActiveRecord::Base.connection.exec_query('describe wires')
```

## Get current database connection config

https://stackoverflow.com/questions/8673193

```ruby
ActiveRecord::Base.connection_db_config.configuration_hash
# Before Rails 6.2
ActiveRecord::Base.connection_config # or User.connection_config
```

## Generate a decimal migration with precision and scale

```sh
rails generate migration add_field_to_model 'field:decimal{8,2}'
```

## Manually manipulate schema_versions in the console

http://stackoverflow.com/a/28446829/167369

```ruby
class SchemaMigration < ActiveRecord::Base; self.primary_key = :version; end
```

## Process records in multiple threads

You could put a script like this in `/bin` and make it executable (`chmod +x`).

Note that this might not evenly distribute the work. Because `in_batches` groups by `id` and any given batch of ids might not need processing (or be deleted).

```ruby
#!/usr/bin/env ruby

require_relative '../config/environment'

NUMBER_OF_THREADS = 5
REPORTING_INTERVAL = 25

def process(record)
  puts "Insert business logic here." # TODO
end

records = Record.all

threads = []
time = Time.current
i = 0
total_count = records.size
last_id = records.order(id: :asc).last.id
batch_size = (last_id / NUMBER_OF_THREADS.to_f).ceil
NUMBER_OF_THREADS.times do |thread_id|
  threads << Thread.new do
    begin
      j = 0
      start = thread_id * batch_size + 1
      finish = start + batch_size - 1
      puts "thread_id=#{thread_id}, starting_id=#{start}, finishing_id=#{finish}"
      records.in_batches(start: start, finish: finish).each_record do |record|
        process(record)
        i += 1
        j += 1
        if (j % REPORTING_INTERVAL).zero?
          remaining = ((Time.current - time) / i) * (total_count - i)
          puts "thread_id=#{thread_id}, i=#{i}/#{total_count}, j=#{j}/#{batch_size}, time_remaining=#{remaining / 60.0}m"
        end
      end
    rescue => error
      puts "#{error}\n\n### RETRYING ###\n\n"
      retry
    end
  end
end
threads.each(&:join)
puts "Total time: #{(Time.current - time) / 60} minutes"
```
