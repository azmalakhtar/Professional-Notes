[Android How-To](Android%20How-To.md)

## Step 1. Create an Entity 
Create a data class that represents a row in your database.
One field should be marked as `@PrimaryKey`, so that the Room knows which column is primary key.
```kotlin
@Entity(tableName = "items")
data class Item(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val name: String,
    val price: Double,
    val quantity: Int
)
```

## Step 2. Create a Database Access Object (DAO) Interface
A DAO represents how the app communicates with the database. It exposes methods that corresponds to some query on the database.
```kotlin
@Dao
interface ItemDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(item: Item) // add suspend so that it runs on a background thread

    @Update
    suspend fun update(item: Item)

    @Delete
    suspend fun delete(item: Item)

    @Query("SELECT * FROM items WHERE id = :id")
    fun getItem(id: Int): Flow<Item> // no need to add suspend as flow automatically runs on a background thread

    @Query("SELECT * FROM items ORDER BY name ASC")
    fun getAllItems(): Flow<List<Item>>
}
```

## Step 3. Create a Database Class
```kotlin
@Database(entities = [Item::class], version = 1, exportSchema = false)
abstract class InventoryDatabase : RoomDatabase() {
    abstract fun itemDao(): ItemDao

    companion object {
        @Volatile // volatile variables are not cached, read & write are done to and from main memory
        private var Instance: InventoryDatabase? = null

        fun getDatabase(context: Context): InventoryDatabase {
            return Instance ?: synchronized(this) {
                Room.databaseBuilder(context, InventoryDatabase::class.java, "item_database")
                    .build()
                    .also { Instance = it }
            }
        }
    }
}
```