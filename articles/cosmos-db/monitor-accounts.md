---
title: Azure Cosmos DB performans ve depolama ölçümlerini izleyin
description: Azure Cosmos DB hesabınız için istekleri ve sunucu hataları gibi performans ölçümlerini ve depolama alanı tüketimi gibi kullanım ölçümleri izlemeyi öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 02bbde9a2d744c79cc8a7e95b0732b775c4dc695
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "66241619"
---
# <a name="monitor-performance-and-storage-metrics-in-azure-cosmos-db"></a>Azure Cosmos DB performans ve depolama ölçümlerini izleyin

Azure Cosmos DB hesaplarınızda izleyebilirsiniz [Azure portalında](https://portal.azure.com/). Her Azure Cosmos DB hesabı için aktarım hızı, depolama, kullanılabilirlik, gecikme süresi ve tutarlılık izlemek eksiksiz ölçümleri kullanılabilir.

Hesap sayfasında, yeni ölçümler sayfası veya Azure İzleyici ölçümleri gözden geçirilebilir.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Ölçümleri sayfasında performans ölçümlerini görüntüleme
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **tüm hizmetleri**, kaydırma **veritabanları**, tıklayın **Azure Cosmos DB**ve ardından Azure adına tıklayın Cosmos DB hesabı için performans ölçümlerini görüntülemek istiyorsunuz.
2. Yeni Sayfa yüklediğinde, kaynak menüsünün altında **izleme**, tıklayın **ölçümleri**.
3. Ölçümler sayfası açıldığında, gelen gözden geçirmek istediğiniz koleksiyonu seçmek **koleksiyonlar** açılır.

   Azure portalında mevcut olan koleksiyon ölçümler paketini görüntüler. Aktarım hızı, depolama, kullanılabilirlik, gecikme süresi ve tutarlılık ölçümler üzerinde ayrı sekmeler verildiğini unutmayın. Sağlanan üst köşesindeki çift oku tıklatın ölçümlere ilişkin ek ayrıntılar her ölçümleri bölmesinde sağ almak için.

   ![Ölçüm paketi gösteren izleme lens ekran görüntüsü](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Azure izleme kullanarak performans ölçümlerini görüntüleme
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **İzleyici** sol taraftaki çubukta.
2. Kaynak menüden **ölçümleri**.
3. İçinde **İzleyici - ölçümler** penceresi, **kaynak grubu** kaynak grubunu, izlemek istediğiniz Azure Cosmos DB hesabı ile ilişkili açılan menüsünde seçin. 
4. İçinde **kaynak** veritabanı hesabı izlemek için açılan menüsünde seçin.
5. Listesinde **kullanılabilir ölçümler**, görüntülenecek bir ölçüm seçin. Çoklu seçme için CTRL tuşunu kullanın. 

## <a name="view-performance-metrics-on-the-account-page"></a>Hesap sayfasındaki performans ölçümlerini görüntüleme
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **tüm hizmetleri**, kaydırma **veritabanları**, tıklayın **Azure Cosmos DB**ve ardından Azure adına tıklayın Cosmos DB hesabı için performans ölçümlerini görüntülemek istiyorsunuz.
2. **İzleme** lens varsayılan olarak aşağıdaki kutucuklara görüntüler:
   
   * Geçerli gün için toplam istek sayısı.
   * Kullanılan depolama alanı.
   
   ![İstekler ve depolama kullanımını gösteren izleme lens ekran görüntüsü](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Sağ üst tarafındaki çift-oka tıklayarak **istekleri** kutucuk ayrıntılı bir açılır **ölçüm** sayfası.
4. **Ölçüm** sayfası toplam istek ayrıntılarını gösterir. 

## <a name="set-up-alerts-in-the-portal"></a>Portalında uyarıları ayarlama
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **tüm hizmetleri**, tıklayın **Azure Cosmos DB**ve ardından, istediğiniz performansını ayarlamak Azure Cosmos DB hesabının adına tıklayın Ölçüm uyarıları.
2. Kaynak menüden **uyarı kuralları** uyarı kuralları sayfasını açmak için.  
   ![Seçili uyarı kuralları bölümünün ekran görüntüsü](./media/monitor-accounts/madocdb10.5.png)
3. İçinde **uyarı kuralları** sayfasında **uyarısı Ekle**.  
   ![Uyarı Ekle düğmesi vurgulanmış uyarı kuralları sayfasının ekran görüntüsü](./media/monitor-accounts/madocdb11.png)
4. İçinde **bir uyarı kuralı Ekle** sayfasında, belirtin:
   
   * Ayarladığınız uyarı kuralı adı.
   * Yeni uyarı kuralı açıklaması.
   * Ölçüm için uyarı kuralı.
   * Ne zaman uyarı etkinleştirir belirlemek koşulu, eşiği ve dönem. Örneğin, bir sunucu hatası sayısı 5'ten büyük son 15 dakika boyunca.
   * Uyarı tetiklendiğinde olup diğer yöneticiler ve Hizmet Yöneticisi e-posta gönderilir.
   * Uyarı bildirimleri için ek e-posta adresleri.  
     ![Bir uyarı kuralı Sayfası Ekle ekran görüntüsü](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB program aracılığıyla izleyin
Hesap düzeyindeki ölçümleri hesap depolama kullanım ve toplam istekleri gibi Portalı'nda SQL API'leri üzerinden kullanılabilir değil. Ancak, SQL API'leri kullanarak koleksiyon düzeyinde kullanım verileri alabilir. Koleksiyon düzeyi verileri almak için aşağıdakileri yapın:

* REST API'sini kullanmayı [koleksiyonunda bir GET gerçekleştirmek](https://msdn.microsoft.com/library/mt489073.aspx). Koleksiyon kotası ve kullanım bilgileri yanıtındaki x-ms-resource-quota ve x-ms-resource-kullanım üstbilgileri döndürülür.
* .NET SDK'yı kullanmak için [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) döndüren yöntemi bir [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) gibi çeşitli kullanım özellikleri içeren  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**ve daha fazlası.

Ek ölçümlere erişmek için kullanmanız [Azure İzleyici SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Insights). Kullanılabilir ölçüm tanımlarını çağrılarak alınabilir:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Tek tek ölçümleri almak için sorgu aşağıdaki biçimi kullanın:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Daha fazla bilgi için [Azure İzleyici REST API aracılığıyla kaynak ölçümleri alınırken](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). "Azure Insights" adlandırıldı Not "Azure İzleyici".  Bu blog girişi eski adıdır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB kapasite planlaması hakkında daha fazla bilgi için bkz: [Azure Cosmos DB kapasite Planlayıcısı hesaplayıcı](https://www.documentdb.com/capacityplanner).

