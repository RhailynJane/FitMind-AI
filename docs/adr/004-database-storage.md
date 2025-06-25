### ADR: Choose Backend Database
## 📚 Table of Contents

- [Summary](#summary)
- [Issue](#issue)
- [Decision](#decision)
- [Status](#status)
- [Assumptions](#assumptions)
- [Constraints](#constraints)
- [Positions: Comparative Analysis](#positions-comparative-analysis)
- [Argument](#argument)
- [Implications](#implications)
- [Related Decisions](#related-decisions)
- [Related Requirements](#related-requirements)
- [Related Artifacts](#related-artifacts)
- [Related Principles](#related-principles)
- [Notes](#notes)
- [References](#references)

---

## Summary
Select a backend database solution to support the FitMind AI app's need to store and manage user profiles, workout media, AI-generated plans, and progress logs.

## Issue
We need a backend database that integrates well with our JavaScript-based stack (Node.js, TypeScript, React Native) and can handle both structured and semi-structured data types while supporting scalability and fast development.

## Decision
MongoDB was selected as the backend database.

## Status
Accepted

## Details
 # Assumptions
•	The backend will be built with Node.js and TypeScript.
•	Developers have moderate JavaScript experience but limited SQL exposure.
•	Data structures (e.g., workouts, AI-generated plans) are expected to evolve rapidly.
•	We require a cloud-scalable and mobile-friendly backend.
 # Constraints
•	Project Timeline
•	Limited team expertise in SQL.
•	AI and analytics features depend on flexible data models.
 # Positions: Comparative Analysis
Option	Pros	Cons
MongoDB (NoSQL)	- Schema flexibility
- Native Node.js support
- Scalable via MongoDB Atlas	- No foreign keys
- Complex joins are inefficient
- Requires app-level validation
PostgreSQL (SQL)	- Strong relational integrity
- Great for analytics
- Robust constraints	- Slower dev cycles
- Steeper learning curve
- Less ideal for dynamic/nested data
Firebase Firestore	- Real-time sync
- Serverless
- Mobile-first integration	- Limited query flexibility
- Vendor lock-in
- Expensive at scale
None (In-memory)	- Extremely fast dev
- No storage/compliance needs	- Data lost on restart
- No syncing
- No persistence or personalization

## Argument
MongoDB offers the flexibility needed for rapid MVP development and supports semi-structured and evolving data. It integrates easily with Node.js via Mongoose and allows for horizontal scaling via MongoDB Atlas. Though it lacks relational constraints, this tradeoff is manageable for the MVP scope.

## Implications
 # Positive
•	Fast iteration using JavaScript-native tools
•	Flexible schema fits AI and dynamic workout data
•	Horizontal scalability and JSON-friendly structure
•	Pairs well with mobile local storage strategies
# Negative
•	Requires app-managed validation and consistency
•	Less effective for complex analytical reporting
•	Risk of data inconsistency without strict schemas

## Related Decisions
•	ADR: Choose Hybrid Storage Strategy (Local + Remote)

## Related Requirements
•	Scalable, cloud-hosted database
•	Support for both structured and unstructured data
•	Easy backend integration with JavaScript stack

## Related Artifacts
•	MongoDB schema design drafts
•	Mongoose ODM configuration
•	MVP development plan

## Related Principles
•	Build for MVP speed and flexibility
•	Cloud-native and mobile-first architecture

## Notes
Validation logic will be implemented in the application using Mongoose schemas. Future analytics use cases may require ETL pipelines or data modeling extensions.

## References
[1] MongoDB, "MongoDB Manual," MongoDB Documentation, 2024. [Online]. Available: https://www.mongodb.com/docs/manual/
[2] C. S. A. Fernandes, “NoSQL databases for flexible fitness app data,” Journal of Software Engineering, vol. 15, no. 3, pp. 120–130, 2022.
[3] J. Patel and M. Thompson, “Choosing databases for scalable mobile applications,” IEEE Software, vol. 37, no. 2, pp. 45–53, 2020.



### ADR: Choose Hybrid Storage Strategy (Local + Remote)
## 📚 Table of Contents

- [Summary](#summary)
- [Issue](#issue)
- [Decision](#decision)
- [Status](#status)
- [Assumptions](#assumptions)
- [Constraints](#constraints)
- [Positions: Comparative Analysis](#positions-comparative-analysis)
- [Argument](#argument)
- [Implications](#implications)
- [Related Decisions](#related-decisions)
- [Related Requirements](#related-requirements)
- [Related Artifacts](#related-artifacts)
- [Related Principles](#related-principles)
- [Notes](#notes)
- [References](#references)

---

## Summary
Define a storage strategy that allows the FitMind AI mobile app to work offline, support cross-device sync, and secure personal data, while meeting tight MVP deadlines.

## Issue
We need to support offline workouts and cross-device sync in an environment with intermittent connectivity, while also enabling long-term scalability and secure data handling.

## Decision
Hybrid storage combining local storage (AsyncStorage) and remote backend (MongoDB Atlas) with periodic sync.

## Status
Accepted

## Details
 # Assumptions
•	Users expect offline access to workout content.
•	Data must be synchronized securely across devices.
•	The backend uses MongoDB Atlas for persistent storage.
•	We must handle sensitive health-related user data.
•	Real-time sync is not required in MVP, but offline support is.
 # Constraints
•	Project Timeline 
•	App must work reliably in low-connectivity environments.
•	No real-time sync architecture due to time constraints.
 # Positions: Comparative Analysis
Option	Pros	Cons
Remote Storage Only	- Centralized data
- Simplified analytics
- Scalable	- No offline access
- High latency
- Poor UX in poor signal
Local Storage Only	- Very fast
- Works offline
- Easy to implement	- No sync
- No backups
- Risk of data loss
Hybrid (Chosen)	- Offline-first UX
- Sync + backup support
- Balanced model	- Requires sync/conflict logic
- Dual-layer security complexity
None (Memory only)	- Extremely fast
- Zero infra	- Data lost on app exit
- Useless for persistent features

## Argument
Hybrid storage offers the best compromise for our use case. Local AsyncStorage provides fast, offline-first access, while remote MongoDB storage ensures persistence and cross-device functionality. Sync logic can be optimized over time, but even a simple push/pull strategy supports MVP goals.

## Implications
 # Positive
•	Smooth offline experience
•	Reliable cloud backup and sync
•	Responsive UI from local caching
•	Scalability for future growth
 # Negative
•	Increased complexity due to sync conflicts
•	Security and validation needed in both layers
•	Extra testing for sync scenarios

## Related Decisions
•	ADR: Choose Backend Database

## Related Requirements
•	Support for offline workouts and progress logging
•	Cross-device data sync
•	Secure handling of health data

## Related Artifacts
•	Sync logic spec document
•	AsyncStorage wrapper module
•	MongoDB data sync APIs

## Related Principles
•	Offline-first design
•	User data safety and ownership
•	Scalable-by-default

## Notes
Syncing will initially be periodic (e.g., on app open/close or manual trigger), with conflict resolution based on timestamps or client priority.

## References
[1] MongoDB, "MongoDB Manual," MongoDB Documentation, 2024. [Online]. Available: https://www.mongodb.com/docs/manual/
[4] F. Gomez et al., “Hybrid cloud-mobile architectures for fitness apps,” IEEE Access, vol. 8, pp. 11100–11112, 2020.
[5] R. Kumar and S. Sharma, “Data persistence challenges in mobile app design,” IEEE Software, vol. 36, no. 5, pp. 38–45, 2019.


