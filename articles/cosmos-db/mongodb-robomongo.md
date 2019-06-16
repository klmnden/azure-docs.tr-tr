---
title: Azure Cosmos DB'ye bağlanmak için robo 3t'yi kullanma
description: Azure Cosmos DB MongoDB Robo 3T ve Azure Cosmos DB'nin API'sini kullanarak bağlanma hakkında bilgi edinin
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: ab066fef4bffe35408fdc5f3ad6d66796b4d0818
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61372564"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>Robo 3T API'si ile Azure Cosmos DB'nin MongoDB için kullanın.

Robo 3T kullanma Cosmos hesabına bağlanmak için şunları yapmalısınız:

* İndirme ve yükleme [Robo 3T](https://robomongo.org/)
* Cosmos DB sahip [bağlantı dizesi](connect-mongodb-account.md) bilgileri

## <a name="connect-using-robo-3t"></a>Robo 3T kullanarak bağlan
Robo 3T Bağlantı Yöneticisi için Cosmos hesabınızı eklemek için aşağıdaki adımları gerçekleştirin:

1. Yönergeleri kullanarak Azure Cosmos DB'nin API'sini MongoDB ile yapılandırılmış Cosmos hesabınız için bağlantı bilgilerini almak [burada](connect-mongodb-account.md).

    ![Bağlantı dizesi dikey penceresinin ekran görüntüsü](./media/mongodb-robomongo/connectionstringblade.png)
2. Çalıştırma *Robomongo.exe*

3. Altından bağlantı düğmesini **dosya** bağlantılarınızı yönetme. ' A tıklayarak **Oluştur** içinde **MongoDB bağlantılarını** açılır penceresinde **bağlantı ayarlarını** penceresi.

4. İçinde **bağlantı ayarlarını** penceresinde, bir ad seçin. Ardından, bulma **konak** ve **bağlantı noktası** adım 1'den bağlantı bilgilerinizi ve girmeyi **adresi** ve **bağlantı noktası**sırasıyla.

    ![Robomongo'yu görüntüsü bağlantıları yönetme](./media/mongodb-robomongo/manageconnections.png)
5. Üzerinde **kimlik doğrulaması** sekmesinde **gerçekleştirme kimlik doğrulaması**. Ardından, veritabanınızın girin (varsayılan değer *yönetici*), **kullanıcı adı** ve **parola**.
Her ikisi de **kullanıcı adı** ve **parola** bağlantı bilgilerinizi adım 1'de bulunabilir.

    ![Robomongo'yu kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/authentication.png)
6. Üzerinde **SSL** sekmesinde, onay **kullanım SSL protokolü**, ardından değiştirme **kimlik doğrulama yöntemi** için **otomatik olarak imzalanan sertifika**.

    ![Robomongo'yu SSL sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/SSL.png)
7. Son olarak, tıklayın **Test** bağlanın, ardından mümkün olduğunu doğrulamak için **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Studio 3T kullanma](mongodb-mongochef.md) Azure Cosmos DB'nin MongoDB API'si ile.
- MongoDB keşfedin [örnekleri](mongodb-samples.md) Azure Cosmos DB'nin MongoDB API'si ile.
