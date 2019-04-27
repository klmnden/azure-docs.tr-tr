---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory kullanarak Java ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Java ile Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
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
ms.openlocfilehash: 8b558fca964f33d47d331e007329d1bae2626877
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60878110"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Java kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>   

Bu makalede, Azure Data Lake depolama Gen1 son kullanıcı kimlik doğrulaması yapmak için Java SDK'sını kullanma hakkında bilgi edinin. Java SDK'sını kullanarak hizmetten hizmete kimlik doğrulaması ile Data Lake depolama Gen1 bkz [Java ile hizmetten hizmete kimlik doğrulama ile Data Lake depolama Gen1](data-lake-store-service-to-service-authenticate-java.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. Adımları tamamlamış olmanız gerekir [Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.

* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
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
        import com.microsoft.azure.datalake.store.oauth2.DeviceCodeTokenProvider;

4. Aşağıdaki kod parçacığını kullanarak daha önce oluşturduğunuz Active Directory yerel uygulaması için belirteci edinmek için Java uygulamanızda kullanmak `DeviceCodeTokenProvider`. Değiştirin **burayı DOLDURUN** Azure Active Directory yerel uygulama için gerçek değerlerle.

        private static String nativeAppId = "FILL-IN-HERE";
            
        AccessTokenProvider provider = new DeviceCodeTokenProvider(nativeAppId);   

Data Lake depolama Gen1 SDK'sı, Data Lake depolama Gen1 hesabıyla iletişim kurmak için gereken güvenlik belirteçlerini yönetmenizi sağlayacak kullanışlı yöntemler sağlar. Bununla birlikte, SDK yalnızca bu yöntemlerin kullanılmasını zorunlu kılmaz. [Azure Active Directory SDK’sını](https://github.com/AzureAD/azure-activedirectory-library-for-java) veya kendi özel kodunuzu kullanma gibi diğer belirteç edinme yöntemlerinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Lake depolama Gen1 ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz Java SDK'sını kullanarak. Şimdi, Azure Data Lake depolama Gen1 ile çalışmak için Java SDK'sını kullanma hakkında konuşmak aşağıdaki makaleleri da bakabilirsiniz.

* [Data Lake depolama Gen1 üzerinde Java SDK'sını kullanarak veri işlemleri](data-lake-store-get-started-java-sdk.md)


