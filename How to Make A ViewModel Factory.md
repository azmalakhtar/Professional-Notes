[Android How-To](Android%20How-To.md)

There are many ways. One of them is this:
1. Create a companion object inside the viewmodel.
2. Use `viewModelFactory` to make the `ViewModelProvider.Factory`.
```kotlin
    companion object {
        val Factory: ViewModelProvider.Factory = viewModelFactory {
            initializer {
                val application = (this[APPLICATION_KEY] as AmphibianApplication)
                val amphibanRepository = application.container.amphibianRepository
                AmphibianViewModel(amphibianRepository = amphibanRepository)
            }
        }
    }
```

---
### References
- [Android Basics with Compose Course](https://developer.android.com/codelabs/basic-android-kotlin-compose-add-repository?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-5-pathway-2%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-add-repository#5)