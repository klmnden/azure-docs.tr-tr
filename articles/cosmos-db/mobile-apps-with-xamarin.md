---
title: Xamarin ve Azure Cosmos DB mobil uygulamalar | Microsoft Docs
description: Xamarin iOS, Android veya formlar oluşturan bir Öğreticisi Azure Cosmos DB kullanarak uygulama. Azure Cosmos DB hızlı, planet ölçek, mobil uygulamaları için bulut veritabanı değil.
services: cosmos-db
documentationcenter: .net
author: SnehaGunda
manager: kfile
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: d81343ed894185cb60340f3eccdf2bff2d7ca1e2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Xamarin ve Azure Cosmos DB mobil uygulamaları derleme

Çoğu mobil uygulamaların bulutta verileri depolamak gerekir ve Azure Cosmos DB mobil uygulamaları için bir bulut veritabanıdır. Her şeyi mobil geliştiricinin ihtiyacı vardır. Bu tam olarak yönetilen isteğe bağlı olarak ölçeklenen bir hizmet olarak veritabanıdır. Kullanıcılarınızın dünya bulunan her yerde, verilerinizi uygulamanıza saydam, kullanıma sunabilirsiniz. Kullanarak [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md), orta katman olmadan Azure Cosmos DB ile doğrudan etkileşim kurmak Xamarin mobil uygulamaları etkinleştirebilirsiniz.

Bu makale, Xamarin ve Azure Cosmos DB mobil uygulamaları oluşturmak için bir öğretici sağlar. Öğreticinin tam kaynak kodunu bulabilirsiniz [Xamarin ve Azure Cosmos DB github'da](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), kullanıcıları ve izinleri yönetme de dahil olmak üzere.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Mobil uygulamaları için Azure Cosmos DB özellikleri
Azure Cosmos DB mobil uygulama geliştiricileri için aşağıdaki anahtar yetenekleri sağlar:

![Mobil uygulamaları için Azure Cosmos DB özellikleri](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Şemasız verileri üzerinden zengin sorgular. Azure Cosmos DB verileri heterojen koleksiyonları şemasız JSON belgeleri olarak depolar. Sunduğu [zengin ve hızlı sorguları](sql-api-sql-query.md) şemaları veya dizinleri hakkında endişelenmeye gerek kalmadan.
* Hızlı işleme. Okuma ve yazma Azure Cosmos DB belgeler için yalnızca birkaç milisaniye olarak alır. Geliştiriciler ihtiyaç duydukları, üretilen iş belirtebilirsiniz ve Azure Cosmos DB, tüm tek bölge ve tüm bölgeli hesapları için % 99,99 kullanılabilirlik SLA ile gevşek tutarlılığı geliştirir ve kullanılabilirlik tüm bölgeli veritabanı hesaplarda %99.999 okuma .
* Sınırsız ölçekleme. Azure Cosmos DB koleksiyonlarınızı [uygulamanız büyüdükçe büyümesine](partition-data.md). Küçük boyutlu veriler ve saniye başına istek yüzlerce verimi ile başlayabilirsiniz. Koleksiyonunuz için veri ve büyük verimi petabaytlarca istekleri saniye başına milyonlarca yüzlerce büyüyebilir.
* Genel olarak dağıtılmış. Mobil uygulama hareket, genellikle dünya genelindeki kullanıcılardır. Azure Cosmos veritabanı bir [Genel dağıtılmış veritabanı](distribute-data-globally.md). Verilerinizi kullanıcılarınıza erişilebilir olması için Eşlem'i tıklatın.
* Yerleşik zengin yetkilendirme. Azure Cosmos DB ile gibi popüler düzenleri kolayca uygulayabilirsiniz [kullanıcı başına veri](https://aka.ms/documentdb-xamarin-todouser) veya çok kullanıcılı paylaşılan karmaşık özel yetkilendirme kodu olmadan veri.
* Jeo-uzamsal sorgular. Birçok mobil uygulamaları bugün coğrafi bağlamsal deneyimleri sunar. Birinci sınıf desteğiyle [Jeo-uzamsal türleri](geospatial.md), bu deneyimleri yapmanın kolay oluşturma Azure Cosmos DB yapar.
* İkili dosya eklerini. Uygulama verilerinizi genellikle ikili BLOB'ları içerir. Ekler için yerel destek uygulama verileriniz için bir tane Mağazanız Azure Cosmos DB kullanmayı daha kolay hale getirir.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Azure Cosmos DB ve Xamarin Öğreticisi
Aşağıdaki öğreticide bir mobil uygulama Xamarin ve Azure Cosmos DB kullanarak nasıl oluşturulacağını gösterir. Öğreticinin tam kaynak kodunu bulabilirsiniz [Xamarin ve Azure Cosmos DB github'da](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>başlarken
Azure Cosmos DB ile kullanmaya başlamak kolaydır. Azure Portalı'na gidin ve yeni bir Azure Cosmos DB hesabı oluşturun. Tıklatın **Hızlı Başlangıç** sekmesi. Zaten Azure Cosmos DB hesabınıza bağlı Xamarin Forms Yapılacaklar listesi örnek indirin. 

![Mobil uygulamaları için Azure Cosmos DB hızlı başlangıç](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Veya varolan bir Xamarin uygulaması varsa, ekleyebilirsiniz [Azure Cosmos DB NuGet paketini](sql-api-sdk-dotnet-core.md). Xamarin Forms paylaşılan kitaplıklar ve Xamarin.IOS, Xamarin.Android, Azure Cosmos DB destekler.

### <a name="work-with-data"></a>Verilerle çalışma
Veri kayıtlarını Azure Cosmos DB içinde heterojen koleksiyonları şemasız JSON belgeleri olarak depolanır. Aynı koleksiyonunda farklı yapıları belgelerle depolayabilirsiniz:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

Xamarin projelerinizi şemasız verileri üzerinde dil ile tümleşik sorgu kullanabilirsiniz:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Kullanıcı ekle
Birçok başlatılan örnekleri alın gibi indirdiğiniz Azure Cosmos DB örnek uygulamanın kodu bir ana anahtar sabit kodlanmış kullanarak hizmete kimliğini doğrular. Bu varsayılan, herhangi bir yere dışındaki yerel öykünücüsünde çalıştırmak istediğiniz bir uygulama için iyi bir uygulama değildir. Ana anahtar elde yetkisiz bir kullanıcı varsa, tüm verileri Azure Cosmos DB hesabınızı boyunca tehlikeye girebilir. Bunun yerine, yalnızca oturum açmış kullanıcı kayıtlarını erişmek için uygulamanızı istiyor. Azure Cosmos DB geliştiricilerin uygulama okuma izni veya okuma/izni yazma koleksiyonu, bir bölüm anahtarı göre gruplandırılmış belgeleri ya da belirli bir belge için olanak sağlar. 

Çok kullanıcılı Yapılacaklar listesi uygulaması için yapılacaklar listesi uygulaması değiştirmek için aşağıdaki adımları izleyin: 

  1. Oturum açma, Facebook, Active Directory veya başka bir sağlayıcı kullanarak uygulamanıza ekleyin.

  2. Bir Azure Cosmos DB UserItems koleksiyonuyla oluşturma **/userId** bölüm anahtarı olarak. Koleksiyonunuz için bölüm anahtarı belirtilmesine hızlı sorguları sunmaya devam ederken, uygulama kullanıcılarınızın sayısı arttıkça sonsuz ölçeklendirmek Azure Cosmos DB sağlar.

  3. Azure Cosmos DB kaynak belirteci Aracısı ekleyin. Bu basit Web API, kullanıcıların ve sorunları yalnızca kendi bölümü belgelere erişimi ile oturum açmış kullanıcılar için kısa süreli belirteçler kimliğini doğrular. Bu örnekte, kaynak belirteci Aracısı App Service içinde barındırılır.

  4. Kaynak belirteci Aracısı Facebook ile kimlik doğrulaması için uygulamayı değiştirmek ve oturum açmış Facebook kullanıcılar için kaynak belirteçleri isteyin. Ardından UserItems koleksiyonundaki verilerine erişebilir.  

Bu desene tam kod örneğini bulabilirsiniz [kaynak belirteci Aracısı github'da](http://aka.ms/documentdb-xamarin-todouser). Bu diyagramda çözümü gösterilir:

![Azure Cosmos DB kullanıcılar ve izinler Aracısı](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

İki kullanıcı aynı Yapılacaklar listesi erişmesini isterseniz, kaynak belirteci Aracısı erişim belirteci ek izinler ekleyebilirsiniz.

### <a name="scale-on-demand"></a>İsteğe bağlı olarak ölçeklendirin
Azure Cosmos DB yönetilen bir hizmet olarak veritabanıdır. Kullanıcı tabanınızı büyüdükçe, VM'ler sağlama veya çekirdek artırma hakkında endişelenmeniz gerekmez. Tüm Azure Cosmos DB bildirmek için gereken budur (verim) saniyede kaç işlemi uygulamanız gerekir. Üretilen iş aracılığıyla belirtebilirsiniz **ölçek** verimlilik saniye başına istek birimleri (RUs) adlı bir ölçü kullanarak sekmesi. Örneğin, bir 1 KB belge üzerinde okuma işlemi 1 gerektirir RU. Uyarılar için de ekleyebilirsiniz **işleme** trafiği büyüme izlemek ve yangın uyarıları gibi üretilen işi programlı olarak değiştirmek için ölçüm.

![İsteğe bağlı Azure Cosmos DB ölçek işleme](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Planet ölçek gidin
Uygulama kazançlar popülerliği dünya çapında kullanıcıların elde. Beklenmedik olaylar için hazırlanması istediğiniz olabilir veya. Azure Portalı'na gidin ve Azure Cosmos DB hesabınızı açın. Verilerinizi sürekli çoğaltma herhangi bir sayıda bölgeler dünya genelindeki yapmak için Eşlem'i tıklatın. Bu özellik, kullanıcılarınızın nerede olursa olsun, verilerinizi kullanılabilir hale getirir. Yük devretme ilkeleri olasılıkları için hazırlanması için de ekleyebilirsiniz.

![Coğrafi bölgeler arasında Azure Cosmos DB ölçek](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Tebrikler. Çözüm tamamladınız ve Xamarin ve Azure Cosmos DB mobil uygulamaya sahip. Yerel iOS/Android uygulamaları ve Azure Cosmos DB JavaScript SDK'sı Azure Cosmos DB REST API'lerini kullanarak Cordova uygulamaları geliştirmek için benzer adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Kaynak kodu görüntüleme [Xamarin ve Azure Cosmos DB github'da](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Karşıdan [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md).
* Daha fazla kod örnekleri için bulma [.NET uygulamaları](sql-api-dotnet-samples.md).
* Hakkında bilgi edinin [Azure Cosmos DB zengin sorgu özellikleri](sql-api-sql-query.md).
* Hakkında bilgi edinin [Jeo-uzamsal destek Azure Cosmos veritabanı](geospatial.md).



