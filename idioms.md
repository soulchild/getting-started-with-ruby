# Ruby idioms

## Shortcuts

### Calculated default value

Assign calculated value to bar if bar is undefined:

```ruby
bar ||= begin
 # calculate stuff which gets assigned to bar
end 
```

## Classes & methods

### Class methods

#### Standard ways

```ruby
class Person
 def self.foo
   puts "I'm a class method"
 end
end

Person.foo
```   
    
#### Somewhat esoteric, but quite often seen in the wild (especially in Rails land) 

```ruby
class Person
 class << self
   def foo
     puts "I'm a class method"
   end
 end
end
    
Person.foo
```

### Arbitrary number of parameters

#### Named parameters

```ruby
def foo(args)
 puts "%s %s" % [ args[:firstname], args[:lastname] ]
end
foo( { :firstname => "Vince", :lastname => "Noir" } )

# or even shorter without braces
foo( :firstname => "Howard", :lastname => "Moon" )
```

#### Arbitrary number of parameters with the splat operator

```ruby
def foo(*args)
 puts args.join " | "
end
foo("Bollo")
foo("Tony", "Harrison")
```

Or if you don't care whether there are parameters:

```ruby
def foo(*)  
end
```
   
### Building DSLs with yield and instance_eval

See: [http://rubylearning.com/blog/2010/11/30/how-do-i-build-dsls-with-yield-and-instance_eval/](How do I build DSLs with yield and instance_eval?)

#### yield

```ruby
class Recipe
 attr_accessor :ingredients
 
 def initialize
   self.ingredients = []
   yield self if block_given?
 end
 
 def ingredient(name)
     self.ingredients << name
 end
end

Recipe.new do |r|
 r.ingredient "3 Eggs"
 r.ingredient "100g Cheese"
end
```

#### instance_eval

```ruby
class Recipe
 attr_accessor :ingredient
 
 def initialize(&block)
   self.ingredients = []
   instance_eval &block
 end
 
 def ingredient(name)
   self.ingredients << name
 end
end
    
Recipe.new do
 ingredient "3 Eggs"
 ingredient "100g Cheese"
end
```

### Monkey patching

```ruby
class Gorilla
 def say
   puts "I gotta bad feeling about this"
 end
end
    
# somewhere else, we can just re-open the class and monkey-patch it
class Gorilla
 def say
   puts "I dj at fabric"
 end
end
```