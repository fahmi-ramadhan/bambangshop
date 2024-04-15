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
    | variable | type | description |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)

- ✅ Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
- **STAGE 1: Implement models and repositories**
  - ✅ Commit: `Create Subscriber model struct.`
  - ✅ Commit: `Create Notification model struct.`
  - ✅ Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - ✅ Commit: `Implement add function in Subscriber repository.`
  - ✅ Commit: `Implement list_all function in Subscriber repository.`
  - ✅ Commit: `Implement delete function in Subscriber repository.`
  - ✅ Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - ✅ Commit: `Create Notification service struct skeleton.`
  - ✅ Commit: `Implement subscribe function in Notification service.`
  - ✅ Commit: `Implement subscribe function in Notification controller.`
  - ✅ Commit: `Implement unsubscribe function in Notification service.`
  - ✅ Commit: `Implement unsubscribe function in Notification controller.`
  - ✅ Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - ✅ Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - ✅ Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - ✅ Commit: `Implement publish function in Program service and Program controller.`
  - ✅ Commit: `Edit Product service methods to call notify after create/delete.`
  - ✅ Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Dalam kasus BambangShop ini, _interface_ (atau _trait_ di Rust) sebenarnya tidak terlalu diperlukan. Meskipun pada _Observer pattern_, `Subscriber` biasanya berupa _interface_, di sini `Subscriber` _struct_ sudah cukup karena fungsinya hanya menampung data URL dan nama subscriber. _Interface_ pada _Observer pattern_ lebih berguna ketika terdapat banyak jenis objek yang ingin menjadi _observer_ dengan fungsionalitas yang berbeda-beda.

2. Dalam kasus BambangShop ini, penggunaan `DashMap` lebih sesuai dibandingkan dengan `Vec` karena `DashMap` memungkinkan akses berdasarkan _key_ (`id` untuk `Product` dan `url` untuk `Subscriber`) yang unik sehingga operasi penambahan, pencarian, dan penghapusan menjadi lebih efisien. Sementara itu, jika menggunakan `Vec`, operasi tersebut akan memerlukan iterasi melalui seluruh _list_.

3. Dalam konteks pemrograman dengan Rust, penggunaan `DashMap` dan _Singleton pattern_ bukanlah hal yang saling eksklusif. `DashMap` digunakan untuk menyediakan akses konkuren yang aman ke map yang penting dalam _multithreading_. Sementara itu, _Singleton pattern_ digunakan untuk memastikan bahwa hanya ada satu _instance_ dari suatuu objek dalam aplikasi. Dalam kasus `SUBSCRIBERS` pada proyek ini, kita sebenarnya sudah menerapkan _Singleton pattern_ dengan menggunakan `lazy_static!` untuk membuat _instance_ `DashMap` yang berisfat global dan statis. Jadi, kita memerlukan keduanya: `DashMap` untuk akses _thread-safe_ dan _Singleton pattern_ untuk memastikan hanya ada satu `DashMap` `SUBSCRIBERS` dalam aplikasi.

#### Reflection Publisher-2

1. Pemisahan "Service" dan "Repository adalah penerapan dari Single Responsibility Principle (SRP) dalam SOLID, yang menyatakan bahwa setiap modul atau kelas harus memiliki satu fungsionalitas khusus. Dengan memisahkan logika bisnis ke dalam "Service" dan akses data ke dalam "Repository", struktur kode kita menjadi lebih rapi dan mudah dihapami, serta memudahkan pengujian dan juga pemeliharaan.

2. Jika kita hanya menggunakan Model tanpa memisahkan logika bisnis dan akses data ke dalam "Service" dan "Repository", kompleksitas kode akan meningkat secara signifikan. Setiap Model akan bertanggung jawab tidak hanya untuk merepresentasikan data, tetapi juga untuk logika bisnis dan operasi data. Ini akan membuat Model menjadi terlalu besar dan sulit untuk dipahami dan dikelola. Selain itu, perubahan kecil pada satu Model dapat berdampak pada Model lainnya karena ketergantungan yang tinggi antara mereka. Misalnya, perubahan pada `Product` dapat memengaruhi `Subscriber` dan `Notification`.

3. Dalam tutorial ini, Postman membantu saya untuk menguji _endpoint-endpoint_ yang telah dibuat pada aplikasi ini. Saya dapat dengan mudah mengirim HTTP _request_ dengan menyesuaikan HTTP _method_-nya dan melihat _response_ yang diterima. Menurut saya, fitur untuk Postman untuk membuat koleksi _request_ yang terorganisir, menyimpan variabel, dan berbagi koleksi dengan rekan tim akan memudahkan kolaborasi dalam proyek kelompok saya.

#### Reflection Publisher-3

1. Dalam tutorial ini, kita menggunakan variasi _Push model_ dari _Observer pattern_. Dalam model ini, _Publisher_ mem-_push_ data ke _Subscriber_ setiap kali ada perubahan. Pada kasus ini, ketika ada perubahan pada `Product` (misalnya produk baru ditambahkan), `ProductService` dalam `src/service/product.rs` akan memberi tahu semua `Subscriber` tentang perubahan terseut melalui `NotificationService` dalam `src/service/notification.rs`. Jadi, bukan _Subscriber_ yang meminta data dari _Publisher_, tetapi _Publisher_ yang aktif mem-_push_ data ke _Subscriber_.

2. Jika menggunakan variasi _Pull model_ dari _Observer pattern_, keuntungannya ada pada efisiensi karena _Subscriber_ hanya akan meminta data dari _Publisher_ ketika mereka membutuhkannya, yang berarti tidak ada data yang dikirimkan yang mungkin tidak digunakan. Hal ini juga memberikan lebih banyak kontrol kepada _Subscriber_ atas kapan dan bagaimana mereka menerima data. Sementara itu, kerugiannya ada pada kompleksitas dan _overhead_ komunikasi yang lebih tinggi karena setiap _Subscriber_ harus secara aktif memeriksa perubahan dengan cukup sering.

3. Jika kita memutuskan untuk tidak menggunakan _multi-threading_ dalam proses notifikasi, maka setiap notifikasi akan diproses secar berurutan. Atinya, ketika sebuah produk baru ditambahkan dan sistem perlu mengirim notifikasi ke semua _Subscriber_, sistem akan mengirim notifikasi satu per satu dan hal ini berpotensi membuat sistem menjadi lambat, terutama jika jumlah _Subscriber_ ada banyak sekali.
