Google-Maps-TSP-Solver
======================

Google Maps TSP Solver that computes the fastest route that visits a given set of locations using JavaScript.  It's a fork of Geir Engdahl's TSP Solver project, but is/will be optimized for headless environments (node.js, Titanium)  multiple APIs.

*Work in progress.*

### Algorithms

Different algorithms are selected based on the number of input locations, in order to produce results in a responsive manner. For large sets of points, the returned solution will be approximate. It is an NP-complete problem after all.

- tspK3 - Uses 3-opt algorithm to find a good solution to the TSP. The 3-opt 
algorithm is a local optimization technique similar to the 2-opt step 
which is performed after each wave of ants in the ant colony solver. 
Since 3-opt is much more expensive than 2-opt, it's too slow to use it 
after each ant wave. But it is used after the ant colony / 2-opt combo 
is done, and usually improves the solution quite a bit. The solution 
quality is never degraded by this step. 

- tspAntColonyK2 - Computes a near-optimal solution to the TSP problem, using Ant Colony Optimization and local optimization in the form of k2-opting each candidate route.

- tspBruteForce - Returns the optimal solution to the TSP problem. If mode is 1, it will return the optimal solution to the related problem of finding a path from node 0 to node numActive - 1, visiting the in-between nodes in the best order.


###Usage

```
// Your normal Google Map object initialization
var myOptions = {
  zoom: zoom,
  center: center,
  mapTypeId: google.maps.MapTypeId.ROADMAP
};
myMap = new google.maps.Map(div, myOptions);
directionsPanel = document.getElementById("my_textual_div");

// Create the tsp object
tsp = new BpTspSolver(myMap, directionsPanel);

// Set your preferences
tsp.setAvoidHighways(true);
tsp.setTravelMode(google.maps.DirectionsTravelMode.WALKING);

// Add points (by coordinates, or by address).
// The first point added is the starting location.
// The last point added is the final destination (in the case of A - Z mode)
tsp.addWaypoint(latLng, addWaypointCallback);  // Note: The callback is new for version 3, to ensure waypoints and addresses appear in the order they were added in.
tsp.addAddress(address, addAddressCallback);

// Solve the problem (start and end up at the first location)
tsp.solveRoundTrip(onSolveCallback);
// Or, if you want to start in the first location and end at the last,
// but don't care about the order of the points in between:
tsp.solveAtoZ(onSolveCallback);

// Retrieve the solution (so you can display it to the user or do whatever :-)
var dir = tsp.getGDirections();  // This is a normal GDirections object.
// The order of the elements in dir now correspond to the optimal route.

// If you just want the permutation of the location indices that is the best route:
var order = tsp.getOrder();

// If you want the duration matrix that was used to compute the route:
var durations = tsp.getDurations();

// There are also other utility functions, see the source.
```

Original source is located at:  [https://code.google.com/p/google-maps-tsp-solver/](https://code.google.com/p/google-maps-tsp-solver/)

### License

MIT

### Authors

- James Tolley <info [at] gmaptools.com>
- Geir K. Engdahl <geir.engdahl (at) gmail.com>
