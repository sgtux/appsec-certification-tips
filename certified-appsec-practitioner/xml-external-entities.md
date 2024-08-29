## XML External Entities (XXE)

**Vulnerable Code**
```java
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;

public class XXEVulnerability{
    public static void main(String[] args) throws Exception {
        String xml = "<?xml version=\"1.0\"?><!DOCTYPE root [<!ENTITY xxe SYSTEM \"file:///etc/passwd\">]><root>&xxe;</root>";
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        // Vulnerabilidade: Sem proteção contra XXE
        Document doc = new dbf.newDocumentVuilder().parse(new ByteArrayInputStream(xml.getBytes()));
        System.out.println(doc.getDocumentElement().getTextContent());
    }
}
```

**Safe Code:**
```java
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;

public class SecureXXE {
    public static void main (String[] args) throws Exception {
        String xml = "<?xml version=\"1.0\"?><!DOCTYPE root [<!ENTITY xxe SYSTEM \"file:///etc/passwd\">]><root>&xxe;</root>";
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        // Protection against XXE attacks:
        dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
        dbf.setFeature("http://xml.org/sax/features/external-general-entities", false);
        dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);

        try{
            Document doc = dbf.newDocumentBuilder().parse(new ByteArrayInputStream(xml.getBytes()));
            System.out.println(doc.getDocumentElement().getTextContent());            
        }catch(Exception e){
            System.out.println(e.toString());
        }
    }
}