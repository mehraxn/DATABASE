**README: Sensor Temperature ER Diagram**
Below is the text and a brief explanation of how the ER diagram was derived, followed by answers to specific questions about the modeling choices.

---

### Text Description

🔴A set of sensors are available, each identified by a unique code and present within a building.&#x20;

🔴It is requested to store the different temperature values detected by each sensor at different time points.

---

### ER Diagram Explanation

#### Entities

- **SENSOR**: Represents each sensor, with a unique **ID**.
- **TIME**: Represents different time points, with a **Timestamp** to record when a temperature is detected.

#### Relationship

- **RETRIEVES**: Connects **SENSOR** and **TIME**, indicating that a sensor retrieves a **Temperature** value at a specific point in time.

#### Attributes

- **SENSOR**: Has an **ID** (unique identifier).
- **TIME**: Has a **Timestamp** (unique identifier).
- **RETRIEVES**: Has a **Temperature** attribute.

#### Cardinality

- **From SENSOR to RETRIEVES: (0, N)**
  - A sensor may retrieve zero or multiple temperature readings over time.
- **From TIME to RETRIEVES: (1, N)**
  - Each time point must have at least one temperature reading (and potentially more from different sensors).

---

### Questions & Answers

#### Why do we consider a separate entity for the time?

We create a separate **TIME** entity because *time points* can be treated as data objects in their own right. By having a distinct entity, we can clearly reference, store, and manage details about each time point (e.g., querying data by date or timestamp, relating multiple sensors to the same timestamp). It also makes the design more flexible if additional time-related attributes (e.g., day of the week) need to be added in the future.

#### Why is Timestamp an identifier for the time?

The **Timestamp** attribute uniquely identifies each time point in the system. Since each sensor reading is associated with a specific instant in time, using that instant’s timestamp as an identifier ensures no ambiguity in recording or retrieving the temperature readings. Essentially, no two readings share the exact same entity record for time unless they occur at precisely the same moment.

