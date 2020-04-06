---
name: AOSP (Pixel, Nexus…)
manufacturer:
  - stock_android
  - google
redirect_from: /vendors/stock_android.html
explanation: "
We are giving AOSP or stock Android a zero negative score for reference. Even if it is not perfect, and you can still expect issues with background processing (see below) on devices with Android 6+, it is still the best we have :(. The truth is, if OEMs would stop adding battery saving features atop AOSP, Android would be a much better place for users and app developers.


So even stock Android may kill your app when doing tasks valuable to the user. Especially if users enable "Background restrictions" for a particular app.


Moreover, in Android 6.0, Google dramatically restricted background processing with their "Doze mode" [https://developer.android.com/training/monitoring-device-state/doze-standby](https://developer.android.com/training/monitoring-device-state/doze-standby).


This in an attempt to unify various battery saving features across the Android ecosystem, which fell flat. It wasn't only that
Doze mode did not keep various vendors from implementing their own battery saving. Doze mode made some background
processing scenarios impossible on Android, or paradoxically _more_ battery consuming e.g. gathering sensor data through
sensor batching (see [Solution for developers](#dev-solution)).


A serious bug in Doze mode in Android 6.0 even prevented foreground services from doing their job (see [Solution for devs](#dev-solution) for a workaround), but luckily this was fixed in Android 7.0.

"


user_solution: "

## Android P

There's a special option in **Settings > Apps > Your app > Advanced > Battery > Background restrictions**. If users accidentally enable this option, it will break their apps. And users do enable that option!

## Pie and pre-Pie

If you see background processing issues, overall it is a good idea to make your app _not battery optimized_ to ensure it gets the freedom it needs to perform in the background.


For that:

1. Go to **Settings > Apps > Your app > Advanced > Battery > Battery optimization**

2. Change view to **All apps**

3. Search for your app

4. Choose **Not optimized**


## Android O

Make sure **Settings > Apps > Your app > Advanced > Battery > Background limitations** is not enabled. If the app is not yet optimized for Oreo API level it will break their background processing.


## If all fails


If all fails you can turn doze mode completely off in **Settings > Developer options**. (If you don't know how to enable developer options, a web search-engine should help.)
"

developer_solution: "


## Android P Background restriction


For apps whose core functionality is background processing this option basically means 'Break the app core functionality' and this is not always obvious for users. You would be surprised how many support cases we look back on having spent hours debugging, only to find users enabled this. In our opinion this option is unnecessary or even evil. If users don't want the app to do its job, they can still—force close or uninstall. Maybe this is a matter of naming as most users don't know what are background processes, but I guess whatever you do in terms of explanation it is so hard to understand (even for experienced users) what are all the consequences of this that there always will be people enabling it unintentionally.


## Doze mode


Some background processing scenarios are not possible anymore with Doze mode. For example low battery sensor logging
using sensor batching is no longer possible, due to the 9 minute limit on consecutive alarms. The only workaround is keeping a partial wake lock all the time, which means dramatically more battery is consumed for the same job.


Or, a nasty hack to schedule a user-visible alarm though the setAlarmClock() method which can trigger more often.


Tips:

* You can ask the user to make your app _not battery optimized_. See [https://developer.android.com/training/monitoring-device-state/doze-standby](https://developer.android.com/training/monitoring-device-state/doze-standby)


## Android 6.0


A serious bug in Doze mode in Android 6.0 which we (authors of this site) reported to Google (Dianne Hackborn) during the 6.0 BETA does not allow foreground services to keep a wake lock every time an activity or a broadcast receiver kicks in, see [https://plus.google.com/u/0/+AndroidDevelopers/posts/94jCkmG4jff](https://plus.google.com/u/0/+AndroidDevelopers/posts/94jCkmG4jff) and search for Petr Nalevka and Dianne Hackborn.



A workaround is to keep your foreground service in a separate process without any other Android components (read Activities, Receivers, Services…) in that process. This workaround is needed for all Android 6.0 devices but not needed on later devices where this is already fixed.


"

---
