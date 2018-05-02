# dbKoda Logging API

## `(l|logger).LEVEL(item1, item2, ...)`

Log `LEVEL`s from most important to least important are `error`,`warn`,`notice`,`info`and`debug`. 

### Log general info

`l.info('The controller is launching at %s:%d', ipAddr, portNum)`

You can use all formats from `node`'s [util.format](https://nodejs.org/api/util.html#util\_util\_format\_format\_args).

### Log error

`l.error('The mongo shell service has crashed due to: ', error)`

`logger.error('The mongo shell service has crashed due to: ', error)`

`console.error('The mongo shell service has crashed due to: ', error)`

By default, `error` level logs will be reported to `raygun` apart from reporting to `console` and `file`, unless user has turned `telemetryEnabled` to `false`. All errors happened during app launching, whose ending is marked by user config loaded, will be reported to `raygun` no matter what, with `beforeConfigLoaded` as the user ID.

### Log error but escape `raygun`

`l._error(...)`

or

```javascript
l.error(..., {[Symbol.for('info')]: {
  raygun: false
}})
```

### Providing more info to `raygun`

```javascript
l.error(..., {[Symbol.for('info')]: {
  // any custom data
  customData: {
    winstonTimestamp: 1525217691390,
    mongoShellVersion: 'v3.6.4'
  },
  // any request data that leads to this error
  request: {
    query: {
      enabled: false,
      database: 'test'
    },
    protocol: 'http'
  },
  // array of strings
  tags: ['beta', 'new feature'],
  // callback function that will be called once the error has been reported to `raygun`. This will be called no matter whether `raygun` is enabled or not
  callback(res) {
    // res is response object returned from `raygun`

    // do whatever
    process.exit(1);
  }
}})
```
