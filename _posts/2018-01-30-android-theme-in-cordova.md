---
layout: post
title: Using a custom Android theme in Cordova App
tags: ionic Cordova
update: 2017-11-29
---

Steps to use a custom Android theme with your Cordova App.  
This affects the look of native dialogs such as the date-picker provided by
[Cordoava-plugin-datepicker](https://github.com/VitaliiBlagodir/cordova-plugin-datepicker),
the color of the some native-spinners (e.g. at App-Start), the statusbar and navigationbar color and
the color of the header in App-Quick-Change (when tapping the square).


### Step 1

Generate your custom `styles.xml`:
```xml
<resources>
    <color name="primaryColor">#0277BD</color>
    <color name="primaryColorDark">#004C8C</color>
    <style name="doneoTheme" parent="@android:style/Theme.DeviceDefault.NoActionBar">
        <!-- on startup the background and app-name is shown, we use the same color to see just background.. -->
        <item name="android:textColorPrimary">@color/primaryColorDark</item>
        <item name="android:colorBackground">@color/primaryColorDark</item>

        <!-- native spinners etc are derived from this -->
        <item name="android:colorAccent">@color/primaryColor</item>

        <!-- AppSwitcher and Statusbar backgrounds-->
        <item name="android:colorPrimary">@color/primaryColorDark</item>
        <item name="android:colorPrimaryDark">@color/primaryColorDark</item>

        <!-- the bottom nav-bar -->
        <item name="android:navigationBarColor">@color/primaryColorDark</item>

        <!-- date- and timepickers need their own themes -->
        <item name="android:datePickerDialogTheme">@style/MyPickerDialogTheme</item>
        <item name="android:timePickerDialogTheme">@style/MyPickerDialogTheme</item>
    </style>

    <style name="MyPickerDialogTheme" parent="android:Theme.Material.Light.Dialog">
        <item name="android:colorAccent">@color/primaryColor</item>
    </style>

    <!-- see https://stackoverflow.com/a/29014475 -->
</resources>
```

Please note we defined a basic theme using our primaryColor for almost anything and
we also defined another theme (inherited from `android:Theme.Material.Light.Dialog`)
for date and time pickers.  
The most important variable to set is `android:colorAccent` as from this variable most other
color related variables are derived.

### Step 2

Make sure our theme will be used for the app, therefore we prepare a [hook](http://cordova.apache.org/docs/en/latest/guide/appdev/hooks/index.html)-file
which manipulated the `AndroidManifest.xml`-file.  
Give it a useful name, such as `hooks/use.android.theme.js`:
```js
#!/usr/bin/env node
// see https://stackoverflow.com/a/35128023
module.exports = function(ctx) {
    var fs = ctx.requireCordovaModule('fs'),
        path = ctx.requireCordovaModule('path'),
        xml = ctx.requireCordovaModule('cordova-common').xmlHelpers;

    var manifestPath = path.join(ctx.opts.projectRoot, 'platforms/android/app/src/main/AndroidManifest.xml');
    var doc = xml.parseElementtreeSync(manifestPath);
    if (doc.getroot().tag !== 'manifest') {
        throw new Error(manifestPath + ' has incorrect root node name (expected "manifest")');
    }

    //adds the tools namespace to the root node
    doc.getroot().attrib['xmlns:tools'] = 'http://schemas.android.com/tools';
    //add tools:replace in the application node
    doc.getroot().find('./application').attrib["android:theme"] = "@style/doneoTheme";
    doc.getroot().find('./application/activity').attrib["android:theme"] = "@style/doneoTheme";

    //write the manifest file
    fs.writeFileSync(manifestPath, doc.write({indent: 4}), 'utf-8');

};
```

### Step 3

Let Cordova do the work, add the following to `config.xml`:
```xml
   <platform name="android">
        ...
        <resource-file src="model/android_theme/styles.xml" target="app/src/main/res/values/styles.xml" />
        <hook src="hooks/use.android.theme.js" type="before_compile" />
    </platform>
```
The first command copies the `styles.xml` where our App can find it later.  
The second command runs the hook to manipulate `AndroidManifest.xml`.

### Step 4

To use our new theme with the datepickers provided by [Cordoava-plugin-datepicker](https://github.com/VitaliiBlagodir/cordova-plugin-datepicker)
we need to explicitly set the `androidTheme`-option to `0` to tell the date-picker-plugin
to use the Android theme used by `MainActivity`!

```js
    constructor(
        public datePicker: DatePicker,
    ){}

    public showPicker(){
        let options = {
            ...
            androidTheme: 0, //we need to set to 0, to use theme of MainActivity
            ...
        }
        this.datePicker.show(options)
            .then( (value: Date) => {
                ...			
            })
            .catch ((err) => {
				...
            });
    }
```
