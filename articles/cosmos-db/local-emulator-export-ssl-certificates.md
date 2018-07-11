---
title: Azure Cosmos DB Öykünücüsü sertifikalarını dışarı aktarma | Microsoft Docs
description: Windows Sertifika Deposu’nu kullanmayan dil ve çalışma zamanlarında geliştirirken, SSL sertifikalarını dışarı aktarmanız ve yönetmeniz gerekir. Bu gönderi adım adım yönergeler verir.
services: cosmos-db
keywords: Azure Cosmos DB Öykünücüsü
author: voellm
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 237ab0ed99793f54574c4b81e883acb8c2bc57a9
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082110"
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Azure Cosmos DB Öykünücü sertifikalarını Java, Python ve Node.js ile kullanmak için dışarı aktarma

[**Öykünücüyü İndirin**](https://aka.ms/cosmosdb-emulator)

Azure Cosmos DB Öykünücüsü, SSL bağlantılarının kullanımı dahil geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Bu gönderide, kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanan Java ve [yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html) kullanan Python ve [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) kullanan Node.js gibi Windows Sertifika Deposu’yla tümleştirilmeyen diller ve çalışma zamanları ile kullanım için SSL sertifikalarının nasıl dışarı aktarılacağı gösterilmektedir. [Geliştirme ve test için Azure Cosmos DB Emulator kullanma](./local-emulator.md) konusunda öykünücü hakkında daha fazla bilgi edinebilirsiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Sertifikalar döndürülüyor
> * SSL sertifikasını dışarı aktarma
> * Java, Python ve Node.js’de sertifika kullanmayı öğrenme

## <a name="certification-rotation"></a>Sertifika döndürme

Azure Cosmos DB Yerel Öykünücüsündeki sertifikalar öykünücü ilk kez çalıştırıldığında oluşturulur. İki sertifika vardır. Yerel öykünücüye bağlanmak için bir tane ve öykünücü içindeki gizli dizileri yönetmek için bir tane. Dışarı aktarmak istediğiniz sertifika, "DocumentDBEmulatorCertificate" kısa adına sahip bağlantı sertifikasıdır.

İki sertifika da aşağıda Windows Tepsisinde çalışan Azure Cosmos DB Öykünücüsü’nde gösterildiği gibi **Verileri Sıfırla**’ya tıklanarak yeniden oluşturulabilir. Sertifikaları yeniden oluşturursanız ve bunları Java sertifika deposuna yüklediyseniz ya da başka bir yerde kullandıysanız sertifikaları güncelleştirmeniz gerekir, aksi takdirde uygulamanız artık yerel öykünücüye bağlanamaz.

![Azure Cosmos DB yerel öykünücüsü sıfırlama verileri](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a>Azure Cosmos DB SSL sertifikasını dışarı aktarma

1. Certlm.msc’yi çalıştırarak Windows Sertifika yöneticisini çalıştırın ve Kişisel->Sertifikalar klasörüne gidip **DocumentDbEmulatorCertificate** kısa adına sahip sertifikayı açın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. **Ayrıntılar**’a ve ardından **Tamam**’a tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. **Dosyaya Kopyala...** seçeneğini belirleyin.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. **Hayır, özel anahtarı dışarı aktarma**’ya tıklayın ve **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. **Base-64 ile kodlanmış X.509 (.CER)** seçeneğine ve ardından **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Sertifikaya bir ad verin. Bu durumda **documentdbemulatorcert** dosyasını seçin ve **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. **Son**'a tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 8](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a>Java içinde sertifika kullanma

Java uygulamaları veya Java istemcisini kullanan MongoDB uygulamalarını çalıştırırken sertifikayı Java varsayılan sertifika deposuna yüklemek "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" bayraklarını geçirmekten daha kolaydır. Örneğn dahil edilen [Java Demo uygulaması](https://localhost:8081/_explorer/index.html) varsayılan sertifika deposuna bağlıdır.

X.509 sertifikasını varsayılan Java sertifika deposuna içeri aktarmak için, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeleri izleyin. Anahtar aracını çalıştırırken %JAVA_HOME% dizininde çalışacağınıza dikkat edin.

"CosmosDBEmulatorCertificate" SSL sertifikası yüklendiğinde uygulamanızın yerel Azure Cosmos DB Öykünücüsüne bağlanıp kullanabilmesi gerekir. Sorun yaşamaya devam ederseniz [SSL/TLS Bağlantılarında hata ayıklama](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) makalesindeki yönergeleri izleyin. Sertifika %JAVA_HOME%/jre/lib/security/cacerts deposuna yüklenmemiş olabilir. Örneğin birden çok Java sürümü yüklüyse, uygulamanız güncelleştirdiğinizden farklı bir cacerts deposu kullanıyor olabilir.

## <a name="how-to-use-the-certificate-in-python"></a>Python içinde sertifika kullanma

Varsayılan olarak SQL API’si için [Python SDK (sürüm 2.0.0 veya üzeri)](sql-api-sdk-python.md) yerel öykünücüye bağlanırken SSL sertifikasını kullanmaya çalışmaz. Ancak SSL doğrulama kullanmak istiyorsanız [Python yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html) belgelerindeki örnekleri izleyebilirsiniz.

## <a name="how-to-use-the-certificate-in-nodejs"></a>Node.js içinde sertifika kullanma

Varsayılan olarak SQL API’si için [Node.js SDK (sürüm 1.10.1 veya üzeri)](sql-api-sdk-node.md) yerel öykünücüye bağlanırken SSL sertifikasını kullanmaya çalışmaz. Ancak SSL doğrulamasını kullanmak istiyorsanız [Node.js belgelerindeki](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) örnekleri izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Döndürülen sertifikalar
> * Dışarı aktarılan SSL sertifikası
> * Java, Python ve Node.js’de sertifika kullanmayı öğrendiniz

Şimdi Azure Cosmos DB hakkında daha fazla bilgi için kavramlar bölümüne geçebilirsiniz. 

> [!div class="nextstepaction"]
>[Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)
