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
ms.openlocfilehash: c3cbfda674abaeea1adf35c3ee0d2b5ddf6b2f84
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153738"
---
Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. **Veri Gezgini** > **Yeni Koleksiyon**’a tıklayın. 
    
    **Koleksiyon Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Koleksiyon Ekle dikey penceresi](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection-preview.png)

2. **Koleksiyon Ekle** sayfasında, yeni koleksiyon için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|*Görevler*|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?.
    Koleksiyon kimliği|*Öğeleri*|Yeni koleksiyonunuzun adı olarak *Öğeler* girin. Koleksiyon kimliği karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.
    Veritabanı aktarım hızı sağlama|Boş bırakın|Azure Cosmos DB, aktarım hızı (aynı aktarım hızı bir veritabanındaki tüm koleksiyonlar paylaşım) veritabanı düzeyinde veya koleksiyon düzeyindeki sağlayabilirsiniz. Bu belirli bir koleksiyon için koleksiyon düzeyinde sağlama aktarım hızı için boş bırakın.
    Depolama kapasitesi|*Sınırsız*|Depolama kapasitesini seçin **sınırsız**. 
    Bölüm anahtarı|*/ Category*|Girin "/ kategorisi" Bölüm anahtarı olarak. Bir bölüm anahtarı ayarı, uygulamanızın depolama ve aktarım hızı gereksinimlerini karşılamak için koleksiyonunuzun ölçeklendirmek Azure Cosmos DB sağlar. Genel olarak, iyi bir seçim bölüm anahtarı, depolama ve istek hacmi eşit dağıtımı iş yükünüz arasında çok çeşitli farklı değerleri ve sonuçları olan biridir. [Bölümleme hakkında daha fazla bilgi edinin.](../articles/cosmos-db/partitioning-overview.md)
    Performans|*400 RU/sn*|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. 
    
    Önceki ayarlara ek olarak, isterseniz koleksiyon için **Benzersiz anahtarlar** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Koleksiyon oluştururken benzersiz anahtar ilkesi oluşturulduğunda, bölüm anahtarı başına bir veya birden çok değerin benzersiz olduğundan emin olursunuz. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](../articles/cosmos-db/unique-keys.md) makalesine bakın.
    
    **Tamam** düğmesine tıklayın.

    Veri Gezgini, yeni veritabanını ve koleksiyonu görüntüler.

    ![Yeni veritabanını ve koleksiyonu gösteren Azure portalı Veri Gezgini](./media/cosmos-db-create-collection/azure-cosmos-db-data-explorer-preview.png)