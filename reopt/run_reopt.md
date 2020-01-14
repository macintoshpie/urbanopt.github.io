---
layout: default
title: Example REopt Project
parent: REopt
nav_order: 1
---

[REopt Lite](https://reopt.nrel.gov/tool) is a technoeconomic decision making model for distributed energy resource (DER) deployment. Given a set of inputs it leverages mixed-integer linear programming to select the optimal design and dispatch of solar PV, storage, wind and diesel generator technologies - it also provides key economic metrics for the system. Integration of this model with URBANopt allows individual buildings (i.e Feature Reports) and collections of buildings (i.e. Scenario Reports) to be assessed for cost-optimal DER solutions. 

To run an example projected incorporating REopt Lite functionality, clone the [example project](https://github.com/urbanopt/urbanopt-example-geojson-reopt-project) to your local machine. Next, run the following commands to set up the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install` to ensure all dependencies in your [Gemfile](https://github.com/urbanopt/urbanopt-example-geojson-reopt-project/blob/master/Gemfile) are available
2. `bundle update` to update your gems to the latest available versions
3. `bundle exec rake openstudio:runner:init`  
   This command creates a `runner.conf` file that can be used to configure URBANopt. We
   recommend setting `num_parallel` based on the number of the number of Features you have or cores in
   your CPU.
4. `bundle exec rake` to execute [Rake tasks](rake_tasks.md) as described in the next section.

![example_project_layout](../doc_files/building_types_ISO.jpg)