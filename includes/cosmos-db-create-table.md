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
ms.openlocfilehash: 37d7b1d44c2a4b2f3cd2fd3ac881b106d5056279
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
Şimdi bir veritabanı ve tablo oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. **Veri Gezgini** > **Yeni Tablo**’ya tıklayın. 
    
    **Tablo Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalında Veri Gezgini](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. **Tablo Ekle** sayfasında, yeni tablo için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Tablo kimliği|sample-table|Yeni tablonuzun kimliği. Tablo adı karakter gereksinimleri, veritabanı kimliklerine ilişkin karakter gereksinimleri ile aynıdır. Veritabanı adı 1 ile 255 karakter arasında olmalı, `/ \ # ?` içermemeli ve boşlukla bitmemelidir.
    Depolama kapasitesi| Sabit (10 GB)|Varsayılan **Sabit (10 GB)** değerini kullanın. Bu değer, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.

    **Tamam**’a tıklayın.

    Veri Gezgini, yeni veritabanını ve tabloyu görüntüler.

    ![Yeni veritabanını ve koleksiyonu gösteren Azure portalı Veri Gezgini](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)