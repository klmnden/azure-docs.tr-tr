---
title: Azure Cosmos DB öykünücüsü sertifikaları verme | Microsoft Docs
description: Diller ve Windows sertifika deposunda kullanmayın çalışma zamanları geliştirirken vermek ve SSL sertifikalarını yönetmek gerekir. Bu post adım adım yönergeler sağlar.
services: cosmos-db
documentationcenter: ''
keywords: Azure Cosmos DB öykünücüsü
author: voellm
manager: kfile
editor: ''
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87d453cd544b3e913209f50e4e08b77282efab39
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Java, Python ve Node.js ile kullanmak için Azure Cosmos DB öykünücüsü sertifikaları verme

[**Öykünücüyü yükleyin**](https://aka.ms/cosmosdb-emulator)

Azure Cosmos DB öykünücüsü Geliştirme amaçlı SSL bağlantılarını kullanımı dahil olmak üzere Azure Cosmos DB hizmet öykünen yerel bir ortam sağlar. Bu post diller ve kendi kullanan Java gibi Windows sertifika deposunda entegre değil çalışma zamanları kullanmak için SSL sertifikaları vermek gösterilmiştir [sertifika deposu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) ve kullandığı Python [yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html) ve kullandığı Node.js [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback). Daha fazla bilgiyi öykünücüsünde hakkında [geliştirme ve sınama için Azure Cosmos DB öykünücüsünü kullanma](./local-emulator.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Sertifikaları döndürme
> * SSL sertifikasını dışarı aktarma
> * Java, Python ve Node.js sertifika kullanmayı öğrenme

## <a name="certification-rotation"></a>Sertifika döndürme

Azure Cosmos DB yerel öykünücüsü sertifikalarda öykünücü bir ilk çalıştırıldığında oluşturulur. İki sertifika vardır. Biri yerel öykünücüsü, diğeri öykünücü içinde parolaları yönetmek için bağlanmak için kullanılır. Dışarı aktarmak istediğiniz sertifika kolay adı "DocumentDBEmulatorCertificate" bağlantı sertifikası yok.

Her iki sertifikanın tıklayarak üretilebilir **sıfırlama verileri** Azure Cosmos DB Windows tepsisinde çalıştıran öykücüsünden aşağıda gösterildiği gibi. Aksi takdirde sertifikaları yeniden oluşturmak ve bunları Java sertifika deposuna yüklediyseniz veya bunları başka bir yerde bunları güncelleştirmeniz gerekecektir kullanılan, uygulamanız artık yerel öykünücüsü bağlanır.

![Verileri Azure Cosmos DB yerel öykünücüsünü sıfırlayın](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a>Azure Cosmos DB SSL sertifikasını dışarı aktarma

1. Windows Sertifika Yöneticisi'ni başlatın certlm.msc çalıştıran ve kişisel-> sertifikaları klasöre gidin ve sertifikanın kolay adla açın **DocumentDbEmulatorCertificate**.

    ![Azure Cosmos DB yerel öykünücüsü verme 1. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. Tıklayın **ayrıntıları** sonra **Tamam**.

    ![Azure Cosmos DB yerel öykünücüsü verme 2. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. Tıklatın **dosyaya Kopyala...** .

    ![Azure Cosmos DB yerel öykünücüsü verme 3. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü verme 4. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. Tıklatın **Hayır, özel anahtarı verme**, ardından **sonraki**.

    ![Azure Cosmos DB yerel öykünücüsü verme 5. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. Tıklayın **Base-64 ile kodlanmış X.509 (. CER)** ve ardından **sonraki**.

    ![Azure Cosmos DB yerel öykünücüsü verme 6. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Sertifika bir ad verin. Bu durumda **documentdbemulatorcert** ve ardından **sonraki**.

    ![Azure Cosmos DB yerel öykünücüsü verme 7. adım](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. **Son**'a tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü 8. adımda dışarı aktarma](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a>Java sertifikada kullanma

Java uygulamalarını veya Java istemcisi kullanan MongoDB uygulamaları çalıştırırken geçirme daha Java varsayılan sertifika deposuna sertifika yüklemek daha kolay "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>"bayraklar. Örneğin dahil [Java Demo uygulama](https://localhost:8081/_explorer/index.html) varsayılan sertifika deposuna bağlıdır.

' Ndaki yönergeleri izleyin [Java CA sertifikalarını depolamak için bir sertifika ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) X.509 sertifikası varsayılan Java sertifika deposuna içeri aktarmak için. İçinde kalmasını keytool çalıştırırken % JAVA_HOME % dizininde unutmayın çalışacaksınız.

Bir kez "CosmosDBEmulatorCertificate" SSL sertifika yüklü uygulamanızı bağlanmak ve yerel Azure Cosmos DB öykünücüsünü kullanma yapabiliyor olmanız gerekir. Sorun yaşamaya devam ederseniz izlemek isteyebilir [hata ayıklama SSL/TLS bağlantılarını](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) makalesi. Sertifika %JAVA_HOME%/jre/lib/security/cacerts deposuna yüklenmemiş olasılığı yüksektir. Java uygulamanızı yüklü birden fazla sürümünü varsa örnek olandan güncelleştirildi, farklı cacerts deposu kullanabilecek için.

## <a name="how-to-use-the-certificate-in-python"></a>Sertifika Python içinde kullanma

Varsayılan olarak [Python SDK(version 2.0.0 or higher)](sql-api-sdk-python.md) SQL API değil deneyin ve yerel öykünücüsü bağlanırken SSL sertifikası kullanın. Ancak, SSL doğrulama kullanmak istediğiniz örneklerde takip edebilirsiniz, [Python yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html) belgeleri.

## <a name="how-to-use-the-certificate-in-nodejs"></a>Sertifika Node.js içinde kullanma

Varsayılan olarak [Node.js SDK(version 1.10.1 or higher)](sql-api-sdk-node.md) SQL API değil deneyin ve yerel öykünücüsü bağlanırken SSL sertifikası kullanın. Ancak, SSL doğrulama kullanmak istediğiniz örneklerde takip edebilirsiniz, [Node.js belgelerine](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Döndürülen sertifikaları
> * SSL sertifikasını dışarı
> * Java, Python ve Node.js sertifikayı kullanmak üzere öğrendiniz

Şimdi bir Azure işlevleri HTTP tetikleyicisi bir Azure Cosmos DB giriş bağlaması öğretici ile oluşturmak için devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB girişten ile bir Azure işlevi oluşturma](tutorial-functions-http-trigger.md) 
