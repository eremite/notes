# Notes on Ruby on Rails

## SELECT DISTINCT field from Model

```ruby
Model.distinct.pluck(:field)
```

## Count records by field

```ruby
Model.group(:field).count(:field)
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

## Some options to pass to guard for long running test suites

```ruby
options = {
  :keep_failed => false,
  :all_after_pass => false,
}
```

## SQL logging in the console

https://stackoverflow.com/a/2936016/167369

```ruby
ActiveRecord::Base.logger = Logger.new(STDOUT)
```

## Execute an arbitrary SQL query

```ruby
ActiveRecord::Base.connection.exec_query('describe wires')
```

## Migrate to the last migration on the master branch

```bash
bundle exec rake db:migrate VERSION=`git ls-tree --name-only --full-tree master:db/migrate | tail -n1`
```

## List all rake tasks

```
rake -T # those with descriptions
rake -P # all
```

## Starting a new project (with Docker)

```bash
APP=appname

#docker run --rm -v $(pwd):/usr/src/$APP rails rails new /usr/src/$APP
docker run --rm -it -e APP=$APP --volumes-from data rails bash --login
gem install rails:4.2.0.beta4
cd /data
/usr/local/bundle/gems/railties-4.2.0.beta4/bin/rails new
exit

cd /data/$APP
sudo chown -R dev:dev .
git init
mkdir -p $META/$APP/symlinks/.git
touch $META/$APP/symlinks/notes.md
mv .git/config $META/$APP/symlinks/.git/
.git/hooks/create_symlinks

git add .
git commit -m "Initial commit of bare Rails app."

cp $META/templates/dockerignore .dockerignore
cp $META/templates/fig.yml .
cp $META/templates/Dockerfile .
cp $META/templates/database.yml config/
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

## Get current database connection config

https://stackoverflow.com/questions/8673193

```ruby
ActiveRecord::Base.connection_config # or User.connection_config
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
