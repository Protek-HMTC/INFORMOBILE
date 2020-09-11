# MVVM Architecture

![MVVM](img/1.png)

`MVVM` (Model-View-ViewModel) adalah salah satu konsep arsitektur yang digunakan untuk Android. Arsitektur `MVVM` sendiri adalah arsitektur yang memungkinkan terjadi perubahan di view secara cepat ketika ada data yang berubah. Lalu disini ada beberapa prinsip yang ditekankan untuk arsitektur ini :

- **Pemisahan Fokus**. Menulis semua kode di activity atau fragment merupakan suatu kesalahan. Class ini seharusnya hanya menangani logika yang menangani UI dan interaksi dengan sistem operasi. Dengan menjaga class seramping mungkin, Anda akan terhindar dari banyak masalah terkait lifecycle.
- **Menjalankan UI dari Model**. Model adalah komponen yang bertanggung jawab untuk menangani data untuk sebuah aplikasi. Model tidak terikat dengan view, sehingga tidak terpengaruh oleh lifecycle. Selain itu pengguna tidak akan kehilangan data jika aplikasi dimatikan dan aplikasi tetap berfungsi saat jaringan tidak stabil. Lalu aplikasi Anda akan lebih mudah diuji dan konsisten.

## Menggunakan MVVM Architecture

Penggunaan arsitektur `MVVM` sendiri cukup mudah. Kita akan melihat bagaimana penggunaan sederhana dari arsitektur ini. Berikut langkah-langkahnya :

1. Buat suatu class yang mewarisi kelas `ViewModel`
```kotlin
class UserProfileViewModel : ViewModel() {
    // Agar data bisa berubah dengan bebas dan mengabari ke activity/fragment ketika berubah
    // Maka kita gunakan Tipe Data MutableLiveData
   private var user = MutableLiveData<User>()

   fun setUser(user_data: User) {
       // Proses update data di class ViewModel yang dibuat
       user.postValue(user_data)
   }

   fun getUser() : LiveData<User> = user
}
```

2. Lalu kita tinggal panggil class tersebut di activity maupun fragment
```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    ...
    override fun onCreate(savedInstanceState: Bundle?) {
        ...

        // Proses mendapat object ViewModel di activity
        userProfileViewModel = ViewModelProvider(this, ViewModelProvider.NewInstanceFactory()).get(UserProfileViewModel::class.java)

        userProfileViewModel.getUser().observe(this, {user ->
            // Ubah UI ketika data berubah
        })

    }

    override fun onClick(v: View?) {
        if(v?.id == R.id.button_submit) {
            // Proses update data di ViewModel, inserted_user merupakan contoh variable yang dibuat di activity
            userProfileViewModel.setUser(inserted_user)
        }
    }

    ...
}
```

## Sumber

- https://developer.android.com/jetpack/guide
- https://medium.com/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b