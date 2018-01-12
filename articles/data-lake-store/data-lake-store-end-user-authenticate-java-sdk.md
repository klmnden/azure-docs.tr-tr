---
title: "Son kullanıcı kimlik doğrulaması: Java ile Azure Active Directory kullanarak Data Lake Store | Microsoft Docs"
description: "Java ile Azure Active Directory kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması elde öğrenin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: b1fbf3c1e0e5715ff7effcdac45ff03f2b55f9ee
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="end-user-authentication-with-data-lake-store-using-java"></a>Java kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>   

Bu makalede, Azure Data Lake Store ile son kullanıcı kimlik doğrulaması yapmak için Java SDK'sını kullanma hakkında bilgi edinin. Java SDK'yı kullanarak Data Lake Store ile hizmet kimlik doğrulaması için bkz: [hizmeti için kimlik doğrulaması Java kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-java.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [son kullanıcı kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.

* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın.

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
   
    İlk bağımlılık, maven deposundan Data Lake Store SDK’sını (`azure-data-lake-store-sdk`) kullanmaktır. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake Store SDK’sı; log4j, Java günlük kaydı, logback gibi birçok popüler günlük altyapısından birini seçmenizi veya günlük kaydı seçmemenizi sağlayan [slf4j](http://www.slf4j.org/) günlük cephesini kullanır. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](http://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.DeviceCodeTokenProvider;

4. Aşağıdaki kod parçacığını kullanarak daha önce oluşturduğunuz Active Directory yerel uygulama için belirteci almak için Java uygulamanızda kullanmak `DeviceCodeTokenProvider`. Değiştir **burada doldurma** Azure Active Directory yerel uygulama için gerçek değerlerle.

        private static String nativeAppId = "FILL-IN-HERE";
            
        AccessTokenProvider provider = new DeviceCodeTokenProvider(nativeAppId);   

Data Lake Store SDK’sı, Data Lake Store hesabıyla iletişim kurmak için gereken güvenlik belirteçlerini yönetmenizi sağlayacak kullanışlı yöntemler sunar. Bununla birlikte, SDK yalnızca bu yöntemlerin kullanılmasını zorunlu kılmaz. [Azure Active Directory SDK’sını](https://github.com/AzureAD/azure-activedirectory-library-for-java) veya kendi özel kodunuzu kullanma gibi diğer belirteç edinme yöntemlerinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Java SDK'yı kullanarak Azure Data Lake Store ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmak için Java SDK'sını kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [Data Lake Store Java SDK'sını kullanarak veri işlemleri](data-lake-store-get-started-java-sdk.md)


