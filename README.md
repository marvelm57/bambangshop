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
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
> Dalam _Observer design patterns_, **Subscriber**(atau Observer) biasanya digunakan untuk mewakili objek yang ingin **_observe_/_track_** perubahan yang terjadi pada **Publisher**(atau Subject). Pada kasus BambangShop, menggunakan _interface_ **Subscriber** dapat memberikan **fleksibilitas** karena memungkinkan untuk memiliki berbagai jenis **_observer_** yang mungkin memiliki **perilaku yang berbeda** saat menerima **pembaruan dari _subject_**. Oleh karena itu, menggunakan _interface_(atau **trait**) **Subscriber** dapat bermanfaat dalam mengimplementasikan _Observer design pattern_ dalam BambangShop.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
> Jika kita memerlukan **akses yang cepat** ke URL berdasarkan ID yang unik, maka menggunakan **DashMap** **lebih efisien** dan menguntungkan untuk _dataset_ yang besar. Hal ini dikarenakan **DashMap** memiliki **kompleksitas waktu yang lebih baik**, yaitu `O(1)` atau konstan. Sedangkan, penggunaan **Vec** memiliki kompleksitas waktu `O(N)` atau _linear_ sehingga **tidak menguntungkan** untuk **dataset yang besar**, meskipun mungkin sudah cukup jika kita hanya perlu menyimpan daftar _observer_ atau URL yang unik.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
> Dalam kasus static variabel SUBSCRIBERS, menggunakan **DashMap** untuk **_thread-safe_** merupakan pendekatan yang **lebih baik** dibandingkan mengimplementasi _Singleton pattern_, terutama jika beberapa _thread_ dapat **mengakses** atau **memodifikasi _subscriber_** secara **konkuren**. Hal ini dikarenakan penerapan **_Singleton pattern_** hanya memastikan **satu _instance_** dari **SUBSCRIBERS** sehingga tidak dapat memastikan **akses _subscriber_** secara **konkuren**. Oleh karena itu, penggunaan **DashMap** diperlukan karena terjamin dapat meng-_handle_ **concurrency** dibandingkan _Singleton pattern_.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
> Dengan memisahkan **Service** dan **Repository** dari **Model**, kita telah mengimplementasikan **SRP** di mana masing-masing **modul** tersebut hanya akan **memiliki** **satu peran**. Misalnya, **Model** akan berfokus pada **representasi data**, **Service** akan memuat semua **_business logic_**, dan **Repository** akan bertanggung jawab pada **penyimpanan** dan **pengambilan data**. Dengan melakukan pemisahan kepentingan tersebut, tentu akan membuat kode menjadi **lebih terstrukur**, **_maintainable_** dan **_testable_**.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
> Jika hanya menggunakan **Model**, maka setiap model tersebut tidak hanya **mengcover _business logic_**, tetapi juga _logic_ untuk **_data storage_**, **_services_** lainnya, dan interaksi dengan model lainnya. Hal ini akan **meningkatkan kompleksitas kode** dan membuat **kode sulit dipahami** dan **di-_maintain_**. **Kode** menjadi **lambat** dan **tidak efisien**, serta akan sulit untuk dilakukan _testing_.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
> **Postman** memungkinkan kita untuk **mengirim _request_ ke API** dan **mendapat _response_** sehingga **memudahkan** kita **untuk menguji API _endpoint_** dan memastikan bahwa API tersebut berfungsi sebagaimana mestinya. Selain itu, **Postman** juga memiliki **fitur** yang memungkinkan untuk **berbagi _collection_** dan **_environment_** sehingga dapat **memfasilitasi** kita untuk **berkolaborasi** dengan orang lain atau **member grup**.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
> Pada BambangShop, kita menggunakan variasi **Push model** yang terlihat pada method `notify()` dalam `src/service/notification.rs`. Pada method tersebut, **Publisher** akan iterasi ke **list subscriber** dan secara aktif **memanggil** method **update**.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
> Jika menggunakan **Pull Method**, **Subcriber** hanya akan _request_ **update** ketika dibutuhkan sehingga **penggunaan resource** dapat **lebih efisien**. Namun, kekurangannya adalah **Subscriber** mungkin **tidak** akan **mendapat update secara langsung**.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
> Jika tidak menggunakan **multi-threading**, maka dapat menyebabkan **menurunnya _performance_** karena terjadi **delay** di mana pengiriman notifikasi dilakukan secara berurutan, satu per satu.
