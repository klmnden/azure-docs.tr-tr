---
title: 'Hizmetten hizmete kimlik doğrulaması: Azure Active Directory kullanarak Java ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Active Directory'yi kullanarak Java ile hizmetten hizmete kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c32eada2acca73e089c2296ce8e59c529d7af665
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879174"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Java ile hizmetten hizmete kimlik doğrulama ile Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için Java SDK'sını kullanma hakkında bilgi edinin. Java SDK'sını kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1 desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Active Directory "Web" uygulaması oluşturma**. Adımları tamamlamış olmanız gerekir [hizmetten hizmete kimlik doğrulaması Azure Active Directory kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.

* (İsteğe bağlı) Bir IDE [Intellij Idea](https://www.jetbrains.com/idea/download/) veya [Eclipse](https://www.eclipse.org/downloads/) veya benzer.

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması
1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın.

2. Maven **pom.xml** dosyanıza aşağıdaki bağımlılıkları ekleyin. Aşağıdaki kod parçacığını **\</project>** etiketinin önüne ekleyin:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    İlk bağımlılık, Data Lake depolama Gen1 SDK kullanmaktır (`azure-data-lake-store-sdk`) maven deposundan. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake depolama Gen1 SDK'sını kullanan [slf4j](https://www.slf4j.org/) birçok popüler günlük altyapılarını log4j, Java günlük kaydı, logback vs. seçmenize olanak sağlayan günlük cephe veya günlük yok. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](https://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

4. Aşağıdaki kod parçacığı, daha önce bir alt sınıflarından birini kullanarak oluşturduğunuz Active Directory Web uygulaması için belirteci edinmek için Java uygulamanızda kullanmak `AccessTokenProvider` (aşağıdaki örnekte `ClientCredsTokenProvider`). Belirteç sağlayıcısı, bellekteki belirteci almak için kullanılan kimlik bilgilerini önbelleğe alır ve süre sonu yaklaştığında belirteci otomatik olarak yeniler. Kendi alt sınıfları oluşturmak mümkündür `AccessTokenProvider` böylece belirteçlerin müşteri kodunuza göre elde edilir. Şimdilik yalnızca şimdi SDK'da verileni kullanalım.

    **BURAYI DOLDURUN** alanını, Azure Active Directory Web uygulaması için gerçek değerlerle değiştirin.

        private static String clientId = "FILL-IN-HERE";
        private static String authTokenEndpoint = "FILL-IN-HERE";
        private static String clientKey = "FILL-IN-HERE";
    
        AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);   

Data Lake depolama Gen1 SDK'sı, Data Lake depolama Gen1 hesabıyla iletişim kurmak için gereken güvenlik belirteçlerini yönetmenizi sağlayacak kullanışlı yöntemler sağlar. Bununla birlikte, SDK yalnızca bu yöntemlerin kullanılmasını zorunlu kılmaz. [Azure Active Directory SDK’sını](https://github.com/AzureAD/azure-activedirectory-library-for-java) veya kendi özel kodunuzu kullanma gibi diğer belirteç edinme yöntemlerinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Data Lake depolama Gen1 ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz Java SDK'sını kullanarak. Data Lake depolama Gen1 ile çalışmak için Java SDK'sını kullanma hakkında konuşmak Aşağıdaki makaleler artık göz atabilirsiniz.

* [Data Lake depolama Gen1 üzerinde Java SDK'sını kullanarak veri işlemleri](data-lake-store-get-started-java-sdk.md)


