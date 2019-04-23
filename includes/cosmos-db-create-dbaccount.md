---
title: include dosyası
description: include dosyası
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805337"
---
1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğini belirleyin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. Üzerinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos hesabı için temel ayarları girin. 
 
    |Ayar|Değer|Açıklama |
    |---|---|---|
    |Abonelik|Abonelik adı|Bu Azure Cosmos hesap için kullanmak istediğiniz Azure aboneliğini seçin. |
    |Kaynak Grubu|Kaynak grubu adı|Bir kaynak grubu seçin ya da seçin **Yeni Oluştur**, ardından yeni bir kaynak grubu için benzersiz bir ad girin. |
    | Hesap Adı|Benzersiz bir ad girin|Azure Cosmos hesabınızı tanımlamak için bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3-31 karakter uzunluğunda olmalıdır.|
    | API|Çekirdek (SQL)|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, beş API sunar: Çekirdek (SQL) ve MongoDB, Gremlin graf verilerini, Azure tablosu ve Cassandra belge veriler için. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>Seçin **çekirdek (SQL)** SQL söz dizimini kullanarak bir belge veritabanı ve sorgu oluşturmak için. <br><br>[SQL API'si hakkında daha fazla bilgi](../articles/cosmos-db/documentdb-introduction.md).|
    | Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Bunları verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.|
   
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. **İncele ve oluştur**’u seçin. Atlayabilirsiniz **ağ** ve **etiketleri** bölümler. 

1. Hesap ayarları gözden geçirin ve ardından **Oluştur**. Hesabı oluşturmak için birkaç dakika sürer. Portal sayfasında görüntülenecek bekleyin **dağıtımınız tamamlandıktan**. 

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Seçin **kaynağa Git** Azure Cosmos DB hesap sayfasına gidin. 

    ![Azure Cosmos DB hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
