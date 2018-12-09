---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 1562e311ef9253c16b97e88decae615ff0d365c4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111817"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğini belirleyin.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Üzerinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabı için temel ayarları girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Kimlikte sağlanan benzersiz adın aynısını girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini kullanabilirsiniz. Bu, 3 ila 50 karakter uzunluğunda olmalıdır.
    API|Core(SQL)|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, beş API sunar: belge veritabanları için grafik, Gremlin Core(SQL) veritabanlarını, MongoDB belge veritabanları, Azure tablosu ve Cassandra. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>Seçin **Core(SQL)** çünkü bu makalede, bir belge veritabanı ve sorgu SQL söz dizimini kullanarak oluşturursunuz. <br><br>[SQL API'si hakkında daha fazla bilgi](../articles/cosmos-db/documentdb-introduction.md).|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    Seçin **gözden geçir + Oluştur**. Atlayabilirsiniz **ağ** ve **etiketleri** bölümü. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın **Tebrikler! Azure Cosmos DB hesabınız oluşturuldu** sayfasını görüntülemesini bekleyin.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

