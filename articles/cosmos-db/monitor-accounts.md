---
title: Azure Cosmos DB istekleri ve depolama izleme | Microsoft Docs
description: "Azure Cosmos DB hesabınız için istekleri ve sunucu hataları gibi performans ölçümleri ve depolama alanı tüketimi gibi kullanım ölçümleri izleme öğrenin."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 4c6a2e6f-6e78-48e3-8dc6-f4498b235a9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: mimig
ms.openlocfilehash: f07489172306b4f6d03b5a9b1399ed92e007c3c1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-azure-cosmos-db"></a>Azure Cosmos DB izleme
Azure Cosmos DB hesaplarınızı izleyebilirsiniz [Azure portal](https://portal.azure.com/). Her Azure Cosmos DB hesabı için tam ölçümleri verimlilik, depolama, kullanılabilirlik, gecikme ve tutarlılık izlemek kullanılabilir paketidir.

Ölçümleri hesap sayfasında, yeni ölçümleri sayfa veya Azure İzleyicisi'nde incelenebilir.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Ölçümleri sayfasında görünümü performans ölçümleri
1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **daha Hizmetleri**, kaydırın **veritabanları**, tıklatın **Azure Cosmos DB**ve kendisi için istediğiniz performans ölçümleri görüntülemek üzere Azure Cosmos DB hesap adına tıklayın.
2. Yeni Sayfa yüklediğinde, kaynak menüsünün altında **izleme**, tıklatın **ölçümleri**.
3. Ölçümleri sayfası açıldığında, gelen gözden geçirmek için koleksiyon seçin **Collection(s)** açılır.

   Azure portal koleksiyonu ölçümleri kullanılabilir suite görüntüler. Verimlilik, depolama, kullanılabilirlik, gecikme ve tutarlılık ölçümleri ayrı sekmelerde verildiğini unutmayın. Ek ayrıntılar, Metrikler üst çift oka tıklayarak her ölçümleri bölmesinde sağ elde etmek için.

   ![Ölçümleri suite gösterir izleme Mercek ekran görüntüsü](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Azure Monitoring kullanarak görünüm performans ölçümleri
1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **İzleyici** sol çubuğunda.
2. Kaynak menüye tıklayın **ölçümleri**.
3. İçinde **İzleyicisi - ölçümleri** penceresi, **kaynak grubu** kaynak grubu izlemek istediğiniz Azure Cosmos DB hesabıyla ilişkili açılan menüsünde seçin. 
4. İçinde **kaynak** veritabanı hesabı izlemek için aşağı açılan menüsünde seçin.
5. Listesinde **kullanılabilir ölçümler**, ölçümleri görüntülemek için seçin. Çoklu seçim için CTRL tuşunu kullanın. 

## <a name="view-performance-metrics-on-the-account-page"></a>Hesap sayfasında görünümü performans ölçümleri
1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **daha Hizmetleri**, kaydırın **veritabanları**, tıklatın **Azure Cosmos DB**ve kendisi için istediğiniz performans ölçümleri görüntülemek üzere Azure Cosmos DB hesap adına tıklayın.
2. **İzleme** Mercek varsayılan olarak aşağıdaki kutucuklara görüntüler:
   
   * Geçerli gün için toplam istek sayısı.
   * Kullanılan depolama alanı.
   
   ![İsteklerin ve depolama kullanımını göstermektedir izleme Mercek ekran görüntüsü](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Sağ üst köşesinde çift oka tıklayarak **istekleri** döşeme açılır bir ayrıntılı **ölçüm** sayfası.
4. **Ölçüm** sayfa toplam istek ayrıntılarını gösterir. 

## <a name="set-up-alerts-in-the-portal"></a>Portal'da uyarılarını ayarlama
1. İçinde [Azure portal](https://portal.azure.com/), tıklatın **daha Hizmetleri**, tıklatın **Azure Cosmos DB**ve kendisi için istediğinizi performansını ayarlamak Azure Cosmos DB hesap adına tıklayın Ölçüm uyarılar.
2. Kaynak menüye tıklayın **uyarı kuralları** uyarı kuralları sayfasını açın.  
   ![Uyarı ekran görüntüsü bölümü seçili kuralları](./media/monitor-accounts/madocdb10.5.png)
3. İçinde **uyarı kuralları** sayfasında, **uyarı Ekle**.  
   ![Uyarı Ekle düğmesi vurgulanan uyarı kuralları sayfasının ekran görüntüsü](./media/monitor-accounts/madocdb11.png)
4. İçinde **uyarı kuralı eklemek** sayfasında, belirtin:
   
   * Ayarladığınız uyarı kuralı adı.
   * Yeni uyarı kuralı açıklaması.
   * Uyarı kuralı ölçümü.
   * Ne zaman uyarı etkinleştirir belirlemek koşul, eşik ve süresi. Örneğin, bir sunucu hatası sayısı 5'ten büyük son 15 dakikadan fazla.
   * Uyarı oluşturulduğunda olup coadministrators ve Hizmet Yöneticisi e-posta gönderilir.
   * Uyarı bildirimleri için ek e-posta adresleri.  
     ![Bir uyarı kuralı sayfasının ekran görüntüsü Ekle](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB programlı olarak izleyin
Hesap depolama kullanım ve toplam istekleri gibi portalında kullanılabilir hesap düzeyindeki ölçümlerini SQL API kullanılabilir değil. Ancak, SQL API'lerini kullanarak koleksiyon düzeyinde kullanım verileri alabilir. Koleksiyon düzeyinde veri almak için aşağıdakileri yapın:

* REST API kullanmak için [koleksiyonda bir GET gerçekleştirmek](https://msdn.microsoft.com/library/mt489073.aspx). Koleksiyon için kota ve kullanım bilgilerini x-ms-resource-quota ve x-ms-kaynak kullanım üstbilgilerini yanıta döndürülür.
* .NET SDK'yı kullanmak için [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) döndürür yöntemi bir [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) gibi bir dizi kullanım özellikleri içeren **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**ve daha fazlası.

Ek ölçümler erişmek için [Azure İzleyici SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Kullanılabilir ölçüm tanımlarını çağırarak alınabilir:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Tek tek ölçümleri almak için sorgu aşağıdaki biçimi kullanın:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Daha fazla bilgi için bkz: [alma kaynak ölçümleri Azure İzleyici REST API'si aracılığıyla](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). "Azure Öngörüler" adlandırıldı Not "Azure İzleyicisi".  Bu blog girdisi eski adına başvuruyor.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB kapasite planlaması hakkında daha fazla bilgi için bkz: [Azure Cosmos DB kapasite Planlayıcısı hesaplayıcı](https://www.documentdb.com/capacityplanner).

