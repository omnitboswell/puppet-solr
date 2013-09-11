puppet-solr
===========

Puppet module for installing solr with a stand alone jetty server.  The server will default to port 8983 and store it's data in /var/lib/solr.  Configuration files can be found at /etc/solr.  

This install has been tested on:

* Ubuntu 12.04
* RHEL 6.4

Using this manifest
-----------

To download a copy of solr into /opt/solr and start a dedicated jetty
server for solr.

1. Check out this repository in your modules directory
2. Add the following to your base manifest (Note that including an appropriate JDK is left to you):

```pp
package { 'default-jdk': }

include solr
```


You can also install a tomcat server to host solr.  If so you don't need
to include, just add the module to your modules path and include this in
your manifest.  This example sets up a tomcat server and provides a
zookeeper host to connect in order to run solrCloud.

```pp

package { 'default-jdk': }

class { "solr::tomcat6":
  zookeeper_hosts => "ec2-72-44-55-216.compute-1.amazonaws.com:2181/cld2", 
  cloud_shards => 1
}
```

For more tomcat configuration options see the tomcat6.pp file in
manifests.



Working with Solr Cloud
-----------------------
Either solr::jetty or solr:tomcat6 can be used to host solrCloud.

```pp
package { 'default-jdk' }

class {'solr::jetty':
  number_of_cloud_shards => 2,
  zookeeper_hosts        => ["example.com:2181", "anotherserver.org:2181/alternate_root"]
}
```
