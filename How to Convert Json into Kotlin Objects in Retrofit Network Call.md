[Android How-To](Android%20How-To.md)

## Step 1: Add `kotlinx.Serialization` Library Dependency
1. Open build.gradle.kts (Module :app).
2. In the plugins block, add kotlinx serialization plugin.
	```kotlin
	id("org.jetbrains.kotlin.plugin.serialization") version "1.8.10"
	```
3. In the dependencies section, add the following code to include the kotlinx.serialization dependency.
	```kotlin
	// Kotlin serialization 
		implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.5.1")
	```
4. Add the kotlinx-serialization-converter
	```kotlin
	// Retrofit with Kotlin serialization Converter
	implementation("com.jakewharton.retrofit:retrofit2-kotlinx-serialization-converter:1.0.0")
	implementation("com.squareup.okhttp3:okhttp:4.11.0")
	```
5. Open the build.gradle.kts(project level) & add the following in plugins block.
	```kotlin
	id("org.jetbrains.kotlin.plugin.serialization") version "1.8.10"
	```

## Step 2: Use the library
Pass it as an argument to the builder
```kotlin
private val retrofit = Retrofit.Builder()
        .addConverterFactory(Json.asConverterFactory("application/json".toMediaType()))
        .baseUrl(BASE_URL)
        .build()
```

## Step 3: Change the return type of API interface to the appropriate kotlin object
```kotlin
interface MarsApiService {
    @GET("photos")
    suspend fun getPhotos(): List<MarsPhoto>
}
```
The expected class should have the same structure as the json & should be marked with `@Serializable` annotation.


---
### References
- [Android Basics with Compose Course](https://developer.android.com/codelabs/basic-android-kotlin-compose-getting-data-internet?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-5-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-getting-data-internet#7)