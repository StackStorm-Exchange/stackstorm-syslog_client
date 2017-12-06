# Syslog Client Integration Pack

Pack that provides actions for sending events to Syslog servers. 

The main idea of the syslog pack is to be able to use the notification feature in StackStorm to
generate logs for executed actions without having to explicitly create log steps in a workflow.

**NOTE:** This pack is a syslog client only. If you are looking for low-volume syslog integration,
check out the [Ghost2Logger](https://github.com/StackStorm-Exchange/stackstorm-ghost2logger) pack.
For high volumes, we recommend using Elastic Stack or Splunk, and forwarding specific events to ST2.

## Actions

* `format_execution_result` - Transform trigger into a message for syslog.
* `write_syslog` - Send a message to a syslog server.
* `log_via_syslog` - Format an execution and send the write the result as a message to syslog.

## Legal

Obligatory kudos to https://icons8.com/ for the icon.
