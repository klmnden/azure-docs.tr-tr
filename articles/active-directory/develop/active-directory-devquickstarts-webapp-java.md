---
title: "Azure AD Java web uygulaması Başlarken | Microsoft Docs"
description: "Kullanıcılar bir iş veya Okul hesabıyla oturum açtığında bir Java web uygulaması oluşturun."
services: active-directory
documentationcenter: java
author: navyasric
manager: mtillman
editor: 
ms.assetid: 2b92b605-9cd5-4b99-bcbb-66c026558119
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 281ce23d7fdb1a94bcc7389f3326d45a9b4e99b5
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="azure-ad-java-web-app-getting-started"></a>Azure AD Java web uygulaması Başlarken
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Yalnızca birkaç kod satırıyla, tek bir oturum açma ve oturum kapatma sağlayarak, Azure Active Directory (Azure AD), web uygulaması kimlik yönetimi dış için kolaylaştırır. Java (ADAL4J) için topluluk odaklı Azure Active Directory kimlik doğrulama kitaplığı Microsoft uyarlamasını kullanarak Java web uygulamaları ve kullanıcıların imzalayabilirsiniz.

Bu makalede ADAL4J kullanmayı gösterir:

* Kullanıcıların kimlik sağlayıcısı olarak Azure AD kullanarak web uygulamaları için oturum açın.
* Bazı kullanıcı bilgileri görüntüler.
* Kullanıcıların uygulamaları dışında oturum açın.

## <a name="before-you-get-started"></a>Başlamadan önce

* Karşıdan [uygulama çatıyı](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip), veya indirme [tamamlanan örnek](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).
* Ayrıca uygulamanın kaydedileceği Azure AD kiracısı gerekir. Azure AD kiracısı, zaten yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

Hazır olduğunuzda, sonraki dokuz bölümlerdeki yordamları izleyin.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>1. adım: Azure AD ile yeni uygulamayı Kaydet
Kullanıcıların kimliğini doğrulamak için önce aşağıdakileri yaparak kiracınızda kaydetmeniz uygulama ayarlamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızın adını tıklatın. Altında **Directory** listesinde, uygulama kaydetmek istediğiniz Active Directory Kiracı seçin.
3. Tıklatın **tüm hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Oluşturmak için istemleri izleyerek bir **Web uygulaması ve/veya Webapı**.
  * **Ad** kullanıcılara uygulamasının açıklar.
  * **Oturum açma URL'si** uygulamasının temel URL'si. Çatıyı ait varsayılan http://localhost: 8080/adal4jsample/URL'dir.
6. Kayıt tamamladıktan sonra Azure AD uygulama benzersiz uygulama kimliği atar. Sonraki bölümlerde kullanmak için uygulama sayfasında değerini kopyalayın.
7. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. **Uygulama kimliği URI'si** uygulama için benzersiz bir tanımlayıcıdır. Adlandırma kuralı `https://<tenant-domain>/<app-name>` (örneğin, `http://localhost:8080/adal4jsample/`).

Uygulama için Portalı'nda olduğunda, oluşturma ve uygulama için bir anahtar kopyalayın **ayarları** sayfası. Kısa bir süre sonra bu anahtar gerekir.

## <a name="step-2-set-up-the-app-to-use-the-adal4j-and-prerequisites-by-using-maven"></a>2. adım: Maven kullanarak ADAL4J ve önkoşulları kullanmak uygulama ayarlama
Bu adımda Openıd Connect kimlik doğrulama protokolünü kullanacak biçimde ADAL4J yapılandırın. Oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcı oturumlarını yönetmek, kullanıcı bilgilerini almak ve benzeri için ADAL4J kullanın.

Proje kök dizininde açma/oluşturma `pom.xml`, bulun `// TODO: provide dependencies for Maven`ve şununla değiştirin:

```Java

    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4jsample</artifactId>
    <packaging>war</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>adal4jsample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.1</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
        <!-- Spring 3 dependencies -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>sample-for-adal4j</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <warName>${project.artifactId}</warName>
                    <source>${project.basedir}\src</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>utf-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

    </project>
```

## <a name="step-3-create-the-java-web-app-files-web-inf"></a>3. adım: Java web uygulama dosyaları (WEB-INF) oluşturma
Bu adımda, Java web uygulaması Openıd Connect kimlik doğrulama protokolünü kullanacak biçimde yapılandırın. Oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcının oturum yönetmek, kullanıcı hakkında bilgi almak ve benzeri için ADAL4J kullanın.

1. \Webapp\WEB-INF altında bulunan web.xml dosyasını açın\, ve XML'de uygulama yapılandırma değerlerini girin. XML dosyası aşağıdaki kodu içermelidir:

    ```xml

    <?xml version="1.0"?>
    <web-app id="WebApp_ID" version="2.4"
        xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
        <display-name>Archetype Created Web Application</display-name>
        <context-param>
            <param-name>authority</param-name>
            <param-value>https://login.microsoftonline.com/</param-value>
        </context-param>
        <context-param>
            <param-name>tenant</param-name>
            <param-value>YOUR_TENANT_NAME</param-value>
        </context-param>
        <filter>
            <filter-name>BasicFilter</filter-name>
            <filter-class>com.microsoft.aad.adal4jsample.BasicFilter</filter-class>
            <init-param>
                <param-name>client_id</param-name>
                <param-value>YOUR_CLIENT_ID</param-value>
            </init-param>
            <init-param>
                <param-name>secret_key</param-name>
                <param-value>YOUR_CLIENT_SECRET</param-value>
            </init-param>
        </filter>
        <filter-mapping>
            <filter-name>BasicFilter</filter-name>
            <url-pattern>/secure/*</url-pattern>
        </filter-mapping>
        <servlet>
            <servlet-name>mvc-dispatcher</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>mvc-dispatcher</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
        </context-param>
        <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
        </listener>
    </web-app>
    ```

 * YOUR_CLIENT_ID olan **uygulama kimliği** kayıt portalında uygulamanıza atanan.
 * YOUR_CLIENT_SECRET olan **uygulama gizli anahtarı** portalında oluşturduğunuz.
 * YOUR_TENANT_NAME olan **Kiracı adı** uygulamanızın (örneğin, contoso.onmicrosoft.com).

 XML dosyasında gördüğünüz BasicFilter ziyaret ettiğiniz her kullanan mvc dağıtıcı olarak adlandırılan bir JavaServer sayfaları (JSP) veya Java Servlet web uygulaması yazıyorsanız / URL güvenli. Aynı kodu kullan / korumalı içeriği ve Azure ad kimlik doğrulama zorlamak için bir yer olarak güvenli.

2. \Webapp\WEB-INF altında bulunan mvc dağıtıcısı servlet.xml dosyası oluşturma\, ve aşağıdaki kodu girin:

    ```xml

    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="
            http://www.springframework.org/schema/beans     
            http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context-3.0.xsd">
        <context:component-scan base-package="com.microsoft.aad.adal4jsample" />
        <bean
            class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix">
                <value>/</value>
            </property>
            <property name="suffix">
                <value>.jsp</value>
            </property>
        </bean>
    </beans>
    ```

 Bu kod yay kullanmak için web uygulaması bildirir ve sonraki bölümde yazma JSP dosyası nerede bulacağını gösterir.

## <a name="step-4-create-the-jsp-view-files-for-basicfilter-mvc"></a>4. adım: dosyaları JSP görüntüle (BasicFilter MVC için) oluşturma
Yarı çift yönlü aracılığıyla web uygulamanızı WEB INF dosyasında ayarlama. Ardından, web uygulaması yürüten BasicFilter model görünümü denetleyici (MVC) JSP dosyaları oluşturun. Yapılandırma sırasında dosyaları oluşturma sırasında İpuçlu.

Daha önce sahip olduğunuz XML yapılandırma dosyalarında Java söylediyse bir `/` JSP dosyaları ve yükler kaynağa sahip bir `/secure` BasicFilter olarak adlandırılan bir filtre geçirir kaynak.

JSP dosyaları oluşturmak için aşağıdakileri yapın:

1. İndex.jsp dosyası oluşturun (\webapp altında bulunan\)ve ardından aşağıdaki kodu yapıştırın:

    ```jsp
    <html>
    <body>
        <h2>Hello World!</h2>
        <ul>
        <li><a href="secure/aad">Secure Page</a></li>
        </ul>
    </body>
    </html>
    ```

 Bu kod yalnızca filtre tarafından korunan güvenli bir sayfa yönlendirir.

2. Aynı dizinde gerçekleşebilir hataları yakalamak için bir error.jsp dosyası oluşturun:

    ```jsp

    <html>
    <body>
        <h2>ERROR PAGE!</h2>
        <p>
            Exception -
            <%=request.getAttribute("error")%></p>
        <ul>
            <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
        </ul>
    </body>
    </html>
    ```
3. Güvenli Web sayfası yapmak için dizin şimdi \webapp\secure böylece \secure adlı \webapp altında bir klasör oluşturun.
4. \Webapp\secure dizininde bir aad.jsp dosyası oluşturun ve sonra aşağıdaki kodu yapıştırın:

    ```jsp

    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>AAD Secure Page</title>
        </head>
        <body>

        <h1>Directory - Users List</h1>
        <p>${users}</p>
        <ul>
            <li><a href="<%=request.getContextPath()%>/secure/aad?cc=1">Get new Access Token via Client Credentials</a></li>
        </ul>
        <ul>
            <li><a href="<%=request.getContextPath()%>/secure/aad?refresh=1">Get new Access Token via Refresh Token</a></li>
        </ul>
        <ul>
            <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
        </ul>
        </body>
    </html>
    ```

    Bu sayfa BasicFilter servlet'i okur ve ardından üzerinde ADAJ4J kullanarak yürütür belirli isteklere yönlendirir.

Şimdi servlet'i işini yapabilmesi için Java dosyaları ayarlamanız gerekir.

## <a name="step-5-create-some-java-helper-files-for-basicfilter-mvc"></a>5. adım: (BasicFilter MVC için) bazı Java yardımcı dosyalar oluşturma
Bu adımda Amacımız Java oluşturmak için dosyaları verilmiştir:

* Oturum açma ve oturum kapatma kullanıcının izin verir.
* Kullanıcı hakkında bazı verileri alın.

    > [!NOTE]
    > Kullanıcı ilişkin veri almak için Azure AD'den grafik API'sini kullanın. Graph, tek tek kullanıcılar dahil olmak üzere, Kurumunuz hakkında bilgi edinmek için kullanabileceğiniz güvenli bir Web hizmeti API'dir. Bu yaklaşım belirteçleri, gizli verileri önceden doldurma daha iyi olduğundan, sağlar:
    > * Verileri isteyin kullanıcıları yetkisine sahiptir.
    > * Belirtecin (önbelleğinden bir işletim sistemi kısıtlamaları kaldırılmış telefon ya da web tarayıcısı bir masaüstünde, örneğin) şablonlarınızdan gerçekleşebilir herkes kullanıcı veya kuruluş hakkında önemli ayrıntıları alınamıyor.

Bu iş için bazı Java dosyaları yazmak için:

1. Kök dizininizde tüm Java dosyalarını depolamak için adal4jsample adlı bir klasör oluşturun.

    Bu örnekte, Java dosyalarında ad alanı com.microsoft.aad.adal4jsample kullanıyor. Çoğu IDE bu amaç için (örneğin, /com/microsoft/aad/adal4jsample) bir iç içe bir klasör yapısı oluşturun. Bu da yapabilirsiniz, ancak gerekli değildir.

2. Bu klasörün içinde belirteçlerinizi JSON verilerini ayrıştırma yardımcı olmak için kullanacağız JSONHelper.java adlı bir dosya oluşturun. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.lang.reflect.Field;
    import java.util.Arrays;
    import java.util.Enumeration;
    import java.util.List;

    import javax.servlet.http.HttpServletRequest;

    import org.apache.commons.lang3.text.WordUtils;
    import org.apache.log4j.Logger;
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;

    /**
     * This class provides the methods to parse JSON data from a JSON-formatted
     * string.
     *
     * @author Azure Active Directory contributor
     *
     */
    public class JSONHelper {

        private static Logger logger = Logger.getLogger(JSONHelper.class);

        JSONHelper() {
            // PropertyConfigurator.configure("log4j.properties");
        }

        /**
         * This method parses a JSON array out of a collection of JSON objects
         * within a string.
         *
         * @param jSonData
         *            The JSON string that holds the collection
         * @return A JSON array that contains all the collection objects
         * @throws Exception
         */
        public static JSONArray fetchDirectoryObjectJSONArray(JSONObject jsonObject) throws Exception {
            JSONArray jsonArray = new JSONArray();
            jsonArray = jsonObject.optJSONObject("responseMsg").optJSONArray("value");
            return jsonArray;
        }

        /**
         * This method parses a JSON object out of a collection of JSON objects
         * within a string.
         *
         * @param jsonObject
         * @return A JSON object that contains the DirectoryObject
         * @throws Exception
         */
        public static JSONObject fetchDirectoryObjectJSONObject(JSONObject jsonObject) throws Exception {
            JSONObject jObj = new JSONObject();
            jObj = jsonObject.optJSONObject("responseMsg");
            return jObj;
        }

        /**
         * This method parses the skip token from a JSON-formatted string.
         *
         * @param jsonData
         *            The JSON-formatted string
         * @return The skipToken
         * @throws Exception
         */
        public static String fetchNextSkiptoken(JSONObject jsonObject) throws Exception {
            String skipToken = "";
            // Parse the skip token out of the string.
            skipToken = jsonObject.optJSONObject("responseMsg").optString("odata.nextLink");

            if (!skipToken.equalsIgnoreCase("")) {
                // Remove the unnecessary prefix from the skip token.
                int index = skipToken.indexOf("$skiptoken=") + (new String("$skiptoken=")).length();
                skipToken = skipToken.substring(index);
            }
            return skipToken;
        }

        /**
         * @param jsonObject
         * @return
         * @throws Exception
         */
        public static String fetchDeltaLink(JSONObject jsonObject) throws Exception {
            String deltaLink = "";
            // Parse the skip token out of the string.
            deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.deltaLink");
            if (deltaLink == null || deltaLink.length() == 0) {
                deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.nextLink");
                logger.info("deltaLink empty, nextLink ->" + deltaLink);

            }
            if (!deltaLink.equalsIgnoreCase("")) {
                // Remove the unnecessary prefix from the skip token.
                int index = deltaLink.indexOf("deltaLink=") + (new String("deltaLink=")).length();
                deltaLink = deltaLink.substring(index);
            }
            return deltaLink;
        }

        /**
         * This method creates a string consisting of a JSON document with all
         * the necessary elements set from the HttpServletRequest request.
         *
         * @param request
         *            The HttpServletRequest
         * @return The string containing the JSON document
         * @throws Exception
         *             If there is any error processing the request.
         */
        public static String createJSONString(HttpServletRequest request, String controller) throws Exception {
            JSONObject obj = new JSONObject();
            try {
                Field[] allFields = Class.forName(
                        "com.microsoft.windowsazure.activedirectory.sdk.graph.models." + controller).getDeclaredFields();
                String[] allFieldStr = new String[allFields.length];
                for (int i = 0; i < allFields.length; i++) {
                    allFieldStr[i] = allFields[i].getName();
                }
                List<String> allFieldStringList = Arrays.asList(allFieldStr);
                Enumeration<String> fields = request.getParameterNames();

                while (fields.hasMoreElements()) {

                    String fieldName = fields.nextElement();
                    String param = request.getParameter(fieldName);
                    if (allFieldStringList.contains(fieldName)) {
                        if (param == null || param.length() == 0) {
                            if (!fieldName.equalsIgnoreCase("password")) {
                                obj.put(fieldName, JSONObject.NULL);
                            }
                        } else {
                            if (fieldName.equalsIgnoreCase("password")) {
                                obj.put("passwordProfile", new JSONObject("{\"password\": \"" + param + "\"}"));
                            } else {
                                obj.put(fieldName, param);
                            }
                        }
                    }
                }
            } catch (JSONException e) {
                e.printStackTrace();
            } catch (SecurityException e) {
                e.printStackTrace();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }            
            return obj.toString();
        }

        /**
         *
         * @param key
         * @param value
         * @return string format of this JSON object
         * @throws Exception
         */
        public static String createJSONString(String key, String value) throws Exception {

            JSONObject obj = new JSONObject();
            try {
                obj.put(key, value);
            } catch (JSONException e) {
                e.printStackTrace();
            }

            return obj.toString();
        }

        /**
         * This is a generic method that copies the simple attribute values from an
         * argument jsonObject to an argument generic object.
         *
         * @param jsonObject
         *            The jsonObject from where the attributes are to be copied.
         * @param destObject
         *            The object where the attributes should be copied to.
         * @throws Exception
         *             Throws an Exception when the operation is unsuccessful.
         */
        public static <T> void convertJSONObjectToDirectoryObject(JSONObject jsonObject, T destObject) throws Exception {

            // Get the list of all the field names.
            Field[] fieldList = destObject.getClass().getDeclaredFields();

            // For all the declared field.
            for (int i = 0; i < fieldList.length; i++) {
                // If the field is of type String, that is
                // if it is a simple attribute.
                if (fieldList[i].getType().equals(String.class)) {
                    // Invoke the corresponding set method of the destObject using
                    // the argument taken from the jsonObject.
                    destObject
                            .getClass()
                            .getMethod(String.format("set%s", WordUtils.capitalize(fieldList[i].getName())),
                                    new Class[] { String.class })
                            .invoke(destObject, new Object[] { jsonObject.optString(fieldList[i].getName()) });
                }
            }
        }

        public static JSONArray joinJSONArrays(JSONArray a, JSONArray b) {
            JSONArray comb = new JSONArray();
            for (int i = 0; i < a.length(); i++) {
                comb.put(a.optJSONObject(i));
            }
            for (int i = 0; i < b.length(); i++) {
                comb.put(b.optJSONObject(i));
            }
            return comb;
        }

    }

    ```

3. Azure AD uç noktanızı HTTP verilerden ayrıştırma yardımcı olmak için kullanacağınız HttpClientHelper.java adlı bir dosya oluşturun. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.io.BufferedReader;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.io.OutputStreamWriter;
    import java.net.HttpURLConnection;

    import org.json.JSONException;
    import org.json.JSONObject;

    /**
     * This is Helper class for all RestClient class.
     *
     * @author Azure Active Directory Contributor
     *
     */
    public class HttpClientHelper {

        public HttpClientHelper() {
            super();
        }

        public static String getResponseStringFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

            BufferedReader reader = null;
            if (isSuccess) {
                reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            } else {
                reader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
            }
            StringBuffer stringBuffer = new StringBuffer();
            String line = "";
            while ((line = reader.readLine()) != null) {
                stringBuffer.append(line);
            }

            return stringBuffer.toString();
        }

        public static String getResponseStringFromConn(HttpURLConnection conn, String payLoad) throws IOException {

            // Send the http message payload to the server.
            if (payLoad != null) {
                conn.setDoOutput(true);
                OutputStreamWriter osw = new OutputStreamWriter(conn.getOutputStream());
                osw.write(payLoad);
                osw.flush();
                osw.close();
            }

            // Get the message response from the server.
            BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String line = "";
            StringBuffer stringBuffer = new StringBuffer();
            while ((line = br.readLine()) != null) {
                stringBuffer.append(line);
            }

            br.close();

            return stringBuffer.toString();
        }

        public static byte[] getByteaArrayFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

            InputStream is = conn.getInputStream();
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            byte[] buff = new byte[1024];
            int bytesRead = 0;

            while ((bytesRead = is.read(buff, 0, buff.length)) != -1) {
                baos.write(buff, 0, bytesRead);
            }

            byte[] bytes = baos.toByteArray();
            baos.close();
            return bytes;
        }

        /**
         * for bad response, whose responseCode is not 200 level
         *
         * @param responseCode
         * @param errorCode
         * @param errorMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processResponse(int responseCode, String errorCode, String errorMsg) throws JSONException {
            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            response.put("errorCode", errorCode);
            response.put("errorMsg", errorMsg);

            return response;
        }

        /**
         * for bad response, whose responseCode is not 200 level
         *
         * @param responseCode
         * @param errorCode
         * @param errorMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processGoodRespStr(int responseCode, String goodRespStr) throws JSONException {
            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            if (goodRespStr.equalsIgnoreCase("")) {
                response.put("responseMsg", "");
            } else {
                response.put("responseMsg", new JSONObject(goodRespStr));
            }

            return response;
        }

        /**
         * for good response
         *
         * @param responseCode
         * @param responseMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processBadRespStr(int responseCode, String responseMsg) throws JSONException {

            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            if (responseMsg.equalsIgnoreCase("")) { // good response is empty string
                response.put("responseMsg", "");
            } else { // bad response is json string
                JSONObject errorObject = new JSONObject(responseMsg).optJSONObject("odata.error");

                String errorCode = errorObject.optString("code");
                String errorMsg = errorObject.optJSONObject("message").optString("value");
                response.put("responseCode", responseCode);
                response.put("errorCode", errorCode);
                response.put("errorMsg", errorMsg);
            }

            return response;
        }

    }

    ```

## <a name="step-6-create-the-java-graph-api-model-files-for-basicfilter-mvc"></a>6. adım: Java Graph API Model dosyalarında (BasicFilter MVC için) oluşturun.
Daha önce belirtildiği gibi oturum açmış kullanıcı ilişkin veri almak için grafik API'sini kullanın. Bu işlemi kolaylaştırmak için dizin nesnesini temsil eden bir dosya ve böylece Java OO desenini kullanılabilir kullanıcıyı temsil etmek için bir dosya oluşturun.

1. Herhangi bir dizin nesnesi hakkındaki temel verileri depolamak için kullandığınız DirectoryObject.java adlı bir dosya oluşturun. Bu dosya daha sonra gerçekleştirebileceğiniz diğer tüm grafik sorgular için kullanabilirsiniz. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java

    package com.microsoft.aad.adal4jsample;

    /**
     * @author Azure Active Directory Contributor
     *
     */
    public abstract class DirectoryObject {

        public DirectoryObject() {
            super();
        }

        /**
         *
         * @return
         */
        public abstract String getObjectId();

        /**
         * @param objectId
         */
        public abstract void setObjectId(String objectId);

        /**
         *
         * @return
         */
        public abstract String getObjectType();

        /**
         *
         * @param objectType
         */
        public abstract void setObjectType(String objectType);

        /**
         *
         * @return
         */
        public abstract String getDisplayName();

        /**
         *
         * @param displayName
         */
        public abstract void setDisplayName(String displayName);

    }

    ```

2. Dizininden herhangi bir kullanıcı hakkındaki temel verileri depolamak için kullandığınız User.java adlı bir dosya oluşturun. Aşağıdaki kodu yapıştırın şekilde dizin verilerini temel alıcı ve ayarlayıcıya yöntemler şunlardır:

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.security.acl.Group;
    import java.util.ArrayList;

    import javax.xml.bind.annotation.XmlRootElement;

    import org.json.JSONObject;

    /**
    *  The **User** class holds together all the members of a WAAD User entity and all the access methods and set methods.
    *  @author Azure Active Directory Contributor
    */
    @XmlRootElement
    public class User extends DirectoryObject{

        // The following are the individual private members of a User object that holds
        // a particular simple attribute of a User object.
        protected String objectId;
        protected String objectType;
        protected String accountEnabled;
        protected String city;
        protected String country;
        protected String department;
        protected String dirSyncEnabled;
        protected String displayName;
        protected String facsimileTelephoneNumber;
        protected String givenName;
        protected String jobTitle;
        protected String lastDirSyncTime;
        protected String mail;
        protected String mailNickname;
        protected String mobile;
        protected String password;
        protected String passwordPolicies;
        protected String physicalDeliveryOfficeName;
        protected String postalCode;
        protected String preferredLanguage;
        protected String state;
        protected String streetAddress;
        protected String surname;
        protected String telephoneNumber;
        protected String usageLocation;
        protected String userPrincipalName;
        protected boolean isDeleted;  // this will move to dto

        /**
         * These four properties are for future use.
         */
        // managerDisplayname of this user.
        protected String managerDisplayname;

        // The directReports holds a list of directReports.
        private ArrayList<User> directReports;

        // The groups holds a list of group entities this user belongs to.
        private ArrayList<Group> groups;

        // The roles holds a list of role entities this user belongs to.
        private ArrayList<Group> roles;

        /**
         * The constructor for the **User** class. Initializes the dynamic lists and managerDisplayname variables.
         */
        public User(){
            directReports = null;
            groups = new ArrayList<Group>();
            roles = new ArrayList<Group>();
            managerDisplayname = null;
        }
    //    
    //    public User(String displayName, String objectId){
    //        setDisplayName(displayName);
    //        setObjectId(objectId);
    //    }
    //    
    //    public User(String displayName, String objectId, String userPrincipalName, String accountEnabled){
    //        setDisplayName(displayName);
    //        setObjectId(objectId);
    //        setUserPrincipalName(userPrincipalName);
    //        setAccountEnabled(accountEnabled);
    //    }
    //    

        /**
         * @return The objectId of this user.
         */
        public String getObjectId() {
            return objectId;
        }

        /**
         * @param objectId The objectId to set to this User object.
         */
        public void setObjectId(String objectId) {
            this.objectId = objectId;
        }

        /**
         * @return The objectType of this user.
         */
        public String getObjectType() {
            return objectType;
        }

        /**
         * @param objectType The objectType to set to this User object.
         */
        public void setObjectType(String objectType) {
            this.objectType = objectType;
        }

        /**
         * @return The userPrincipalName of this user.
         */
        public String getUserPrincipalName() {
            return userPrincipalName;
        }

        /**
         * @param userPrincipalName The userPrincipalName to set to this User object.
         */
        public void setUserPrincipalName(String userPrincipalName) {
            this.userPrincipalName = userPrincipalName;
        }

        /**
         * @return The usageLocation of this user.
         */
        public String getUsageLocation() {
            return usageLocation;
        }

        /**
         * @param usageLocation The usageLocation to set to this User object.
         */
        public void setUsageLocation(String usageLocation) {
            this.usageLocation = usageLocation;
        }

        /**
         * @return The telephoneNumber of this user.
         */
        public String getTelephoneNumber() {
            return telephoneNumber;
        }

        /**
         * @param telephoneNumber The telephoneNumber to set to this User object.
         */
        public void setTelephoneNumber(String telephoneNumber) {
            this.telephoneNumber = telephoneNumber;
        }

        /**
         * @return The surname of this user.
         */
        public String getSurname() {
            return surname;
        }

        /**
         * @param surname The surname to set to this User object.
         */
        public void setSurname(String surname) {
            this.surname = surname;
        }

        /**
         * @return The streetAddress of this user.
         */
        public String getStreetAddress() {
            return streetAddress;
        }

        /**
         * @param streetAddress The streetAddress to set to this user.
         */
        public void setStreetAddress(String streetAddress) {
            this.streetAddress = streetAddress;
        }

        /**
         * @return The state of this user.
         */
        public String getState() {
            return state;
        }

        /**
         * @param state The state to set to this User object.
         */
        public void setState(String state) {
            this.state = state;
        }

        /**
         * @return The preferredLanguage of this user.
         */
        public String getPreferredLanguage() {
            return preferredLanguage;
        }

        /**
         * @param preferredLanguage The preferredLanguage to set to this user.
         */
        public void setPreferredLanguage(String preferredLanguage) {
            this.preferredLanguage = preferredLanguage;
        }

        /**
         * @return The postalCode of this user.
         */
        public String getPostalCode() {
            return postalCode;
        }

        /**
         * @param postalCode The postalCode to set to this user.
         */
        public void setPostalCode(String postalCode) {
            this.postalCode = postalCode;
        }

        /**
         * @return The physicalDeliveryOfficeName of this user.
         */
        public String getPhysicalDeliveryOfficeName() {
            return physicalDeliveryOfficeName;
        }

        /**
         * @param physicalDeliveryOfficeName The physicalDeliveryOfficeName to set to this User object.
         */
        public void setPhysicalDeliveryOfficeName(String physicalDeliveryOfficeName) {
            this.physicalDeliveryOfficeName = physicalDeliveryOfficeName;
        }

        /**
         * @return The passwordPolicies of this user.
         */
        public String getPasswordPolicies() {
            return passwordPolicies;
        }

        /**
         * @param passwordPolicies The passwordPolicies to set to this User object.
         */
        public void setPasswordPolicies(String passwordPolicies) {
            this.passwordPolicies = passwordPolicies;
        }

        /**
         * @return The mobile of this user.
         */
        public String getMobile() {
            return mobile;
        }

        /**
         * @param mobile The mobile to set to this User object.
         */
        public void setMobile(String mobile) {
            this.mobile = mobile;
        }

        /**
         * @return The password of this user.
         */
        public String getPassword() {
            return password;
        }

        /**
         * @param password The mobile to set to this User object.
         */
        public void setPassword(String password) {
            this.password = password;
        }

        /**
         * @return The mail of this user.
         */
        public String getMail() {
            return mail;
        }

        /**
         * @param mail The mail to set to this User object.
         */
        public void setMail(String mail) {
            this.mail = mail;
        }

        /**
         * @return The MailNickname of this user.
         */
        public String getMailNickname() {
            return mailNickname;
        }

        /**
         * @param mail The MailNickname to set to this User object.
         */
        public void setMailNickname(String mailNickname) {
            this.mailNickname = mailNickname;
        }

        /**
         * @return The jobTitle of this user.
         */
        public String getJobTitle() {
            return jobTitle;
        }

        /**
         * @param jobTitle The jobTitle to set to this User object.
         */
        public void setJobTitle(String jobTitle) {
            this.jobTitle = jobTitle;
        }

        /**
         * @return The givenName of this user.
         */
        public String getGivenName() {
            return givenName;
        }

        /**
         * @param givenName The givenName to set to this User object.
         */
        public void setGivenName(String givenName) {
            this.givenName = givenName;
        }

        /**
         * @return The facsimileTelephoneNumber of this user.
         */
        public String getFacsimileTelephoneNumber() {
            return facsimileTelephoneNumber;
        }

        /**
         * @param facsimileTelephoneNumber The facsimileTelephoneNumber to set to this User object.
         */
        public void setFacsimileTelephoneNumber(String facsimileTelephoneNumber) {
            this.facsimileTelephoneNumber = facsimileTelephoneNumber;
        }

        /**
         * @return The displayName of this user.
         */
        public String getDisplayName() {
            return displayName;
        }

        /**
         * @param displayName The displayName to set to this User object.
         */
        public void setDisplayName(String displayName) {
            this.displayName = displayName;
        }

        /**
         * @return The dirSyncEnabled of this user.
         */
        public String getDirSyncEnabled() {
            return dirSyncEnabled;
        }

        /**
         * @param dirSyncEnabled The dirSyncEnabled to set to this User object.
         */
        public void setDirSyncEnabled(String dirSyncEnabled) {
            this.dirSyncEnabled = dirSyncEnabled;
        }

        /**
         * @return The department of this user.
         */
        public String getDepartment() {
            return department;
        }

        /**
         * @param department The department to set to this User object.
         */
        public void setDepartment(String department) {
            this.department = department;
        }

        /**
         * @return The lastDirSyncTime of this user.
         */
        public String getLastDirSyncTime() {
            return lastDirSyncTime;
        }

        /**
         * @param lastDirSyncTime The lastDirSyncTime to set to this User object.
         */
        public void setLastDirSyncTime(String lastDirSyncTime) {
            this.lastDirSyncTime = lastDirSyncTime;
        }

        /**
         * @return The country of this user.
         */
        public String getCountry() {
            return country;
        }

        /**
         * @param country The country to set to this user.
         */
        public void setCountry(String country) {
            this.country = country;
        }

        /**
         * @return The city of this user.
         */
        public String getCity() {
            return city;
        }

        /**
         * @param city The city to set to this user.
         */
        public void setCity(String city) {
            this.city = city;
        }

        /**
         * @return The accountEnabled attribute of this user.
         */
        public String getAccountEnabled() {
            return accountEnabled;
        }

        /**
         * @param accountEnabled The accountEnabled to set to this user.
         */
        public void setAccountEnabled(String accountEnabled) {
            this.accountEnabled = accountEnabled;
        }

        public boolean isIsDeleted() {
            return this.isDeleted;
        }

        public void setIsDeleted(boolean isDeleted) {
            this.isDeleted = isDeleted;
        }

        @Override
        public String toString() {
            return new JSONObject(this).toString();
        }

        public String getManagerDisplayname(){
            return managerDisplayname;
        }

        public void setManagerDisplayname(String managerDisplayname){
            this.managerDisplayname = managerDisplayname;
        }
    }

    /**
    * The DirectReports class holds the essential data for a single DirectReport entry. That is,
    * it holds the displayName and the objectId of the direct entry. It also provides the
    * access methods to set or get the displayName and the ObjectId of this entry.
    */
    //class DirectReport extends User{
    //
    //    private String displayName;
    //    private String objectId;
    //     
    //    /**
    //     * Two arguments Constructor for the DirectReport class.
    //     * @param displayName
    //     * @param objectId
    //     */
    //    public DirectReport(String displayName, String objectId){
    //        this.displayName = displayName;
    //        this.objectId = objectId;
    //    }
    //
    //    /**
    //     * @return The displayName of this direct report entry.
    //     */
    //    public String getDisplayName() {
    //        return displayName;
    //    }
    //
    //    
    //    /**
    //     *  @return The objectId of this direct report entry.
    //     */
    //    public String getObjectId() {
    //        return objectId;
    //    }
    //
    //}

    ```

## <a name="step-7-create-the-authentication-model-and-controller-files-for-basicfilter"></a>7. adım: kimlik doğrulaması model ve denetleyici dosyaları (BasicFilter için) oluşturma
Biz Java ayrıntılı olabilir, ancak bitmek üzere kabul ediyorsunuz. İstekleri işlemek üzere BasicFilter servlet'i yazmadan önce ADAL4J gereken bazı daha fazla yardımcı dosyaları yazmanız gerekir.

1. Oturum açmış kullanıcı durumunu belirlemek için kullanılacak yöntemleri erişmenizi sağlayan AuthHelper.java adlı bir dosya oluşturun. Yöntemler şunlardır:

 * **isAuthenticated()**: kullanıcı oturumu olup olmadığını döndürür.
 * **containsAuthenticationData()**: Returns whether the token has data.
 * **isAuthenticationSuccessful()**: Returns whether the authentication was successful for the user.

 AuthHelper.java dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java
    package com.microsoft.aad.adal4jsample;

    import java.util.Map;

    import javax.servlet.http.HttpServletRequest;

    import com.microsoft.aad.adal4j.AuthenticationResult;
    import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
    import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
    import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

    public final class AuthHelper {

        public static final String PRINCIPAL_SESSION_NAME = "principal";

        private AuthHelper() {
        }

        public static boolean isAuthenticated(HttpServletRequest request) {
            return request.getSession().getAttribute(PRINCIPAL_SESSION_NAME) != null;
        }

        public static AuthenticationResult getAuthSessionObject(
                HttpServletRequest request) {
            return (AuthenticationResult) request.getSession().getAttribute(
                    PRINCIPAL_SESSION_NAME);
        }

        public static boolean containsAuthenticationData(
                HttpServletRequest httpRequest) {
            Map<String, String[]> map = httpRequest.getParameterMap();
            return httpRequest.getMethod().equalsIgnoreCase("POST") && (httpRequest.getParameterMap().containsKey(
                            AuthParameterNames.ERROR)
                            || httpRequest.getParameterMap().containsKey(
                                    AuthParameterNames.ID_TOKEN) || httpRequest
                            .getParameterMap().containsKey(AuthParameterNames.CODE));
        }

        public static boolean isAuthenticationSuccessful(
                AuthenticationResponse authResponse) {
            return authResponse instanceof AuthenticationSuccessResponse;
        }
    }
    ```

2. ADAL4J gerektiren bazı değişmez değişkenler verir AuthParameterNames.java adlı bir dosya oluşturun. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java
    package com.microsoft.aad.adal4jsample;

    public final class AuthParameterNames {

        private AuthParameterNames() {
        }

        public static String ERROR = "error";
        public static String ERROR_DESCRIPTION = "error_description";
        public static String ERROR_URI = "error_uri";
        public static String ID_TOKEN = "id_token";
        public static String CODE = "code";
    }
    ```

3. MVC örüntüsü denetleyicisidir AadController.java adlı bir dosya oluşturun. Dosya JSP denetleyicisi sunar ve uygulama için güvenli aad URL uç noktasını kullanıma sunar. Dosyası, ayrıca grafik sorgu içerir. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

    ```Java
    package com.microsoft.aad.adal4jsample;

    import java.net.HttpURLConnection;
    import java.net.URL;

    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpSession;

    import org.json.JSONArray;
    import org.json.JSONObject;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;

    import com.microsoft.aad.adal4j.AuthenticationResult;

    @Controller
    @RequestMapping("/secure/aad")
    public class AadController {

        @RequestMapping(method = { RequestMethod.GET, RequestMethod.POST })
        public String getDirectoryObjects(ModelMap model, HttpServletRequest httpRequest) {
            HttpSession session = httpRequest.getSession();
            AuthenticationResult result = (AuthenticationResult) session.getAttribute(AuthHelper.PRINCIPAL_SESSION_NAME);
            if (result == null) {
                model.addAttribute("error", new Exception("AuthenticationResult not found in session."));
                return "/error";
            } else {
                String data;
                try {
                    data = this.getUsernamesFromGraph(result.getAccessToken(), session.getServletContext()
                            .getInitParameter("tenant"));
                    model.addAttribute("users", data);
                } catch (Exception e) {
                    model.addAttribute("error", e);
                    return "/error";
                }
            }
            return "/secure/aad";
        }

        private String getUsernamesFromGraph(String accessToken, String tenant) throws Exception {
            URL url = new URL(String.format("https://graph.windows.net/%s/users?api-version=2013-04-05", tenant,
                    accessToken));

            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            // Set the appropriate header fields in the request header.
            conn.setRequestProperty("api-version", "2013-04-05");
            conn.setRequestProperty("Authorization", accessToken);
            conn.setRequestProperty("Accept", "application/json;odata=minimalmetadata");
            String goodRespStr = HttpClientHelper.getResponseStringFromConn(conn, true);
            // logger.info("goodRespStr ->" + goodRespStr);
            int responseCode = conn.getResponseCode();
            JSONObject response = HttpClientHelper.processGoodRespStr(responseCode, goodRespStr);
            JSONArray users = new JSONArray();

            users = JSONHelper.fetchDirectoryObjectJSONArray(response);

            StringBuilder builder = new StringBuilder();
            User user = null;
            for (int i = 0; i < users.length(); i++) {
                JSONObject thisUserJSONObject = users.optJSONObject(i);
                user = new User();
                JSONHelper.convertJSONObjectToDirectoryObject(thisUserJSONObject, user);
                builder.append(user.getUserPrincipalName() + "<br/>");
            }
            return builder.toString();
        }

    }

    ```

## <a name="step-8-create-the-basicfilter-file-for-basicfilter-mvc"></a>8. adım: (BasicFilter MVC için) BasicFilter dosyası oluşturma
Artık dosyaları JSP görüntüle gelen istekleri işleyen BasicFilter.java dosyası oluşturabilirsiniz. Dosyası oluşturmak için aşağıdaki kodu yapıştırın:

```Java

package com.microsoft.aad.adal4jsample;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URI;
import java.net.URLEncoder;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;
import com.nimbusds.oauth2.sdk.AuthorizationCode;
import com.nimbusds.openid.connect.sdk.AuthenticationErrorResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public class BasicFilter implements Filter {

    private String clientId = "";
    private String clientSecret = "";
    private String tenant = "";
    private String authority;

    public void destroy() {

    }

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {

        if (request instanceof HttpServletRequest) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            try {

                String currentUri = request.getScheme()
                        + "://"
                        + request.getServerName()
                        + ("http".equals(request.getScheme())
                                && request.getServerPort() == 80
                                || "https".equals(request.getScheme())
                                && request.getServerPort() == 443 ? "" : ":"
                                + request.getServerPort())
                        + httpRequest.getRequestURI();
                String fullUrl = currentUri
                        + (httpRequest.getQueryString() != null ? "?"
                                + httpRequest.getQueryString() : "");
                // check if user has a session
                if (!AuthHelper.isAuthenticated(httpRequest)) {
                    if (AuthHelper.containsAuthenticationData(httpRequest)) {
                        Map<String, String> params = new HashMap<String, String>();
                        for (String key : request.getParameterMap().keySet()) {
                            params.put(key,
                                    request.getParameterMap().get(key)[0]);
                        }
                        AuthenticationResponse authResponse = AuthenticationResponseParser
                                .parse(new URI(fullUrl), params);
                        if (AuthHelper.isAuthenticationSuccessful(authResponse)) {

                            AuthenticationSuccessResponse oidcResponse = (AuthenticationSuccessResponse) authResponse;
                            AuthenticationResult result = getAccessToken(
                                    oidcResponse.getAuthorizationCode(),
                                    currentUri);
                            createSessionPrincipal(httpRequest, result);
                        } else {
                            AuthenticationErrorResponse oidcResponse = (AuthenticationErrorResponse) authResponse;
                            throw new Exception(String.format(
                                    "Request for auth code failed: %s - %s",
                                    oidcResponse.getErrorObject().getCode(),
                                    oidcResponse.getErrorObject()
                                            .getDescription()));
                        }
                    } else {
                            // not authenticated
                            httpResponse.setStatus(302);
                            httpResponse
                                    .sendRedirect(getRedirectUrl(currentUri));
                            return;
                    }
                } else {
                    // if authenticated, how to check for valid session?
                    AuthenticationResult result = AuthHelper
                            .getAuthSessionObject(httpRequest);

                    if (httpRequest.getParameter("refresh") != null) {
                        result = getAccessTokenFromRefreshToken(
                                result.getRefreshToken(), currentUri);
                    } else {
                        if (httpRequest.getParameter("cc") != null) {
                            result = getAccessTokenFromClientCredentials();
                        } else {
                            if (result.getExpiresOnDate().before(new Date())) {
                                result = getAccessTokenFromRefreshToken(
                                        result.getRefreshToken(), currentUri);
                            }
                        }
                    }
                    createSessionPrincipal(httpRequest, result);
                }
            } catch (Throwable exc) {
                httpResponse.setStatus(500);
                request.setAttribute("error", exc.getMessage());
                httpResponse.sendRedirect(((HttpServletRequest) request)
                        .getContextPath() + "/error.jsp");
            }
        }
        chain.doFilter(request, response);
    }

    private AuthenticationResult getAccessTokenFromClientCredentials()
            throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", new ClientCredential(clientId,
                            clientSecret), null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private AuthenticationResult getAccessTokenFromRefreshToken(
            String refreshToken, String currentUri) throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByRefreshToken(refreshToken,
                            new ClientCredential(clientId, clientSecret), null,
                            null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;

    }

    private AuthenticationResult getAccessToken(
            AuthorizationCode authorizationCode, String currentUri)
            throws Throwable {
        String authCode = authorizationCode.getValue();
        ClientCredential credential = new ClientCredential(clientId,
                clientSecret);
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByAuthorizationCode(authCode, new URI(
                            currentUri), credential, null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private void createSessionPrincipal(HttpServletRequest httpRequest,
            AuthenticationResult result) throws Exception {
        httpRequest.getSession().setAttribute(
                AuthHelper.PRINCIPAL_SESSION_NAME, result);
    }

    private String getRedirectUrl(String currentUri)
            throws UnsupportedEncodingException {
        String redirectUrl = authority
                + this.tenant
                + "/oauth2/authorize?response_type=code%20id_token&scope=openid&response_mode=form_post&redirect_uri="
                + URLEncoder.encode(currentUri, "UTF-8") + "&client_id="
                + clientId + "&resource=https%3a%2f%2fgraph.windows.net"
                + "&nonce=" + UUID.randomUUID() + "&site_id=500879";
        return redirectUrl;
    }

    public void init(FilterConfig config) throws ServletException {
        clientId = config.getInitParameter("client_id");
        authority = config.getServletContext().getInitParameter("authority");
        tenant = config.getServletContext().getInitParameter("tenant");
        clientSecret = config.getInitParameter("secret_key");
    }

}

```

Bu servlet ADAL4J uygulamasını çalıştırmak için beklediğiniz tüm yöntemlerini gösterir. Yöntemler şunlardır:

* **getAccessTokenFromClientCredentials()**: Gets the access token from the secret.
* **getAccessTokenFromRefreshToken()**: Gets the access token from a refresh token.
* **getAccessToken()**: (kullanacağınız) bir Openıd Connect akışından erişim belirtecini alır.
* **createSessionPrincipal()**: grafik API'sine erişim için kullanılacak bir oturum sorumlusu oluşturur.
* **getRedirectUrl()**: portalda girdiğiniz değerle karşılaştırmak için Redirecturl'yi alır.

## <a name="step-9-compile-and-run-the-sample-in-tomcat"></a>9. adım: Derleme ve örnek Tomcat'te çalıştırma

1. Kök dizinine değiştirin.
2. Henüz koyduğunuz birlikte kullanarak örneği oluşturmak için `maven`, aşağıdaki komutu çalıştırın:

    `$ mvn package`

 Bu komut için bağımlılıkları yazdı pom.xml dosyasını kullanır.

Şimdi /targets dizininizde bir adal4jsample.war dosyası olmalıdır. Tomcat kapsayıcı dosyasında dağıtın ve http://localhost: 8080/adal4jsample/URL'yi ziyaret edin.

> [!NOTE]
> En son Tomcat sunucuları .war dosyasıyla kolayca dağıtabilirsiniz. Http://localhost: 8080/Yöneticisi/gidin ve adal4jsample.war dosya karşıya yükleme yönergelerini izleyin. İçinde autodeploy sizin için doğru bitiş noktası ile.


## <a name="next-steps"></a>Sonraki adımlar
Artık çalışan bir kullanıcıların kimlik doğrulaması, güvenli bir şekilde web OAuth 2.0 kullanan API'leri çağırmak ve kullanıcılar hakkındaki temel bilgileri elde Java uygulaması sahipsiniz. Kiracı kullanıcılarla zaten doldurulmuş yapmadıysanız, bunu yapmak için iyi bir zamandır sunulmuştur.

Ek başvuru için (yapılandırma değerleriniz olmadan) tamamlanan örnek iki yöntemden birini alabilirsiniz:

* Olarak karşıdan bir [.zip dosyası](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip).
* Aşağıdaki komutu girerek GitHub dosyasından kopyalama:

 ```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```
