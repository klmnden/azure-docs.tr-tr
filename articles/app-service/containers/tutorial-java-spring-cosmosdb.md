---
title: -Azure App Service Linux üzerinde Java web uygulaması derleme
description: Oluşturmanızı, dağıtmanızı ve Spring önyükleme Java Web uygulamalarını Azure App Service Linux ve Azure Cosmos DB ile ölçeklendirin.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 069bc213695de813ad6b878db54f38a909efd1df
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956025"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>Öğretici: Spring ve Azure Cosmos DB'yi kullanarak bir Java web uygulaması derleme

Bu öğreticide, Azure üzerinde oluşturma, yapılandırma, dağıtma ve ölçeklendirme Java web uygulamalarını sürecinde açıklanmaktadır. İşlemi tamamladığınızda, olacaktır bir [Spring Boot](https://projects.spring.io/spring-boot/) veri depolarken uygulama [Azure Cosmos DB](/azure/cosmos-db) üzerinde çalışan [Linux üzerinde Azure App Service'te](/azure/app-service/containers).

![Azure uygulama hizmetinde çalıştırılan Java uygulaması](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Cosmos DB veritabanı oluşturun.
> * Örnek uygulama veritabanına bağlanmak ve yerel olarak test etme
> * Örnek uygulamayı Azure'a dağıtma
> * App Service'in Stream tanılama günlükleri
> * Örnek uygulamayı inceleyin ölçeklendirmek için ek örnek ekleyin

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview), kendi bilgisayarınızda yüklü. 
* [Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>TODO örnek uygulamayı kopyalayın ve depo hazırlama

Bu öğreticide, bir web tarafından yedeklenen bir Spring REST API çağrılarının kullanıcı Arabirimi ile bir örnek Yapılacaklar listesi uygulaması kullanılır. [Spring verilerini Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb). Uygulama kodunu kullanılabilir [github'da](https://github.com/Microsoft/spring-todo-app). Spring ve Cosmos DB'yi kullanarak Java uygulamaları yazma hakkında daha fazla bilgi için bkz: [Spring Boot Başlatıcı Azure Cosmos DB SQL API öğreticisiyle](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) ve [Spring verilerini Azure Cosmos DB Hızlı Başlangıç](https://github.com/Microsoft/spring-data-cosmosdb#quick-start).


Örnek depoyu kopyalamak ve örnek uygulama ortamı ayarlamak için terminalde aşağıdaki komutları çalıştırın.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Bir Azure Cosmos DB oluşturun

Aboneliğinizde bir Azure Cosmos DB veritabanı oluşturmak için aşağıdaki adımları izleyin. Yapılacaklar listesi uygulaması, bu veritabanına bağlanmak ve uygulamayı çalıştırdığınız ne olursa olsun uygulama durumu kalıcı hale çalıştırırken, kendi veri deposu.

1. Oturum açma, Azure CLI ve birden fazla oturum açma kimlik bilgilerinizi bağlı varsa aboneliğinizi isteğe bağlı olarak ayarlayın.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Kaynak grubu adı belirterek bir Azure kaynak grubu oluşturun.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Azure Cosmos DB ile oluşturma `GlobalDocumentDB` türü. Cosmos DB adı yalnızca küçük harf kullanmanız gerekir. Not `documentEndpoint` komutu gelen yanıt kodu.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Uygulamaya bağlanmak için Azure Cosmos DB anahtarınızı alın. Tutun `primaryMasterKey`, `documentEndpoint` nearby gibi bir sonraki adım gerekir.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>TODO uygulaması özelliklerini yapılandırın

Bilgisayarınızda bir terminal açın. Yeni oluşturduğunuz Cosmos DB veritabanınıza için özelleştirmek için kopyalanan deponun içinde örnek komut dosyasını kopyalayın.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
Düzen `.scripts/set-env-variables.sh` sık kullandığınız düzenleyicide ve Azure Cosmos DB bağlantı bilgileri kaynağı. App Service Linux yapılandırma için önce olduğu gibi aynı bölgede kullanın (`your-resource-group-region`) ve kaynak grubu (`your-azure-group-name`) Cosmos DB veritabanı oluşturulurken kullanılır. Herhangi bir Azure dağıtımındaki herhangi bir web uygulaması adını yineleyemez bu yana, benzersiz bir WEBAPP_NAME seçin.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

# App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Ardından, betiği çalıştırın:

```bash
source .scripts/set-env-variables.sh
```
   
Bu ortam değişkenleri olarak kullanılan `application.properties` Yapılacaklar listesi uygulaması içinde. Özellikler dosyası alanları Spring veriler için bir varsayılan havuz yapılandırmasını ayarlayın:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

Ardından örnek uygulamanın kullandığı `@Document` ek açıklama alındığı `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document` depolanan ve Cosmos DB tarafından yönetilen bir varlık türü ayarlamak için:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Örneği çalıştırmak için Maven'i kullanın.

```bash
mvn package spring-boot:run
```

Çıktı aşağıdaki gibi görünmelidir.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

Uygulama başlatıldıktan sonra bu bağlantıyı kullanarak yerel olarak Spring TODO uygulaması erişebilirsiniz: [ http://localhost:8080/ ](http://localhost:8080/).

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Özel durumlar yerine "TodoApplication başlatıldı" iletisi görürseniz, bu maddeyi `bash` betik önceki adımda dışarı ortam değişkenlerini doğru ve değerleri Azure Cosmos DB veritabanı için doğru olduğundan emin oluşturdunuz.

## <a name="configure-azure-deployment"></a>Azure dağıtımını yapılandırma

Açık `pom.xml` dosyası `initial/spring-boot-todo` dizin ve aşağıdakileri ekleyin [Azure App Service için Maven Plugin](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) yapılandırma.

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Linux üzerinde App Service'e dağıtma

Kullanım `azure-webapp:deploy` TODO uygulaması, Linux üzerinde Azure App Service'e dağıtmak için Maven hedefi.

```bash

# Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

Dağıtılan uygulamanızın URL'sine çıktısını içeren (Bu örnekte, `https://spring-todo-app.azurewebsites.net` ). Web tarayıcınıza bu URL'yi kopyalayın veya uygulamanızı yüklemek için Terminal penceresinde aşağıdaki komutu çalıştırın.

```bash
open https://spring-todo-app.azurewebsites.net
```

Uzak Adres çubuğundaki URL ile çalışan uygulamasını görmeniz gerekir:

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]


## <a name="scale-out-the-todo-app"></a>TODO uygulama ölçeklendirme

Uygulamayı başka bir çalışan büyütülebilir:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir öğretici (bkz. [Sonraki adımlar](#next)) için gereksinim duymuyorsanız, Cloud Shell'de aşağıdaki komutu çalıştırarak bu kaynakları silebilirsiniz: 
  
```bash
az group delete --name <your-azure-group-name>
```

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

[Java geliştiricileri için Azure](/java/azure/)
[Spring önyükleme](https://spring.io/projects/spring-boot), [verileri Cosmos DB için Spring](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), [Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction) ve [App Service Linux ](app-service-linux-intro.md).

Geliştirici Kılavuzu'nda Linux üzerinde App Service'te Java uygulamaları çalıştırma hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"] 
> [Java'da App Service Linux geliştirme Kılavuzu](configure-language-java.md)
