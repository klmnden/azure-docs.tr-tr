---
title: Azure Cosmos DB Robomongo'yu kullanma
description: "Bir Azure Cosmos DB ile Robomongo'yu kullanma hakkında bilgi edinin: API MongoDB hesabı"
keywords: robomongo'yu
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: 78f0158c9a80a60717b81b4788531c7efd979111
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863814"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Robomongo'yu kullanma ile bir Azure Cosmos DB: MongoDB hesabı için API
Bir Azure Cosmos DB'ye bağlanmak için: API Robomongo'yu kullanarak MongoDB hesabı için şunları yapmalısınız:

* İndirme ve yükleme [Robomongo'yu](https://robomongo.org/)
* Sahip Azure Cosmos DB: MongoDB hesabı için API [bağlantı dizesi](connect-mongodb-account.md) bilgileri

## <a name="connect-using-robomongo"></a>Robomongo kullanarak bağlanma
Azure Cosmos DB eklemek için: API Robomongo'yu MongoDB bağlantıları için MongoDB hesabı için aşağıdaki adımları gerçekleştirin.

1. Almak, Azure Cosmos DB: MongoDB hesabı bağlantı bilgilerini yönergeleri kullanarak API [burada](connect-mongodb-account.md).

    ![Bağlantı dizesi dikey penceresinin ekran görüntüsü](./media/mongodb-robomongo/connectionstringblade.png)
2. Çalıştırma *Robomongo.exe*

3. Altından bağlantı düğmesini **dosya** bağlantılarınızı yönetme. ' A tıklayarak **Oluştur** içinde **MongoDB bağlantılarını** açılır penceresinde **bağlantı ayarlarını** penceresi.

4. İçinde **bağlantı ayarlarını** penceresinde, bir ad seçin. Ardından, bulma **konak** ve **bağlantı noktası** adım 1'den bağlantı bilgilerinizi ve girmeyi **adresi** ve **bağlantı noktası**sırasıyla.

    ![Robomongo'yu bağlantıları Yönet'ın ekran görüntüsü](./media/mongodb-robomongo/manageconnections.png)
5. Üzerinde **kimlik doğrulaması** sekmesinde **gerçekleştirme kimlik doğrulaması**. Ardından, veritabanınızın girin (varsayılan değer *yönetici*), **kullanıcı adı** ve **parola**.
Her ikisi de **kullanıcı adı** ve **parola** bağlantı bilgilerinizi adım 1'de bulunabilir.

    ![Robomongo'yu kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/authentication.png)
6. Üzerinde **SSL** sekmesinde, onay **kullanım SSL protokolü**, ardından değiştirme **kimlik doğrulama yöntemi** için **otomatik olarak imzalanan sertifika**.

    ![Robomongo'yu SSL sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/SSL.png)
7. Son olarak, tıklayın **Test** bağlanın, ardından mümkün olduğunu doğrulamak için **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB'yi keşfedin: MongoDB için API [örnekleri](mongodb-samples.md).
