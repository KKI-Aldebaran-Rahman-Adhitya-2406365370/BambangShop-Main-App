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
1. A single Model struct such as Subscriber is enough for this system because the subscribers are external web applications communicating through HTTP REST APIs. Since the network abstracts away what the external app is, the Main App only needs to know the url and the name of the external app. This is enough for a struct to implement. Therefore, the complexity of an interface or a trait is not necessary in this situation.
2. For this case, I think a DashMap is necessary to implement the intended goal. Using a Vec would require a function to manually loop through the vector just to find something that is unique. A DashMap is more efficient here because it has a key-value pair mechanism where a unique key is mapped to a value. This runs in O(1) time (faster than a Vec which runs in O(N) time).
3. Both the Singleton pattern and DashMap are required for this case. The Singleton pattern guarantees that there is exactly one global SUBSCRIBERS database in the entire application's memory. However, race conditions may happen when two or more users try to subscribe to the same product type at the exact second or close enough by a few milliseconds. DashMap fixes this problem because it is a concurrent and specialised hashmap. By using DashMap inside the Singleton pattern, thread-safety is achieved which allows multiple threads to add or delete subscribers at the same time without needing to lock the entire system.

#### Reflection Publisher-2
1. Based on what I understand about design principles, separating "Service" and "Repository" is necessary because it implements the single responsibility principle. Model only handles the structure of the data, repository only handles the access and storage of data, and service only handles the logic of the application. This also implements separation of concerns because, for example, editing a repository won't affect a model. This separation allows for modular code that is readable and maintainable.
2. If the application were to only use models, the code in each model would become highly dependant and difficult to maintain. For example, the product model would have to save its own database, search through the subscriber model's database, and find which users to notify in the notification model. The impact of this is that the models would have to know how each other model works. Using services eliminates this complexity by putting logic in their respective places such as models, repositories, and services.
3. Postman helps software development by acting as a simulated front-end. This simulation allows me to test backend API requests and responses without having to build a user interface. In future group projects, Postman can be useful by allowing me and my team members to save our API requests into a folder so that testing is done uniformly throughout the group. Another use case of Postman is the ability to change environment variables such as setting a base url to a localhost:8000 and changing it to another URL when needed without having to rewrite the tests.

#### Reflection Publisher-3
