---
title: Azure Cosmos DB Robomongo kullanın | Microsoft Docs
description: 'Bir Azure Cosmos DB ile Robomongo kullanmayı öğrenin: API MongoDB hesabı'
keywords: robomongo
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: b6d64d7d7b30d4175fb8c8bf6c98127465427d4e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795042"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Bir Azure Cosmos DB ile Robomongo kullanın: API MongoDB hesabı
Bir Azure Cosmos DB'ye bağlanmasına: API MongoDB hesabının Robomongo kullanarak, şunları yapmalısınız:

* İndirme ve yükleme [Robomongo](https://robomongo.org/)
* Azure Cosmos DB sahip: API MongoDB hesabı için [bağlantı dizesi](connect-mongodb-account.md) bilgileri

## <a name="connect-using-robomongo"></a>Robomongo kullanarak bağlan
Azure Cosmos DB eklemek için: API MongoDB hesap Robomongo MongoDB bağlantılar için aşağıdaki adımları gerçekleştirin.

1. Azure Cosmos DB almak: API MongoDB hesap bağlantı bilgilerini yönergeleri kullanarak [burada](connect-mongodb-account.md).

    ![Bağlantı dizesi dikey penceresi ekran görüntüsü](./media/mongodb-robomongo/connectionstringblade.png)
2. Çalıştırma *Robomongo.exe*

3. Altında bağlantı düğmesini **dosya** bağlantılarınızı yönetmek için. Ardından **oluşturma** içinde **MongoDB bağlantıları** açılır pencere **bağlantı ayarlarını** penceresi.

4. İçinde **bağlantı ayarlarını** penceresinde, bir ad seçin. Ardından, bulma **konak** ve **bağlantı noktası** adım 1, bağlantı bilgileri ve girmeyi **adresi** ve **bağlantı noktası**sırasıyla.

    ![Robomongo bağlantılarını yönet ekran görüntüsü](./media/mongodb-robomongo/manageconnections.png)
5. Üzerinde **kimlik doğrulaması** sekmesini tıklatın, **gerçekleştirme kimlik doğrulama**. Ardından, veritabanınızın girin (varsayılan değer *yönetici*), **kullanıcı adı** ve **parola**.
Her ikisi de **kullanıcı adı** ve **parola** bağlantı bilgilerinizi adım 1'de bulunabilir.

    ![Robomongo kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/authentication.png)
6. Üzerinde **SSL** sekmesi, onay **SSL kullan Protokolü**, sonra değiştirmek **kimlik doğrulama yöntemini** için **otomatik olarak imzalanan sertifika**.

    ![Robomongo SSL sekmesinin Ekran görüntüsü](./media/mongodb-robomongo/SSL.png)
7. Son olarak, tıklatın **Test** , ardından bağlanabiliyor olduğunu doğrulamak için **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB keşfedin: API MongoDB için [örnekleri](mongodb-samples.md).
