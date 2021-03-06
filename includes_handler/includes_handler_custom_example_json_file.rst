.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


The `json_file <https://github.com/opscode/chef/blob/master/lib/chef/handler/json_file.rb>`_ handler is available from the ``chef_handler`` cookbook and can be used with exceptions and reports. It serializes run status data to a |json| file. This handler may be enabled in one of the following ways.

By adding the following lines of |ruby| code to either the |client rb| file or the |solo rb| file, depending on how the |chef client| is being run:

.. code-block:: ruby

   require 'chef/handler/json_file'
   report_handlers << Chef::Handler::JsonFile.new(:path => "/var/chef/reports")
   exception_handlers << Chef::Handler::JsonFile.new(:path => "/var/chef/reports")

By using the `chef_handler <https://docs.chef.io/resource_chef_handler.html>`_ resource in a recipe, similar to the following:

.. code-block:: ruby

   chef_handler "Chef::Handler::JsonFile" do
     source "chef/handler/json_file"
     arguments :path => '/var/chef/reports'
     action :enable
   end

After it has run, the run status data can be loaded and inspected via |ruby irb|:

.. code-block:: ruby

   irb(main):001:0> require 'rubygems' => true
   irb(main):002:0> require 'json' => true 
   irb(main):003:0> require 'chef' => true
   irb(main):004:0> r = JSON.parse(IO.read("/var/chef/reports/chef-run-report-20110322060731.json")) => ... output truncated
   irb(main):005:0> r.keys => ["end_time", "node", "updated_resources", "exception", "all_resources", "success", "elapsed_time", "start_time", "backtrace"]
   irb(main):006:0> r['elapsed_time'] => 0.00246

