# Syslog Client Integration Pack

Pack that provides actions for sending events to Syslog servers.

The main idea of the syslog pack is to be able to use the notification feature in StackStorm to
generate logs for executed actions without having to explicitly create logging steps in a workflow.

**NOTE:** This pack is a syslog client only. If you are looking for low-volume syslog integration,
check out the [Ghost2Logger](https://github.com/StackStorm-Exchange/stackstorm-ghost2logger) pack.
For high volumes, we recommend using Elastic Stack or Splunk, and forwarding specific events to ST2.

## Actions

* `format_execution_result` - Format execution result using jinja template into a message suitable for syslog.
* `write_syslog` - Send a message to a syslog server.
* `log_via_syslog` - Format an execution and send the write the result as a message to syslog.

## Configuration

The pack must be configured with a valid syslog server host/port and protocol before it can be used.

 **parameter** | **description**
 --- | ---
 _**server**_ | The DNS or IP address of the syslog server. _Default: **localhost**_
 _**port**_ |  The port the syslog daemon is listening on. _Default: **514**_
 _**protocol**_ | The protocol to use UDP/TCP.  _Default: **UDP**_


## Example Workflow and Output

### Action-chain meta
`action//notification_example.yaml`
```
---
description: Execute a logger and echo of a fixed message.
enabled: true
entry_point: 'chains/notification_example.yaml'
name: notify_by_syslog
runner_type: "action-chain"
notify:
  on-complete:
    routes:
    - syslog
parameters:
  cmd:
    description: Arbitrary Linux command to be executed on the remote host(s).
    required: true
type: string
```

### Notification action-chain
`action/chains/notification_example.yaml`
```
---
    chain:
        -
            name: say_it
            ref: core.local
            parameters:
                cmd: 'echo "{{ cmd }}"'
            on-success: run_it
        -
            name: run_it
            ref: core.local
            parameters:
                cmd: "{{ cmd }}"
default: say_it
```

### Action Chain syslog output

```
Nov 30 15:55:34 localhost write_syslog[41557]:
Action core.local completed.
status : succeeded
execution: 5a201be2dc599aa0f328a92b

web_url: https://localhost/#/history/5a201be2dc599aa0f328a92b/general

Took 0.307441\u03BCs to complete.

result :
--------
stdout : date
Nov 30 15:55:35 localhost write_syslog[41581]:
Action core.local completed.
status : succeeded
execution: 5a201be3dc599aa0f328a934

web_url: https://localhost/#/history/5a201be3dc599aa0f328a934/general

Took 0.312612\u03BCs to complete.

result :
--------
stdout : Thu Nov 30 15:55:32 CET 2017
Nov 30 15:55:36 localhost write_syslog[41592]:
Action test.notify_by_syslog_json completed.
status : succeeded
execution: 5a201be2dc599aa1390e1895

web_url: https://localhost/#/history/5a201be2dc599aa1390e1895/general

Took 3s to complete.

result :
--------
tasks : [{u'liveaction_id': u'5a201be2dc599aa0f328a92a', u'workflow': None, u'created_at': u'2017-11-30T14:55:30.638047+00:00', u'updated_at': u'2017-11-30T14:55:31.749809+00:00', u'state': u'succeeded', u'result': {u'succeeded': True, u'failed': False, u'return_code': 0, u'stderr': u'', u'stdout': u'date'}, u'id': u'say_it', u'execution_id': u'5a201be2dc599aa0f328a92b', u'name': u'say_it'}, {u'liveaction_id': u'5a201be3dc599aa0f328a933', u'workflow': None, u'created_at': u'2017-11-30T14:55:31.777045+00:00', u'updated_at': u'2017-11-30T14:55:32.945583+00:00', u'state': u'succeeded', u'result': {u'succeeded': True, u'failed': False, u'return_code': 0, u'stderr': u'', u'stdout': u'Thu Nov 30 15:55:32 CET 2017'}, u'id': u'run_it', u'execution_id': u'5a201be3dc599aa0f328a934', u'name': u'run_it'}]
published :
```

## Legal

Obligatory kudos to https://icons8.com/ for the icon.
