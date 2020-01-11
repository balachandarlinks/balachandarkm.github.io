---

layout: post
firstPublishedAt: 1571081616798
latestPublishedAt: 1576600300074
slug: accept-language-http-header-in-android-app-network-requests
title: Accept-Language HTTP Header in Android App Network Requests

---

[Accept-Language](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language) header is used in network requests to inform the languages supported by a client. Until API 23 (6.0), Android supported just one locale and so passing the value for Accept-Language header is straight forward.

![](../images/accept-language-http-header-in-android-app-network-requests/1.png)

```
Accept-Language: en-IN
```

Note that we need to pass the [language tag](<https://developer.android.com/reference/java/util/Locale.html#toLanguageTag()>) of the currently selected locale.

Android 7.0 (API 24) introduced [multi-locale support](https://developer.android.com/guide/topics/resources/multilingual-support) to provide enhanced experience for multilingual users. For example, Chrome browser is using it understand when the user may need the web-page translation service. If the webpage is in one of the user preferred language then it don’t need to show the translation service.

So now, we need to pass multiple languages in Accept-Language header to share the language preferences for multi lingual Android users. Find below the syntax for passing multiple languages,

```
Accept-Language: en-IN, fr-FR;q=0.9, it-IT;q=0.8
```

_q — weight of the language between 0 to 1. It is using the [quality-value](https://developer.mozilla.org/en-US/docs/Glossary/quality_values) syntax._

[LocaleList](https://developer.android.com/reference/android/os/LocaleList) (Introduced in API 24) exposes the languages selected by multi lingual users. But there is no API to provide the value for accept-language header and we are on our own.

There is also a compatibility API called [LocaleListCompat](https://developer.android.com/reference/kotlin/androidx/core/os/LocaleListCompat). We can use it to seamlessly get the user preferred languages across all Android APIs.

```
fun getAcceptedLanguageHeaderValue(): String {
    var weight = 1.0F
    return getPreferredLocaleList()
        .*map ***{ it**.toLanguageTag() **}
        **.*reduce ***{ **accumulator, languageTag **->
            **weight -= 0.1F
            "$accumulator,$languageTag;q=$weight"
        **}
**}

fun getPreferredLocaleList(): List<Locale> {
    val adjustedLocaleListCompat = LocaleListCompat.getAdjustedDefault()
    val preferredLocaleList = *mutableListOf*<Locale>()
    for (index in 0 *until *adjustedLocaleListCompat.size()) {
        preferredLocaleList.add(adjustedLocaleListCompat.get(index))
    }
    return preferredLocaleList
}
```

Note that I have used [getAdjustedDefault()](<https://developer.android.com/reference/android/support/v4/os/LocaleListCompat.html#getAdjustedDefault()>) instead of [getDefault()](<https://developer.android.com/reference/android/support/v4/os/LocaleListCompat.html#getDefault()>) as It don’t guarantee Locale.getDefault() as the first one in the list of locales returned.

Incase if you are using [OkHttp](https://square.github.io/okhttp/) for networking and also looking for a way to seamlessly add this header to all your network requests then have a look at the [OkHttp interceptors](https://square.github.io/okhttp/interceptors/).

Thanks for your time :)
