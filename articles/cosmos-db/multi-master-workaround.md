---
title: Bölümlendirme anahtarı sağ seçerek bölgeli yazma ve okuma gerçekleştirmek | Microsoft Docs
description: Bölüm anahtarı seçerek Azure Cosmos DB ile birden çok coğrafi bölgeler arasında yerel okuma ve yazma işlemleri ile uygulama Mimari Tasarım hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/6/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 18f036a259bbec98382927ad1d9e8f654b56850b
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850370"
---
# <a name="perform-multi-region-writes-and-reads-by-choosing-the-right-partitioning-key"></a>Bölümlendirme anahtarı sağ seçerek bölgeli yazma ve okuma gerçekleştirin

Azure Cosmos DB destekleyen anahtar teslimi [genel çoğaltma](distribute-data-globally.md), iş yükünü başka bir yerindeki düşük gecikme süresi erişimi olan birden çok bölgeye verilerini dağıtmak sağlar. Bu model yayımcı/tüketici iş yükleri için yaygın olarak kullanılan bir yazıcı tek bir coğrafi bölge içinde ve genel olarak dağıtılmış okuyucuları (okuma) diğer bölgelerdeki olduğu. 

İçinde yazarlar ve okuyucular genel olarak dağıtılan uygulamaları geliştirmek için Azure Cosmos DB'ın genel çoğaltma desteği de kullanabilirsiniz. Bu belgede Azure Cosmos DB kullanarak dağıtılmış yazarların yerel yazma ve yerel okuma erişimi elde sağlayan bir deseni açıklar.

## <a id="ExampleScenario"></a>İçerik Yayımlama - örnek bir senaryo
Genel olarak dağıtılmış çok-region/çok-ana okuma yazma desenleri Azure Cosmos DB ile nasıl kullanabileceğiniz açıklamak için gerçek dünya senaryosu bakalım. Azure Cosmos DB'de yerleşik bir içerik yayımlama platform göz önünde bulundurun. Bu platform için harika kullanıcı deneyimi Yayımcılar ve tüketicileri için uyması gereken bazı gereksinimler şunlardır.

* Yazarlar ve abonelerin world yayılır 
* Yazarlar kendi yerel (en yakın) bölgesine (yazma) makaleleri yayımlamanız gerekir
* Yazarları okuyucular/dünya çapında dağıtılan aboneleri kendi makalelerin vardır. 
* Yeni makaleler yayımlandığında aboneleri bir bildirim almanız gerekir.
* Aboneler kendi yerel bölgesinden makaleleri okuyabilir olması gerekir. Bunlar ayrıca bu makaleler incelemeler eklemeniz mümkün olması gerekir. 
* Herkes makaleleri yazarı dahil olmak üzere tüm değerlendirmeleri makaleler için yerel bir bölgesinden bağlı mümkün görünümü olmalıdır. 

Kullanıcıları ve yayımcılarına makaleler, milyarlarca ile milyonlarca varsayılarak yakında biz yere göre erişim güvence altına almak birlikte ölçeği sorunları üstesinden gelir gerekir. Çoğu ölçeklenebilirlik sorunları olduğu gibi ile iyi bölümleme stratejisine içinde çözüm arasındadır. Ardından, makaleler, gözden geçirme ve bildirimleri belgeleri olarak model, Azure Cosmos DB hesaplarını yapılandırın ve veri erişim katmanı uygulama ne bakalım. 

Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [bölümlendirme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md).

## <a id="ModelingNotifications"></a>Modelleme bildirimleri
Bildirimler, bir kullanıcı için belirli veri akışları ' dir. Bu nedenle, bildirimler belgeler için erişim düzenlerini her zaman tek bir kullanıcı bağlamında olur. Örneğin, "bir kullanıcıya bir bildirim post" veya "belirli bir kullanıcı için tüm bildirimleri fetch". Bu nedenle, bu türü için anahtar bölümlendirme, en iyi seçenek olacaktır `UserId`.

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

## <a id="ModelingSubscriptions"></a>Modelleme abonelikleri
Abonelikler, makalelerin ilgi alanı veya belirli bir yayımcı belirli bir kategorideki gibi çeşitli ölçütlerine oluşturulabilir. Bu nedenle `SubscriptionFilter` bölüm anahtarı için iyi bir seçimdir.

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <a id="ModelingArticles"></a>Modelleme makaleleri
Bir makale aracılığıyla bildirimleri tanımlandıktan sonra sonraki sorguları genellikle temel alan `Article.Id`. Seçme `Article.Id` bölümü olarak anahtar nedenle makaleleri bir Azure Cosmos DB koleksiyonu içinde depolamak için en iyi dağıtım sağlar. 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }
        
        //... 
    }

## <a id="ModelingReviews"></a>Modelleme incelemeleri
Makaleler gibi incelemeler çoğunlukla yazılmış ve makale bağlamında okuyun. Seçme `ArticleId` bir bölümü olarak anahtar en iyi dağıtım ve makalesiyle ilişkilendirilen incelemeleri, etkili erişim sağlar. 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <a id="DataAccessMethods"></a>Veri erişim katmanı yöntemleri
Şimdi ana veri erişim yöntemleri uygulamak için ihtiyacımız bakalım. Yöntemlerin listesi İşte, `ContentPublishDatabase` gerekir:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Azure Cosmos DB hesabı yapılandırması
Yerel okuma ve yazma, bölüm gerekir güvence altına almak için veri bölüme anahtar, ancak ayrıca temel alınarak coğrafi erişim desenini bölgeleri tam değil. Model bir coğrafi olarak çoğaltılmış Azure Cosmos DB veritabanı hesabı her bölge için sahip kullanır. Örneğin, iki bölgede ile İşte bir kurulum bölgeli yazmalar için:

| Hesap Adı | Yazma Bölgesi | Okuma Bölgesi |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

Aşağıdaki diyagramda, okuma ve yazma işlemleri bu kurulum ile tipik bir uygulamada nasıl gerçekleştirilir gösterilmektedir:

![Azure Cosmos DB birden çok yöneticili mimarisi](./media/multi-master-workaround/multi-master.png)

Nasıl çalışır durumda bir DAL istemcilere başlatılacağını gösteren bir kod parçacığı aşağıda verilmiştir `West US` bölge.
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

Önceki kurulum ile veri erişim katmanı dağıtıldığı üzerinde dayalı yerel hesap tüm yazma işlemlerini iletebilirsiniz. Okuma okunurken verilerin genel görünümünü almak için her iki hesap tarafından gerçekleştirilir. Bu yaklaşım, gerektiği kadar bölgeler için genişletilebilir. Örneğin, üç coğrafi bölgeler Kurulum'a şöyledir:

| Hesap Adı | Yazma Bölgesi | Bölge 1 okuma | Bölge 2 okuma |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Veri erişim katmanı uygulaması
Şimdi iki yazılabilir bölgeleri ile bir uygulama için veri erişim katmanı (DAL) uyarlamasını bakalım. DAL, aşağıdaki adımları uygulamanız gerekir:

* Birden çok örneğini oluşturma `DocumentClient` her hesap için. İki bölgede ile her DAL örneği varsa `writeClient` ve bir `readClient`. 
* Uygulama dağıtılan bölgeyi temel alan, uç noktaları için yapılandırma `writeclient` ve `readClient`. DAL Örneğin, dağıtılmış `West US` kullanan `contentpubdatabase-usa.documents.azure.com` yazma işlemlerini gerçekleştirme. DAL dağıtılmış `NorthEurope` kullanan `contentpubdatabase-europ.documents.azure.com` yazmalar için.

Önceki Kurulum'a veri erişimi yöntemleri uygulanabilir. Karşılık gelen yazma işlemleri iletmek yazma `writeClient`.

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

Bildirimler ve incelemeler okumak için bölgeler hem UNION sonuçları aşağıdaki kod parçacığında gösterildiği gibi okumanız gerekir:

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

Bu nedenle, bir iyi bölümleme anahtar ve hesap tabanlı statik bölümleme seçerek, bölgeli yerel yazma ve okuma Azure Cosmos DB kullanarak elde edebilirsiniz.

## <a id="NextSteps"></a>Sonraki adımlar
Bu makalede, Azure Cosmos Örnek senaryo olarak içerik yayımlama kullanarak DB ile genel dağıtılmış bölgeli okuma yazma desenleri nasıl kullanabileceğiniz açıklanmaktadır.

* Azure Cosmos DB nasıl desteklediği hakkında bilgi edinin [genel dağıtım](distribute-data-globally.md)
* Hakkında bilgi edinin [Azure Cosmos veritabanı otomatik ve el ile yük devretme](regional-failover.md)
* Hakkında bilgi edinin [Azure Cosmos DB ile genel tutarlılık](consistency-levels.md)
* Kullanarak birden fazla bölge ile geliştirme [Azure Cosmos DB - SQL API](tutorial-global-distribution-sql-api.md)
* Kullanarak birden fazla bölge ile geliştirme [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)
* Kullanarak birden fazla bölge ile geliştirme [Azure Cosmos DB - tablo API](tutorial-global-distribution-table.md)
