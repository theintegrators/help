# Logging Template for ICA applications

## Basic Syntax
```
<DATETIME> [<LOGLEVEL>] <SOURCE> <ACTION>/<TARGET>.
```
Example:
```
2025-02-11T07:36:19.670Z [INFO] mode/startUp close/10SV01.
```

Example using string literals:
```
2025-02-11T07:36:19.670Z [INFO] hmi/system/mode/input log/'received input'.
```

## Source
* sources can be nested
### HMI
```
<DATETIME> [INFO] hmi/<NESTED-SOURCE> <ACTION>.
```
Example
```
2025-02-11T07:36:19.670Z [INFO] hmi/system/mode/input called/system/mode/run.
```

### Alarms
```
<DATETIME> [INFO] alarm/<ALARM NAME> <ACTION>.
```
Example
```
2025-02-11T07:36:19.670Z [INFO] alarm/10LT01/LLL fail-safe/lock/50DOP09.
```

## Targets
* Targets can be anything that has state. Valves, pumps, system modes and modules, etc.
* Targets can have nested paths
* Targets not required
```
/P01
/SV01
/pump/P01
/valve/SV01
/valve/BFV01
/mode/system/bypass
```

Example of an action without a target (because the target is also the source):
```
2025-02-11T07:36:19.670Z [INFO] alarm/10LT01/LLL activate/alarm.
```
```
2025-02-11T07:36:19.670Z [INFO] alarm/10LT01/LLL activate
```

## Actions
* Actions can be nested.
* Actions are followed by the target if there is one.

### Fail-Safe
#### Moved to fail-safe position

Template:
```
<DATETIME> [INFO] <SOURCE> fail-safe/lock/<TARGET>.
```
Example:
```
2025-02-11T07:36:19.670Z [INFO] alarm/10LT01/LLL fail-safe/lock/P01.
```

#### Freed from fail-safe position.
* This does not imply that the actuator can indeed move from fail-safe. Only that this alarm will not prevent it from moving away from its fail-safe position.

Template:
```
<DATETIME> [INFO] <SOURCE> fail-safe/free/<TARGET>.
```
Example:
```
2025-02-11T07:36:19.670Z [INFO] alarm/10LT01/LLL fail-safe/free/pump/P01.
```

### Actuator started
```
<DATETIME> [INFO] <SOURCE> started/<TARGET>.
```
Example 1:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/run started/P01.
```

### Actuator stopped
```
<DATETIME> [INFO] <SOURCE> stopped/<TARGET>.
```
Example 1:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/bypass stopped/P01.
```

### Actuator opened
```
<DATETIME> [INFO] <SOURCE> opened/<TARGET>.
```
Example 1:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/run opened/SV01.
```

### Actuator closed
```
<DATETIME> [INFO] <SOURCE> closed/<TARGET>.
```
Example 1:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/bypass closed/SV01.
```

### Actuator called
```
<DATETIME> [INFO] <SOURCE> called/<TARGET>.
```
Example 1:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/run called/P01.
```
Example 2:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system/run called/mode/system/bypass.
```

### Actuator interrupted
```
<DATETIME> [INFO] <SOURCE> interrupted/<TARGET>.
```
Example:
```
2025-02-11T07:36:19.670Z [INFO] /mode/system interrupted/P01.
```



## Errors
This refers to errors in the program it self. It is not referring to the system for which the program was written for. For example,
an operator on-site disconnects a key actuator in their system. This is not a program error, this is a user/system error and that should
reflect in the logs differently. As another example, say the control engine is programed to receive two different string values ("active" or "deactive") from the HMI,
but what is acually received (and cannot be handeled) is an unexpected "de-active." This is a program error. "Error" messages are to help developers find bugs that occur in production.

Invalid HMI input example:
```
2025-02-11T07:36:19.670Z [ERROR] hmi/system/active/deactive/input log/error/'received invalid input'.
```
User/System error example:
```
2025-02-11T07:36:19.670Z [INFO] /modbus-client/P01 log/'cannot reach P01'.
```
