# Notes on ruby

## Count duplicates in an array

```ruby
a = [1, 1, 2]
b = a.inject(Hash.new(0)) {|h,i| h[i] += 1; h }
```

## Benchmark in the console

```ruby
# require 'benchmark'
Benchmark.bm do |bm|
  bm.report('first:') { [1, 2].include?(2) }
  bm.report('second:') { 1 == 2 || 2 == 2 }
end;0
```

## Reuse YAML block

```yaml
DEFAULTS: &DEFAULTS
  password_digest: 4bf32ea4c909968aed72131cebf20d08d3a6d0ca
base:
  <<: *DEFAULTS
```

## Regex named capture groups

```ruby
'Widgets: 17'.match(/(?<name>.*):\s*(?<count>\d+)/)
```

## Arbitrarily deep nested hash

http://stackoverflow.com/a/19304522

```ruby
hash = Hash.new { |h,k| h[k] = Hash.new(&h.default_proc)
```

## Threads

```ruby
threads = []
5.times {
  threads << Thread.new { puts "Do work" }
}
threads.each(&:join)
```
