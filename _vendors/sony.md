---
name: Sony
manufacturer:
  - sony
award: 2
position: 11
redirect_from: /vendors/sony.html
explanation: "
Sony gets a high bar in our listing of toxic Android vendors, as historically Sony introduced the first very effective non-standard background process optimization and opened Pandora's box.


It is called **Stamina mode**, and it instantly breaks all background processes and all alarms if enabled.
"
user_solution: "
Never use Stamina mode if you want your device to do something useful when not actively using it.


Try to make your app not battery optimized in *Phone settings > Battery > Three dots in the top right corner > Battery optimisation > Apps > your app*."

developer_solution: "
There is no workaround to prevent background process optimizations in *Stamina mode*, but at least apps can detect that Stamina mode is enabled with the following command:


```
if (Build.MANUFACTURER.equals(\"sony\") && android.provider.Settings.Secure.getInt(context.getContentResolver(), \"somc.stamina_mode\", 0) > 0) {
    // show warning
}
```


The problem is this will only tell if Stamina is enabled, but not if it is currently applied, but we can assume it is when not charged and battery is under X% (TBS)


"

---
