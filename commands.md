# Network

## View Active Established Connections On a Port

```
ss -an state established src :<port>
```

-n: Prevent hostname resolving. Show numerical address
-a: List all sockets
state: Filter by connection state - listening - established - closed

# Console

## Monitor Command Output Over Time

```
watch -n -d "<command>"
```

-n: Interval between updates in seconds
-d: Show differences between previous and current output
