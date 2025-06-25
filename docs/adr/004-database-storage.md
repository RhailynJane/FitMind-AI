ADR: Choose Backend Database
Context
The FitMind AI mobile app requires a backend database to store and manage diverse data types such as:
•	User profiles
•	Workout library (media and descriptions)
•	AI-generated workout plans
•	Progress logs and visual analytics
We are using Node.js, TypeScript, and React Native, and our team has moderate experience with JavaScript but limited SQL exposure. A fast, flexible, and scalable solution is essential to meet our project timeline.

Options
Option 1: MongoDB (NoSQL Document Store)
A schema-less NoSQL database that stores data in BSON/JSON-like documents.
Pros:
•	Naturally fits semi-structured and nested data [1]
•	High compatibility with Node.js and Mongoose ODM [1]
•	Scalable via cloud platforms like MongoDB Atlas [1]
•	Flexible schema simplifies iterative development [2]
Cons:
•	Lacks relational integrity features like foreign keys [3]
•	Application must enforce consistency and validation logic [3]
•	Less efficient for complex joins or analytical reporting [3]

Option 2: PostgreSQL (Relational SQL DBMS)
An advanced open-source RDBMS with rich SQL support.
Pros:
•	Strong support for data integrity through schemas and constraints [3]
•	Ideal for structured, normalized data with clear relationships
•	Robust transaction management and indexing
Cons:
•	Slower development due to schema rigidity
•	Less intuitive for nested or dynamic content [3]
•	Higher learning curve for JavaScript-based teams

Option 3: Firebase Firestore (Cloud NoSQL)
A serverless NoSQL cloud database with real-time sync.
Pros:
•	Built-in real-time and offline sync features [4]
•	Backend-less architecture reduces infrastructure needs
•	Simple to integrate into mobile apps
Cons:
•	Limited query functionality and custom server logic [4]
•	Can become expensive at scale
•	Vendor lock-in risk and lower backend flexibility

Option 4: None (No Persistent Backend)
The app uses only temporary memory or local storage.
Pros:
•	Very fast development and deployment
•	No data privacy/storage compliance required
•	Minimal backend resources
Cons:
•	User data lost on app restart or crash [5]
•	No ability to sync across devices
•	Incompatible with AI coaching or analytics features [5]

Decision
We selected MongoDB as our backend database due to:
•	Seamless integration with our Node.js backend
•	Flexibility in storing evolving workout and user data
•	Reduced schema overhead during MVP development
•	Deployment and scaling support through MongoDB Atlas

Consequences
Positive:
•	Faster development using familiar tools
•	Flexible data modeling for AI and analytics features
•	Ready for horizontal scaling
•	Enables JSON mirroring with local storage for hybrid sync
Negative:
•	Requires strong application-level validation
•	May limit complex reporting functions
•	Less rigid structure than SQL may increase data consistency risks


ADR: Choose Hybrid Storage Strategy (Local + Remote)
Context
The app must support:
•	Offline access to workouts
•	Cross-device sync of user data
•	Secure handling of personal health data
•	Scalable storage
•	Rapid MVP delivery within 5 weeks

Options
Option 1: Remote Storage Only
All data stored remotely in a cloud DB like MongoDB Atlas or PostgreSQL.
Pros:
•	Centralized data and easier backups [1]
•	Simplifies analytics and cross-device sync
•	Scalable and secure [1]
Cons:
•	Fails without internet [4]
•	Higher latency compared to local access [4]
•	Poor experience in low-connectivity situations

Option 2: Local Storage Only
Use device storage only (e.g., AsyncStorage or SQLite).
Pros:
•	Very low latency [5]
•	Works fully offline [5]
•	Quick and simple to implement
Cons:
•	No backup or cross-device support [5]
•	High risk of data loss if device is lost or reset
•	Poor fit for long-term growth or user engagement

Option 3: Hybrid Storage (Local + Remote)
Use both local and remote storage with periodic sync.
Pros:
•	Offline-first user experience [4]
•	Central data backup and cross-device support [1]
•	Balance between performance and feature depth [4]
Cons:
•	Requires sync logic and conflict resolution [4]
•	More complex testing and edge-case handling
•	Dual-layer security responsibilities

Option 4: None (No Storage)
Data stored only in memory, cleared on app exit.
Pros:
•	Extremely lightweight
•	Easiest to build
•	No backend or data compliance issues
Cons:
•	Loses all progress when closed [5]
•	No AI personalization or analytics
•	Completely impractical for fitness tracking

Decision
We chose Hybrid Storage:
•	MongoDB Atlas for persistent remote storage
•	AsyncStorage for offline session support
•	Sync logic to reconcile updates and changes across sessions
•	Local encryption to safeguard sensitive data

Consequences
Positive:
•	Seamless offline access
•	Reliable long-term data storage
•	Responsive UX with local caching
•	Scalable architecture for future expansion
Negative:
•	Higher complexity from sync/conflict resolution
•	More effort for secure sync and testing
•	Dual maintenance of local and remote states

References (IEEE Style)
[1] MongoDB, "MongoDB Manual," MongoDB Documentation, 2024. [Online]. Available: https://www.mongodb.com/docs/manual/
[2] C. S. A. Fernandes, “NoSQL databases for flexible fitness app data,” Journal of Software Engineering, vol. 15, no. 3, pp. 120–130, 2022.
[3] J. Patel and M. Thompson, “Choosing databases for scalable mobile applications,” IEEE Software, vol. 37, no. 2, pp. 45–53, 2020.
[4] F. Gomez et al., “Hybrid cloud-mobile architectures for fitness apps,” IEEE Access, vol. 8, pp. 11100–11112, 2020.
[5] R. Kumar and S. Sharma, “Data persistence challenges in mobile app design,” IEEE Software, vol. 36, no. 5, pp. 38–45, 2019.

