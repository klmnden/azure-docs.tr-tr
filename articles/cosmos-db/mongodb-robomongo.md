---
title: Azure Cosmos DB Robomongo kullanın | Microsoft Docs
description: 'Bir Azure Cosmos DB ile Robomongo kullanmayı öğrenin: API MongoDB hesabı'
keywords: robomongo
services: cosmos-db
author: AndrewHoh
manager: kfile
documentationcenter: ''
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 7d318880b7b0078e4c03acb66885f4aed5534ba1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
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
