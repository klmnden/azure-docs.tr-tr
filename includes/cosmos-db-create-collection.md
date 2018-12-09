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
ms.openlocfilehash: e287741fd6643c2eba192a9e29f46219faf520ec
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111678"
---
Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. **Veri Gezgini** > **Yeni Koleksiyon**’a tıklayın. 
    
    **Koleksiyon Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, koleksiyon Ekle bölmesi](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. **Koleksiyon Ekle** sayfasında, yeni koleksiyon için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adı 1 ila 255 karakter içermeli ve içeremezler `/, \\, #, ?`, veya bir boşluk.
    Koleksiyon kimliği|Öğeler|Yeni koleksiyonunuzun adı olarak *Öğeler* girin. Koleksiyon kimliği karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.
    Bölüm anahtarı| <Your partition key>| Bir bölüm anahtarı girmeniz */userid*.
    Aktarım hızı|400 RU|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. 
    
    Önceki ayarlara ek olarak, isterseniz koleksiyon için **Benzersiz anahtarlar** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Koleksiyon oluştururken benzersiz anahtar ilkesi oluşturulduğunda, bölüm anahtarı başına bir veya birden çok değerin benzersiz olduğundan emin olursunuz. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](../articles/cosmos-db/unique-keys.md) makalesine bakın.
    
    **Tamam** düğmesine tıklayın.

    Veri Gezgini, yeni veritabanını ve koleksiyonu görüntüler.

    ![Yeni veritabanını ve koleksiyonu gösteren Azure portalı Veri Gezgini](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)