# Domain Objects

### Facility

A facility models a healthcare center - it can be anything from a (relatively small) doctor's office 
to a (large) hospital.

### Resource

A resource models anything that needs to be tracked from a supply and demand perspective. 
Examples are ICU beds or ventilators.

### User

A user models anyone interacting with the system ... anonymous users don't need to log into the system.

### Role

Anonymous users are read-only users. Users that can modify data in the system are organized into 4 roles -

1. System
2. Facility Admin
3. Facility Supply
4. Facility Demand

'System' users can add/remove other (non-system) Users, Facilities, Resources and Roles.

'Facility Admin' users can add/remove Users to/from Facilities.

'Facility Supply' users can can modify a Facility's supply 
(i.e. the capacity of a Facility for a particular Resource)

'Facility Demand' users can can modify a Facility's demand 
(i.e. the volume a Facility is facing for a particular Resource)

### UserFacilityRole

A User can have multiple Roles in a Facility that lets him/her modify data in the system.

### Supply

A number per Resource per Facility - can be aggregated on a zip/county/state basis

### Demand

A number per Resource per Facility - can be aggregated on a zip/county/state basis

# System Architecture

**Database:** Microsoft SQL Server, preferably Azure SQL with Replicas

**Application:** Java/Spring

**Database scaling:** Anonymous/read-only requests are handled by Replicas, 
write requests are handled by the master database

**Application scaling:** Additional app servers deployed as necessary

**Elasticity:** None initially, K8s later

# Screens

### Home 

* Displays supply v/s demand for the top, say 2 Resources in a (local) zip/county/state

* Drilling down into a Resource offers more/less granular filtering ... by zip/county/state/Facility

### Signup

A page where Facility administrators can sign up to be part of the system. Required fields are -

* Last, First name
* A login ID
* Email
* Phone
* Facility name they're registering for

Submission of this form kicks off a validation workflow - the email and/or phone needs to verified; 
the person needs to be vetted from a Facility 'ownership' perspective. The User is subsequently notified 
by email on completing his/her signup process (where s/he will go through the password creation process).

### Pending signups

A page where 'System' Users can triage pending signup requests. 
Upon successful triage, an 'Accept' request sent from this page will create the User and send him/her a
signup completion email.
 
### Signup completion 

This page will typically be the target of a link in an email sent to the User to complete signup.
S/he will choose and confirm a password on this page. 

### Login

A page where the User can log into the system.

### Facility Data Entry

A page where a logged in User can view/edit Facility data depending upon his/her Role. 
A Master-Detail view for existing Facilities and/or a 'New Form' for a new one.

### User Data Entry

A page where a logged in User can view/edit User data depending upon his/her Role. 
A Master-Detail view for existing Users and/or a 'New Form' for a new one.

Note that Facility Admins will invite Facility Users on this page, 
i.e. they will create new Users and the associated workflow will send him/her a signup completion email.

### Resource Data Entry

A page where a logged in User can view/edit Resource data depending upon his/her Role. 
A Master-Detail view for existing Resources and/or a 'New Form' for a new one.

### Supply Data Entry

A page where a logged in User can view/edit Supply data depending upon his/her Role. 
A Master-Detail view for existing Facilities and/or a 'New Form' for a new one.

### Demand Data Entry

A page where a logged in User can view/edit Demand data depending upon his/her Role. 
A Master-Detail view for existing Facilities and/or a 'New Form' for a new one.
