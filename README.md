# django-copy-tables

This utility allows for copying tables from a production/staging server to a development copy. This allows developing against full datasets.

## Installation
It's on pypi, so install via pip (or add to your requirements.txt)
```
pip3 install django-copy-tables
```

in your settings.py
* add to your INSTALLED_APPS "django_copy_tables"
* add a line: 
```
TABLE_COPY_PASSWORD="a password"
```

at the end of your urls.py add:
```
from django_copy_tables import get_table
urlpatterns.append(path('copyTables', get_table))
```


### Usage
Once your source distribution has been updated and the view/password is live, then on your local install you can use the standard manage.py with the command copyTables

The general syntax is:
```
./manage.py copyTables <module name> <table name> <src url> <copy pass> [--clear] [--step X] [--ignore colname]
```
For example if you have a table named Author in module named Blog at http://my.blog with copy password zz11 you can copy that table to your local machine with:
```
./manage.py copyTables Blog Author http://my.blog zz11
```

By default it does not delete records locally, but will overwrite local records with the same primary key as the remote. To delete the local records before syncing use --clear.

By default it will copy 100 records at a time. If your records are large you may want to set --step to something lower.

If you are syncing a table with complex data structures (looping generally) sometimes you need to ignore a foreign key column to get the initial record loaded, load some other data, then load it again without ignoring. that is what the --ignore flag is for. it will ignore the column completly (the model will be set to the default generally.) Run it again without the clear or ignore flags after the other data is in place.

