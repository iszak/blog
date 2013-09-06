---
layout: post
title:  "Generating schema graphs with SQLAlchemy and Macports"
date:   2013-09-01 15:35:00
categories: macports sqlachemy schema graph
---

For a while now I've been looking for a tool which I could point to a database
and it will generate a schema graph, now I usually use [Navicat Data Modeler][1]
to initially design schemas which has this functionality but costs a premium
of which I could not justify. So I went looking for an open source alternative,
unfortunately most of the dependencies that these open source tools required
weren't available in the [MacPorts][2] repository or I had difficulties
installing them.

Finally someone recommended today [SQL Alchemy][3] with
[SQL Alchemy Schema Display][4] which I have heard of before in my brief
encounter in the Python world and I knew it was a solid choice. I was sold and
so these are the steps I took to get it installed on my setup

Please note this guide makes the assumption you're using **MacPorts on OSX with Python 2.7 and Pip**



### GraphViz
SQL Alchemy depends on [GraphViz][5], which is available in MacPorts, but for some
reason it requires you to deactive "nawk" port to install it, why it couldn't do
this automatically and then re-active it is beyond me, perhaps it's a bug. If
you don't have nawk you can simply run sudo port install graphviz.

{% highlight bash %}
    sudo port install nawk
    sudo port deactivate nawk
    sudo port install graphviz
    sudo port activate nawk
{% endhighlight %}



### Database support
Depending on what database you want to connect to you will need to install the
required driver, I use MySQL at my place of employment but PostgreSQL personally
so I needed both.


#### MySQL
To enable MySQL support you need mysql_config in your $PATH variable, in my case
MacPorts names mysql_config to mysql_config5 to possibly prevent conflicts. An
alias may work, but I decided to create a symbolic link, if anyone knows of a
better solution let me know.

{% highlight bash %}
    sudo ln -s /usr/local/bin/mysql_config /opt/local/bin/mysql_config5
    sudo pip install mysql-python
{% endhighlight %}


#### PostgreSQL
To enable PostgreSQL support you need to install this pip module, no issues.

{% highlight bash %}
    sudo pip install psycopg2
{% endhighlight %}



### PyParsing
Another dependency is pyparsing, however by default MacPorts installs the latest
which is 2.x and will not work with Python 2.7 it seems.

{% highlight bash %}
    sudo pip uninstall pyparsing
    sudo pip install pyparsing==1.5.7
{% endhighlight %}


### PyDot
SQL Alchemy Schema Display depends directly on PyDot but PyDot indirectly depends
on PyParsing so we need both.

{% highlight bash %}
    sudo pip install pydot
{% endhighlight %}



### SqlAlchemy
Finally, the tool that will use all these components.

{% highlight bash %}
    sudo pip install sqlalchemy
    sudo pip install sqlalchemy_schemadisplay
{% endhighlight %}



### Generating
Now to generate an image of your schema you need to provide a host, engine,
database name, username and password. I've created a sample script below which
I use just populate your details and run "python /path/to/file.py"

{% highlight python %}
    from sqlalchemy import MetaData
    from sqlalchemy_schemadisplay import create_schema_graph

    # Database
    host     = 'localhost'
    engine   = 'postgresql'
    database = 'database'
    username = 'username'
    password = 'password'

    # General
    data_types = False
    indexes    = False


    # Generation
    dsn = engine + '://' + username + ':' + password + '@' + host + '/' + database;

    graph = create_schema_graph(
      metadata       = MetaData(dsn),
      show_datatypes = data_types,
      show_indexes   = indexes
    )

    graph.write_png('schema.png')
{% endhighlight %}

That's it, you've now got an image of your database schema with relationships
and indexes or data types depending on your configuration. Hopefully I didn't
miss any of the dependencies as when I was installing this it was a lot of trial
and error. If you do spot an error contact me on Twitter and I'll happily edit
it with creditation.

[1]: http://www.navicat.com/products/navicat-data-modeler
[2]: http://www.macports.org/
[3]: http://www.sqlalchemy.org/
[4]: http://www.sqlalchemy.org/trac/wiki/UsageRecipes/SchemaDisplay
[5]: http://www.graphviz.org/
