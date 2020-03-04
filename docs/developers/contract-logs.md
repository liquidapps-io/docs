Contract Logs
========

The DAPP Network uses custom event logs to communicate between consumer contracts and the DAPP Service Providers (specialized EOSIO nodes running the DAPP Network logic and servicing requests).  For this reason any print statements added to a contract which utilizes a DAPP Network service must end with a newline (`\n`) in order to not interfere with the event parsing.

For example:

```cpp
print("{\"type\":\"debug\",\"message\":\"YOUR LOGS HERE\"}\n");
```

```cpp
print("Hello World!");
print("\n");
```