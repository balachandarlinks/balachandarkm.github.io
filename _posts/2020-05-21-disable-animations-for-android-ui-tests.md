---

layout: post
firstPublishedAt: 1590080429915
latestPublishedAt: 1590080429915
slug: disable-animations-while-running-android-ui-tests
title: Disable animations while running Android UI Tests

---

![Disable animations for UI tests](https://cdn-images-1.medium.com/max/1944/1*pHeBmKu7ULNERbB1uT792g.png)

Android UI tests require all animations to be disabled to reduce test flakiness. Cloud testing solutions like firebase test lab or CI environments have animations disabled by default in the emulators and devices. But there is no first party solution to do it while running tests locally other than manually disabling animations in developer settings. It is slightly uncomfortable as you may want to enable animations again while manually using the app after completing a UI test run.

I recently wanted to disable animations while running UI tests for a project and found a neat way by combining the tricks from these two articles ([1](https://artemzin.com/blog/easiest-way-to-give-set_animation_scale-permission-for-your-ui-tests-on-android/) and [2](https://testyour.app/blog/emulator)).

Here is the gist of my solution,

Link: https://gist.github.com/balachandarlinks/91c4ff8a907824d75f3feadccd774c87

Our custom TestRunner disables animations before running UI tests and enables it after running UI tests.

Bonus: Incase if you are wondering on what other options are available under settings then check out [https://developer.android.com/reference/android/provider/Settings.Global](https://developer.android.com/reference/android/provider/Settings.Global)

Thanks for your time :)
