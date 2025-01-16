# Case description and datasets for optimization use case of cargo shipments

This repository contains imaginary case description and data sets for bulk cargo shipping from South America to Far East. The case and the data sets are used for quantum calcution experiments to optimize cargo allocation, scheduling and resources. 

This particular case has three types of fundamental problems:
* Highly constrained combinatorial problem (HCCP)
* Multi vehiche routing problem (MVRP)
* Capacitated Multi-Vehicle Routing Problem (CMVRP)

### Highly Constrained Combinatorial Problem (HCCP)
A highly constrained combinatorial problem involves finding the best solution from a finite set of possible solutions, subject to numerous and strict constraints. These problems are characterized by:

* Discrete Decision Variables: The solutions are composed of discrete elements (e.g., assignments, routes, schedules).
* Combinatorial Explosion: The number of possible solutions grows exponentially as the problem size increases.
* Complex Constraints: These often include rules, limits, or relationships that must be satisfied simultaneously, which can make the problem intractable for larger instances.

### Multi-Vehicle Routing Problem (MVRP)
The Multi-Vehicle Routing Problem (MVRP) is an extension of the Vehicle Routing Problem (VRP) and is a classic example of a highly constrained combinatorial problem.

Problem Definition:
* Objective: Minimize the total cost (e.g., distance, time, fuel) or maximize efficiency (e.g., timely deliveries).
* Vehicles: Multiple vehicles, often starting from one or more depots, must service a set of customers or locations.
* Constraints:
    * Capacity of vehicles (e.g., weight or volume limits).
    * Time windows for serving customers.
    * Vehicle availability or route limits (e.g., maximum distance per route).
    * Specific customer requirements (e.g., precedence, pairing).

### Capacitated Multi-Vehicle Routing Problem (CMVRP)
The Capacitated Multi-Vehicle Routing Problem (CMVRP) is a specific variant of the Multi-Vehicle Routing Problem (MVRP) where the vehicles used for routing have limited capacities in terms of the load they can carry. This capacity constraint makes the problem more challenging and realistic for applications in logistics, supply chain management, and transportation.

Problem Definition:
* Objective:
    * Minimize total costs, which can include:
    * Total distance traveled by all vehicles.
    * Total time taken for servicing.
    * Operational costs (e.g., fuel or driver wages).
* Key Features:
    * Multiple Vehicles: A fleet of vehicles is available, each starting from one or more depots.
    * Customer Demands: Each customer has a specific demand that must be satisfied by one vehicle.
    * Capacity Constraints: Each vehicle has a maximum capacity for goods or passengers it can carry. The sum of the demands served by a single vehicle must not exceed its capacity.

## The problem
There are multiple cargos that needs to transported from its origin port to the destination port. How do we satisfy transportation demand at the loading and discharging ports without wasting money on bunker fuel and while maximizing the operating income.

### Problem goal:
Reduce bunker fuel costs and build most optimal cargo loading schedules and itinerary for each vessel, while making sure the delivery requirements (for instance `laycan` and `draft`) are met.

### Proposal:
Build a quantum optimization application which provides schedules and itinerary to a fleet of vessels.


## Data sets
There are several data sets provided for setting the case scene and solving the problem. The data sets are simplified versions of real life but serves the purpose. 

### Ports data
Ports data holds information of 20 major bulk cargo ports in Far East Asia and 10 major bulk cargo ports in East coast of South America. As a basic constrain each port has `draft restriction` indicating that vessel with bigger draft can not enter the port. Also some other contextual descriptive restrictions may be mentioned but not consider in calculations.

### Fleet data
Example fleet contains 89 different vessels with `deadweight capacity` varying from 23529 up to 73296 tons. Each vessel has `number of holds` which allows loading different type of cargo to each hold. For example vessel with 8 holds can load pulp in 3 holds and grain to remaining 5 holds. 

Each vessel also has `draft` measure that determines the minimum depth of water a vessel can safely navigate and enter a port. This constrain means that all vessel cannot visit all ports due to draft restriction. 

> Notice that `bunker fuel cost` at speed of 10 knots per nautical mile for each vessel varies. The bigger the vessel the bigger the consumption. However, bigger vessel can carry more cargo, so that needs to be considered when planning cargo allocations for each vessel.

### Cargo data
Cargo data represents the `amount of cargo` needed to be transport from `origin port` to `destination port` at different `laycan` times  (i.e. delivery time window). Freight rate represent how much income delivering that cargo will generate. 
> Notice that the amount of cargo to transported exceeds the loading capacity (`deadweight`) of any vessel. Thus, multiple voyages are needed to transport the whole cargo amount.

### Port distance matrix
Port distance matrix is simply a table of distances (in nautical miles) between any of the ports in the `ports data`. This matrx is needed to plan the optimal routing and itinerary.

### Vessel positions
Vessel positions data set represent the initial position of each vessel in the setup. The `status` indicates whether vessel is underway to the `port` mentioned or is vessel moored at that `port`. If vessel is underway, then distance to that port is given.


## Problem description

### Variables (not a comprehensive list)
* Amount of cargo to be transported
* Bunker fuel consumption

### Constant (not a comprehensive list)
* Port distances
* Vessel capacity
* Laycan time

### Decision variables
Which vessel should visit which port and in which order => itinerary for vessels

### Objectives (not a comprehensive list)
* Minimize total distance sailed by vessel
* Minimize bunker fuel consumption while satisfying the laycan times

### Constrains (not a comprehensive list)
* Don't exceed the carrying capacity of a vessel
* Meet requirement laycan times (delivery time window)
* Consider the port draft restriction for vessel

## Approach
First test with simple set of data and then iterate with more complex scenarios and scaled data sets.
* For instance, start with single vessel routing
* Scale to capacitated multi vessel routing