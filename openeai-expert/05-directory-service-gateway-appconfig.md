Prompt: Would you generate an AppConfig config document to configure this service?

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AppConfig>
    <Service name="DirectoryServiceGateway">
        <Messaging>
            <Consumer>
                <QueueName>com.erp.Person.BasicPerson.Create-Sync</QueueName>
                <ClientClass>org.openeai.jms.consumer.MessageConsumerClient</ClientClass>
                <CommandClass>com.example.DirectoryServiceGateway$CreateLdapEntryCommand</CommandClass>
            </Consumer>
        </Messaging>

        <LDAP>
            <ProviderUrl>ldap://your-ldap-server:389</ProviderUrl>
            <BaseDN>ou=People,dc=example,dc=com</BaseDN>
            <AdminUser>cn=admin,dc=example,dc=com</AdminUser>
            <AdminPassword>password</AdminPassword>
            <SecurityAuthentication>simple</SecurityAuthentication>
        </LDAP>
        
        <Logging>
            <LogLevel>INFO</LogLevel>
            <LogFile>logs/directory_service.log</LogFile>
        </Logging>
    </Service>
</AppConfig>
```
I’ve generated an AppConfig XML document to configure the Directory Service Gateway, including messaging, LDAP settings, and logging. Let me know if you need any adjustments! 🚀