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

## Sumber :
- https://www.updateilmu.com/wp-content/uploads/2015/01/oop.jpg
- https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/
- https://www.geeksforgeeks.org/classes-objects-java/