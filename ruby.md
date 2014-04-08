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

## Installing a ruby (with brew and rbenv)

```bash
brew uninstall ruby-build && brew install --HEAD ruby-build
rbenv install --list # See available versions
rbenv install 3.0.0
CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl) --with-readline-dir=$(brew --prefix readline)" rbenv install 2.0.0-p195 # with options
```

## Remove all gems

```bash
gem list | cut -d" " -f1 | xargs gem uninstall -aIx
```

## Reuse YAML block

```yaml
DEFAULTS: &DEFAULTS
  password_digest: 4bf32ea4c909968aed72131cebf20d08d3a6d0ca
base:
  <<: *DEFAULTS
```
