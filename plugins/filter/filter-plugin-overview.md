# Filter Plugin Overview

Fluentd has 6 types of plugins: [Input](/plugins/input/input-plugin-overview.md),
[Parser](/plugins/parser/parser-plugin-overview.md), [Filter](/plugins/filter/filter-plugin-overview.md),
[Output](/plugins/output/output-plugin-overview.md), [Formatter](/plugins/formatter/formatter-plugin-overview.md)
and [Buffer](/plugins/buffer/buffer-plugin-overview.md). This article gives an overview of
Filter Plugin.


## Overview

Filter plugins enables Fluentd to modify event streams. Example use
cases are:

1.  Filtering out events by grepping the value of one or more fields.
2.  Enriching events by adding new fields.
3.  Deleting or masking certain fields for privacy and compliance.

## How to Use

It is used with the `<filter>` directive as follows:

``` {.CodeRay}
<filter foo.bar>
  @type grep
  regexp1 message cool
</filter>
```

The above directive matches events with the tag "foo.bar", and if the
"message" field's value contains "cool", the events go through the rest
of the configuration.

Like the `<match>` directive for output plugins, `<filter>` matches
against a tag. Once the event is processed by the filter, the event
proceeds through the configuration top-down. Hence, if there are
multiple filters for the same tag, they are applied in descending order.
Hence, in the following example,

``` {.CodeRay}
<filter foo.bar>
  @type grep
  regexp1 message cool
</filter>

<filter foo.bar>
  @type record_transformer
  <record>
    hostname "#{Socket.gethostname}"
  </record>
</filter>
```

Only the events whose "message" field contain "cool" get the new field
"hostname" with the machine's hostname as its value.

Users can create their own custom plugins with a bit of Ruby. See [this
section](/developer/plugin-development.md/#filter-plugins) for more information.

## List of Filter Plugins

-   [grep](/plugins/filter/filter_grep.md)
-   [record-transformer](/plugins/filter/filter_record_transformer.md)
-   [filter\_stdout](/plugins/filter/filter_stdout.md)


------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.