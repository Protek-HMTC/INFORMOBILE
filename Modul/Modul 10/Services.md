# Services

![services](img/1.png)

`Service` adalah komponen aplikasi yang melakukan operasi jangka panjang di background. `Service` tidak menyediakan User Interface. Ketika distart, maka sebuah `Service` bisa berjalan terus menerus untuk beberapa waktu, bahkan saat user pindah ke aplikasi lain.

Selain itu sebuah komponen bisa dapat dipasangkan dengan sebuah `Service` untuk berinteraksi dengannya dan bahkan melakukan interprocess communication (IPC). Sebagai contoh, `Service` bisa menangani network transaction, memainkan lagu, melakukan file I/O, atau berinteraksi dengan content provider. Semua itu lewat background.

## Tipe-Tipe dari Service

### Foreground

`Foreground Service` melakukan beberapa operasi yang dapat dilihat oleh user. Sebagai contoh, aplikasi pemutar musik akan menggunakan `Foreground Service` untuk memainkan sebuah track audio. `Foreground Service` harus menampilkan sebuah `Notification`. `Foreground Service` akan terus berjalan walaupun user tidak berinteraksi dengan aplikasi.

### Background

`Background Service` melakukan operasi yang tidak terlihat secara langsung oleh pengguna. Misalnya ketika aplikasi menggunakan layanan untuk memadatkan penyimpanannya, aplikasi tersebut biasanya berjalan di latar belakang.

### Bound

![bound service](img/2.png)

Sebuah `Service` adalah `Bound` ketika suatu komponen aplikasi mengikatnya dengan memanggil `bindService()`. `Bound Service` menawarkan sebuah `client-server interface` yang memungkinkan suatu komponen untuk berinteraksi dengan `Service`, mengirim requests, menerima results, dan bahkan melakukan interprocess communication (IPC). Sebuah `Bound Service` hanya berjalan selama ada komponen yang terikat dengannya. Beberapa komponen bisa diikat ke satu `Service`, tetapi ketika tidak ada komponen yang terikat, maka `Service` tersebut akan dihilangkan.

## Menggunakan Service

Ini adalah contoh sederhana langkah-langkah dalam memakai `Service`.

1. Buat suatu class yang mewarisi class `Service()`
```kotlin
class HelloService : Service() {

    private var serviceLooper: Looper? = null
    private var serviceHandler: ServiceHandler? = null

    // Handler that receives messages from the thread
    private inner class ServiceHandler(looper: Looper) : Handler(looper) {

        override fun handleMessage(msg: Message) {
            // Normally we would do some work here, like download a file.
            // For our sample, we just sleep for 5 seconds.
            try {
                Thread.sleep(5000)
            } catch (e: InterruptedException) {
                // Restore interrupt status.
                Thread.currentThread().interrupt()
            }

            // Stop the service using the startId, so that we don't stop
            // the service in the middle of handling another job
            stopSelf(msg.arg1)
        }
    }

    override fun onCreate() {
        // Start up the thread running the service.  Note that we create a
        // separate thread because the service normally runs in the process's
        // main thread, which we don't want to block.  We also make it
        // background priority so CPU-intensive work will not disrupt our UI.
        HandlerThread("ServiceStartArguments", Process.THREAD_PRIORITY_BACKGROUND).apply {
            start()

            // Get the HandlerThread's Looper and use it for our Handler
            serviceLooper = looper
            serviceHandler = ServiceHandler(looper)
        }
    }

    override fun onStartCommand(intent: Intent, flags: Int, startId: Int): Int {
        Toast.makeText(this, "service starting", Toast.LENGTH_SHORT).show()

        // For each start request, send a message to start a job and deliver the
        // start ID so we know which request we're stopping when we finish the job
        serviceHandler?.obtainMessage()?.also { msg ->
            msg.arg1 = startId
            serviceHandler?.sendMessage(msg)
        }

        // If we get killed, after returning from here, restart
        return START_STICKY
    }

    override fun onBind(intent: Intent): IBinder? {
        // We don't provide binding, so return null
        return null
    }

    override fun onDestroy() {
        Toast.makeText(this, "service done", Toast.LENGTH_SHORT).show()
    }
}
```
Beberapa fungsi memang perlu untuk di-override seperti di contoh class diatas

2. Kita deklarasi service di `AndroidManifest.xml`
```xml
<manifest ... >
  ...
  <application ... >
      <service android:name=".HelloService" />
      ...
  </application>
</manifest>
```

3. Lalu kita panggil dengan menggunakan `Intent`
```kotlin
Intent(this, HelloService::class.java).also { intent ->
    startService(intent)
}
```

## Sumber
- https://developer.android.com/guide/components/services