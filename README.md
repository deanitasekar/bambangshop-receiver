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
-   **STAGE 3: Implement services and controllers**
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

1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

    Dalam tutorial kali ini, RwLock<> digunakan untuk sinkronisasi penggunaan Vec<Notification> karena memiliki karakteristik unik yang sesuai dengan kebutuhan akses data. Berbeda dengan Mutex<> yang hanya mengizinkan satu thread untuk mengakses data pada satu waktu, RwLock<> memungkinkan banyak reader dan satu writer pada saat yang bersamaan. Dengan demikian, RwLock<> menjadi pilihan yang lebih efisien dan tepat untuk skenario yang didominasi oleh operasi membaca data.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

    Dalam Rust, mutasi langsung pada variabel statis tidak diperbolehkan karena pendekatannya yang ketat terhadap keamanan dan concurrency. Berbeda dengan Java yang memperbolehkan perubahan isi variabel statis melalui fungsi statis, Rust mengutamakan pencegahan potensi race condition dan data race yang dapat terjadi saat multiple thread berinteraksi dengan data yang sama. Untuk mengatasi keterbatasan ini, Rust menawarkan beberapa solusi seperti lazy_static yang memungkinkan inisialisasi dan mutasi variabel statis melalui mekanisme locking seperti RwLock dan Mutex. Pendekatan ini memastikan bahwa perubahan data pada variabel statis dilakukan dengan aman sehingga meminimalisir risiko konflik yang tidak terduga dalam multi-threading.

#### Reflection Subscriber-2

1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

    Ya, saya sudah mengeksplorasi file-file lain yang tidak dibahas secara lebih lanjut pada tutorial kali ini. Salah satu file yang saya ekplorasi adalah file `lib.rs`, yang berfungsi untuk mendefinisikan modul-modul yang akan digunakan dalam project, termasuk penggunaan library eksternal seperti lazy_static untuk membuat variabel statis (Vec dan DashMap), dan mengimpor berbagai modul dari framework Rocket seperti `rocket::http::Status` untuk menampilkan HTTP response. Selain itu, file ini berperan penting dalam mengatur konfigurasi utama aplikasi, termasuk pembuatan variabel global, struktur dasar konfigurasi aplikasi, dan metode penanganan error. Selain itu, saya melakukan eksplorasi pada file `Cargo.toml` dan `Rocket.toml`. `Cargo.toml` merupakan file konfigurasi utama untuk mengelola dependensi, melakukan konfigurasi proyek, dan mendefinisikan metadata proyek, sedangkan `Rocket.toml` merupakan file konfigurasi khusus untuk proyek yang menggunakan Rocket framework.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

    Setelah menyelesaikan tutorial ini, terlihat jelas bahwa Observer pattern memberikan fleksibilitas signifikan dalam menambahkan Subscriber melalui prinsip Separation of Concern. Pola ini memungkinkan penambahan Subscriber tanpa mempengaruhi struktur Observer yang sudah ada sehingga sistem tetap maintainable. Ketika beberapa instance main app di-spawn, setiap instance masih dapat berperan sebagai Publisher meskipun kompleksitas sistem akan meningkat seiring bertambahnya Subscriber. Meskipun demikian, Observer pattern memfasilitasi message passing yang independen sehingga memudahkan pengembangan dan perluasan sistem notifikasi tanpa menimbulkan ripple effect yang signifikan pada komponen sistem yang sudah ada.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

    Sejauh ini, saya belum membuat test atau meningkatkan dokumentasi pada Postman collection yang saya miliki. Namun, setelah mengeksplorasi fitur Postman, saya menyadari potensi besarnya dalam mendukung proses pengembangan aplikasi. Fitur testing dan dokumentasi API pada Postman sangat membantu untuk melakukan automated testing dan memastikan bahwa aplikasi berjalan sesuai. Kemampuan Postman untuk mengirimkan request dengan berbagai parameter serta memverifikasi reponse dari server, fitur-fitur ini dapat menjadi alat yang sangat berguna untuk menjamin kualitas dan fungsionalitas. Meskipun saya belum secara langsung mengimplementasikannya, potensi manfaat dari fitur-fitur tersebut dapat digunakan untuk Tugas Kelompok.