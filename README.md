# Welcome to URBANopt

## Please return when URBANopt is released to the public

# What is URBANopt?

URBANopt is a Software Development Kit (SDK) to aid in the design of districts where the interactions between individual buildings, district energy systems, distributed energy resources, and electrical system designs are considered. Including these interactions allows URBANopt to address important questions in low energy, grid aware, future thinking urban districts such as tradeoffs between building height and PV (photovoltaic) production, investments in building efficiency vs distributed renewable generation, coordination of multiple buildings to optimize grid metrics, and performance gains of shared thermal district systems vs conventional single building systems.

City planners and utility management can use URBANopt to… **TODO: fill in business case for UO**

URBANopt is a package of code for software engineers to use. This is not a standalone program for end users. This SDK is the engine, the building blocks for others to create a modern user interface for sale to end users. The goal of URBANopt is to enable a thriving public/private partnership of software companies with... **TODO: finish this block**

## Mac installation

Install [Ruby 2.2.4](https://github.com/rbenv/rbenv)\
Install Bundler 1.17

- `gem install bundler -v 1.17`

Ensure your local `.gemrc` file contains:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)\
Add path to Ruby in the new OpenStudio folder by pasting this line to your `.bash_profile` or similar file:

- `export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby`

## Windows installation

Install Ruby using the [RubyInstaller](https://rubyinstaller.org/downloads/archives/) for [Ruby 2.2.4 (x64)](https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.2.4-x64.exe)\
Install Devkit using the [mingw64](https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe) installer\
Install Bundler 1.17

- `gem install bundler -v 1.17`

Ensure your local `.gemrc` file contains:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)\
Create file `C:\ruby-2.2.4-x64-mingw32\lib\ruby\site_ruby\openstudio.rb` and point it to your OpenStudio installation by editing the contents. E.g.:

- `require 'C:\openstudio-2.8.1\Ruby\openstudio.rb'`

Verify your OpenStudio and Ruby configuration by typing this into your command line:

- `ruby -e "require 'openstudio'" -e "puts OpenStudio::Model::Model.new"`

## Linux installation

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)\
Need to manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)\
Gdbm libs:

- `/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory`

and

- `/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory`

You need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`)

## Basic Usage

### Set up

Make a Fork of our example project:
`https://github.com/urbanopt/urbanopt-example-geojson-project`\
Check out to a short path (e.g. `C:\urbanopt-project`) to avoid problems with long file names on Windows

- Reference: <https://ourcodeworld.com/articles/read/109/how-to-solve-filename-too-long-error-in-git-powershell-and-github-application-for-windows>
- Solution: `git config --system core.longpaths true`

Run the following commands to execute the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install`
1. `bundle exec rake`
1. `bundle update`

### Run example project

To run specific rake tasks:

- $ `bundle exec rake <name of the Rake task>`

### Overview of Rake tasks

#### run_all

- In the Rake file methods are defined to create a Scenario CSV for each scenario, that take the `feature_file`, `mapper_files_dir` and the `csv_file` as arguments.\
The run_all Rake task creates and runs a `ScenarioRunnerOSW` for each scenario passing the scenario method as an argument.

The `run_baseline`, `run_high_efficiency` and `run_mixed rake` tasks can be used for running individual scenarios.

#### post_process_all

- This rake task creates and runs `ScenarioDefaultPostProcessor` for each scenario and saves the results.

`post_process_baseline`, `post_process_high_efficiency`, `post_process_mixed` rake tasks can be used for individual scenarios.

#### update_all

- This rake task combines the run_all and post_process_all tasks.

#### default

- This runs the update_all rake task.

#### clear_all

- This rake task clears the scenario results from any previous runs.

The `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for individual scenarios.

### Adding your own measures

The `mapper_files_dir` contains the `baseline.osw` which serves as a simulation input for URBANopt. It is a workflow of OpenStudio measures and dictates the sequence of running the measures.\
Additional measures can be added to the workflow by adding the measure name and directory along with the measure arguments.

### Modifying mapper classes

The Simulation mapper class maps the features in the feature file to arguments required as simulation inputs in the `baseline.osw`. On adding additional measures to the `baseline.osw` file

```Ruby
OpenStudio::Extension.set_measure_argument
```

method should be used and the OpenStudio measure name and arguments and the corresponding feature from the feature file should be added as arguments.

### Adding a custom post processor

Scenario post process require feature reports to aggregate results from feature simulations. A reporting measure is used to query and report specific output data from an Openstudio simulation of each feature. The current default reporting measure is the “default_feature_reports” (add reference). This measure writes a `default_feature_reports.json` file containing information on all features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the features.

Users can create their own OpenStudio reporting measure to generate customized simulation reports. For example, users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects; e.g. reporting specific construction costs. User can then add the new reporting measure to the openstudio `workflow.osw` file **(add reference)** and rerun the simulation.\
The `DefaultPostProcessor` reads these feature reports and aggregates them to create a `ScenarioReport`.

## Advanced Usage
