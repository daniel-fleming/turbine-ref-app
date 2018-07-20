# Turbine + Eureka Reference Application

# Architecture
For the purposes of this reference app all components are stored in the customers app deployment.  This includes the Hystrix dashboard, Turbine stream, Hystrix Fallback method, REST endpoint and Eureka Client configurations.  Based on the manifest config there will be 3 app instances deployed by default.

The Eureka Server is a standalone Eureka server with one deployed instance.

# Running the reference implementation

1. Build and deploy the Eureka server instance to your PCF space   
    ```
    cd turbine-demo-discovery
    mvn clean package && cf push
    ```

2. Build and deploy the customers app instances to your PCF space
    
    Make sure you modify the eureka.serviceUrl.defaultZone property in the application.yml file before proceeding
    ```
    eureka:
        client:
            registerWithEureka: true
            fetchRegistry: true
        serviceUrl:
        defaultZone: https://<YOUR-EUREKA-URL>
    ```

    ```
    cd customers
    mvn clean package && cf push
    ```

3. Navigate to the Eureka instance to view details
    ```
    https://<YOUR-EUREKA-PCF-FQDN>
    ```

4. Verify that the URLs listed under the "Status" column header link to your app and pull up the correct status page

5. Navigate to the Hystrix dashboard
    ```
    https://<YOUR-CUSTOMER-APP-FQDN>/hystrix
    ```

    Enter the following stream URL to view metrics
    ```
    https://<YOUR-CUSTOMER-APP-FQDN>/turbine.stream?cluster=CUSTOMERS
    ```

6. Use a tool like Postman Runner to put load on the customers REST endpoint
    ```
    https://<YOUR-CUSTOMER-APP-FQDN>/customers
    ```

7. In the Hystrix dashboard you should start seeing traffic from multiple hosts from across the cluster.  In the thread pool section you should be able to view multiple thread pools, one from each instance.
