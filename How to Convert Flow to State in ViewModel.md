[Android How-To](Android%20How-To.md)

The recommended way to expose a `Flow` from a `ViewModel` is with a `StateFlow`. Using a `StateFlow` allows the data to be saved and observed, regardless of the UI lifecycle.

```kotlin
    val homeUiState: StateFlow<HomeUiState> =
        itemsRepository.getAllItemsStream().map { HomeUiState(it) }
            .stateIn(
                scope = viewModelScope,
                started = SharingStarted.WhileSubscribed(TIMEOUT_MILLIS),
                initialValue = HomeUiState()
            )
```

