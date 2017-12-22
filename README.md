# Example Cassandra Application 

This simple application illustrates the use of the cassandra data service in a Ruby application running on open-source Cloud Foundry.

## Installation

#### Create a Cassandra service instance

Find your Cassandra service via `cf marketplace`.

```
$ cf marketplace
Getting services from marketplace in org testing / space testing as me...
OK

service       plans     description
cassandra     default   Cassandra service
```

This assumes your Cloud Foundry platform already has a Cassandra service available called `cassandra`. To create an instance of this service, use:

```
$ cf create-service cassandra keyspace cassandra-instance
```

#### Push the Example Application

The example application comes with a Cloud Foundry `manifest.yml` file, which provides all of the defaults necessary for an easy `cf push`.

```
$ cf push
Using manifest file cf-cassandra-example-app/manifest.yml

Creating app cassandra-example-app in org testing / space testing as me...
OK

Using route cassandra-example-app.example.com
Binding cassandra-example-app.example.com to cassandra-example-app...
OK

Uploading cassandra-example-app...
Uploading from: cf-cassandra-example-app
...
Showing health and status for app cassandra-example-app in org testing / space testing as me...
OK
```

If you now curl the application, you'll see that the application has detected that it's not bound to a cassandra instance.

```
$ curl http://cassandra-example-app.example.com/
```

#### Bind the Instance

Now, simply bind the cassandra instance to our application.

```
$ cf bind-service cassandra-example-app cassandra-instance
```

## Usage

You can now read and write records by GETting and POSTing to `/table/key`. Be sure to create the table first. In the example below, we create a table named `myTable`, add a key/value pair named `foo` with a value of `bar`, and retrieve the value back from `foo`.

```
$ curl -X POST http://cassandra-example-app.example.com/myTable
$ curl -X POST http://cassandra-example-app.example.com/myTable/foo/bar
$ curl -X GET  http://cassandra-example-app.example.com/myTable/foo
bar
```

Of course, be sure to replace `example.com` with the actual domain of your Cloud Foundry installation.
