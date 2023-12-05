# ardupilot_fence_exercise
ardupilot fence exercise

## enabling max alt fence 

check the docs here: https://ardupilot.org/copter/docs/common-geofencing-landing-page.html

updating the parameters: 

FENCE_ENABLE = 1
FENCE_ACTION = 0 (so it won't do RTL everytime) 
FENCE_ALT_MAX = 20 (lower it)


## updating messages sent to MissionPlanner

`git status` output: 

```
On branch master
Your branch is behind 'origin/master' by 9524 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   .vscode/settings.json
	modified:   ArduCopter/fence.cpp
```

`git diff ArduCopter/fence.cpp` output: 

```
diff --git a/ArduCopter/fence.cpp b/ArduCopter/fence.cpp
index ab9164ed72..ddca61db2c 100644
--- a/ArduCopter/fence.cpp
+++ b/ArduCopter/fence.cpp
@@ -24,7 +24,11 @@ void Copter::fence_check()
     if (new_breaches) {
 
         if (!copter.ap.land_complete) {
-            GCS_SEND_TEXT(MAV_SEVERITY_NOTICE, "Fence Breached");
+            GCS_SEND_TEXT(MAV_SEVERITY_ERROR, "MY NEW Fence Breached");
+            if (new_breaches & AC_FENCE_TYPE_ALT_MAX) {
+                GCS_SEND_TEXT(MAV_SEVERITY_ERROR, "your'e flying too HIGH!");
+            }
+
         }
 
         // if the user wants some kind of response and motors are armed
```

**important remark ! mission planner overrides fence breach message with his own, so they might not see theirs on the hud but it is shown in the messages. **




