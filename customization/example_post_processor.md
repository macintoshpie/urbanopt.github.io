---
layout: default
title: Example change to Post-Processor
parent: Customization
nav_order: 5
---
## Example: post-processing an added feature report attribute

Here is a short example where a new attribute "number of occupants" is added to the feature_report and the post_processor.

Below are the steps for this process:

- Clone the scenario-gem repository to your local machine
- Open the [schema](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/schema/scenario_schema.json) file and append the new property "number of occupants" to the properties inside the program component.

```JSON
"Program": {
  "type": "object",
  "properties": {
    "number_of_occupants": {
    "type": "number"
    }
  }
}
```

- go to the [program](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/program.rb) class in the [default_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/urbanopt/scenario/default_reports) and add the following:

1) Add `number_of_occupants` to the attribute accesor.

```ruby
  attr_accessor :site_area,......., :number_of_occupants
```

2) Initialize an instance variable `number_of_occupants` by adding it to the initialize method.

```ruby
  def initialize(hash = {})
  ......
    @number_of_occupants = hash[:number_of_occupants]
  ......
  end
```

3) Add a default value for `number_of_occupants` in the defaults method:

```ruby
  def defaults
    hash = {}
    ......
    hash[:number_of_occupants] = nil
    return hash
  end
```

4) Add `number_of_occupants` to the to_hash method.

```ruby
def to_hash
  result = {}
  ......
  result[:number_of_occupants] = @number_of_occupants if @number_of_occupants
  return result
end
```

5) Finally, add `number_of_occupant` to the add_program method that is used by the post processor to aggregate the program attributes values.

```ruby
def add_program(other)
  @number_of_occupants = add_values(@number_of_occupants, other.number_of_occupants)
end
```

- Now let's go to the reporting measure and request the number of occupants from the OpenStudio model and store it in the feature_report. `number of occupants` should be implemented in the `run` method, after the intialization of feature_report:

``` ruby
def run(runner, user_arguments)
  super(runner, user_arguments)
  ....
  # number of occupants. Note: 'building' is an object retrieved from openstudio model
  num_occupants = building.numberOfPeople
  feature_report.program.number_of_occupants = num_occupants
```

- Run the new example project using the modified scenario gem reposiory
  - Edit the Gemfile in your example project by setting allow_local to true
  - This will enable using the local files in the urbanopt-scenario-gem repo that you have been editing