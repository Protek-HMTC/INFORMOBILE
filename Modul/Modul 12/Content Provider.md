# Content Provider

Sebuah `Content Provider` dapat memungkinkan aplikasi lain untuk mendapat data yang sudah disimpan di aplikasi kita. Pada umumnya `Content Provider` akan disambungkan ke `SQLite` yang ada di suatu aplikasi. 

![content-provider-scheme](img/1.png)

`Content Provider` mengatur akses ke penyimpanan data di aplikasi Anda untuk sejumlah komponen seperti gambar diatas, yang meliputi :

- Membagikan akses data ke aplikasi lain
- Mengirim data ke widget
- Mengembalikan hasil custom search untuk aplikasi Anda dengan framework pencarian menggunakan `SearchRecentSuggestionsProvider`
- Sinkronisasi data aplikasi dengan server menggunakan implementasi `AbstractThreadedSyncAdapter`
- Loading data di UI menggunakan `CursorLoader`

## Implementasi Content Provider

Ada beberapa langkah yang dibutuhkan untuk implementasi `Content Provider`, diantaranya :

1. Setup SQLite seperti di modul sebelumnya
2. Mendesain authority, authority disini biasanya adalah nama internal dari aplikasi Android. Digunakan untuk membedakan dengan provider lain.
```com.example.app```
3. Mendesain path, seperti alamat web, kita harus mendesain path agar tahu pengakses provider kita sedang akan meminta data jenis apa dan dari mana. Biasanya nama tabel yang ingin diakses akan di-append ke authority.
```com.example.app/table1```
4. Menangani ID content URI, secara konvensi developer akan mengakses ID dari data yang diinginkan di akhir dari path.
5. Memasukkan ke `UriMatcher`, `UriMatcher` akan membantu menentukan aksi apa yang akan dilakukan sesuai dengan content URI yang diakses oleh aplikasi lain. Biasanya URI yang dimasukkan disini akan di mapping ke nilai Integer.

Berikut contoh langkah 2 - 5 :

```kotlin
private val sUriMatcher = UriMatcher(UriMatcher.NO_MATCH).apply {
    /*
     * The calls to addURI() go here, for all of the content URI patterns that the provider
     * should recognize. For this snippet, only the calls for table 3 are shown.
     */

    /*
     * Sets the integer value for multiple rows in table 3 to 1. Notice that no wildcard is used
     * in the path
     */
    addURI("com.example.app", "table3", 1)

    /*
     * Sets the code for a single row to 2. In this case, the "#" wildcard is
     * used. "content://com.example.app.provider/table3/3" matches, but
     * "content://com.example.app.provider/table3 doesn't.
     */
    addURI("com.example.app", "table3/#", 2)
}
...
class ExampleProvider : ContentProvider() {
    ...
    // Implements ContentProvider.query()
    override fun query(
            uri: Uri?,
            projection: Array<out String>?,
            selection: String?,
            selectionArgs: Array<out String>?,
            sortOrder: String?
    ): Cursor? {
        var localSortOrder: String = sortOrder ?: ""
        var localSelection: String = selection ?: ""
        when (sUriMatcher.match(uri)) {
            1 -> { // If the incoming URI was for all of table3
                if (localSortOrder.isEmpty()) {
                    localSortOrder = "_ID ASC"
                }
            }
            2 -> {  // If the incoming URI was for a single row
                /*
                 * Because this URI was for a single row, the _ID value part is
                 * present. Get the last path segment from the URI; this is the _ID value.
                 * Then, append the value to the WHERE clause for the query
                 */
                localSelection += "_ID ${uri?.lastPathSegment}"
            }
            else -> { // If the URI is not recognized
                // You should do some error handling here.
            }
        }

        // call the code to actually do the query
    }
}
```

Seharusnya kita juga memasukkan ke `AndroidManifest.xml`, tetapi jika kita generate `ContentProvider` tersebut lewat aplikasi, maka hal ini akan otomatis dilakukan.

6. Implementasi fungsi-fungsi di class `ContentProvider`, `ContentProvider` sendiri memiliki beberapa fungsi yaitu :
- `query()`, mengambil data lewat provider. Fungsi ini akan mengembalikan object `Cursor` dan kita bisa memperlakukannya seperti di modul sebelumnya.
- `insert()`, memasukkan data baru ke provider. Akan mengembalikan Content URI untuk data yang baru dimasukkan.
- `update()`, memperbarui data di provider. Akan mengembalikan jumlah baris yang di-update.
- `delete()`, menghapus row lewat provider. Akan mengembalikan jumlah row yang dihapus.
- `getType()`, mengembalikan type MIME yang sesuai dengan sebuah content URI.
- `onCreate()`, menginialisasi provider. Biasanya dipanggil langsung ketika object provider dibuat.

Berikut sedikit contoh untuk insert :

```kotlin
...
class ExampleProvider(context: Context) : ContentProvider() {

    // Definisi dari helper database seperti di modul sebelumnya
    private lateinit var databaseHelper: DatabaseHelper

    override fun onCreate(): Boolean {
        databaseHelper = DatabaseHelper(context)
        return true
    }

    ...

    // Implements the provider's insert method
    override fun insert(uri: Uri, values: ContentValues?): Uri? {
        val added: Long = when (sUriMatcher.match(uri)) {
            1 -> {
                databaseHelper.insert(values)
            }
        }

        context?.contentResolver?.notifyChange(CONTENT_URI, null)

        return Uri.parse("com.example.app/$added")
    }
}
```

7. Yang terakhir adalah menaruh tag permission di `AndroidManifest.xml`
```xml
    <permission
        android:name="com.example.app.READ_DATABASE"
        android:protectionLevel="normal" />
    <permission
        android:name="com.example.app.WRITE_DATABASE"
        android:protectionLevel="normal" />
```

Lalu kita tambahkan atribut dibawah ini ke tag provider di dalam tag application
```xml
android:readPermission="com.example.submission2.READ_DATABASE"
android:writePermission="com.example.submission2.WRITE_DATABASE"
```

## Mengakses ContentProvider dari Apps Lain

Untuk bisa mengakses `ContentProvider` aplikasi lain, maka dibutuhkan object `ContentResolver`. object ini sudah tersedia di activity kalian, jadi bisa langsung dipanggil. Untuk langkah-langkah agar bisa mengakses `ContentProvider` aplikasi lain adalah sebagai berikut :

1. Taruh tag uses-permission sesuai dengan permission di aplikasi yang bertindak sebagai provider
```xml
<uses-permission android:name="com.app.example.READ_DATABASE">
<uses-permission android:name="com.app.example.WRITE_DATABASE">
```

2. Lalu kita tinggal akses menggunakan `contentResolver`, lalu menyesuaikan fungsi dengan tipe data return. Di bawah ini contoh untuk melakukan insert
```kotlin
// Defines a new Uri object that receives the result of the insertion
lateinit var newUri: Uri

...

// Defines an object to contain the new values to insert
val newValues = ContentValues().apply {
    /*
     * Sets the values of each column and inserts the word. The arguments to the "put"
     * method are "column name" and "value"
     */
    put("data1", "data1")
    put("data2", "data2")
}

newUri = contentResolver.insert(
    "content://com.example.app/table3",   // the user dictionary content URI
    newValues                          // the values to insert
)
```

## Sumber

- https://developer.android.com/guide/topics/providers/content-provider-basics
- https://developer.android.com/guide/topics/providers/content-provider-creating