Task 1: Parking Lot System Design

Classes and Interfaces

1. ParkingLot - Represents the entire parking lot system
2. ParkingSpot - Abstract class or interface representing a parking spot
3. SmallSpot - Concrete class for small parking spots (for motorcycles)
4. MediumSpot - Concrete class for medium parking spots (for cars)
5. LargeSpot - Concrete class for large parking spots (for buses)
6. Motorcycle - Class for motorcycles
7. Car - Class for cars
8. Bus - Class for buses

Design Justification

Class Structure Rationale

1. ParkingLot Class: 
   - Acts as the main container and manager for all parking spots
   - Encapsulates the collection of parking spots and provides operations to manage them
   - Follows the principle of having a clear entry point for the system

2. ParkingSpot Hierarchy:
   - Used inheritance with SmallSpot, MediumSpot, and LargeSpot extending ParkingSpot
   - This design allows for extensibility - if new spot types are needed in the future, they can be added without modifying existing code
   - Each spot type can have its own specific attributes (e.g., pricing, location constraints) while sharing common behavior

3. Vehicle Classes:
   - Motorcycle, Car, and Bus are separate concrete classes
   - Each vehicle type has its own size constraint that determines which spot type it can park in
   - Motorcycles can only park in SmallSpots
   - Cars can only park in MediumSpots
   - Buses can only park in LargeSpots
   - This constraint can be enforced through the design by having appropriate methods that check compatibility before allowing parking

4. Size Constraint Enforcement:
   - The relationship between vehicles and parking spots is constrained by size compatibility
   - Motorcycles can only park in SmallSpots
   - Cars can only park in MediumSpots
   - Buses can only park in LargeSpots
   - This constraint can be enforced through the design by having appropriate methods that check compatibility before allowing parking

Design Principles Applied

- Single Responsibility Principle: Each class has a clear, focused responsibility
- Open/Closed Principle: The design is open for extension (new vehicle types, new spot types) but closed for modification
- Separation of Concerns: ParkingLot manages spots, spots manage their state, vehicles are separate entities

Alternative Considerations

An alternative design could use a Vehicle abstract class with inheritance, but the simpler approach of having separate vehicle classes was chosen to keep the design straightforward while still maintaining the size constraint rules.
