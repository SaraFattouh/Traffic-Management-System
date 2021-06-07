# Traffic-Management-System
Simulation of an IOT system that keeps track of and guides autonomous vehicles operating within a city.


Overview:

The system keeps track of and guides autonomous vehicles operating within a city, the system uses a weighted graph representation of the city’s roads and keeps track of each car’s location on the graph, autonomous vehicles send out their destination and the system recommends a path based on the traffic, the system attempts to balance traffic by adding a penalty to the weights of congested roads, the penalty is calculated using the capacity of the road compared to the number of cars currently on that road, this penalty is added to the weights before calculating the path for each car, the system also allows for dynamic routing of traffic to facilitate the mobility of state vehicles such as ambulances and police cars by suggesting the shortest non-penalized path to these vehicles and at the same time routing other traffic away from the suggested path effectivity deleting a path from the graph for a short period of time. The graph can also be visualized to view the current traffic situation.
Components:
- Firebase database that stores data about the current traffic situation.
- Traffic processing background service: contains the processing algorithm that calculates the best path taking into account the penalties, handles dynamic traffic routing for state vehicles.
- Traffic monitoring interface: this contains graph visualization to show the traffic situation.
- MQTT broker that handles subscription based communication.
- Traffic generator: a simulator that generates vehicle and state vehicle instances with appropriate parameters (starting points and destinations).
- Vehicle and state vehicle: simulation of the embedded system contained on the vehicle, communicates via MQTT with the rest of the application, calculates and shows some information (current destination, current path and ETA).


Communication design:

- Vehicles and state vehicles publish their destination to the MQTT broker on topic /vehicles/<license plate number>/destination upon changing their destination and their current location to /vehicles/<license plate number>/location periodically.
- Vehicles and state vehicle instances subscribe to the topic /vehicles/<license plate number>/route and the traffic processing service publishes the recommended route for each vehicle to this topic.
- The traffic processing service subscribes to both /vehicles/+/location and /vehicles/+/destination.
- The traffic processing service updates the firebase database.
- The traffic monitoring interface accesses data in the database.
