Slop
====

Slop is a simple option collector with an easy to remember syntax and friendly API.

Installation
------------

### Rubygems

    gem install slop

### GitHub

    git clone git://github.com/injekt/slop.git
	gem build slop.gemspec
	gem install slop-<version>.gem

Usage
-----

	opts = Slop.parse do
	  on :v, :verbose, 'Enable verbose mode' 	   # boolean value
	  on :n, :name, 'Your name', true              # compulsory argument
	  on :a, :age, 'Your age', :optional => true   # optional argument
	end

	# if ARGV is `-v --name 'lee jarvis'`
	opts.verbose? #=> true
	opts.name?    #=> true
	opts[:name]   #=> 'lee jarvis'
	opts.age?     #=> false
	opts[:age]    #=> nil

You can also return your options as a Hash

	opts.to_hash #=> {:name => 'Lee Jarvis', :verbose => true, :age => nil}

If you don't like the method `on` (because it sounds like the option **expects**
a callback, you can use the `opt` or `option` alternatives)

	on :v, :verbose
	opt :v, :verbose
	option :v, :verbose

If you don't like that Slop evaluates your block, or you want slop access
inside of your block without referring to `self`, you can pass a block to `parse`.

	Slop.parse do |opts|
	  opts.on :v, :verbose
	  opts.on :n, :name, 'Your name', true
	end

Callbacks
---------

If you'd like to trigger an event when an option is used, you can pass a
block to your option. Here's how:

    Slop.parse do
      on :V, :version, 'Print the version' do
	  	puts 'Version 1.0.0'
		exit
	  end
    end

Now when using the `--version` option on the command line, the trigger will
be called and its contents executed.

Ugh, Symbols
------------

Fine, don't use them

	Slop.parse do
	  on :n, :name, 'Your name'
	  on 'n', 'name', 'Your name'
	  on '-n', '--name', 'Your name'
	end

All of these options will do the same thing

Ugh, Blocks
-----------

C'mon man, this is Ruby, GTFO if you don't like blocks.

	opts = Slop.new
	opts.on :v, :verbose
	opts.parse

Smart
-----

Slop is pretty smart when it comes to building your options, for example if you
want your option to have a flag attribute, but no `--option` attribute, you
can do this:

    on :n, "Your name"

and Slop will detect a description in place of an option, so you don't have to
do this:

    on :n, nil, "Your name", true

You can also try other variations:

    on :name, "Your name"
    on :n, :name)
    on :name, true

Lists
-----

You can of course also parse lists into options. Here's how:

	opts = Slop.parse do
	  opt :people, true, :as => Array
	end

	# ARGV is `--people lee,john,bill`
	opts[:people] #=> ['lee', 'john', 'bill']

You can also change both the split delimiter and limit

    opts = Slop.parse do
      opt :people, true, :as => Array, :delimiter => ':', :limit => 2)
    end
    opts[:people] #=> ["lee", "injekt:bob"]

Contributing
------------

If you'd like to contribute to Slop (it's **really** appreciated) please fork
the GitHub repository, create your feature/bugfix branch, add tests, and send
me a pull request. I'd be more than happy to look at it.