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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam pola Observer, secara tradisional Subscriber didefinisikan sebagai sebuah interface. Dalam kasus BambangShop, kita bisa menggunakan sebuah trait untuk mendefinisikan perilaku dari Subscriber, namun sebuah struct Model sudah cukup karena:
   - Kita memiliki implementasi Subscriber yang konsisten dan tunggal
   - Semua subscriber berinteraksi dengan sistem dengan cara yang sama (menerima notifikasi HTTP)
   - Perilakunya cukup sederhana sehingga kita tidak memerlukan beberapa implementasi dari konsep Subscriber
   - Menggunakan struct secara langsung menyederhanakan kode dan sejalan dengan preferensi Rust untuk tipe konkret ketika abstraksi tidak dibutuhkan

2. Menggunakan DashMap (map/dictionary) alih-alih Vec (list) diperlukan dalam kasus ini karena:
   - Kita perlu memastikan keunikan ID dan URL dengan efisien
   - Mencari subscriber berdasarkan ID adalah operasi umum yang harus cepat
   - DashMap menyediakan waktu pencarian O(1) dibandingkan dengan O(n) pada Vec ketika mencari item
   - Kita perlu melakukan penambahan dan penghapusan yang sering, yang lebih efisien dengan DashMap
   - DashMap memberikan jaminan thread-safety yang penting dalam konteks server web

3. DashMap lebih disukai dibandingkan dengan menerapkan pola Singleton karena:
   - Sistem kepemilikan Rust membuat implementasi Singleton tradisional menjadi kompleks
   - DashMap sudah menyediakan jaminan thread-safety yang perlu kita implementasikan secara manual
   - DashMap menangani akses bersamaan dengan efisien, yang krusial untuk aplikasi web
   - Menggunakan pustaka yang sudah teruji mengurangi kemungkinan bug konkuren yang halus
   - Rust mendorong penggunaan sistem tipe dan model kepemilikannya daripada pola OOP tradisional jika memungkinkan

#### Reflection Publisher-2

#### Reflection Publisher-3
