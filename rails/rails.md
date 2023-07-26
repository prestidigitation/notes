# Rspec + Rails Notes

## Setting up Rspec for a Ruby on Rails project

When generating a new rails project, use the -T flag to prevent rails Mini-test from being installed.
```console
$ rails new [app_name] -T
```

Add `rspec-rails` to development and test section of the gem file.
```Ruby
#Gemfile
group :development, :test do
  gem "byebug", platforms: [:mri, :mingw, :x64_mingw]
  gem "rspec-rails"
end
```

Use `bundler` to install `rspec-rails` after adding it to your Gemfile.
```console
$ bundle install
```

Generate rspec config files and supporting code in your rails app.
```console
$ bin/rails generate rspec:install
```

Display documentation strings passed to each describe(), context(), and it() method by adding `--format documentation` to the .rspec file in the root directory of the rails app.
```
--require spec_helper
--format documentation
```

After installing and setting up rspec, any new features that get generated, such as models, will automatically create a corresponding spec file.
```console
$ bin/rails generate model pet name:string
```
The preceding command will create `app/models/pet.rb` as well as `spec/models/pet_spec.rb`.

## Important Terminology

describe:
* A `describe` block contains a group of tests, called examples in Rspec. It can accept a class name or a string as an argument.
```Ruby
describe Pet do
end
```

context:
* Similar to `describe`, since it also accepts class names or strings as an argument and encloses examples in a block. Optional to use, but can add more _context_ to your tests.
```Ruby
describe Pet do
  context "validations" do
  end
end
```

it:
* `it` is used to introduce a test or example.
```Ruby
describe Pet do
  context "validations" do
    it "ensures the presence of a name" do
    end
  end
end
```

The expect keyword specifies an expectation for the test results.
```Ruby
expect("value").to eq("actual value")
```

Putting it all together:
```Ruby
# spec/models/pet_spec.rb
require 'rails_helper'

RSpec.describe Pet, type: :model do
  context 'validations' do
    it "ensures the presence of name" do
      pet = Pet.new(name: "").save
      expect(pet).to eq(false)
    end
  end
end
```