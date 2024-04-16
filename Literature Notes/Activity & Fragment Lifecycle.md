domain: 
course: [[Android Development Kotlin Course Udacity]]
teacher:
date: 2024-03-20
time: 14:49
status: #unprocessed

# Activity & Fragment Lifecycle
## 1 The Case of Missing Data
## 2 Why Track Activity State
- Because it will help us in being a **good Android Citizen**.
- Android OS sometimes kills your app and restart it, so you want to program your app both *proactively* and *defensively*.
	- Proactively -> clean up unused resources so that activities on screen runs smoothly
	- Defensively -> in case OS does something like restarting your app.

## 3 Exercise: Introduction to the Activity Lifecycle Diagram
- Activity Lifecycle States - The status of the Activity
	- Created
	- Started
	- Resumed
	- Destroyed
	- Initialized
- Activity Lifecycle Callbacks - called when activity moves from one state to another.
	- `onCreate`
	- `onStart`
	- `onResume`
	- `onDestroy`
	- `onPause`
	- `onStop`
	- `onRestart`

## 4 Exercise: Introduction to Logging
- Android has a logging API
- Syntax - `Log.i("TAG", "Message")`
- Logs have different levels which are used for different situations.
	- **Verbose**: Show all log messages(the default)
	- **Debug**: Show debug log messages that are useful during development only, as well as the message lower in this lise.
	- **Info**: Show expected log messages for regular usage, as well as the message levels lower in this list.
	- **Warn**: Show possible issues that are not yet errors, as well as the message levels lower in this list.
	- **Error**: Show issues that have caused errors, as well as the message lower in this list.
	- **Assert**: Show issues that the developer expects should never happen.

## 5 Exercise: The Application Class and Timber
- Timber is a library for logging.
- The benefits of using timber are following
	- Generates Tags: Automatically based on class name.
	- Avoid logs in released app apks
	- Easy integration with crash reporting.
- Setup Timber
	1. Add Timber to build.gradle.
	2. Make Application Class
	3. Add Application to Manifest
	4. Initialize Timber in Application Class.
- **Use Application Class with care**. Having lot of functionality in application class is not good for your app. Before adding anything to the application class, think carefully.

## 6 Lifecycle: Open and Close

## 7 Lifecycle: Share dialog
![Activity Lifecycle](https://imgs.search.brave.com/Z777wxL5syZPo7mqyRVWeXY1Cq9AMo7Px6ThQCx_KuI/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9kZXZl/bG9wZXIuYW5kcm9p/ZC5jb20vc3RhdGlj/L2NvZGVsYWJzL2Jh/c2ljLWFuZHJvaWQt/a290bGluLWNvbXBv/c2UtYWN0aXZpdHkt/bGlmZWN5Y2xlL2lt/Zy80Njg5ODg1MThj/MjcwYjM4LnBuZw)


![Activity Lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png)
## 8 `onCreate` vs `onStart`
## 9 Activity Lifecycle States and Callbacks Summary
### General Definitions
- **Visible Lifecycle:** The part of the Lifecycle between onStart and onStop when the Activity is visible.
- **Focus:** An Activity is said to have focus when it's the activity the user can interact with.
- **Foreground:** When the activity is on screen.
- **Background:** When the activity is fully off screen, it is considered in the background.

### Lifecycle States
These are same for both the Fragment Lifecycle and the Activity Lifecycle.
- **Initialized**: This is the starting state whenever you make a new activity. This is a transient state , it immediately goes to Created.
- **Created**: Activity has just been created, but it's not visible and it doesn't have focus.
- **Started**: Activity is visible but doesn't have focus.
- **Resumed**: The state of the activity when it is running. It is visible and has focus.
- **Destroyed**: Activity is destroyed. It can be ejected from memory at any point and should not be referenced or interacted with.

### Activity Lifecycle Callbacks
- `onPause` is a UI blocking callback. Keep the execution of it fast, because the next activity is not shown until this completes.
- `onStop`
	- You can use this to persist data.
	- Stop logic that updates the UI.
- `onDestroy`
	- happens when you navigate back out of the activity (as in press back button or manually call `finish()`).
	- Tear down or release any resources that are related to the activity and are not automatically released for you. Forgetting to do this could cause a memory leak!
	- Logic that refers to the activity or attempts to update the UI after the activity has been destroyed could crash the app!
### Fragment Lifecycle Callbacks
#### Important Fragment Callbacks to Implement
- `onCreate`
	- This is when the fragment is created.
	- Here you should
		- Initialize anything essential for your fragment.
		- **Do not inflate XML**, do that in `onCreateView`, when the system is first drawing the fragment.
		- **Do not reference the activity**, it is still being created. Do this in `onActivityCreated`.
- `onCreateView`
	- called between `onCreate` and `onActivityCreated`.
	- You must return a view in this callback if your fragment has a UI.
	- Here you should:
		- Create your views by inflating your XML
- `onStop`
	- Save any permanent fragment state
#### Other Callbacks
- `onAttach`
	- when the fragment is first attached to the activity
	- only called once during the lifecycle of fragment.
- `onActivityCreated`
	- Called when the activity onCreate method has returned and the activity has been initialized
	- If the fragment is added to an activity that's already created, this still gets called.
	- It's purpose is to contain any code the requires the activity exists and it is called multiple times during the lifecycle of the fragment.
	- Here you should:
		- - Execute any code that requires an activity instance
- `onDestroyView`
	- Unlike activities, **fragment views are destroyed every time they go off screen**.
	- This is called after the view is no longer visible.
	- Do not refer to views in this callback, since they are destroyed
- `onDestroy`
	- called when the activity's `onDestroy` is called.
- `onDetach`
	- Called when the association between the fragment and the activity is destroyed.
### Lifecycle Cheat Sheets
- [**The Android Lifecycle cheat sheet — part I: Single Activity**(opens in a new tab)](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-i-single-activities-e49fd3d202ab) - This is a visual recap of much of the material here.
- [**The Android Lifecycle cheat sheet — part II: Multiple Activities**(opens in a new tab)](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-ii-multiple-activities-a411fd139f24) - This shows the order of lifecycle calls when two activities interact.
- [**The Android Lifecycle cheat sheet — part III: Fragments**(opens in a new tab)](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-iii-fragments-afc87d4f37fd) - This show the order of lifecycle calls when an activity and fragment interact.

## 10 Lifecycle: Navigate Away

## 12 Exercise: Setup and Teardown
## 13 Introduction to the Lifecycle Library
- Lifecycle library introduced Android Lifecycle as an actual object.
- It also introduced `LifecycleOwner` & `LifecycleObserber` interfaces.
	- `LifecycleOwner`: A class that has a lifecycle, ex. Activity and Fragments
	- `LifecycleObserver`: Observer of a `LifecycleOwner`, such as an Activity or Fragment
- 

## 14 Exercise: Lifecycle Observation
- It is based on the observer pattern
- Using observer pattern, the responsibility of setup and teardown shifts from the Activity/Fragment to `LifecycleObserver`.
- Steps
	1. Subscribe to observe the owner.
	2. Annotate methods you want to execute when a certain event happens.

## 15 Process Shutdown
- The main concern of the Android OS is to keep foreground apps running smoothly.
- It does that by
	- limiting the amount of processing background apps can do
	- and in some cases, killing the background apps.
		- it does so when the OS is already pretty stressed out, which means no additional callback or code is called.
- 

## 16 [Process Shutdown Demo](https://www.youtube.com/watch?v=UwMcGa5zVtc&t=95s)
- **Terminating A Process**
	1. Make sure adb is installed,
	2. Make sure your app is in the background.
	3. Run the command `adb shell am kill com.example.android.dessertpusher`.
		- you can replace the `com.example.android.dessertpusher` with the name of your app.
- When killing you app, the OS saves some of the state in a bundle, like
	- Back stack information
	- view data, like text in edit text
- When open the app after OS has killed it, OS opens the app with the saved state.
- But OS doesn't save all the data. To save the data important for your app, use `onSaveInstanceState`.
- 
## 17 Exercise: `onSaveInstanceState`
- `onSaveInstanceState` is called after `onStop` and `onRestoreInstanceState` is called after `onStart`.
- **Save the data in onSaveInstanceState**
	- `outState.putInt(KEY_REVENUE, revenue)`
	- you can put in the bundle any primitive value or any object that be serialized and deserialized.
	- keep the saved data short, because this data is stored in RAM and every device has an limit of how large data you can store. If you exceed that size, then an error occurs.
- **Retrieve and use the data in onCreate if you're restarting to Activity**
	- in onCreate, there is a savedInstanceState bundle passed. If the activity is created fresh, then that bundle is null, but if the activity starts after saving the data in onSaveInstanceState then the bundle is not null and you can retrieve the saved values.
#todo save revenue, dessertsSold and dessertTimer.secondCount

## 18 Configuration Changes
- A major change a device undergoes that causes the activity to be rebuilt.
- Examples of configuration changes are:
	- User changes device language
	- User plugs in physical keyboard
	- User rotates device

## 19 The Future of Lifecycles