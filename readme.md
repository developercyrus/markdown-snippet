**Clarification and Requirement**
1. There's an adhoc import function required (by Peter Pan in meeting @ 2016-12-01)
2. Escalation is triggered by days rather by times. If a regular import is failed, there will be N days of grace period for re-importing. At day N, it will escalate to HIT (by Peter Pan in meeting @ 2016-12-01). 
3. [Issue 14](https://www.google.com.hk) enhance the escalation logic  
4. ~~As long as in the grace period, the re-import will be triggered if the last import result is non-success (fail, missing), regardless of regular/ adhoc import. Suppose a regular schedule is set at 19th to run, and there're 4 days grace period. In event id 6, the flow will escalate to HIT because the import result is missing in the last adhoc import (by Mary in meeting @ 2017-01-06).~~

    | event id | schedule | day | event | last import result
    |----------|----------|-----|---------------------------|--------------
    | 1 |           | 18  | Sys admin upload interface   |                                            
    | 2 | start     | 19  | Regular import trigger, but import fail. Sys admin re-upload interface    | fail                
    | 3 |           | 20  | Re-import trigger, and import success.  | success                                     
    | 4 |           | 21  | Someone perform adhoc import, but import fail   | fail                                      
    | 5 |           | 22  | Regular import trigger again. Since sys admin unware, no interface file is upload, so import is missed | miss
    | ~~6~~ | ~~start + 4~~ | ~~23~~  | ~~Since the last import result is non-success (miss), and it's over the grace period, it will *still* escalate to HIT *although there was success case before*~~ | ~~miss~~ 

**Implementation**
1. Regular import is scheduled by XML 
