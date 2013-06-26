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
Model.uniq.pluck(:field)
Model.find(:all, :select => 'DISTINCT field').map(&:field)
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
