# multi-container-app
A URL-shortening and link management application that takes a big URL and converts it into a new short URL that’s easy to share. 

![image](https://github.com/pritamnikam/multi-container-app/blob/main/multi-container%20application.png)

Let’s refer to the services as backend and frontend services.
We use an existing codebase to create a multi-container application for this type of application.

There are two directories named backend and frontend. The backend directory contains the back-end code, which exposes the API for generating shortened URLs. The front-end code—which is responsible for the user interface—is located in the frontend directory. There are a few prerequisites that must be met before we can run the applications with docker-compose.

- Both the backend and frontend need to be containerized.
- The frontend service should be able to access the APIs of the backend service. This means that the frontend service must be able to communicate with the backend service.
- Since containers are transient, we need persistent storage—a database—to keep URLs.
- The backend service should be able to connect to the database.
