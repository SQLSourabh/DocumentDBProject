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

Code Sample
-------------
<Code>
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), AuthorizationKey);

               //Connect to the DB. If DB is not present create it
               var db = client.CreateDatabaseQuery().Where(d => d.Id == "ContosoServiceRequest").AsEnumerable().FirstOrDefault();
               if (db == null)
               {
                   db = client.CreateDatabaseAsync(new Database { Id = "ContosoServiceRequest" }).Result;
               }

               //Connect to the Collection. If DB is not present create it
               var collection = client.CreateDocumentCollectionQuery(db.SelfLink).Where(d => d.Id == "ServiceRequest").AsEnumerable().FirstOrDefault();
               if (collection == null)
               {
                   var collectionSpec = new DocumentCollection { Id = "ServiceRequest" };
                   var requestOptions = new RequestOptions { OfferType = "S2" };
                   collection = client.CreateDocumentCollectionAsync(db.SelfLink, collectionSpec, requestOptions).Result;
               }
               //Insert Document into the Collection using the Stream.
               Document curDoc =  await client.CreateDocumentAsync(collection.SelfLink, Resource.LoadFrom<Document>(stream));
               
               foreach (string x in attchmentfiles)
               {
                   //MessageBox.Show(@x.InnerText);
                   if (x != "")
                   {
                       using (FileStream fileStream = new FileStream(x, FileMode.Open))
                       {
                           //Create the attachment
                               string slug = x.Substring(x.LastIndexOf("\\") + 1, (x.Length - x.LastIndexOf("\\")) - 1);
                               string contenttype = GetFileContentType(@x);
                               await client.CreateAttachmentAsync(curDoc.AttachmentsLink, fileStream, new MediaOptions { ContentType = contenttype, Slug = slug});
                               fileStream.Dispose();
                       }
                   }
               }
</Code>









