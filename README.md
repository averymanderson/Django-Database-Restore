Django Database Restore
=========

Let's face it. Sometimes you just F up. Django Database Restore allows you to use a several day old backup db and merge in all items created since the datetime of that db backup

What?
-----

Django Database Restore is set of two Python scripts for those times when you fuck it up but don't realize until 4 days later. 

db_cache.py finds every instance of every model that has a created field who's value is after a specified date and saves that instance and all model instances that reference that instance to a JSON file
db_restore.py adds those model instances back from the output JSON file

How?
----
### Backup
Take down your site for maintenance and backup your current db instance in case something goes horribly wrong

### Modify the script files with your settings
In **db_cache.py**:

* Change "backup_date" to be the date and time from when you have the backup db you are using to restore
* Change the "approot" and "appfolder" to reflect your Django settings
* Optional: change the name/location of the output file by changing "dest"

In **db_restore.py**:

* Change the "approot" and "appfolder" to reflect your Django settings
* Optional: if you changed the destination file change the name/location of the input file by changing "orig"

Upload db_cache.py and db_restore.py to a scripts directory in your app

### Execute db_cache.py
Navigate to the scripts directory

    $ python db_cache.py

### Examine your JSON output file
Do a once over on the JSON output file before running db_restore.py. Make sure you don't have any non-standard fields that haven't been dealt with in the db_cache.py file (Checkout Missing Features and Future Work if you're unsure)

Each item in your JSON file will take this format:


	
	"app": app_name,
	"model": model_name,
	"data": {
		"data_field": value_of_instance_field,
	}



### Execute db_restore.py

	$ python db_restore.py


### Complete
Double check the current state of your database. If all seems in order, reinstate your site


Why?
----

Sometimes we're in a hurry. Sometimes we don't have good systems in place for deployment. Sometimes we're fixing things on the fly and there's no time to double check. And yes, sometimes we just f up.

Sometimes when we f up, we don't catch it until days, weeks, months, later. Django Database Restore let's you restore to your last good db backup, but still keep any table rows that have been added since that date so you don't need to completely suffer the consequences of an uncaught screw-up.

Missing Features and Further Work
---------------------------------

Django Database Restore does not support updating instances whose "modified" fields are past the db_backup datetime.

Django Database Restore does not support Point fields

Copyright and License
---------------------
The MIT License (MIT)
Copyright © 2012 Avery Anderson, http://www.averymanderson.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

