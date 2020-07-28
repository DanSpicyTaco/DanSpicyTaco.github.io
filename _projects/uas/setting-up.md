# Setting Up

## Simulation

The simulation environment is going to be:

1. Create a "virtual control stick" that can simulate the flight of a UAV
2.

The DJI SDK provides two example applications that are useful for creating a simulation.
First, the Mobile Controller creates a “Virtual Control Stick” that simulates the flight of a UAV when moved. Using the
simulator inside the mobile application means that flight controls will not be sent to the UAV. However,
donwnlink video will still be available. This link will remain the point of interest for the attacker, and the
attacks can be run the same. Next, the DJI Remote Logger [7] allows for UAV data, including GPS location,
altitude and sensor measurements, to be exported to another application. This will help move UAV data
from the mobile application to a webserver. On the webserver, a plot of the path can be created in real-time,
or be stored for display using another simulation software.

Plan of attack:

1. Create the virtual control stick
2. Create video downlink
3. Create remote logging
4. Create the watchman
