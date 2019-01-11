---
title: include dosyası
description: include dosyası
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 12/26/2018
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 9a1469ff15070461968532aa4e1fda023569f219
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211923"
---
1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüde **Kaynak oluştur**'a, **Veritabanları**'na ve ardından **Azure Cosmos DB**' altında **Oluştur**’a tıklayın.
   
   ![Diğer Hizmetler ve Azure Cosmos DB seçeneklerinin vurgulandığı Azure portalı ekran görüntüsü](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. İçinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Kimlikte sağlanan benzersiz adın aynısını girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini kullanabilirsiniz. 3 ila 31 karakter uzunluğunda olmalıdır.
    API|MongoDB API için Azure Cosmos DB|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, beş API sunar: Gremlin graf veritabanları, Azure Cosmos DB belge veritabanı, Azure tablosu ve Cassandra API MongoDB belge veritabanları için çekirdek (SQL). Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>Seçin **MongoDB** çünkü bu hızlı başlangıçta, MongoDB API'si ile birlikte çalışan bir tablo oluşturuluyor.|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

    Seçin **gözden geçir + Oluştur**. Atlayabilirsiniz **ağ** ve **etiketleri** bölümü. 

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın **Tebrikler! MongoDB için protokol uyumluluğunu kablo ile Cosmos hesabınız hazır** sayfası.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
