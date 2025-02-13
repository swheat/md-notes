Prompt: Would you update this example to show what the Directory Service Gateway would look like using the command pattern as a request command running in the context of the MessageConsumerClient?

```Java
import javax.naming.Context;
import javax.naming.NamingException;
import javax.naming.directory.*;
import java.util.Hashtable;
import org.openeai.jms.consumer.MessageConsumerClient;
import org.openeai.jms.command.RequestCommand;
import org.openeai.jms.objects.person.BasicPerson;

public class DirectoryServiceGateway {

    private static final String LDAP_URL = "ldap://your-ldap-server:389";
    private static final String LDAP_BASE_DN = "ou=People,dc=example,dc=com";
    private static final String LDAP_ADMIN_USER = "cn=admin,dc=example,dc=com";
    private static final String LDAP_ADMIN_PASSWORD = "password";
    
    public static void main(String[] args) {
        MessageConsumerClient consumerClient = new MessageConsumerClient("com.erp.Person.BasicPerson.Create-Sync");
        consumerClient.registerCommand(new CreateLdapEntryCommand());
        consumerClient.start();
    }
    
    public static class CreateLdapEntryCommand implements RequestCommand<BasicPerson> {
        
        @Override
        public void execute(BasicPerson person) {
            createLdapEntry(person);
        }
        
        private void createLdapEntry(BasicPerson person) {
            Hashtable<String, String> env = new Hashtable<>();
            env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
            env.put(Context.PROVIDER_URL, LDAP_URL);
            env.put(Context.SECURITY_AUTHENTICATION, "simple");
            env.put(Context.SECURITY_PRINCIPAL, LDAP_ADMIN_USER);
            env.put(Context.SECURITY_CREDENTIALS, LDAP_ADMIN_PASSWORD);
            
            try {
                DirContext ctx = new InitialDirContext(env);
                Attributes attrs = new BasicAttributes(true);
                Attribute objClass = new BasicAttribute("objectClass");
                objClass.add("inetOrgPerson");
                attrs.put(objClass);
                attrs.put("cn", person.getFirstName() + " " + person.getLastName());
                attrs.put("sn", person.getLastName());
                attrs.put("mail", person.getEmail());
                attrs.put("uid", person.getInstitutionalId());
                
                String entryDN = "uid=" + person.getInstitutionalId() + "," + LDAP_BASE_DN;
                ctx.createSubcontext(entryDN, attrs);
                System.out.println("LDAP entry created for: " + person.getFirstName() + " " + person.getLastName());
                ctx.close();
            } catch (NamingException e) {
                e.printStackTrace();
            }
        }
    }
}
```
I've updated the Directory Service Gateway to use the command pattern as a request command running in the context of the MessageConsumerClient. Let me know if you need any additional modifications or refinements! 🚀