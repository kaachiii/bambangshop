# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   ✅ Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   ✅ Commit: `Create Subscriber model struct.`
    -   ✅ Commit: `Create Notification model struct.`
    -   ✅ Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   ✅ Commit: `Implement add function in Subscriber repository.`
    -   ✅ Commit: `Implement list_all function in Subscriber repository.`
    -   ✅ Commit: `Implement delete function in Subscriber repository.`
    -   ✅ Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   ✅ Commit: `Create Notification service struct skeleton.`
    -   ✅ Commit: `Implement subscribe function in Notification service.`
    -   ✅ Commit: `Implement subscribe function in Notification controller.`
    -   ✅ Commit: `Implement unsubscribe function in Notification service.`
    -   ✅ Commit: `Implement unsubscribe function in Notification controller.`
    -   ✅ Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   ✅ Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   ✅ Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   ✅ Commit: `Implement publish function in Program service and Program controller.`
    -   ✅ Commit: `Edit Product service methods to call notify after create/delete.`
    -   ✅ Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam kasus ini, jika kita hanya memiliki satu jenis observer, maka kita dapat menggunakan satu Model struct tanpa perlu interface/trait. Namun, menggunakan trait tetap direkomendasikan dalam pola Observer untuk memastikan fleksibilitas di masa depan, terutama jika kita ingin menambahkan lebih banyak observer dengan perilaku berbeda.

2. Menggunakan DashMap lebih efektif daripada Vec dalam kasus ini karena memungkinkan pencarian cepat (O(1)) dibandingkan dengan pencarian linier di Vec (O(n)). Selain itu, DashMap mendukung akses paralel yang aman, yang berguna jika ada banyak operasi pembacaan atau penulisan data secara bersamaan. 

3. DashMap diperlukan karena mendukung akses multithreading tanpa perlu manual mengelola locks. Singleton hanya memastikan ada satu instance dari daftar subscriber, tetapi tidak otomatis membuatnya thread-safe. Jika hanya menggunakan Singleton, tetap diperlukan Mutex atau RwLock.

#### Reflection Publisher-2
1. Kita perlu memisahkan Service dan Repository agar Repositori dan Service fokus pada fungsinya masing-masing. Repository menangani akses ke database, sedangkan Service mengelola logika bisnis. Hal ini sejalan dengan salah satu prinsip SOLID, yaitu Single Responsibility Principle (SRP) sehingga kode lebih terstruktur dan mudah dipelihara tanpa mengganggu fungsi lainnya.

2. Jika hanya menggunakan Model tanpa layer lain, maka akan terjadi coupling tinggi, di mana logika bisnis, akses database, dan manipulasi data bercampur dalam satu tempat. Ketika Program, Subscriber, dan Notification saling berinteraksi, perubahan pada satu model dapat berdampak luas ke model lain sehingga meningkatkan kompleksitas dan sulitnya pemeliharaan kode. Dengan memisahkan layer seperti Service dan Repository membantu kita mengurangi ketergantungan antar-model dan membuat kode lebih fleksibel.

3. Dengan Postman, kita dapat mengirim berbagai HTTP method, menambahkan credential, form data, dan melihat response secara real-time. Fitur seperti automated testing, environment variables, dan collection runner juga sangat berguna dalam proyek team untuk memastikan API tetap berjalan dengan baik selama pengembangan.

#### Reflection Publisher-3
1. Kita menggunakan Push Model, di mana publisher langsung mengirim data ke subscriber saat terjadi perubahan (create, delete, update). NotificationService mengiterasi semua subscriber dan memberikan update otomatis tanpa subscriber harus meminta data.

2. Jika kita menggunakan Pull Model keuntungannya adalah lebih efisien karena subscriber hanya mengambil data saat dibutuhkan. Selain itu, model ini juga lebih scalable karena setiap subscriber dapat menarik data secara independen sehingga memberikan fleksibilitas dalam pengambilan data. Namun, kekurangan dari model ini adalah dapat menyebabkan latensi karena subscriber harus secara aktif meminta data, serta meningkatkan kompleksitas karena subscriber harus mengetahui struktur data publisher.

3. Jika tanpa multithreading, notifikasi akan dikirim secara sequential (satu per satu), menyebabkan antrean panjang dan bottleneck. Hal ini menurunkan efisiensi, terutama jika jumlah subscriber besar atau ada keterlambatan dalam pengiriman. Program juga dapat menjadi tidak responsif jika proses notifikasi memakan waktu lama.
