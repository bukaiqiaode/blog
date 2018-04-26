## Definition of the diagram
```shell
title CI process

participant Target 
participant Runner 
participant Jenkins
participant SCM
participant Engineer
participant Issue Tracking System
note over Jenkins, SCM: monitoring
Engineer -> SCM: code check-in
Jenkins -> Runner: notify
Runner -> SCM: pull latest code
Runner -> Target: deploy to
Runner  -> Runner : local configuration
Runner -> Runner : create schedule
Runner -> Target  : execution
Target -> Runner : data collection
Runner -> Issue Tracking System: result & issue
Runner -> Runner : Allure report
Runner -> Jenkins: Submit report
Jenkins -> Engineer: notify
Engineer -> Jenkins: check report
Engineer -> Issue Tracking System: tracking & resolve
```

## The diagram
![Image](https://github.com/bukaiqiaode/blog/blob/master/images/CIProcess.png)

## References
- https://www.websequencediagrams.com/
