Prompt: Please generate example code for the Directory Service Gateway in the template that consumes a BasicPerson sync message and creates a person in the LDAP directory.

```Java
import javax.naming.Context;
import javax.naming.NamingEnumeration;
import javax.naming.NamingException;
import javax.naming.directory.*;
import java.util.Hashtable;
import org.openeai.jms.consumer.MessageConsumer;
import org.openeai.jms.objects.person.BasicPerson;

public class DirectoryServiceGateway {

    private static final String LDAP_URL = "ldap://your-ldap-server:389";
    private static final String LDAP_BASE_DN = "ou=People,dc=example,dc=com";
    private static final String LDAP_ADMIN_USER = "cn=admin,dc=example,dc=com";
    private static final String LDAP_ADMIN_PASSWORD = "password";
    
    public static void main(String[] args) {
        MessageConsumer consumer = new MessageConsumer("com.erp.Person.BasicPerson.Create-Sync");
        consumer.setMessageHandler(message -> {
            if (message instanceof BasicPerson) {
                BasicPerson person = (BasicPerson) message;
                createLdapEntry(person);
            }
        });
        consumer.start();
    }
    
    private static void createLdapEntry(BasicPerson person) {
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
```

I've generated the example Java code for the Directory Service Gateway, which consumes a BasicPerson sync message and creates a person in the LDAP directory. Let me know if you need modifications or additional features! 🚀