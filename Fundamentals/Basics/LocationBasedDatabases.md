## Location-Based Database

### Location Representation
 - Many companies like Google maps, Swiggy, Uber uses Location based databases.
 - The two basic requirements would be :
1. Measurable distance → a. Uniform assignment b. Scalable granuality
2. Proximity → Get me all the others within 10KM.

 - The one way to represent a location is via a coordinate.
 - With coordinates, we can find the `euclidean distance`.
 - But if we plan to find the proximity, it will be challenging, as we have to iterate through all the points in the DB.

 - We can use bits to represent the location.
 - The proximity could be found by matching the first common prefix.
 - But what if the points lies in different quadrants, the MSBs would be different.

### Data Structure—Quad Trees
 - The entire world is divided into four parts, each part is representing one quadrant of the world.
 - And the tree goes on down as much as required.

### Range Queries and Hilbert Curve
 - Range queries in 2-D plane is a problem.
 - The trees, like interval,segment trees etc gives a good optimization for single line Range queries.
 - What if we project the 2-D plane into a 1-D line?
 - Revisit the lectures for better understanding of how to represent a 1-D line into 2-D plane. 
