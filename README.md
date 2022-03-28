# Xdimen
Support multiple screen sizes easily by scalling your dimensions.
### 

### How Xdiemn works?
When you have a UI design with specific diemnsion and you need to support different mobiles or tablets in portrait, landscape or both. This case **Xdimen** can help you to generate scalable diemsions for common devices screen sizes.
 

Xdimen generates an alternative res directory for every common screen width (configurable) and scale your design dimensions.

## Usage
For now publishing gradle on `gradle portal` is pending approval, docs will be updated once it get published.
 
### Add to project
 Xdimen must be applied in an android project.

<details>
<summary>Groovy</summary> 
  
````groovy
  plugins {
    id 'io.github.islamkhsh.xdimen' version "$latest_version"   
    // agb plugin
  } 
````
</details>

<details>
<summary>Kotlin</summary> 
  
````kotlin
  plugins {                         
    id("io.github.islamkhsh.xdimen") version "$latest_version"  
    // agb plugin
  }
````
</details>

### Configure Xdimen
All properties are optionals, only `designWidth` is required. 
Poroperies disscussed in more details in below section.

<details>
<summary>Groovy</summary> 
  
````groovy
  xdimen {
    deleteOldXdimen = true
    designWidth = 411 // required
    designDpi = mdpi()
    targetDevicesWidth = [360, 375, 411]
    dimensRange {
        minDimen = -10
        maxDimen = 600
        step = 0.5d
    }
    fontsRange {
        minDimen = 10
        maxDimen = 60
        step = 1.0d
    }
}
````
</details>

<details>
<summary>Kotlin</summary> 
  
````kotlin
  xdimen {
    deleteOldXdimen.set(true)
    designWidth.set(411) // required
    designDpi.set(mdpi())
    targetDevicesWidth.set(phonePortrait)
    dimensRange {
        minDimen.set(-10)
        maxDimen.set(500)
        step.set(0.5)
    }
    fontsRange {
        minDimen.set(10)
        maxDimen.set(60)
        step.set(1.0)
    }
}
````
</details> 

### Use scaled dimens
After generate xdiemn resources (see next section) you can use the dimens 
```xml
   <TextView android:layout_width="@dimen/x100dp" # width in design is 100dp
             android:layout_height="@dimen/x50_5dp" # height in design is 50.5dp
             android:padding="@dimen/neg_x6_5dp" #  padding in design is -6.5dp
             android:textSize="@dimen/x12sp" # textSize in design is 12sp
             />
```

## Tasks
To excute a task, make sure you excute it on an android project using one of these ways
- `$ gradle :prjectName:taskName`.
- From gradle tool window under `android` group.
- From `Run AnyThing` window.

### `generateXdimen`
Generates alternative resources for every device width in `targetDevicesWidth` set and for every created resource qualifier it scales the dimensions to fit with this width. It uses configured proprties to calculate scaling factor, get range of dimensions to generate and list of screen widths' to target.

### `deleteXdimen` 
Delete the previous generated xdimen resources unless you renamed the res file or dir. It will be excuted before `generateXdimen` if you set `deleteOldXdimen` property by true.

## Configuration properties

<details>
<summary><b>deleteOldXdimen</b></summary>  
  
By setting it when you excute `generateXdimen` task `deleteXdimen` will be excuted first to delete all previous generated xdimen resources and thier dirs if directory contains only `xdiemn.xml` file. 

> If you renamed the `xdimen.xml` file or its directory for any reason, this file won't be deleted. 
  
> Default value: **true**.
</details>

<details>
<summary><b>designWidth</b></summary>  
  
The value of screen width of your design in `dp` unit. It will be used with `designDpi` to calculate the relativeDesignWidth (width realtive to main denisity `mdpi`) an then calculate a scaling factor for every screen sidth in `targetDevicesWidth`.
  
> If your design is in `px` set its width in this proprty and set `designDpi` to be `mdpi` as in mdpi 1px = 1dp.
  
> No default value because it's reuired and must be configured.
</details>

<details>
<summary><b>designDpi</b></summary>  
  
The design screen density (dot per inch) [see more](https://developer.android.com/training/multiscreen/screendensities). This will be used to with `designWidth` to calculate the relativeDesignWidth.
  
> Default value is: **mdpi**
  
> Predefined densities: for every density in [common densities](https://developer.android.com/training/multiscreen/screendensities#TaskProvideAltBmp) there's a mthod with its name ( `ldpi()`, `mdpi()`, `hdpi()`, ... etc), Also there's a method to set custom density `dpi(value)`.
</details>

<details>
<summary><b>targetDevicesWidth</b></summary>  
  
The width of screens of devices which you target. For every width in this list an alternative resource will be generated with scalled dimens.
  
  ex: if list is [350, 400] then 
  ```
  ...
 -> values
    - xdimen.xml
 -> values-w350dp
    - xdimen.xml
 -> values-w400dp
    - xdimen.xml
  ...
```
  And this means that all devices with 
  - screen width < 350 will use dimens in `values/xdimen.xml`.
  - 350dp <= screen width < 400 will use dimens in `values-w350dp/xdimen.xml`.
  - screen width >= 400 will use dimens in `values-w400dp/xdimen.xml`.
  
> You can use predefined set as it's , add or remove from it or provide your own set.
  
> Default value is: **[designWidth]** set of designWidth provided value.
  
> Predefined sets for common devices: 
  - `phonePortrait`: common phones in portrait orientation.
  - `phoneLandscape`: common phones in portrait orientation.
  - `tabletPortrait`: common tablets in portrait orientation.
  - `tabletLandscape`: common tablets in portrait orientation.
  ###
  - `phoneDevices`: common phones in portrait and landscape.
  - `tabletDevices`: common tablets in portrait and landscape.
  - `allDevices`: common phones and tablets in both portrait and landscape.
  ###
  - `devicesInPortrait`: common phones and tablets in portrait.
  - `devicesInLandscape`: common phones and tablets in landscape.
  
 These lists collected from many sources: [Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_high-definition_smartphone_displays#720p_by_1280_(HD_ready)), 
  [ScreenSize](https://screensiz.es/), [Pixensity](https://pixensity.com/list/) and others.
</details>
  
  <details>
<summary><b>dimensRange</b></summary>  
  
The range of dimens you want to be generated and scaled.
- `minDimen`: the minimum dimen to be generated.
- `maxDimen`: the maximum dimen to be generated.
- `step`: the step between between two genarted dimen.
    
> Default value: **minDimen=-10**,  **maxDimen=600**,  **step=1.00**.
 
</details>
  
   <details>
<summary><b>fontsRange</b></summary>  
  
The same of `dimensRange` but for fonts dimens range.
    
> Default value: **minDimen=6**,  **maxDimen=48**,  **step=1.00**.
 
</details>
