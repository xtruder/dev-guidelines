# Logging

![logging](http://i.memecaptain.com/gend_images/waaQEA.jpg)

Logging things in your application is very important, as when there will be errors,
logs will be the ones that will help you identify the culprit

## What and how to log?

It's essentially important to identify what you need to log and how to log to make
logs parsable. I have seen so many cases where output of application was verbose,
but completelly useless. I have also seen cases where logs were unreadable, or logs
were unindentifiable, because log messages were garbage.

### How much logs?

Logging is not about generating a big data that noone will read. More logging or more
verbose logging will only cause problems with storage of logs. While this can be solved,
in long run, it will only cause mess. In contrary too less logging will make application
hard to debug, choose wisely what and how to log.

### Context on logs

While we understand that logs are messages for developers, but when you deploy to production
you will want to search for logs, but your messages don't have contex and you will want to
query, good luck.

#### Bad log context

```
[<date>]: Pizza has been delivered
```

By who, for whom, what pizza?

#### Good log context

```
message: pizza has been delivered
time: <date>
context:
  type: greek
  for: developer
  by: sysadmin
  price: 10$
```
