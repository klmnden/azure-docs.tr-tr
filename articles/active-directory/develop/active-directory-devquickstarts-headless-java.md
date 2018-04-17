---
title: Azure AD Java komut satırı Başlarken | Microsoft Docs
description: Bir API erişmek için kullanıcı oturum açtığında bir Java komut satırı uygulaması oluşturma.
services: active-directory
documentationcenter: java
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: a0e12711e4a7e67861d61ae4575c4956531cf841
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Azure AD ile bir API erişmek için Java komut satırı uygulaması kullanma
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure AD Basit ve kolay web uygulamanızın Kimlik Yönetimi, dış kaynak sağlamak tek sağlama oturum açma ve oturum kapatma yalnızca birkaç satır kod kolaylaştırır.  Java web uygulamaları, bunu topluluk odaklı ADAL4J Microsoft'un uygulaması kullanarak gerçekleştirebilirsiniz.

  ADAL4J için burada kullanacağız:

* Kullanıcı uygulamayı Azure AD kimlik sağlayıcısı olarak kullanarak oturum açın.
* Kullanıcı hakkındaki bazı bilgileri görüntüler.
* Uygulama dışında kullanıcı oturum açabilir.

Bunu yapmak için ihtiyacınız vardır:

1. Bir uygulamayı Azure AD ile kaydetme
2. ADAL4J kitaplığı kullanmak için uygulamanızı ayarlayın.
3. Azure AD ile oturum açma ve oturum kapatma isteklerini yürütmek için ADAL4J kitaplığını kullanın.
4. Kullanıcı hakkında veri çıkışı yazdırın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  Ayrıca, uygulamanızın kaydedileceği Azure AD kiracısı gerekir.  Zaten yoksa, [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Bir uygulamayı Azure AD ile kaydetme
Kullanıcıların kimliğini doğrulamak uygulamanızı etkinleştirmek için önce yeni bir uygulama kiracınızda kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızda altında tıklatıp **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek için istediğiniz yeri seçin.
3. Tıklayın **tüm hizmetleri** sol taraftaki gezinti içinde ve **Azure Active Directory**.
4. Tıklayın **uygulama kayıtlar** ve **Ekle**.
5. Komut istemlerini izleyin ve yeni bir **Web uygulaması ve/veya Webapı**.
  * **Adı** uygulamayı son kullanıcılar uygulamanıza anlatmaktadır
  * **Oturum açma URL'si** , uygulamanızın temel URL.  Çatıyı ait varsayılan `http://localhost:8080/adal4jsample/`.
6. Kayıt tamamladıktan sonra AAD uygulamanızı benzersiz bir uygulama kimliği atar.  Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. **Uygulama kimliği URI'si** uygulamanız için benzersiz bir tanımlayıcıdır.  Kuralı kullanmaktır `https://<tenant-domain>/<app-name>`, örneğin `http://localhost:8080/adal4jsample/`.

Portalında, uygulamanız için bir kez oluşturun. bir **anahtar** gelen **ayarları** sayfasında uygulamanız için ve aşağı kopyalayın.  Kısa süre sonra ihtiyacınız olacaktır.

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. ADAL4J kitaplığı ve Maven kullanarak önkoşulları kullanmak için uygulamanızı ayarlayın
Burada, Openıd Connect kimlik doğrulama protokolünü kullanmak üzere ADAL4J yapılandıracağız.  ADAL4J oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcının oturumunu yönetmek ve başka şeyler arasında kullanıcı hakkında bilgi almak için kullanılır.

* Proje kök dizininde açma/oluşturma `pom.xml` ve bulun `// TODO: provide dependencies for Maven` ve şununla değiştirin:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
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
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
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
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. Java PublicClient dosyası oluşturma
Yukarıda belirtildiği gibi biz grafik API'si oturum açmış olan kullanıcının ilişkin veri almak için kullanır. Bunun için bize kolay olması biz temsil etmek için bir dosya oluşturmalısınız bir **dizin nesnesi** ve temsil etmek için tek bir dosyayı **kullanıcı** böylece Java OO desenini kullanılabilir.

* Adlı bir dosya oluşturun `DirectoryObject.java` hangi (düşündüğünüz diğer grafik yapmak için sorgu bunu daha sonra kullanmak boş) DirectoryObject hakkındaki temel verileri depolamak için kullanacağız. Kes/bu aşağıdan Yapıştır:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-the-sample"></a>Derleme ve örnek çalıştırma
Kök dizinine değiştirme geri çıkışı ve put birlikte kullanma örneği oluşturmak için aşağıdaki komutu çalıştırın `maven`. Bu kullanacağı `pom.xml` bağımlılıkları için yazdığınız dosya.

`$ mvn package`

Şimdi olmalıdır bir `adal4jsample.war` dosyasını, `/targets` dizin. Tomcat kapsayıcısında dağıtan ve URL'yi ziyaret edin 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> En son Tomcat sunucularıyla WAR dağıtmak çok kolaydır. Yalnızca gidin `http://localhost:8080/manager/` ve karşıya yükleme üzerinde yönergeleri izleyin, `adal4jsample.war` dosya. İçinde autodeploy sizin için doğru bitiş noktası ile.
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
Tebrikler! Artık OAuth 2.0 kullanan Web API'leri çağırmak ve kullanıcı hakkındaki temel bilgileri elde çalışan kullanıcıların, güvenli kimlik doğrulama yeteneği olan Java uygulama vardır.  Henüz yapmadıysanız, bazı kullanıcılar ile Kiracı doldurmak için zaman sunulmuştur.

(Yapılandırma değerleriniz olmadan) tamamlanan örnek, başvuru için [burada bir .zip sağlanan](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), veya Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

