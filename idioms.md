# Ruby idioms

## Shortcuts

### Calculated default value

Assign calculated value to bar if bar is undefined:

    bar ||= begin
      # calculate stuff which gets assigned to bar
    end 

## Classes & methods

### Class methods

#### Standard ways

    class Person
      def self.foo
        puts "I'm a class method"
      end
    end
    
    Person.foo
    
#### Somewhat esoteric, but quite often seen in the wild (especially in Rails land) 

    class Person
      class << self
        def foo
          puts "I'm a class method"
        end
      end
    end
    
    Person.foo

### Arbitrary number of parameters

#### Named parameters

    def foo(args)
      puts "%s %s" % [ args[:firstname], args[:lastname] ]
    end
    foo( { :firstname => "Vince", :lastname => "Noir" } )

    # or even shorter without braces
    foo( :firstname => "Howard", :lastname => "Moon" )

#### Arbitrary number of parameters with the splat operator
 
    def foo(*args)
      puts args.join " | "
    end
    foo("Bollo")
    foo("Tony", "Harrison")

Or if you don't care whether there are parameters:
    
    def foo(*)  
    end
     
### Building DSLs with yield and instance_eval

See: [http://rubylearning.com/blog/2010/11/30/how-do-i-build-dsls-with-yield-and-instance_eval/](How do I build DSLs with yield and instance_eval?)

#### yield

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

#### instance_eval

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

### Monkey patching

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
 
<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
<script src="http://cdnjs.cloudflare.com/ajax/libs/highlight.js/8.0/highlight.min.js"></script>
<script src="http://cdnjs.cloudflare.com/ajax/libs/highlight.js/8.0/languages/ruby.min.js"></script>
<link rel="stylesheet" href="http://cdnjs.cloudflare.com/ajax/libs/highlight.js/8.0/styles/monokai_sublime.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>