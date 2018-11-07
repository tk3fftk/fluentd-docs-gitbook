# Centralize Logs from Ruby Applications

The '[fluent-logger-ruby](http://github.com/fluent/fluent-logger-ruby)'
library is used to post records from Ruby applications to Fluentd.

This article explains how to use the fluent-logger-ruby library.


## Prerequisites

-   Basic knowledge of Ruby
-   Basic knowledge of Fluentd
-   Ruby 2.1 or later


Installing Fluentd
------------------

Please refer to the following documents to install fluentd.

-   [Install Fluentd with rpm Package](/articles/install-by-rpm.md)
-   [Install Fluentd with deb Package](/articles/install-by-deb.md)
-   [Install Fluentd with Ruby Gem](/articles/install-by-gem.md)
-   [Install Fluentd from source](/articles/install-from-source.md)


Modifying the Config File
-------------------------

Next, please configure Fluentd to use the [forward Input
plugin](/articles/in_forward.md) as its data source.

``` {.CodeRay}
<source>
  @type forward
  port 24224
</source>
<match fluentd.test.**>
  @type stdout
</match>
```

Please restart your agent once these lines are in place.

``` {.CodeRay}
# for systemd
$ sudo systemctl restart td-agent.service
# for init.d
$ sudo /etc/init.d/td-agent restart
```


Using fluent-logger-ruby
------------------------

First, add the 'fluent-logger' gem to your Gemfile.

``` {.CodeRay}
gem 'fluent-logger', "~> 0.7.1"
```

Next, please initialize and post the records as shown below.

``` {.CodeRay}
require 'fluent-logger'
Fluent::Logger::FluentLogger.open(nil, :host=>'localhost', :port=>24224)
Fluent::Logger.post("fluentd.test.follow", {"from"=>"userA", "to"=>"userB"})
```

Executing the script will send the logs to Fluentd.

``` {.CodeRay}
$ ruby test.rb
```

The logs should be output to `/var/log/td-agent/td-agent.log` or stdout
of the Fluentd process via the [stdout Output plugin](/articles/out_stdout.md).


Production Deployments
----------------------


### Output Plugins

Various [output plugins](/articles/output-plugin-overview.md) are available for
writing records to other destinations:

-   Examples
    -   [Store Apache Logs into Amazon S3](/articles/apache-to-s3.md)
    -   [Store Apache Logs into MongoDB](/articles/apache-to-mongodb.md)
    -   [Data Collection into HDFS](/articles/http-to-hdfs.md)
-   List of Plugin References
    -   [Output to Another Fluentd](/articles/out_forward.md)
    -   [Output to MongoDB](/articles/out_mongo.md) or [MongoDB
        ReplicaSet](/articles/out_mongo_replset.md)
    -   [Output to Hadoop](/articles/out_webhdfs.md)
    -   [Output to File](/articles/out_file.md)
    -   [etc...](http://fluentd.org/plugin/)


### High-Availability Configurations of Fluentd

For high-traffic websites (more than 5 application nodes), we recommend
using a high availability configuration of td-agent. This will improve
data transfer reliability and query performance.

-   [High-Availability Configurations of Fluentd](/articles/high-availability.md)


### Monitoring

Monitoring Fluentd itself is also important. The article below describes
general monitoring methods for td-agent.

-   [Monitoring Fluentd](/articles/monitoring.md)


------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud
Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.