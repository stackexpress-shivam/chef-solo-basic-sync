Getting Started With Chef-solo
==============================

*	__Install Chef On The Example Server:__
  *	Begin by updating the Ubuntu packages:

    > __$ sudo apt-get update__

  * Install Chef and its prerequisites:

    > __$ sudo aptitude install -y ruby ruby1.8-dev build-essential wget libruby1.8 rubygems__
    > __$ sudo gem update --no-rdoc --no-ri__
    > __$ sudo gem install ohai chef --no-rdoc --no-ri__

  * Create The Simplest Chef Configuration:
    * Start by having Chef do something simple: Create a /tmp/helloworld.txt file.

      > __$ mkdir -p ~/chef/cookbooks/helloworld/recipes__
      
      > __$ echo '__
      > __file "/tmp/helloworld.txt" do__
      > __owner "vagrant"__
      > __group "vagrant"__
      > __mode 00544__
      > __action :crea__te
      > __content "Hello, Implementor!"__
      > __end' > ~/chef/cookbooks/helloworld/recipes/default.rb__
 


  * Write a node.json to tell Chef to run the helloworld recipe:
  
    > __$ echo '__
    > __{__
    > __"run_list": [ "recipe[helloworld]" ]__
    > __}' > ~/chef/node.json__

  * Tell Chef where to find the files we just created, using a solo.rb file:

    > __$ echo '__
    > __file_cache_path "/home/vagrant/chef"__
    > __cookbook_path "/home/vagrant/chef/cookbooks"__
    > __json_attribs "/home/vagrant/chef/node.json"__
    > __' > ~/chef/solo.rb__

  * Now ready to run chef-solo.
    > __$ sudo chef-solo -c ~/chef/solo.rb__
    > __We use the -c option to explicitly tell chef-solo where to read its configuration.__

  * To verify:
    > __$ cat /tmp/helloworld.txt__

* __Add More Steps To The Recipe:__
  * Create cron job recipe:
    > __$ mkdir -p ~/chef/cookbooks/crondemo/recipes__
    > __$ echo '__
    > __cron "log something" do__
    > __action :create__
    > __hour "*"__
    > __minute "*"__
    > __command "logger Hello!"__
    > __end__
    > __' > ~/chef/cookbooks/crondemo/recipes/default.rb__

  * Edit the run_list in node.json to add recipe[crondemo]:
"run_list": [ "recipe[helloworld]", "recipe[crondemo]" ]

  * Run Chef again:
    > __$ sudo chef-solo -c ~/chef/solo.rb__

* __Export__
  * You can use Git to distribute the configuration and run on any server you wish to configure.

