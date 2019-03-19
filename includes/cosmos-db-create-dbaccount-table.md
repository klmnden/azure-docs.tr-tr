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
ms.openlocfilehash: 0b9abb80969209fa31f8db6236e57dc817a89f6f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908181"
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol menüde **kaynak Oluştur**, tıklayın **veritabanları**ve ardından **Azure Cosmos DB**. 

3. İçinde **Azure Cosmos DB hesabı oluştur** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Değer|Açıklama
    ---|---|---
    Abonelik|Aboneliğiniz|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>Ardından Kimlikte sağlanan benzersiz adın aynısını girin|**Yeni oluştur**’u seçin. Ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanın. 
    Hesap Adı|Benzersiz bir ad girin|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad girin.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini kullanabilirsiniz. 3 ila 31 karakter uzunluğunda olmalıdır.
    API|Azure Tablosu|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, beş API sunar: Gremlin graf veritabanları, belge veritabanları, Azure tablosu ve Cassandra, MongoDB Core(SQL) belge veritabanları için. Şu anda, her bir API için ayrı bir hesap oluşturmanız gerekir. <br><br>Bu hızlı başlangıçta Tablo API’si ile birlikte çalışan bir tablo oluşturduğunuz için **Azure Tablosu**’nu seçin. <br><br>[Tablo API’si hakkında daha fazla bilgi edinin](../articles/cosmos-db/table-introduction.md)|
    Konum|Kullanıcılarınıza en yakın bölgeyi seçin|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.

4. Bırakabilirsiniz **coğrafi Yedeklilik** ve **çok bölgeli Yazar** seçenekleri olan varsayılan değerlerine **devre dışı** ek RU ücreti ödememek için. Atlayabilirsiniz **ağ** ve **etiketleri**.

5. Seçin **gözden geçir + Oluştur** doğrulama tamamlandıktan sonra seçip **Oluştur** hesabı oluşturmak için. 
 
   ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png)

6. Hesabın oluşturulması birkaç dakika sürer ve göreceğiniz **devam ettiği dağıtımıdır** ileti. Tamamlayın ve ardından bekleyin **kaynağa Git**.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png)
