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

“(kaindraa removed the Postman Collection link to comply with GitHub push protection policies)”


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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Tidak, kita tidak perlu `interface` untuk kasus ini dan single Model `struct` saja sudah cukup. Hal ini karena, setiap subscriber memiliki perilaku yang sama yakni menerima notifikasi melalui HTTP dari product_type yang di-subscribe. 

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Lebih baik menggunakan `DashMap` untuk kasus ini karena `Dashmap` memungkinkan kita menyimpan data dengan key yang terjaga keunikannya, seperti `id` untuk `Program` dan `url` untuk `Subscriber`.

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Ya, kita tetap memerlukan `DashMap`, meskipun sudah menggunakan design singleton. Singleton hanya memastikan datanya tunggal secara global, tetapi tidak membuatnya aman diakses dari banyak thread secara bersamaan (thread safe). `DashMap` dibutuhkan untuk menjamin concurrent access ke data `SUBSCRIBERS`.

#### Reflection Publisher-2

> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Kita perlu memisahkan Service dan Repository dari Model agar untuk menerapkan Single Responsibility Principle (SRP). Dengan pemisahan ini, setiap komponen hanya menangani satu tanggung jawab: Model untuk data, Repository untuk akses data, dan Service untuk logika bisnis. Ini membuat sistem lebih terstruktur, mudah diuji, dan dirawat.

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika hanya menggunakan `Model` tanpa memisahkan `Service` dan `Repository`, maka setiap model seperti `Program`, `Subscriber`, dan `Notification` harus menangani terlalu banyak tanggung jawab sekaligus, mulai dari menyimpan data, menjalankan logika bisnis, hingga mengatur interaksi antar model. Hal ini melanggar prinsip Single Responsibility dan akan meningkatkan kompleksitas kode karena logika seperti mengirim notifikasi saat Program dibuat atau dihapus akan bercampur langsung di dalam Model. Akibatnya, model menjadi sulit dipahami, sulit di-test secara terpisah.

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Ya, saya sudah mengeksplorasi Postman dan menurut saya alat ini sangat membantu karena saya bisa menjalankan HTTP request seperti GET dan POST tanpa perlu membuat antarmuka pengguna (UI) terlebih dahulu. Dengan Postman, saya bisa langsung berinteraksi dengan API yang saya buat, menguji respons, dan memastikan endpoint berjalan dengan benar. Fitur yang menurut saya paling menarik adalah kemampuannya untuk melihat Body, Cookies, dan Header secara langsung, sehingga saya bisa memahami dengan jelas bagaimana data berpindah antara client dan server. Hal ini sangat relevan dan akan sangat membantu untuk proyek-proyek saya di masa depan, terutama jika saya mengembangkan sistem berbasis REST API atau layanan yang melibatkan komunikasi antar server.

#### Reflection Publisher-3
