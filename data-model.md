# Data Model
This data model is an evolving doc to outline what sort of data we need to store in order to make sure the application functions as intended and can grow without major re-architecting or migrating of the fundamental data.

This is assuming the application is built using a relational database in lieu of a document based one. The tables outlined show rough functionality but are by no means complete as certain features will be implemented in different ways depending on the technologies used.

## Users
The users model will be our representation of an individual person within the system. Any PII that is stored will be encrypted. It is extremely important that we adhere to all GDPR guidelines as well and privacy restrictions around minors.

### Fields
  - First name
  - Last name
  - Email
  - Password - encrypted
  - Role
  - timestamps - (created, updated, deleted, last login, agree to terms, etc)
  - Age
  - Location
  - Gender
  - Other demographics

## Group
Groups represent any relationship between users. This could track the relationship between a parent and child, user and practitioner(s), etc.

### Fields
- lead_id
- follower_id
- relationship type

## Exercises
These are relatively self-explanatory, tracking the various exercises one can go through.

### Fields
- Name
- tracks (enum?)

## Data Types
These are the different types of data we collect, HRV, EEG, PulsOX etc.

### Fields
- name
- additional meta data around what we get

## Sessions
Potentially used to overtly track a specific session, tying a user to a given set of data points from the monitor. The majority of use from this would come from meta data surrounding the session. Potentially useless.

### Fields
- user_id
- location
- any user feedback (how are you feeling, etc)
- exercise tracked

## Points
This is a working name. Points would track the individual data points coming out of the devices for a given session. I think we should only store those that are from a successful session so we don’t needless bloat the data. This is going to be the workhorse table with billions of rows. 100 users logging 10 minutes of data per week, at a resolution of one point per millisecond is 3,120,000,000 data points a year. This will need to be sharded or optimized from the beginning for read/write, etc.

### Fields
- user_id
- timestamp
- session_id
- all data from sensor in that moment

## Failed Jobs
This merely tracks any automated tasks that fail on the system side. This could be regular maintenance, scheduled reporting, updating calculated rollups, etc.

### Fields
- job
- connection
- error

## Migrations
Migrations are the updates that we make to the database and tracking them is essential. This table merely shows which migrations happened when.

### Fields
- name
- batch

## Roles
Establishing what role a user is allows us to ensure they only have access to the data that’s appropriate to them. Individual users should never be able to see other user’s information, parents should see their children’s data, practitioners should see their patient’s, etc.

## Permissions
The data that various users have access to given their roles is determined by permissions. This table helps track those. No fields are listed as the implementation can differ heavily.
