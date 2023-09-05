# Legacy Application Connection String Deconstruction

## The Entire Connection string

```xml:
<add name="EntityName" connectionString="metadata=res://*/EF.ModelName.csdl|res://*/EF.ModelName.ssdl|res://*/EF.ModelName.msl;provider=Oracle.ManagedDataAccess.Client;provider connection string=&quot;data source=database.name;user id=[APP_SPECIFIC_SCHEMA_ACCT];Validate Connection=True;Connection Timeout=120;Connection Lifetime=10;Max Pool Size=2;Min Pool Size=1;Pooling=false&quot;" providerName="System.Data.EntityClient" />
```

Contained within the connection string tag there are 3 attributes: name, connectionString, and providerName. This connection string usually sits within a connection or Connections.config at ECA within a collection of connection strings. They handle various things, such as connecting to a reports database or database specific for the application. Do note that there is no password contained within the string, this is due to Oracle wallet.

### name
This is simply the name of the connection. Usually you can default to the name of an edmx file for this attribute in older applications. In applications like OASIS, this is left out.

### connectionString
This is the bulkier part of the connection string. It contains a metadata section first which will name off the edmx model from the legacy project. Make sure these are included. Afterwards, you will then name the database name and user account within the \&quot; tags as well as other database string configurations, like limiting the amount of connections from your application to the database.

### providerName
This is a simple reference to the provider that your application uses to maintain database transactions. Usually the legacy applications use System.Data.EntityClient.