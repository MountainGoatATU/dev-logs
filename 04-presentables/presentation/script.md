---
cssclasses:
  - fountain
---
#presentable
## 1. INTRODUCTION

.SHOW OUR FACES

MARTIN
Introducing Goat Vault - made by team Mountain Goat! Living in the mountains with mountain goats.
  
_No goats were harmed during the making of this video._
  
For internet users who are concerned about their privacy and aren't sure how they can stay safe online, Goat Vault is a password manager that is completely secure - nobody can access your data - not even us.
  
Unlike other password managers, our product teaches the user not only how to stay safe, but why it's important.

CUT TO:

## 2. WHY WE CAME UP WITH THE IDEA

ALEKSS
Why did we choose to make a password manager?

.DISPLAY PASSWORD MANAGER SURVEY STATISTICS

ALEKSS
According to a survey we conducted amongst students, the majority of people do not use good password practices. 
  
Most people often or always re-use passwords, and they rarely or never change their passwords.
## 3. Planning and Research

MARTIN

A successful project needs proper management tools in order to succeed. That is why we implemented Jira in our project development embracing the Scrum framework to delegate tasks and track our progress

Show Jira board, backlog, reports

This allowed us to fairly divide the research part of out project. We used reports and information from organizations like OWASP, NIST. We then discussed our findings with other lecturers providing insight critical for correct use of practices.

## 4. Requirements & Technical Design

JAKUB

Crucial step for development was to have a visual representation of relations and connections between our designed models and services. ERD diagrams and Architecture diagrams were created in order to track the relations of resources across our project. 

Show Architecture Diagram.
C# MAUI represents our frontend which handles hashing using the KDF algorithm (EXPLAIN) and encryption/decryptin with AES-GCM Engine (EXPLAIN) and stores local encrypted copy of vault data in SQLite DB.

The frontend connects to our backend which is hosted on our own server, and built using Python. Our Backend is used as both Auth and Data server. It provides JWT token and uses it to authenticate when using open endpoints, like /user.

Show UML Class Diagrams.
Most of our diagrams are same for both frontend and backend, like the Auth ones (SHOW). We have user route, and user models representing all the CRUD operations. As for the client, VaultEntry model represent passoword record which will be later showed in demo, in consists of ...User can have multiple passwords stored hence we are using VaultData, which is a list of VaultEntries.

Show Flow (Optional, if we have time)
Quickly go through diagrams, and for each show response and request model and reason why is that.


## 5. Development of Prototype 

Sam, Martin

