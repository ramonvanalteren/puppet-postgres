Postgres Puppet module.

All bugs produced by Kris Buytaert (and now Kit Plummer too). 

Init the database before you start configuring files as once hba files etc exists in /var/lib/pgsql/data the initial database creation won't work anymore .

```puppet
    class { "postgres" :
      #version => "84",
      password => "my_postgres_password", } ->
    postgres::initdb{ "host": }

    # Current postmaster.ops template has only listen address configurable,  this can of course be expanded as needed...
    postgres::config{ "host": listen => "*", }
    postgres::hba { "host":
      allowedrules => [
        "host    DATABASE all    10.0.0.0/32  trust",
      ],
    }

    # Start the service
    postgres::enable { "host": }

    # To add a user and password
    # postgres::createuser { "username": passwd => "password", } ->
    # To ensure an user and password is set, creating it or updating the password as needed
    postgres::user { "username": passwd => "password", } ->

    # To create a new database
    postgres::createdb { "newdb" : owner=> "username", }
```
