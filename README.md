Getting Started With Chef-solo
==============================

*    __Install Chef On The Example Server:__
  *	Begin by updating the Ubuntu packages:
    ``` bash
    $ sudo apt-get update
    ```

  * Install Chef and its prerequisites:
    ``` bash
    $ sudo aptitude install -y ruby ruby1.8-dev build-essential wget libruby1.8 rubygems
    $ sudo gem update --no-rdoc --no-ri
    $ sudo gem install ohai chef --no-rdoc --no-ri
    ```

  * Create The Simplest Chef Configuration:
    * Start by having Chef do something simple. Create a /tmp/helloworld.txt file.
    
      ``` bash
      $ mkdir -p ~/chef/cookbooks/helloworld/recipes
      $ echo '
        file "/tmp/helloworld.txt" do
        owner "vagrant"
        group "vagrant"
        mode 00544
        action :create
        content "Hello, Implementor!"
        end' > ~/chef/cookbooks/helloworld/recipes/default.rb
      ```
 


  * Write a node.json to tell Chef to run the helloworld recipe.
    ``` bash
    $ echo '
      {
      "run_list": [ "recipe[helloworld]" ]
      }' > ~/chef/node.json
    ```

  * Tell Chef where to find the files we just created, using a solo.rb file:
    ``` bash
    $ echo '
      file_cache_path "/home/vagrant/chef"
      cookbook_path "/home/vagrant/chef/cookbooks"
      json_attribs "/home/vagrant/chef/node.json"
      ' > ~/chef/solo.rb
    ```

  * Now ready to run chef-solo.
    ``` bash
      $ sudo chef-solo -c ~/chef/solo.rb
    ```
    We use the -c option to explicitly tell chef-solo where to read its configuration.

  * To verify:
    ``` bash
    $ cat /tmp/helloworld.txt
    ```

* __Add More Steps To The Recipe:__
  * Create cron job recipe:
    ``` bash
    $ mkdir -p ~/chef/cookbooks/crondemo/recipes
    $ echo '
      cron "log something" do
      action :create
      hour "*"
      minute "*"
      command "logger Hello!"
      end
      ' > ~/chef/cookbooks/crondemo/recipes/default.rb
    ```

  * Edit the run_list in node.json to add recipe[crondemo]:
    > "run_list": [ "recipe[helloworld]", "recipe[crondemo]" ]

  * Run Chef again:
    ``` bash
    $ sudo chef-solo -c ~/chef/solo.rb
    ```

* __Export__
  * You can use Git to distribute the configuration and run on any server you wish to configure.
