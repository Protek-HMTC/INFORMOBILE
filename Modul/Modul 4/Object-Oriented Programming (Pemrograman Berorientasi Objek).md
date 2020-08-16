# Object-Oriented Programming (Pemrograman Berorientasi Objek)

![oop](img/oop.jpg)

Object-Oriented Programming atau yang biasa disingkat OOP seperti namanya merujuk kepada bahasa yang menggunakan objek dalam programming. Tujuan utama dari OOP adalah menyatukan data dan fungsi yang mengoperasikan dalam satu tempat.

Konsep OOP sendiri sangat banyak digunakan di berbagai bahasa, bahkan bahasa yang digunakan untuk membangun aplikasi Android seperti Java dan Kotlin. Jadi mempelajari OOP sangat penting untuk pemrogram Android. Sekarang mari kita pelajari konsep-konsep dari OOP.

![class and object](img/oop2.jpg)

## Class dan Object
Class dan Object merupakan konsep dasar dari OOP yang berkutat dengan entitas di dunia nyata. 

### Class
Kelas secara singkat merupakan blueprint atau prototype untuk pembuatan object. Di dalam kelas, Anda harus mendeklarasikan atribut dan fungsi apa saja yang dibutuhkan oleh objek yang akan dibuat dari kelas itu nanti. Contoh pembuatan class :

```
// Contoh dalam Java
public class Student {

    // Atribut
    String name;
    int rollNo;

    // Fungsi
    public void setName(String name) {
        this.name = name;
    }

    public void setRollNo(int rollNo) {
        this.rollNo = rollNo;
    }
}
```

```
// Contoh dalam Kotlin
class Student {

    // Atribut
    lateinit var name: String
    var rollNo: Int = 0

    // Fungsi
    fun setValName(name: String) {
        this.name = name
    }

    fun setValRollNo(rollNo: Int) {
        this.rollNo = rollNo
    }
}
```

### Object
Object merupakan unit dasar dalam OOP yang merepresentasikan entitas di dunia nyata. Sebuah object terdiri dari :
- State : Direpresentasikan oleh atribut dalam object
- Behavior : Direpresentasikan oleh fungsi dalam object
- Identity : Memberi nama unik pada suatu object dan memungkinkan satu object untuk berinteraksi dengan object lain

![contoh object](img/oop3.png)

Contoh dari penggunaan object :
```
// Contoh dalam bahasa Java
public class Main {
    public static void main(String[] args) {
        // Pembuatan object dari class Student
        Student student = new Student();

        // Penggunaan fungsi di object student
        student.setName("Jack");
        student.setRollNo(10);

        // Penggunaan atribut di object student
        System.out.println(student.name + " has rollNo " + student.rollNo);
    }
}
```

```
// Contoh dalam bahasa Kotlin
fun main() {
    // Pembuatan object student dari class Student
    var student = Student()

    // Penggunaan fungsi object student
    student.setValName("King")
    student.setValRollNo(20)

    // Penggunaan atribut object student
    println(student.name + " has roll no " + student.rollNo)
}
```

## Encapsulation
Encapsulation adalah mekanisme dalam membungkus data (atribut) dan kode yang menggunakan data (fungsi) bersama dalam satu unit. Di dalam encapsulation, atribut dalam suatu class anak disembunyikan dari class lain dan hanya bisa diakses oleh fungsi dalam class tersebut.

Agar dapat menerapkan encapsulation, maka perlu mengerti apa itu access modifier. Access modifier sendiri membantu dalam pembatasan aksesbilitas dari suatu data. Di bawah ini ada beberapa macam access modifier dan aksesbilitas dari masing-masing access modifier:

![macam-macam access modifier](img/oop4.png)

Berdasarkan tabel tersebut, agar bisa menerapkan encapsulation maka diperlukan access modifier private. Lalu terdapat konsep fungsi getter dan setter. Getter sendiri merupakan fungsi untuk mendapat nilai dari data yang diinginkan. Setter adalah fungsi yang digunakan untuk mengubah nilai suatu data. Dalam kotlin, getter dan setter sudah otomatis dibuat oleh sistem.

Contoh penerapan encapsulation :

```
// Contoh dalam Java
public class Student2 {
    private int rollNo;

    public void setRollNo(int rollNo) {
        this.rollNo = rollNo;
    }

    public int getRollNo() {
        return this.rollNo;
    }
}
```

```
// Contoh dalam Kotlin
class Student {
    var rollNo: Int = 0
        private set

    fun setValRollNo(rollNo: Int) {
        this.rollNo = rollNo
    }
}
```

Kalian bisa mencoba di fungsi main untuk mendapat langsung data. Untuk bahasa Java kalian pasti akan mendapat error jika tidak melewati getter. Untuk Kotlin karena getter memanglah public maka bisa menggunakan cara seperti biasa. Untuk mengubah nilai suatu data maka perlu melewati setter dan tidak bisa langsung mengubah nilai di kedua bahasa tersebut.



## Sumber :
- https://www.updateilmu.com/wp-content/uploads/2015/01/oop.jpg
- https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/
- https://www.geeksforgeeks.org/classes-objects-java/
- https://www.tutorialspoint.com/java/java_encapsulation.htm
- https://www.geeksforgeeks.org/access-modifiers-java/
- https://kotlinlang.org/docs/tutorials/kotlin-for-py/inheritance.html