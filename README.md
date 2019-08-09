# Ruby Gem creation with Bundler

This is a little guide to use *Bundler* help you create your own Ruby gem.

## Bundler

Bundler is a gem that provides a consistent enviroment for Ruby project by tracking and installing the exact gems and versions that are needed. \
[Source](https://bundler.io/ "Official budler page")

## Getting started

First you'll need to install the bundler ruby gem `$ gem install bundler` and if you don't have them, Ruby (the interpreter) and Ruby Gems (gem repository).

## Creating the gem

For creating the gem, you need to use the bundler gem command `$ bundle gem my_gem`.

### Naming your gem

There are stablished conventions for properly naming your gem:
* my_gem -> This creates a single directory `/my_gem`
* my-gem -> This creates two directories `/my/gem`
* this-is-my-gem -> This creates many directories `/this/is/my/gem`


The first time you run the bundler gem command, you'll be asked whether you want to include the `CODE_OF_CONDUCT.md`, `LICENSE.txt` and the testing for the project (could be `minitest` or `rspec`).
Then bundler will generate the needed files for your project.

## The .gemspec file

You'll need to edit some of this files (depending on your project are the files you need to edit or add more). The most important and the one you have to always check is the `my_gem.gemspec` file, because this is the file that's going to build your gem, there you need to edit some lines so your gem can be used properly, mainly every `TODO` that you see, this means the code *needs* to be edited.

```Ruby
spec.summary = %q{TODO: Write a short summary, because RubyGems requires one.}
spec.description = %q{TODO: Write a longer description or delete this line.}
spec.homepage = "TODO: Put your gem's website or public repo URL here."
```
The lines above are very self explanatory on what do you need to put on them. Let's suppose the gem `my_gem` does just the simple task of printing *Hello World* on the screen. Then the code, replacing the `TODO` lines will be something like this:
```Ruby
spec.summary = %q{This gem prints Hello World}
spec.description = %q{This is a Ruby gem that prints the message Hello World on the screen.}
spec.homepage = "https://github.com/sp4ceunicorn/rubygem_ea"
```
Then you have the metadata section, you can either comment all this lines or delete them:
```Ruby
# spec.metadata["allowed_push_host"] = "TODO: Set to 'http://mygemserver.com'"
# spec.metadata["homepage_uri"] = spec.homepage
# spec.metadata["source_code_uri"] = "TODO: Put your gem's public repo URL here."
# spec.metadata["changelog_uri"] = "TODO: Put your gem's CHANGELOG.md URL here."
  ```
Finally (for this file), sometimes you'll get an error that has to do with the `files` section, to solve this you simply need to change the current line:
```Ruby
spec.files = Dir.chdir(File.expand_path('..', __FILE__)) do
    `git ls-files -z`.split("\x0").reject { |f| f.match(%r{^(test|spec|features)/}) }
```
to this one:
```Ruby
spec.files = Dir['lib/**/*.rb']
```
This forces the build command to get all the files on `/lib` and its subdirectories that have the Ruby extension (.rb).

## The code

Now that you have your gem `my_gem` created, you need to do some coding for it to print *Hello World*, for this, you need to go to the `/lib` directory, there you'll find the file `my_gem.rb` already created:
```Ruby
require "my_gem/version"

module MyGem
  class Error < StandardError; end
  # Your code goes here...
end
```
here you have to put the needed code to make the gem work properly, for example:
```Ruby
require "my_gem/version"

module MyGem
  # Your code goes here...
  puts 'Hello World'
end
```
Now your gem is going to print *Hello World*, as simple as that.

If your gem is going to do more than just print a message on the screen, probably you'll need more than one class, wich means more files, this extra files need to be located on the `/lib/my_gem` directory.

## Testing

If you said yes to the testing files creation at the moment of making the gem, you'll find the `/spec` directory on your project (supposing you choose `rspec` to do the testing) inside there are two files, `my_gem_spec.rb` and `spec_helper.rb`.
The first one is the one you are going to use to create the tests:
```Ruby
RSpec.describe MyGem do
  it "has a version number" do
    expect(MyGem::VERSION).not_to be nil
  end

  it "does something useful" do
    expect(false).to eq(true)
  end
end
```
On the second file you only need to specify the classes that have tests:
```Ruby
require "bundler/setup"
require "my_gem"
# Here you'll put all the requires for each of your classes

RSpec.configure do |config|
  # Enable flags like --only-failures and --next-failure
  config.example_status_persistence_file_path = ".rspec_status"

  # Disable RSpec exposing methods globally on `Module` and `main`
  config.disable_monkey_patching!

  config.expect_with :rspec do |c|
    c.syntax = :expect
  end
end
```
### Running the tests

For this you need to be on the directoy of the gem `/my_gem` and run the command `$ rspec spec`.

## What if you need third part gems on your project?

Let's say that now you want the *Hello World* message to be blue (or any other color you like), you might want to use te `colorize` gem, that allows you to change the printing colors on the console. Great! you install the gem `$ gem install colorize` and add it to the project:
```Ruby
require 'my_gem/version'
require 'colorize'

module MyGem
  # Your code goes here...
  puts 'Hello World'.blue
end
```
Now it prints the message on blue, but what if you publish the gem, and the person using it doesn't have the `colorize` gem installed? an error will occur. To avoid this from happening, you just need to run the command to add third part gems directly to your project `$ bundle add colorize` this will modify your `Gemfile` file, here the new gems are going to be added, you can now check if the `colorize` one is in there:
```Ruby
source "https://rubygems.org"

# Specify your gem's dependencies in my_gem.gemspec
gemspec

gem "colorize", "~> 0.8.1"
```
Now everybody can use your gem!

## Building the gem

To complete your gem creation, you need to build the gem, this using the `my_gem.gemspec` and the command `$ gem build my_gem.gemspec`, if everything is well configurated, you'll get a message like this:
```
Successfully built RubyGem
  Name: my_gem
  Version: 0.1.0
  File: my_gem-0.1.0.gem
```
## Installing your gem
Now you can install `my_gem` using `$ gem install my_gem-0.1.0.gem`.

Also you can publish your gem on [Rubygems](https://rubygems.org/ "Gem repository") using `$ gem push my_gem-0.1.0.gem`.

:sparkles: And that's it, now you know the basics for creating a Ruby gem :sparkles: