# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we don’t use Mutex<> instead?
- `RwLock<>` digunakan dalam tutorial ini karena mampu memberikan fleksibilitas yang memungkinkan banyak *thread* untuk membaca data secara bersamaan, sementara akses write dibatasi hanya ke satu *thread* pada waktu yang sama. Hal ini cocok digunakan untuk skenario di mana operasi read lebih dominan daripada operasi write. Berbeda dengan `Mutex<>` yang hanya memungkinkan satu *thread* mengakses suatu data dalam suatu waktu. Penggunaan `RwLock<>` jelas lebih efisien di situasi di mana `Vec<Notification>` akan sering dibaca oleh berbagai *thread* tanpa perubahan data yang sering terjadi. Jika menggunakan `Mutex<>`, paralelisme read seperti ini tidak mungkin dilakukan.

In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why didn't Rust allow us to do so?
- Dalam Rust, variabel static memiliki keterbatasan karena harus memiliki lifetime `'static` dan juga harus bertipe `Sync`, artinya variabel tersebut harus *thread-safe* atau tidak dapat diakses oleh banyak *thread* secara bersamaan dan secara default memiliki sifat *immutable* untuk menghindari *race condition*. Untuk menangani pembatasan ini, Rust menyediakan `lazy_static` yang memungkinkan pembuatan variabel singleton, yang artinya variabel hanya akan diinisialisasi pada saat pertama kali diakses dan hanya akan ada satu *instance* dalam program. Hal ini berbeda dengan Java, di mana variabel static dapat diubah dan mungkin memerlukan penanganan khusus untuk keamanan *thread* dalam konteks *multithreading*.

#### Reflection Subscriber-2

Have you explored things outside of the steps in the tutorial, for example: **src/lib.rs**? If not, explain why you didn’t do so. If yes, explain things that you’ve learned from those other parts of code.
- Dalam Rust, `src/lib.rs` biasanya sering digunakan sebagai pusat di mana elemen-elemen penting yang dibutuhkan di seluruh aplikasi didefinisikan. Sebagai contoh, pada file `lib.rs` terdapat `pub type Result<T, E=Error> = std::result::Result<T, E>;` yang berfungsi untuk mempermudah pengelolaan error dengan menetapkan `Error` sebagai tipe error default untuk tipe Result, yang merupakan pendekatan standar untuk menyederhanakan *error handling*. Selain itu, `lib.rs` mungkin juga mendefinisikan struktur seperti `ErrorResponse`, yang mewakili response kesalahan dengan status code serta pesannya. Ini mencerminkan struktur yang terorganisir dan terpusat, memfasilitasi akses ke *App Configuration*, *root url*, dan pengaturan singleton sehingga memudahkan bagian lain dari aplikasi untuk mengakses dan menggunakan informasi yang umum.

Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than 1 instance of Main app, will it still be easy enough to add to the system?
- **Observer Pattern** memudahkan saya dalam penambahan **Subscriber** baru hanya dengan menambahkannya ke dalam daftar **Observer** yang sudah ada. Ini berarti setiap kali ada **Subscriber** baru yang ingin berlangganan ke jenis produk tertentu, kita hanya perlu memasukkannya ke dalam daftar ini.  Desain penulisan kode pada pattern ini dirancang dengan *Open-Closed Principle* yang memungkinkan penambahan jenis **Subscriber** baru dengan mudah tanpa memerlukan perubahan besar pada kode yang ada. Ketika ada lebih dari satu *instance* dari *Main App*, setiap *instance* memiliki sistem pengamatan yang terpisah sehingga memungkinkan untuk beroperasi secara independen tanpa berbagi notifikasi. Namun, jika ada kebutuhan untuk berbagi notifikasi di antara berbagai *instance*, diperlukan suatu mekanisme penyimpanan bersama yang dapat diakses oleh semua *instance* tersebut. Untuk kasus instansiasi lebih dari 1 *Main App*, cukup dengan mendaftarkan **Subscriber** tersebut ke API yang relevan untuk aplikasi yang berbeda.

Have you tried to make your own Tests, or enhance documentation on your **Postman collection**? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).
- Saya belum mencoba untuk membuat test atau dokumentasi untuk **Postman collection** saya.