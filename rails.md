# Notes on Ruby on Rails

## Paste into console to pretend to download all uploaded images

```ruby
site_path = "http://www.example.com"
[
  [Photo, :image],
].each do |klass, method|
  bases = {}
  FileUtils.rm_rf "#{Rails.root}/public/system/#{klass.to_s.underscore.pluralize}"
  klass.all.each do |model|
    attachment = model.send(method)
    next unless attachment.present?
    attachment.styles.keys.each do |style|
      FileUtils.mkdir_p File.dirname(attachment.path(style))
      if bases[style].nil?
        bases[style] = "#{Rails.root}/public/system/#{klass.to_s.underscore.pluralize}/#{style}.jpg"
        `wget #{site_path}#{attachment.url(style)} -O #{bases[style]}`
      end
      FileUtils.ln_s bases[style], attachment.path(style)
    end
  end
end
```

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

http://stackoverflow.com/questions/1344232#answer-1576221

```ruby
# Turn SQL logging *on* in Rails 2
ActiveRecord::Base.connection.instance_variable_set :@logger, Logger.new(STDOUT)

# Turn SQL logging *off* in Rails 3
ActiveRecord::Base.logger = nil
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
