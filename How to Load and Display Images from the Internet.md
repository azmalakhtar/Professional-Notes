[Android How-To](Android%20How-To.md)

## Step 1: Add the Coil Library
In the build.gradle.kts, add the following dependecy.
```kotlin
// Coil
implementation("io.coil-kt:coil-compose:2.4.0")
```

## Step 2: Use the `AsyncImage` Composable to Display the Image
```kotlin
AsyncImage(
    model = ImageRequest.Builder(LocalContext.current)
        .data("https://example.com/image.jpg")
        .crossfade(true)
        .build(),
    placeholder = painterResource(R.drawable.placeholder),
    contentDescription = stringResource(R.string.description),
    contentScale = ContentScale.Crop,
    modifier = Modifier.clip(CircleShape)
)
```


---
### References
- [Android Basic with Compose Course](https://developer.android.com/codelabs/basic-android-kotlin-compose-load-images?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-5-pathway-2%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-load-images#2)