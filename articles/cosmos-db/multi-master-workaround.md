---
title: Çok bölgeli yazma ve okuma anahtar bölümleme sağ seçerek gerçekleştirmek | Microsoft Docs
description: Bir bölüm anahtarı seçerek birden çok coğrafi bölgede Azure Cosmos DB ile uygulama mimarileri yerel okuma ve yazma işlemleri ile tasarlama hakkında bilgi edinin.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/6/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d38b7cd7d1f28f706e94782602926abc8fc11e3
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056427"
---
# <a name="perform-multi-region-writes-and-reads-by-choosing-the-right-partitioning-key"></a>Çok bölgeli yazma ve okuma anahtar bölümleme sağ seçerek gerçekleştirin

Azure Cosmos DB destekler hazır [küresel çoğaltma](distribute-data-globally.md), verileri iş yüküne herhangi bir yerinden düşük gecikme süreli erişim ile birden çok bölgeye dağıtma sağlar. Bu model yayımcı/tüketici iş yükleri için yaygın olarak kullanılan tek bir coğrafi bölgede bir yazıcı ve de (okuma) başka bölgelerde küresel olarak dağıtılan okuyucular olduğu. 

Azure Cosmos DB'nin genel çoğaltma desteği, Yazıcılar ve okuyucular küresel olarak dağıtılan uygulamalar oluşturmak için de kullanabilirsiniz. Bu belge, Azure Cosmos DB kullanarak dağıtılmış yazarlar için yerel yazma ve yerel okuma erişimi elde sağlayan bir desen özetlenmektedir.

## <a id="ExampleScenario"></a>İçerik Yayımlama - örnek bir senaryo
Azure Cosmos DB ile küresel olarak dağıtılan çok-region/çok-ana okuma yazma desenleri nasıl kullanacağınızı açıklayan bir gerçek dünya senaryosu bakalım. Azure Cosmos DB üzerinde oluşturulmuş bir içerik yayımlama platform göz önünde bulundurun. Bu platform, Yayımcılar ve tüketiciler için harika bir deneyim için karşılaması gereken bazı gereksinimler aşağıda verilmiştir.

* Yazarlar hem de aboneler dünyayı yayılır 
* Yazarlar kendi yerel (yakın) bölgeye (yazma) makaleleri yayımlamanız gerekir
* Yazarlar okuyucular/dünya çapında dağıtılan aboneleri, makale sahip. 
* Aboneler, yeni bir makale yayımlandığında bir bildirim almanız gerekir.
* Aboneleri yerel bölgelerinden makaleleri okuyabilirsiniz olması gerekir. Ayrıca bu makaleler için gözden geçirmeleri eklemek çözebilmeleri. 
* Herhangi bir makale yazarı dahil olmak üzere mümkün görünümü tüm incelemeleri makaleler için yerel bir bölgeden bağlı olmalıdır. 

Kullanıcıları ve yayımcılarına makaleler, milyarlarca ile milyonlarca varsayılarak yakında yerleşim yeri erişim güvence altına almak birlikte, Ölçek sorunları üstesinden gelir sunuyoruz. Çoğu ölçeklenebilirlik sorunları gibi ile iyi bir bölümleme stratejisinde çözüm arasındadır. Ardından, makaleler, gözden geçirme ve bildirimleri belgeleri olarak model, Azure Cosmos DB hesap yapılandırın ve bir veri erişim katmanı nasıl göz atalım. 

Bölümleme ve bölüm anahtarları hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).

## <a id="ModelingNotifications"></a>Modelleme bildirimleri
Belirli bir kullanıcıya veri akışları bildirimlerdir. Bu nedenle, bildirimleri belgeleri için erişim desenleri, her zaman tek bir kullanıcı bağlamında alır. Örneğin, "bir kullanıcıya bir bildirim post" veya "belirli bir kullanıcı için tüm bildirimler fetch". Bu nedenle, bu türü için anahtar bölümlemenin en uygun seçenek olacaktır `UserId`.

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

## <a id="ModelingSubscriptions"></a>Abonelikleri modelleme
Faiz ya da belirli bir yayıncı makaleler belirli bir kategorisi gibi çeşitli ölçütleri için abonelik oluşturulamaz. Bu nedenle `SubscriptionFilter` bölüm anahtarı için iyi bir seçimdir.

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
Bir makale bildirimleri belirlendiğinde, sonraki sorgular genellikle dayalı `Article.Id`. Seçme `Article.Id` bölümü olarak anahtar bu nedenle makale bir Azure Cosmos DB koleksiyonu içinde depolamak için en iyi dağıtım sağlar. 

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
Makaleleri gibi incelemeleri çoğunlukla yazılır ve makale bağlamında okuyun. Seçme `ArticleId` en iyi dağıtım ve verimli erişim gözden geçirmeleri makalesiyle ilişkilendirilen anahtar bölümü olarak sağlar. 

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

## <a id="DataAccessMethods"></a>Veri erişim katmanı yöntemi
Artık ana veri erişim yöntemlerine ihtiyacımız uygulamak için göz atalım. Yöntemleri listesi aşağıda verilmiştir, `ContentPublishDatabase` gerekir:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Azure Cosmos DB hesabı yapılandırması
Yerel okuma ve yazma işlemleri, biz bölüm güvence altına almak için veri bölüme anahtar, ancak ayrıca göre coğrafi erişim modelini bölgeleri tam değil. Bir coğrafi olarak çoğaltılmış Azure Cosmos DB veritabanı hesabını her bölge için açık olması modeli kullanır. Örneğin, iki bölgeleri ile İşte bir kurulum için çok bölgeli yazma:

| Hesap Adı | Yazma Bölgesi | Okuma Bölgesi |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

Okuma ve yazma işlemleri bu kurulum ile tipik bir uygulamada nasıl gerçekleştirilir, aşağıdaki diyagramda gösterilmiştir:

![Azure Cosmos DB çok yöneticili mimarisi](./media/multi-master-workaround/multi-master.png)

İşte nasıl başlatılacağını çalışan bir DAL istemcilere gösteren bir kod parçacığı `West US` bölge.
    
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

Önceki kurulum ile veri erişim katmanı, dağıtıldığı üzerinde temel yerel hesap için tüm yazma işlemlerini iletebilir. Okuma, verileri genel görünümünü elde etmek için her iki hesap okunurken tarafından gerçekleştirilir. Bu yaklaşım çok bölgesini gerektiği şekilde genişletilebilir. Örneğin, üç farklı coğrafi bölgesine bir kurulumla şu şekildedir:

| Hesap Adı | Yazma Bölgesi | Okuma bölgesi 1 | Okuma bölgesi 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Veri erişim katmanı uygulaması
Şimdi iki yazılabilir bölge ile bir uygulama için veri erişim katmanı (DAL) uygulamasını göz atalım. DAL, aşağıdaki adımları uygulamanız gerekir:

* Birden çok örneğini oluşturma `DocumentClient` her hesap için. İki bölgeleri ile her DAL örneği varsa `writeClient` diğeri `readClient`. 
* Uygulamanın dağıtılan bölgeye göre yapılandırma uç noktaları `writeclient` ve `readClient`. DAL gibi dağıtılmış `West US` kullanan `contentpubdatabase-usa.documents.azure.com` yazma işlemleri gerçekleştirmek için. DAL dağıtılmış `NorthEurope` kullanan `contentpubdatabase-europ.documents.azure.com` yazma için.

Önceki kurulum ile veri erişim yöntemlerine uygulanabilir. Yazma işlemleri, karşılık gelen yazma iletmek `writeClient`.

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

Bildirimler ve gözden geçirmeler okumak için hem bölge hem de birleşim sonuçları aşağıdaki kod parçacığında gösterildiği gibi okuması gerekir:

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

Bu nedenle, iyi bir bölümleme anahtarı ve hesap tabanlı statik bölümleme seçerek, çok bölgeli yerel yazma ve okuma Azure Cosmos DB kullanarak elde edebilirsiniz.

## <a id="NextSteps"></a>Sonraki adımlar
Bu makalede, bir örnek senaryo içerik yayımlama kullanarak Azure Cosmos DB ile küresel olarak dağıtılan çok bölgeli okuma yazma desenleri nasıl kullanabileceğiniz açıklanmaktadır.

* Azure Cosmos DB nasıl desteklediği hakkında bilgi edinin [genel dağıtım](distribute-data-globally.md)
* Hakkında bilgi edinin [Azure Cosmos DB'de otomatik ve el ile yük devretme](regional-failover.md)
* Hakkında bilgi edinin [Azure Cosmos DB ile genel tutarlılık](consistency-levels.md)
* Birden fazla bölgeye kullanarak geliştirme [Azure Cosmos DB - SQL API'si](tutorial-global-distribution-sql-api.md)
* Birden fazla bölgeye kullanarak geliştirme [Azure Cosmos DB - MongoDB API'si](tutorial-global-distribution-MongoDB.md)
* Birden fazla bölgeye kullanarak geliştirme [Azure Cosmos DB - tablo API'si](tutorial-global-distribution-table.md)
