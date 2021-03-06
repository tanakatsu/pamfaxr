PamFaxr
=======

Ruby library for sending faxes via the [PamFax](http://www.pamfax.biz/en/partners/tropo/) Simple API.

Overview
--------

You may send faxes using the PamFax API. See the [PamFax Developer](http://www.pamfax.biz/en/partners/tropo/) page for more details and how to get your developer key.

Requirements
------------

Requires these gems, also in the Gemfile:

	* json
	* mime-types

Installation
------------

Add this line to your application's Gemfile:

```
gem 'pamfaxr', github: 'tanakatsu/pamfaxr', branch: 'master'
```

And then execute:

```
$ bundle install
```

In case of installing via Gemfile, you should run your script with `'bundle exec'`.
For example,

```
$ bundle exec ruby yourscript.rb
```

Or, you can install it yourself as:

```
$ gem install specific_install
$ gem specific_install https://github.com/tanakatsu/pamfaxr
```

#### Trouble shooting
If you encounter with the error
"The git source git://github.com/tanakatsu/pamfaxr.git is not yet checked out. Please run bundle install before trying to start your application",

then, try

```
$ bundle update pamfaxr
```

Example
-------

    # Create a new PamFaxr object
	
	pamfaxr = PamFaxr.new :username => 'your_username', 
	                      :password => 'your_password'
	
	# Create a new FaxJob
	pamfaxr.create_fax_job
    
    # Add the cover sheet
	covers = pamfaxr.list_available_covers
	pamfaxr.set_cover(covers['Covers']['content'][1]['id'], 'Foobar is here!')
	
	# Add files
	pamfaxr.add_remote_file('https://s3.amazonaws.com/pamfax-test/Tropo.pdf')
	pamfaxr.add_file('examples/Tropo.pdf')
	
	# Add a recipient
	recipient = pamfaxr.add_recipient('+14155551212')
	
	# Loop until the fax is ready to send
	loop do
	  fax_state = pamfaxr.get_state
	  break if fax_state['FaxContainer']['state'] == 'ready_to_send'
	  sleep 5
	end
	
	# Send the fax
	pamfaxr.send_fax

	# Get a detail of the fax
	pamfax.get_fax_details(recipient['FaxRecipient']['uuid'])

	# Get current user object
	pamfaxr.reload_user

