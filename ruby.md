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
