Problem Statement
====================
Build Scalable Service Request Submission/Actioning System, which allows users to attach attachments, provide details of the service required. Buliding such a system on SQL (structured) environments is challenging becuase of the Size and concurrency contrainsts on the OLTP environment. This project uses DocumentDB and .Net WPF forms to simulate the Forms and the associated processes. 

Technologies Used
======================
1. DocumentDB
2. .Net WPF and DocumentDB/XML classes.

Class Definitions
=====================
SubmitNewForm - WPF class, Which exposes functions to read the user Inputs and them save them to DocumentDB.
UpdateRequest - WPF class, Which exposes functions to read the user Inputs and update/replace an already exisiting document in DocumentDB.
RequestSearch - WPF Class, to get user parameters to search for a Service Request Document.
DocumentDBOperations - Provides functions to Insert/Search/Retrieve/Update a document in DocumentDB.All the functions use JSON/XML serialization to insert/update user inputs into DocumentDB.










