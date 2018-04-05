---
title: Azure Cosmos DB MongoDB API hesabı oluşturma
description: Azure portalında Azure Cosmos DB MongoDB API hesabını nasıl oluşturacağınız açıklanmaktadır
services: cosmos-db
author: mimig1
ms.service: cosmos-db
ms.topic: include
ms.date: 03/20/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 02ea0e011642313b885bc48ec48104fa2789da81
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüde **Kaynak oluştur**'a, **Veritabanları**'na ve ardından **Azure Cosmos DB**' altında **Oluştur**’a tıklayın.
   
   ![Diğer Hizmetler ve Azure Cosmos DB seçeneklerinin vurgulandığı Azure portalı ekran görüntüsü](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. **Yeni hesap** dikey penceresinde, API olarak **MongoDB** seçeneğini belirtin ve Azure Cosmos DB hesabı için istediğiniz yapılandırmayı doldurun.
 
    ![Yeni Azure Cosmos DB dikey penceresinin ekran görüntüsü](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)

    * **Kimlik**, Azure Cosmos DB hesabınızı tanımlamak için kullanmak istediğiniz benzersiz bir ad olmalıdır. Yalnızca küçük harf, rakam ve '-' karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır.
    * **Abonelik**, Azure aboneliğinizdir. Bu sizin için doldurulur.
    * **Kaynak Grubu**, Azure Cosmos DB hesabınız için kaynak grubu adıdır.
    * **Konum**, Azure Cosmos DB örneğinizin bulunduğu coğrafi konumdur. Kullanıcılarınıza en yakın konumu seçin.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.
5. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

    ![Dağıtım başlatıldı bildirimi](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Dağıtım tamamlandığında Tüm Kaynaklar kutucuğundan yeni hesabı açın. 

    ![Tüm Kaynaklar kutucuğunda Azure Cosmos DB hesabı](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
