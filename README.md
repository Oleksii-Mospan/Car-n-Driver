<img src="https://github.com/Oleksii-Mospan/Car-n-Driver/blob/dev/src/main/resources/logo.png?raw=true" alt="Logo of the project" align="right">

# Car'n'Driver service &middot;
[![version](https://img.shields.io/badge/version-1.0.0-yellowgreen.svg)](https://semver.org)

This app is used for manage cars autopark with known drivers.
This is prototype of web application that represents taxi service. It consists of backend and 
frontend parts, and implemented using n-tired architecture and compliance with SOLID. 

#### It supports such functionality as:
- Registration new user as driver
- Login with driver login and password or logout to log in with another driver
- Adding new cars, drivers and manufacturers
- Set driver to needed car
- Open list of all cars with list of drivers
- Open list of cars for current/login driver
- Delete car, driver or manufacturer

__You can try my application here__ [click](https://fast-mountain-11243.herokuapp.com)

## Developing

### Diagram of application

```mermaid
    %%{init: { 'theme': 'forest' } }%%
    flowchart LR
        subgraph Client
            A[WebBrowser]
        end
        Client <--> Filters
    subgraph App
        subgraph Filters
            direction LR
            B(Loggin\nfilter) 
            C(Authentification\nfilter)
        end
        B --> C --> B
        subgraph Servlet\ncontainer
            D[Tomcat]
        end
        Filters --> D
        subgraph Controllers
            F1[Index]
            F2[Drivers]
            F3[Car]
            F4[Manufacturers]
            F5[Login/Logout]
        end
        D --> F1
        D --> F2
        D --> F3
        D --> F4
        D --> F5
        subgraph Services
            G1(Driver)
            G2(Car)
            G3(Manufacturer)
            G4(Authentication)
        end
        G4 --> G1
        F2 --> G1
        F3 --> G1
        F3 --> G2
        F3 --> G3
        F4 --> G3
        F5 --> G4
        subgraph DAO
            DAO1(DriverrDao)
            DAO2(CarDao)
            DAO3(ManufacturerDao)
        end
        end
        G1 <--> DAO1
        G2 <--> DAO2
        G3 <--> DAO3
        DB[(DataBase)]
        DAO1 <--> DB
        DAO2 <--> DB
        DAO3 <--> DB
        
    style App fill:#565,stroke:#333,stroke-width:4px
    style Client stroke:#333,stroke-width:4px
    style Filters fill:#f9f,stroke:#333,stroke-width:4px
    style Servlet\ncontainer fill:#aaf,stroke:#333,stroke-width:4px
    style Controllers fill:#aff,stroke:#333,stroke-width:4px
    style Services fill:#3bf,stroke:#333,stroke-width:4px
    style DAO fill:#3d3,stroke:#333,stroke-width:4px
    style DB stroke:#333,stroke-width:4px
```

### Technologies used:
- JDBC
- MySQL 8
- Servlet
- JSP
- JSTL
- Tomcat 9
- Maven
- Log4j2
- CSS

### Running project on local machine

- Clone this project into your local folder 
- Open the project in an IDE
- Install and configure Local Tomcat Server (set "/" in Deployment tab - car-n-driver:war exploded)
- After run the application you need to create new driver by clicking to Drivers
- Then you can log in by entering login and password
- create a new driver by clicking on the "Registration" button.
#### Setting up local DataBase
- Download and install MySQL, Workbench
- Copy and paste file [init_db.sql](https://github.com/Oleksii-Mospan/Car-n-Driver/blob/main/src/main/resources/init_db.sql) from resources folder to Workbench console and execute code there
- To configure connection of project to database fill free to edit ConnectionUtil.java (src/main/java/taxi/util/ConnectionUtil.java)

```java
private static final String URL = "jdbc:mysql://<URL_TO_DATABASE>/<DATABASE_NAME>?serverTimezone=UTC";
private static final String USERNAME = "<DATABASE_USERNAME>";
private static final String PASSWORD = "<DATABASE_PASSWORD>";
private static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
```
#### where:
<URL_TO_DATABASE> - URL to Database (for example: localhost) <br>
<DATABASE_NAME> - name of database(for example: cars_n_drivers) <br>
<DATABASE_USERNAME> - username to get permission to read and write to database <br>
<DATABASE_PASSWORD - password for \<DATABASE_USERNAME> user <br>

Also, you need to configure local server. I'd use Tomcat 9.0.50 server.
If you use IntelliJ IDEA here is good step-by-step tutorial: https://youtu.be/JIRDMGJ66SE.

### Database diagram

```mermaid
%%{init: {'theme': 'base', 
    'themeVariables': { 'primaryColor': '#aa0066',
                        'background': '#ff0000',
                        'textColor': '#000000',
                        'noteTextColor': '#ffffff',
                        'lineColor': '#dddddd'}}}%%
    erDiagram
        CARS { 
                Long id PK
                String model
                Long manufacturer_id FK
            }
            
        DRIVERS { 
                Long id PK
                String name
                String licenseNumber
                String login
                String password
            }
            
        MANUFACTURERS { 
                Long id PK
                String name
                String country
            }
            
        CARS_DRIVERS {
                Long car_id FK
                Long driver_id FK
            }
            
       CARS ||..|{ MANUFACTURERS : manufacture_id
       CARS_DRIVERS ||..|| CARS : car_id
       CARS_DRIVERS ||..|| DRIVERS : driver_id
       
```

# P.S.
Some features I learned while programming this project:
- Configuring MySQL database (queries, creation of tables, types of fields, etc.).
- Working with JDBC and MySQL database.
- Configuring servlets and controllers.
- Implementation of simple Authorization by login and password.
- Project built with SOLID principles.
- Using Session to store some information.
- Using filters to show only permitted pages.
- Using Dependency Injection for creating instances.
- Creating README.md file.
- Creating diagrams using [mermaid](https://mermaid-js.github.io/mermaid/#/) tool.  

