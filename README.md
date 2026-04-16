# Step-by-step Guide to deploy a Multi-Container Application with Docker Compose and Data Persistence
Most modern applications are often composed of web servers and databases that must work together seamlessly. Managing these services manually can quickly become complex, time-consuming and prone to human errors. Docker Compose simplifies this process by allowing developers to define, configure, and orchestrate multi-container applications using a single YAML file. 

This comprehensive step-by-step guide walks you through the process of deploying a Multi-Container Application with Docker Compose and Data Persistence. In this lab,  we will explore how to use Docker Compose to deploy a simple multi-container setup involving a web application and a PostgreSQL database. We will also introduce Data Persistence using Docker volumes, to ensure that our database retains its data even when containers are stopped or removed. The aim of this project is to learn : 
- How to use Docker Compose to define and run multiple related containers.
- How to link a web application container to a database container.
- How to use Docker Volumes to ensure data persists even if containers are removed.
- How to manage application dependencies using requirements.txt.
- Understand the basics of a simple database (PostgreSQL).

## Prerequisites
- In order to proceed, we need to make sure beforehand to : 
- Have Docker installed. 
- Understand how to Dockerize a single application.
- Have a Basic understanding of databases.

## Step-by-Step Instructions : 
### Step 1: Create Your Multi-Container Application Files
In this lab, we'll create a simple Flask application that connects to a PostgreSQL database. The Flask app will store and retrieve a simple message from the database. We'll need Python files for the app, a requirements.txt, and a new file called docker-compose.yml to define our services. To complete Step 1, follow the instructions below : 
- Create a new folder on your computer named docker-compose-app.
- Inside docker-compose-app, create a subfolder named app.
- Inside the app folder, create a new file named app.py.
- Open app/app.py with a text editor and paste the following content:
<img width="803" height="891" alt="1" src="https://github.com/user-attachments/assets/445e451d-b1cf-43e4-aa7d-fddf3dd7930e" />
<img width="706" height="375" alt="2" src="https://github.com/user-attachments/assets/6733e2cf-792a-4c8a-a00e-ee2099cb310b" />

- In the app folder, create a subfolder named templates.
- Inside the templates folder, create a new file named index.html.
- Open app/templates/index.html with a text editor and paste the following content:
<img width="867" height="887" alt="3" src="https://github.com/user-attachments/assets/daf1e7bf-bbd1-47b6-8e33-fecda44703d7" />
<img width="652" height="647" alt="4" src="https://github.com/user-attachments/assets/c804ab9b-fdc3-4cfc-a770-9a38e65c2753" />

- In the app folder, create a new file named requirements.txt.
- Open app/requirements.txt and paste the following content:
<img width="708" height="93" alt="5" src="https://github.com/user-attachments/assets/ee6ba6cb-14ce-4d34-b055-377baa5fe39e" />


- We now need psycopg2-binary to connect to the PostgreSQL database.
- Save all files. Your docker-compose-app folder structure should now look like this:
<img width="711" height="175" alt="6" src="https://github.com/user-attachments/assets/e833d067-f314-40d7-9d32-396342754f11" />


### Step 2: Create a Dockerfile for the Flask App 
Now that we have configured the Application structure and code, we need to create a Dockerfile to build our Flask application's image, even though Docker Compose will manage it. To complete Step 2, follow the instructions below : 
- Inside the app folder (not docker-compose-app), create a new file named Dockerfile.
- Open app/Dockerfile and paste the following content:
- Save the Dockerfile.
<img width="588" height="345" alt="7" src="https://github.com/user-attachments/assets/0c27c208-614b-4239-be48-bfa7bbdf4b35" />


### Step 3: Create the docker-compose.yml File
The docker-compose.yml file is where you define all the services (containers) that make up your application, how they connect, and how their data is managed. It's written in a YAML format. To complete Step 3, follow the instructions below : 
- Go back to the main docker-compose-app folder (the parent folder).
- Create a new file named docker-compose.yml.
- Open docker-compose.yml with your text editor and paste the following content:
- Save the docker-compose.yml file.
<img width="709" height="845" alt="8" src="https://github.com/user-attachments/assets/fbf82583-47e0-441c-8329-d4cb8068b524" />

- Your docker-compose-app folder structure should now look like this:
<img width="432" height="174" alt="9" src="https://github.com/user-attachments/assets/1d8ff3c1-d586-4ccb-9ce9-5cbb56ca209c" />


### Step 4: Build and Run Your Multi-Container Application
With docker-compose.yml in place, starting your entire application is now a single command. Docker Compose will read the file, build the web service image, and then start both the db and web containers in the correct order. To complete Step 4, follow the instructions below :
- Open your command line or terminal. Navigate to your docker-compose-app folder using the command cd docker-compose-app.
- Run the following command to build the images and start the containers: docker compose up -d --build
<img width="1223" height="726" alt="10" src="https://github.com/user-attachments/assets/230dc930-a9d4-4ec5-8cae-5c159dc88037" />

- Here is a breakdown of the command : 
**docker compose up**: This command tells Docker Compose to start the services defined in docker-compose.yml.
**d**: Detached mode, runs containers in the background.
**-build**: It tells Docker Compose to build (or rebuild) the images if they don't exist or if their source code has changed. If you omit this, it will use existing images.
**Expected output** : You will see output showing Docker Compose pulling the PostgreSQL image, building your Flask app image, and starting both services. This might take a few minutes the first time.

- Verify that both containers are running using the command : docker compose ps. You should see both web and db services listed with a State of running.
<img width="1179" height="108" alt="11" src="https://github.com/user-attachments/assets/34b7184e-8127-47d6-8423-b80a29be245b" />

- If you are deploying your website from an AWS EC2 instance, before trying to access it from the browser, make sure the security groups of your running instance are properly configured to allow inbound traffic. To configure the security group : 
- On your AWS Console, Go to EC2 Dashboard -> Select your workstation (EC2 instance) -> Go to Security -> Select the security group related to it -> Click on Edit Inbound rules -> Add rules : HTTP Traffic on port 80 from anywhere ; TCP Custom Traffic on port 5000 from anywhere
- Then, Click save rules.
<img width="1424" height="608" alt="12" src="https://github.com/user-attachments/assets/05c2ed64-5848-453e-b80f-4dfe00321c17" />


### Step 5: Access Your Application and Test Data Persistence
Your Flask application is now running and connected to its database. You can access it via your browser and test that the messages you add are saved permanently, thanks to the Docker Volume. To complete Step 5, follow the instructions below :
- Open your web browser. In the address bar, type: <http://localhost:5000> . Press Enter. You should see the "Simple Message Board."
<img width="1417" height="576" alt="13" src="https://github.com/user-attachments/assets/12602cb3-755e-40f5-a7bc-0f628b449df6" />

- Add a few messages in the input field and click "Add Message." You should see them appear in the list.
<img width="1407" height="632" alt="14" src="https://github.com/user-attachments/assets/a42d78ed-ae11-4046-a715-c29aa08bba50" />


- Test Data Persistence: Go back to your terminal and stop the application using the command : docker compose down.
This command stops and removes the containers, but crucially, it does not remove the db_data volume by default. Verify containers are gone: docker compose ps.
- Now, restart the application using the command : docker compose up -d
We did not use --build this time, as the images already exist.
<img width="1286" height="196" alt="15" src="https://github.com/user-attachments/assets/f55c13ac-1eae-4650-a6e5-850ec86e615c" />

- Once it's running, go back to your browser and refresh http://localhost:5000.
**Expected output** : You should see all the messages added earlier! This proves that your data was saved in the db_data volume and persisted even after the containers were stopped and restarted.
<img width="1405" height="578" alt="16" src="https://github.com/user-attachments/assets/2659d54e-74cf-4d79-a273-3880895ae0c1" />


### Step 6: Clean Up (Optional but Recommended)
To completely remove everything related to this application, including the persistent data volume, you need a special command. To complete Step 6, follow the instructions below :
- Stop and remove containers, networks, and volumes using the command : docker compose down --volumes
<img width="1201" height="229" alt="17" src="https://github.com/user-attachments/assets/c5f3f636-ae64-4994-87f8-ed065cd59778" />

- Here is a breakdown of the command : 
**-volumes**: This is the key part! It tells Docker Compose to also remove the volumes defined in the docker-compose.yml file (in our case, db_data). This command in real-world scenarios, deletes your data!
- Verify your application is gone using the commands : 
- docker compose ps (should show no services)
- docker volume ls (the my_app_db_volume should no longer be listed)


### Summary
This breakdown provides a step-by-step guide to deploy a multi-container application using Docker Compose. By completing this lab, we had an overview of : 
- How to use Docker Compose to define and run multiple related containers in a single yaml file.
- How to link a web application container to a database container, to deploy a web app and a database with minimal effort while maintaining clear service communication.
- How to use Docker Volumes to ensure data persists even if containers are removed, allowing PostgreSQL database to store and retain data across container restarts.
This project demonstrated how Docker Compose is a powerful tool for managing multi-container applications, forming a critical foundation for building scalable, maintainable, and real-world production-style containerized applications.
