# Notes on ruby

## Count duplicates in an array
    a = [1, 1, 2]
    b = a.inject(Hash.new(0)) {|h,i| h[i] += 1; h }
