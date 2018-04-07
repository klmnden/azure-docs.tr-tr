---
title: Sunucusuz veritabanı bilgi işlem - Azure işlevleri ve Azure Cosmos DB | Microsoft Docs
description: Nasıl Azure Cosmos DB ve Azure işlevleri birlikte olay denetimli sunucusuz bilgisayar uygulamaları oluşturmak için kullanılabilir öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 9b1ffe7e63157f86a1cfe643e297c0cb3eb5c235
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-serverless-database-computing-using-azure-functions"></a>Azure Cosmos DB: Azure işlevleri kullanarak sunucusuz veritabanı hesaplama

Sunucusuz computing yinelenebilir ve durum bilgisiz olan tek tek parçaları mantığı üzerinde odaklanma tüm olanağı hakkındadır. Bu parça hiçbir altyapı Yönetimi gerektirir ve bunlar yalnızca saniye veya milisaniye, bunlar için çalıştırmak için kaynaklarını tüketebilir. Azure ekosistemindeki tarafından kullanılabilir kılınan işlevler sunucusuz bilgisayar taşıma özünde olan [Azure işlevleri](https://azure.microsoft.com/services/functions).

Arasında yerel tümleştirme ile [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) ve Azure işlevleri, oluşturabileceğiniz veritabanı tetikleyiciler, giriş bağlamaları ve çıktı bağlamaları doğrudan Azure Cosmos DB hesabınızdan. Azure işlevleri ve Azure Cosmos DB kullanarak oluşturun ve olay denetimli sunucusuz düşük gecikmeli erişim ile temel bir genel kullanıcı için zengin veri dağıtma.

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB ve Azure işlevleri, veritabanları ve sunucusuz uygulamaları aşağıdaki yollarla tümleştirmenize olanak sağlar:

* Olay kaynaklı oluşturma **Azure Cosmos DB tetikleyici** bir Azure işlevinde. Bu tetikleyici dayanan [akış değiştirme](change-feed.md) , Azure Cosmos DB kapsayıcı değişiklikleri izlemek için akışlar. Bir kapsayıcıya herhangi bir değişiklik yapıldığında, Akış akış değişiklik Azure işlevi çağırır tetikleyici gönderilir.
* Alternatif olarak, bir Azure işlevi kullanarak bir Azure Cosmos DB koleksiyonu bağlama bir **bağlama giriş**. Bir işlev çalıştırıldığında giriş bağlamaları kapsayıcıdan verilerini okur.
* Bir Azure Cosmos DB koleksiyonu kullanılarak için bir işlev bağlama bir **bağlama çıktı**. Bir işlev tamamlandığında, çıkış bağlamaları veri bir kapsayıcıya yazma.

> [!NOTE]
> Şu anda Azure Cosmos DB tetikleyicisi, giriş bağlamaları ve çıkış bağlamaları yalnızca SQL API ve Graph API hesaplarıyla çalışır.

Aşağıdaki diyagram bu üç tümleştirmeler her gösterir: 

![Azure Cosmos DB ve Azure işlevlerinin nasıl tümleştirme](./media/serverless-computing-database/cosmos-db-azure-functions-integration.png)

Azure Cosmos DB tetikleyici, giriş bağlama ve çıktı bağlama aşağıdaki bileşimlerde kullanılabilir:
* Bir Azure Cosmos DB tetikleyicisi, farklı bir Azure Cosmos DB kapsayıcı için bir çıktı bağlama ile kullanılabilir. Bir işlev değişikliği akıştaki bir öğe üzerinde bir eylem gerçekleştirdikten sonra (geldiğini aynı kapsayıcı için etkili bir şekilde bir özyinelemeli döngü oluşturacak yazma) başka bir kapsayıcıya yazabilirsiniz. Veya, etkili bir şekilde bir çıkış bağlama kullanarak farklı bir kapsayıcı için bir kapsayıcı değişen tüm öğeleri geçirmek için bir Azure Cosmos DB tetikleyicisi kullanabilirsiniz.
* Giriş bağlamalar ve Azure Cosmos DB için çıktı bağlamaları aynı Azure işlevinde kullanılabilir. Bu, belirli veri giriş bağlama ile bulmak, Azure işlevinde değiştirme ve ardından onu aynı kapsayıcı ya da farklı bir kapsayıcı değiştirme sonrasında kaydetmek istediğinizde iyi durumda çalışır.
* Bir giriş bağlaması bir Azure Cosmos DB kapsayıcısına bir Azure Cosmos DB tetikleyicisi aynı işlevi kullanılabilir ve ile ya da bağlama çıktısı olmadan kullanılabilir. Bu birleşim güncel para birimi exchange bilgilerini (bir giriş bağlaması bir exchange kapsayıcısına oturum çekilen) uygulamak için yeni siparişler değişiklik akışına alışveriş sepeti hizmetinizi kullanabilirsiniz. Güncelleştirilmiş alışveriş sepeti toplam geçerli para birimi dönüştürme uygulanan bir çıktı bağlama kullanarak üçüncü bir kapsayıcıya yazılabilir.

## <a name="use-cases"></a>Uygulama alanları

Aşağıdaki kullanım örnekleri verilerinizden en iyi Azure Cosmos DB - olay tabanlı Azure işlevleri verilerinizi bağlayarak yapabileceğiniz birkaç yolu gösterir.

### <a name="iot-use-case---azure-cosmos-db-trigger-and-output-binding"></a>IOT - Azure Cosmos DB tetikleyici kullanım ve bağlama çıkış

Onay altyapısı ışık bağlı otomobil içinde görüntülendiğinde, IOT uygulamalarında, bir işlev çağırabilirsiniz.

**Uygulama:** Azure Cosmos DB tetikleyici ve bir çıkış bağlama kullanın

1. Bir **Azure Cosmos DB tetikleyici** bağlı otomobil içinde bir gelen onay altyapısı ışık gibi Oto uyarıları ilgili olaylarını tetiklemek için kullanılır.
2. Onay altyapısı ışık geldiğinde, algılayıcı verilerini Azure Cosmos Veritabanına gönderilir.
3. Azure Cosmos DB oluşturur veya yeni algılayıcı verileri belgeleri güncelleştirir ve sonra bu değişiklikleri Azure Cosmos DB tetikleyiciye akışı.
4. Tüm değişiklikleri aracılığıyla akış değişikliği akışı gibi tetikleyici her veri değişikliği algılayıcı veri toplama için çağrılır.
5. Bir eşik koşul işlevinde garanti departmana algılayıcı verileri göndermek için kullanılır.
6. Sıcaklık Ayrıca belirli bir değere ise, bir uyarı da sahibine gönderilir.
7. **Bağlama çıktı** işlev onay altyapısı olay hakkında bilgi depolamak için başka bir Azure Cosmos DB koleksiyonunda araba kaydı güncelleştirir.

Aşağıdaki resimde, bu tetikleyici için Azure Portalı'nda yazılan kod gösterir.

![Azure portalında bir Azure Cosmos DB tetikleyicisi oluşturma](./media/serverless-computing-database/cosmos-db-trigger-portal.png)

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Finans - Zamanlayıcı tetikleyicisi kullanım ve bağlama giriş

Banka hesap bakiyesini belirli bir miktar düştüğünde, finansal uygulamalarında, bir işlev çağırabilirsiniz.

**Uygulama:** Zamanlayıcı tetikleyicisi bir Azure Cosmos DB ile giriş bağlama

1. Kullanarak bir [Zamanlayıcı tetikleyicisi](../azure-functions/functions-bindings-timer.md), aralıklarla kullanarak bir Azure Cosmos DB kapsayıcısındaki depolanan banka hesabı Bakiye bilgilerini alabilir bir **bağlama giriş**.
2. Kullanıcı tarafından ayarlanan Yetersiz bakiye eşiğin altına Bakiye ise, ardından bir eylem ile Azure işlevinden izlenmesi.
3. Çıktı bağlama olabilir bir [SendGrid tümleştirme](../azure-functions/functions-bindings-sendgrid.md) konumuna gönderen bir e-posta hizmet hesabından her yetersiz Bakiye hesapları için tanımlanan e-posta adreslerine.

Aşağıdaki görüntüler bu senaryo için Azure portalında kodu gösterir.

![Finansal senaryosu için Zamanlayıcı tetikleyicisi için Index.js dosyası](./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png)

![Finansal senaryosu için Zamanlayıcı tetikleyicisi için Run.csx dosyası](./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png)

### <a name="gaming-use-case---azure-cosmos-db-trigger-and-output-binding"></a>Oyun - Azure Cosmos DB tetikleyici kullanım ve bağlama çıkış

Yeni bir kullanıcı oluşturulduğunda, oyunlar, bunları kullanarak biliyor olabilirsiniz diğer kullanıcıların arayabilirsiniz [Azure Cosmos DB grafik API'sini](graph-introduction.md). [Azure Cosmos DB SQL veritabanına] kolay alma için sonuçları sonra yazabilirsiniz.

**Uygulama:** Azure Cosmos DB tetikleyici ve bir çıkış bağlama kullanın

1. Bir Azure Cosmos DB kullanarak [grafik veritabanı](graph-introduction.md) tüm kullanıcılar depolamak için bir Azure Cosmos DB tetikleyicisi ile yeni bir işlev oluşturabilirsiniz. 
2. Yeni bir kullanıcı eklenir, işlevi çağrılır ve sonuç kullanılarak depolandığını her bir **bağlama çıktı**.
3. İşlev, yeni bir kullanıcıya doğrudan ilişkili olan ve bu veri kümesi işlevi için döndürür tüm kullanıcıları aramak için grafik veritabanını sorgular.
4. Bu veriler daha sonra daha sonra kolayca yeni kullanıcı bağlı arkadaşlarının gösteren ön uç uygulama tarafından alınabilmesi için bir Azure Cosmos DB depolanır.

### <a name="retail-use-case---multiple-functions"></a>Perakende kullanım örneği - birden çok işlevi

Bir kullanıcı bir öğeyi sepetine eklediğinde perakende uygulamalarında, artık oluşturmak ve isteğe bağlı bir iş ardışık düzeni bileşenleri için işlevleri çağırma esnekliğine sahipsiniz.

**Uygulama:** bir koleksiyona dinleme birden çok Azure Cosmos DB Tetikleyicileri

1. Bunların tümü aynı dinleme Azure Cosmos DB Tetikleyicileri her - alışveriş sepeti veri, akış değiştirmek ekleyerek, birden çok Azure işlevleri oluşturabilirsiniz. Birden çok işlevleri için dinlerken aynı akış değiştirmek, yeni bir kira koleksiyon her işlevi için gerekli dikkat edin. Kira Koleksiyonlar hakkında daha fazla bilgi için bkz: [değişiklik akış işlemci kitaplığı anlama](change-feed.md#understand-cf).
2. Alışveriş sepeti kullanıcılara yeni bir öğe eklendiğinde, her işlev bağımsız olarak alışveriş sepeti kapsayıcıdan akış değişiklik tarafından çağrılır.
    * Bir işlev geçerli sepet içeriğini kullanıcı de ilginizi çekiyor diğer öğeleri görünümünü değiştirmek için kullanabilir.
    * Başka bir işlev stok toplamları güncelleştirebilir.
    * Başka bir işlev müşteri bilgileri belirli bir promosyon kullanılmasının gönderen pazarlama departmanı için ürünleri gönderebilir. 

    Departmanı değişiklik akışına dinleyerek Azure Cosmos DB tetikleyici oluşturun ve kritik sipariş işleminde olayları işlemeyi gecikme olmaz emin olun.

Tüm bunların kullanım örnekleri, işlev uygulamanın kendi ayrılmış olduğundan, her zaman yeni uygulama örneği Döndür gerek yoktur. Bunun yerine, Azure işlevleri gerektiği gibi ayrık işlemleri tamamlamaları için tekil işlevler döner.

## <a name="tooling"></a>Araçları

Azure Cosmos DB ve Azure işlevleri arasında yerel tümleştirme Azure portalında ve Visual Studio 2017'de kullanılabilir.
* Azure işlevleri portalındaki Azure Cosmos DB tetikleyici oluşturabilirsiniz. Hızlı Başlangıç yönergeleri için bkz: [Azure portalda bir Azure Cosmos DB tetikleyicisi oluşturma](https://aka.ms/cosmosdbtriggerportalfunc) ![Azure işlevleri portalda bir Azure Cosmos DB tetikleyicisi oluşturma](./media/serverless-computing-database/azure-function-cosmos-db-trigger.png) 
* Azure işlevleri Portalı'nda tetikleyicileri diğer türleri için de Azure Cosmos DB giriş bağlamaları ve çıktı bağlamaları ekleyebilirsiniz. Hızlı Başlangıç yönergeleri için bkz: [Azure işlevleri ve Cosmos DB kullanarak yapılandırılmamış verileri depolamak](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md).
    ![Azure işlevleri portalda bir Azure Cosmos DB tetikleyicisi oluşturma](./media/serverless-computing-database/function-portal-input-binding.png)
*   Azure Cosmos DB Portalı'nda var olan bir Azure işlevi uygulamaya aynı kaynak grubunda bir Azure Cosmos DB tetikleyicisi ekleyebilirsiniz.
    ![Azure işlevleri portalda bir Azure Cosmos DB tetikleyicisi oluşturma](./media/serverless-computing-database/cosmos-db-portal.png)
* Visual Studio 2017 ' tümleşik şablonu kullanarak bir Azure Cosmos DB tetikleyicisi oluşturabilirsiniz:

    >[!VIDEO https://www.youtube.com/embed/iprndNsUeeg]


## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>Neden Azure işlevleri tümleştirme sunucusuz bilgi işlem için nasıl seçecekler?

Azure işlevleri çalışma veya sağlama veya altyapısını yönetme, isteğe bağlı çalıştırılabilir mantığı kısa parçalarını ölçeklenebilir birim oluşturma olanağı sağlar. Azure işlevleri kullanarak, Azure Cosmos DB veritabanınızdaki değişikliklere yanıt vermesi için Gelişmiş bir uygulama oluşturmak zorunda kalmadan, belirli görevler için küçük yeniden kullanılabilir işlevler oluşturabilirsiniz. Ayrıca, ayrıca Azure Cosmos DB veri girişi veya çıkışı bir Azure işlevi bir HTTP gibi olaya yanıt olarak kullanabileceğiniz istekleri veya zaman aşımına tetikleyici.

Azure Cosmos DB sunucusuz, bilgi işlem mimarisi için önerilen veritabanı şu nedenlerden dolayı şöyledir:

* **Tüm verilerinizi anında erişim**: çünkü depolanan her değer ayrıntılı erişiminiz Azure Cosmos DB [otomatik olarak dizinler](indexing-policies.md) varsayılan olarak, tüm veri ve bu dizinleri hemen kullanılabilir hale getirir. Bu, sürekli olarak sorgu, güncelleştirme ve veritabanınızı yeni öğeler eklemek ve Azure işlevleri aracılığıyla anlık erişime sahip olduğunuz anlamına gelir.

* **Şemasız**. Azure Cosmos DB şemasız - olduğundan hiçbir veri çıkışı bir Azure işlevi benzersiz olarak işleyebilen. Bu "hiçbir şey işlemek" yaklaşım için basit yapar oluşturan çeşitli işlevler Azure Cosmos DB'de tüm çıktı.

* **Ölçeklenebilir işleme**. Üretilen iş yukarı ve aşağı anında Azure Cosmos DB'de genişletilebilir. Yüzlerce veya binlerce sorgulama ve aynı koleksiyona yazma işlevlerin varsa, ölçeği artırabilirsiniz, [RU/s](request-units.md) yükü işlemek üzere. Tüm işlevleri, ayrılmış RU/s kullanarak paralel olarak çalışabilir ve verilerinizi olması garanti [tutarlı](consistency-levels.md).

* **Genel çoğaltma**. Azure Cosmos DB veri çoğaltabilirsiniz [dünya](distribute-data-globally.md) gecikme, coğrafi bulma verilerinizi kullanıcılarınızın bulunduğu en yakın azaltmak için. Olarak tüm Azure Cosmos DB sorgularıyla olay denetimli Tetikleyicileri verilerden veri DB'den Azure Cosmos kullanıcıya en yakın okunur.

Verileri depolamak için Azure işlevleri ile tümleştirmek için arıyorsanız ve derin dizin gerekmez veya ekleri ve medya dosyalarını depolamak gerekirse [Azure Blob Storage tetikleyici](../azure-functions/functions-bindings-storage-blob.md) daha iyi bir seçenek olabilir.

Azure işlevleri yararları: 

* **Olay kaynaklı**. Azure işlevleri olay denetimli ve Azure Cosmos DB'den akışı bir değişiklik dinlemek. Dinleme mantığı oluşturmanız gerekmez, bunun anlamı, yalnızca için dinliyor değişiklikleri takip. 

* **Sınırsız**. İşlevler yürütme paralel ve hizmet dönüş gerektiği kadar yukarı. Parametrelerini ayarlayın.

* **Hızlı Görevler için iyi**. Bir olay tetikler ve işlev tamamlandıktan hemen sonra bunları kapatır her hizmet işlevlerin yeni örneği döner. Yalnızca işlevlerinizi çalıştıran zaman için ödeme yaparsınız.

Akış, Logic Apps, Azure işlevleri veya Web işleri uygulamanız için en iyi olup emin değilseniz bkz [akış, Logic Apps, İşlevler ve Web işleri arasında seçim yapma](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Cosmos DB ve Azure işlevleri şimdi gerçekte bağlanın: 

* [Azure portalında bir Azure Cosmos DB tetikleyicisi oluşturma](https://aka.ms/cosmosdbtriggerportalfunc)
* [Bir Azure Cosmos DB giriş bağlaması ile bir Azure işlevleri HTTP tetikleyicisi oluşturun](https://aka.ms/cosmosdbinputbind)
* [Azure işlevleri ve Cosmos DB kullanarak yapılandırılmamış veri depolayın](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md)
* [Azure Cosmos DB bağlamalar ve Tetikleyicileri](../azure-functions/functions-bindings-cosmosdb.md)


 



