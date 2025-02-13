Prompt: Would you draw a text-only schematic of this integration?

Here is a text-only schematic of the ERP-LMS-Directory Server integration using OpenEAI.

```
+------------------+        Create-Sync        +------------------+
|   ERP System    |------------------------->|  OpenEAI Core    |
|  (Authoritative)|                           | Messaging Engine |
|     Source      |<-------------------------|                  |
+------------------+        Response          +------------------+
|
|
| BasicPerson.Create-Sync
|
v
+------------------+        Create User        +------------------+
|      LMS        |<-------------------------|  OpenEAI LMS GW   |
| (Consumes Sync) |------------------------->|  (Message Sub)    |
+------------------+        Ack Response      +------------------+
|
|  
| LMSUser.Create
|
v
+------------------+
|   LMS Database  |
|  (User Records) |
+------------------+


+------------------+        Create User        +------------------+
| Directory Server|<-------------------------| OpenEAI Dir Sync  |
| (Consumes Sync) |------------------------->|  (Message Sub)    |
+------------------+        Ack Response      +------------------+
|
|
| DirectoryUser.Create
|
v
+------------------+
|   LDAP Server   |
| (Authentication)|
+------------------+
```

### **Flow Summary:**
1. **ERP System** generates a `BasicPerson.Create-Sync` message.
2. **OpenEAI Messaging Engine** routes the message to subscribers.
3. **LMS Gateway** receives the message and provisions the user.
4. **Directory Sync Gateway** receives the message and updates authentication records.
5. Both systems send back acknowledgment responses.
6. Data is persisted in the respective databases.

Let me know if you need any modifications! 🚀
