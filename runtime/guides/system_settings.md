# System Settings

When creating actions or debugging issues, it is important to know the system settings and limitations. Here are the ones you should consider when designing your actions.

| Limit | Description | Configurable | Default |  Range  | 
|---|---| --- | --- | --- |
| timeout | A container is not allowed to run longer than N milliseconds | per action | 60000 milliseconds | 100ms - 60000ms |
| memory | A container is not allowed to allocate more than N MB of memory | per action | 256MB | 128MB - 4096MB |
| minuteRate (ations)| no more than N actions may be invoked per namespace per minute. If exceded, the error is `429: TOO MANY REQUESTS` | not configurable, per namespace | 600/minute | 600/minute |
| logs | A container is not allowed to write more than N MB to stdout | per action | 10MB | 0MB - 10MB |
| concurrent | No more than N activations may be submitted per namespace either executing or queued for execution. If exceded, the error is `429: TOO MANY REQUESTS` | Not configurable, per namespace | 100 | 100 |
| codeSize | The maximum size of the action including dependencies, archived | not configurable, per action | 22MB | 0MB - 22MB |
| parameters | The maximum size of the parameters that can be attached | not configurable, per action/package/trigger | 1MB | 0 - 1MB |
| payload | The maximum POST content size plus any carried parameters for an action invocation or trigger firing | not configurable, per action/trigger | 1MB | 0 - 1MB |
| result | The maximum size of the action result | not configurable, per action | 1MB |  |
| minuteRate (triggers) | No more than N triggers may be fired per namespace per minute. If exceded, the error is `429: TOO MANY REQUESTS` | not configurable, per namespace | 600/minute | 600/minute |
    
    