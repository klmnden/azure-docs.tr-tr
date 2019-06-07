---
title: include dosyası
description: include dosyası
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.topic: include
ms.date: 11/19/2018
ms.author: dech
ms.custom: include file
ms.openlocfilehash: 331886f01345aba576cd8f96f95077f9bbdae704
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754342"
---
Artık, bir veritabanı ve kapsayıcı oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Tıklayın **Veri Gezgini** > **yeni kapsayıcı**. 
    
    **Ekle kapsayıcı** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, kapsayıcısı Ekle dikey penceresi](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection-preview.png)

2. İçinde **kapsayıcısı Ekle** sayfasında, yeni bir kapsayıcı için ayarları girin.

    |Ayar|Önerilen değer|Açıklama
    |---|---|---|
    |**Veritabanı Kimliği**|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?. Denetleme **sağlama veritabanı aktarım hızını** seçeneği, bu veritabanındaki tüm kapsayıcılar arasında veritabanına sağlanan işleme paylaşmak olanak tanır. Bu seçenek ile maliyet tasarrufları da yardımcı olur. |
    |**Aktarım hızı**|400|Aktarım hızını 400 istek birimi (RU/sn) saniyede bırakılacak. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.| 
    |**Kapsayıcı kimliği**|Öğeler|Girin *öğeleri* yeni kapsayıcınız için adı. Kapsayıcı kimlikleri aynı karakter gereksinimleri veritabanı adlarına sahip.|
    |**Bölüm anahtarı**| /kategori| Bu makalede açıklanan örnek kullanır */Category* bölüm anahtarı olarak. Bir bölüm anahtarı ayarı, uygulamanızın depolama ve aktarım hızı gereksinimlerini karşılamak için koleksiyonunuzun ölçeklendirmek Azure Cosmos DB sağlar. Genel olarak, iyi bir seçim bölüm anahtarı, depolama ve istek hacmi eşit dağıtımı iş yükünüz arasında çok çeşitli farklı değerleri ve sonuçları olan biridir. [Bölümleme hakkında daha fazla bilgi edinin.](../articles/cosmos-db/partitioning-overview.md)|
    
    Önceki ayarlara ek olarak, isteğe bağlı olarak ekleyebilirsiniz **benzersiz anahtarlar** kapsayıcısı için. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Bir kapsayıcı oluştururken bir benzersiz anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerlerin benzersiz olmasını sağlamak. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](../articles/cosmos-db/unique-keys.md) makalesine bakın.
    
    **Tamam**’ı seçin. Veri Gezgini yeni veritabanını ve kapsayıcı görüntüler.

