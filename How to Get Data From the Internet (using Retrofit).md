[Android How-To](Android%20How-To.md)

## Step 1: Add the required dependencies
In `build.gradle.kts(Module :app)`
```kotlin
// Retrofit
implementation("com.squareup.retrofit2:retrofit:2.9.0")
// Retrofit with Scalar Converter
implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
```

## Step 2: Define the API Interface
```kotlin
interface AmphibianApiService {
    @GET("amphibians")
    suspend fun getAmphibiansData(): String
}
```

## Step 3: Make A Retrofit Object
Specifying base url, converter factory etc
```kotlin
const val BASE_URL = "https://android-kotlin-fun-mars-server.appspot.com"
val retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL)
    .addConverterFactory(ScalarsConverterFactory.create())
    .build()
```

## Step 4: Build the API
```kotlin
object AmphibianApi {
    val retrofitService by lazy {
        retrofit.create(AmphibianApiService::class.java)
    }
}
```

---
### References
- [Android Basics with Compose Course](https://developer.android.com/codelabs/basic-android-kotlin-compose-getting-data-internet?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-5-pathway-1%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-getting-data-internet#4)