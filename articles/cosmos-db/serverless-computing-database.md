---
title: Bilgi işlem sunucusuz veritabanı - Azure işlevleri ve Azure Cosmos DB
description: Nasıl Azure Cosmos DB ile Azure işlevleri birlikte olay temelli sunucusuz bilgi işlem uygulamaları oluşturmak için kullanılabileceğini öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 54de2d2f9b5691a47ff56891185c7655661092dd
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833608"
---
# <a name="serverless-database-computing-using-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ile Azure işlevleri'ni kullanarak sunucusuz veritabanı bilgi işlem

Sunucusuz bilgi işlem tekrarlanabilir ve durum bilgisi olan tek tek parçalarını mantığı üzerinde odaklanma tüm olanağı hakkındadır. Bu parçaları bir altyapı Yönetimi gerektirir ve saniye veya milisaniye, bunlar için çalıştırmak için yalnızca kullandıkları kaynaklar. Azure ekosistemindeki tarafından kullanıma sunulan işlevleri, sunucusuz bilgi işlem taşıma özünde olan [Azure işlevleri](https://azure.microsoft.com/services/functions). Diğer Azure görüyor sunucusuz yürütme ortamları hakkında bilgi edinmek için [azure'da sunucusuz](https://azure.microsoft.com/solutions/serverless/) sayfası. 

Arasında yerel tümleştirme ile [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) ve Azure işlevleri, oluşturabileceğiniz veritabanı tetikleyici, giriş bağlamaları ve çıkış bağlamaları doğrudan Azure Cosmos DB hesabınızdan. Azure işlevleri ve Azure Cosmos DB kullanarak oluşturun ve genel kullanıcı tabanı için zengin veri düşük gecikme süreli erişim ile olay tabanlı sunucusuz uygulamalar dağıtın.

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB ile Azure işlevleri, sunucusuz uygulamalar ve veritabanları aşağıdaki yollarla tümleştirmenize olanak etkinleştir:

* Olay temelli oluşturma **Azure Cosmos DB tetikleyicisi** bir Azure işlev. Bu tetikleyiciyi dayanan [değişiklik akışını](change-feed.md) Azure Cosmos DB kapsayıcınız için değişiklikleri izlemek üzere akışları. Bir kapsayıcıya herhangi bir değişiklik yapıldığında, değişiklik akışı akışı Azure işlevi çağırır tetikleyici gönderilir.
* Alternatif olarak, bir Azure işlevi kullanarak bir Azure Cosmos DB kapsayıcısı bağlama bir **giriş bağlama işlemini**. Bir işlev çalıştırıldığında giriş bağlamaları bir kapsayıcıdan verileri okuyamadı.
* Bind bir işlevi kullanarak bir Azure Cosmos DB kapsayıcısı bir **çıktı bağlaması**. Bir işlev tamamlandığında, çıkış bağlamaları veri bir kapsayıcıya yazma.

> [!NOTE]
> Şu anda, Azure Cosmos DB tetikleyicisi, giriş bağlamaları ve çıkış bağlamaları yalnızca SQL API'si ile kullanım için desteklenir. Tüm diğer Azure Cosmos DB API'leri için API'niz için statik bir istemci kullanarak veritabanı işlevinizden erişmelidir.


Aşağıdaki diyagram, her biri üç bu tümleştirmelere gösterir: 

![Azure Cosmos DB ile Azure işlevleri nasıl tümleştirin](./media/serverless-computing-database/cosmos-db-azure-functions-integration.png)

Azure Cosmos DB tetikleyicisi, giriş bağlama ve çıkış bağlaması aşağıdaki birleşimler kullanılabilir:
* Bir Azure Cosmos DB tetikleyicisi, farklı bir Azure Cosmos DB kapsayıcısı için bir çıkış bağlaması ile kullanılabilir. Bir işlev, değişiklik akışı bir öğe üzerinde bir eylem gerçekleştirdikten sonra (aynı kapsayıcı geldiği için etkili bir şekilde yinelemeli döngü oluşturursunuz yazma) başka bir kapsayıcıya yazabilirsiniz. Veya bir Azure Cosmos DB tetikleyicisi bir kapsayıcıdan değişen tüm öğeleri bir çıkış bağlaması kullanarak farklı bir kapsayıcıya etkili bir şekilde geçirmek için kullanabilirsiniz.
* Giriş bağlamaları ve çıkış bağlamaları Azure Cosmos DB için aynı Azure işlevinde kullanılabilir. Aynı kapsayıcı ya da farklı bir kapsayıcıya değiştirme sonrasında kaydedin giriş bağlama işlemiyle belirli verileri bulmak ve Azure işlevinde değiştirmek istediğinizde bu iyi durumda çalışır.
* Bir Azure Cosmos DB kapsayıcısı için bir giriş bağlaması aynı işlevi bir Azure Cosmos DB tetikleyicisi olarak kullanılabilir ve ile veya bir çıkış de bağlaması olmadan kullanılabilir. Bu birleşim güncel para birimi exchange bilgilerini (bir exchange kapsayıcısı için bir giriş bağlaması oturum çekilen) uygulamak için yeni siparişler değişiklik akışını alışveriş sepeti hizmetinde kullanabilirsiniz. Güncelleştirilmiş alışveriş sepeti toplam uygulanan geçerli para birimi dönüştürme işlemine bir çıkış bağlaması kullanan üçüncü bir kapsayıcıya yazılabilir.

## <a name="use-cases"></a>Uygulama alanları

Aşağıdaki kullanım örnekleri - Azure Cosmos DB verilerinizi en iyi olay kaynaklı Azure işlevleri'ne verilerinize bağlanarak yapabileceğiniz birkaç yolu gösterilmektedir.

### <a name="iot-use-case---azure-cosmos-db-trigger-and-output-binding"></a>IOT kullanım örneği - Azure Cosmos DB tetikleyicisi ve çıktı bağlaması

Onay altyapısı ışık bağlı Arabada görüntülendiğinde, IOT uygulamalarında, bir işlevi çağırabilirsiniz.

**Uygulama:** Bir Azure Cosmos DB tetikleyicisi ve bir çıkış bağlaması kullanın

1. Bir **Azure Cosmos DB tetikleyicisi** bağlı Arabada yakında onay altyapısı ışığı gibi araba uyarılara olaylarını tetiklemek için kullanılır.
2. Onay altyapısı ışık söz konusu olduğunda, algılayıcı verilerini Azure Cosmos DB için gönderilir.
3. Azure Cosmos DB oluşturur veya yeni algılayıcı verileri belgeleri güncelleştirir ve sonra bu değişiklikleri için Azure Cosmos DB tetikleyicisi aktarılır.
4. Değişiklik akışı akışı yapılan tüm değişiklikler gibi tetikleyici algılayıcı veri toplama için her veri değişikliği çağrılır.
5. Eşik koşuluna işlevinde garanti departmanına sensör verilerini göndermek için kullanılır.
6. Sıcaklık aynı zamanda belirli bir değerin üzerinde ise bir uyarı da sahibine gönderilir.
7. **Çıktı bağlaması** işlev denetimi altyapısı olayla ilgili bilgileri depolamak için başka bir Azure Cosmos DB kapsayıcısındaki araba kaydı güncelleştirir.

Aşağıdaki görüntüde Bu tetikleyici için Azure Portalı'nda yazılan kod gösterilmektedir.

![Azure portalında bir Azure Cosmos DB tetikleyicisi oluşturma](./media/serverless-computing-database/cosmos-db-trigger-portal.png)

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Finans kullanım örneği - Zamanlayıcı tetikleyici ve bağlama giriş

Belirli bir miktar altında bir banka hesabı Bakiye düştüğünde, finansal uygulamalarında, bir işlevi çağırabilir.

**Uygulama:** Bir zamanlayıcı tetikleyicisi ile Azure Cosmos DB giriş bağlama

1. Kullanarak bir [Zamanlayıcı tetikleyicisi](../azure-functions/functions-bindings-timer.md), aralıklarla kullanarak bir Azure Cosmos DB kapsayıcısında depolanan banka hesabı Bakiye bilgilerini alabilirsiniz bir **giriş bağlama işlemini**.
2. Bakiye düşük Bakiye eşiğin altında kullanıcı tarafından ayarlanan ise, ardından bir işlemle Azure işlevini iletişim kurun.
3. Çıkış bağlaması olabilir bir [SendGrid tümleştirme](../azure-functions/functions-bindings-sendgrid.md) , bir e-posta gönderen bir hizmet hesabından her düşük Bakiye hesapları için tanımlanan e-posta adresleri.

Aşağıdaki kod bu senaryo için Azure Portalı'nda gösterilmektedir.

![Finansal senaryosu için bir zamanlayıcı tetikleyicisi için Index.js dosyasını](./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png)

![Run.csx dosyasının bir finansal senaryosu için bir zamanlayıcı tetikleyicisi için](./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png)

### <a name="gaming-use-case---azure-cosmos-db-trigger-and-output-binding"></a>Oyun kullanım örneği - Azure Cosmos DB tetikleyicisi ve çıktı bağlaması

Yeni bir kullanıcı oluşturulduğunda, oyun, bunları kullanarak biliyor olabilirsiniz diğer kullanıcılar arayabilirsiniz [Azure Cosmos DB Gremlin API](graph-introduction.md). Ardından sonuçları kolayca almak için [Azure Cosmos DB SQL veritabanı] yazabilirsiniz.

**Uygulama:** Bir Azure Cosmos DB tetikleyicisi ve bir çıkış bağlaması kullanın

1. Bir Azure Cosmos DB kullanarak [grafik veritabanı](graph-introduction.md) tüm kullanıcılar depolamak için bir Azure Cosmos DB tetikleyicisi ile yeni bir işlev oluşturabilirsiniz. 
2. Yeni bir kullanıcı eklenir, işlev çağrılır ve sonuç kullanarak depolanır her bir **çıktı bağlaması**.
3. Grafik veritabanı, yeni bir kullanıcıya doğrudan ilişkili ve söz konusu veri kümesi işlevi için döndüren tüm kullanıcılar için aranacak işlev sorgular.
4. Bu veriler daha sonra daha sonra kolayca yeni kullanıcı bağlı arkadaşlarının gösteren ön uç uygulama tarafından alınabilmesi için bir Azure Cosmos DB depolanır.

### <a name="retail-use-case---multiple-functions"></a>Perakende kullanım örneği - birden çok işlevi

Bir kullanıcı bir öğeyi sepetine eklediğinde perakende uygulamalarında, şimdi oluşturun ve isteğe bağlı bir iş ardışık düzeni bileşenleri için işlevleri çağırmak için esnekliğiniz vardır.

**Uygulama:** Bir kapsayıcıya dinleyen birden çok Azure Cosmos DB tetikleyici

1. Her - Azure Cosmos DB Tetikleyicileri tümü aynı dinleme değişiklik, alışveriş sepeti veri akışını ekleyerek, birden çok Azure işlevleri oluşturabilirsiniz. Birden çok işlevleri için dinlerken aynı değişiklik akışını, her işlev için yeni bir kira koleksiyonu gereklidir dikkat edin. Kira Koleksiyonlar hakkında daha fazla bilgi için bkz. [değişiklik akışı işlemci kitaplığı anlama](change-feed-processor.md).
2. Alışveriş sepeti kullanıcılara yeni bir öğe eklendiğinde, her işlevin bağımsız olarak değişiklik alışveriş sepeti kapsayıcıdan akışı tarafından çağrılır.
   * Bir işlev, geçerli sepet içeriğini kullanıcı ilgisini diğer öğelerin görünümünü değiştirmek için kullanabilirsiniz.
   * Başka bir işleve stok toplamları güncelleştirebilir.
   * Başka bir işleve müşteri bilgilerini belirli ürünleri bunları promosyon çocuğu gönderen pazarlama departmanı için gönderebilir. 

     Her departman için değişiklik akışını dinleyen tarafından bir Azure Cosmos DB tetikleyicisi oluşturma ve kritik sipariş işleme olayları sürecinde gecikme olmaz emin olun.

Tüm bunların kullanım örnekleri, işlev uygulaması ayrılmış olduğundan, yeni uygulama örnekleri her zaman döndürme gerekmez. Bunun yerine, Azure işlevleri, gerektiği şekilde parçalı işlemleri tamamlamaları için tekil işlevler döner.

## <a name="tooling"></a>Araç kullanımı

Azure Cosmos DB ile Azure işlevleri arasında yerel tümleştirme, Azure portalında ve Visual Studio 2019 kullanılabilir.

* Azure işlevleri portalda bir Azure Cosmos DB tetikleyicisi oluşturabilirsiniz. Hızlı yönergeler için bkz: [Azure portalında bir Azure Cosmos DB tetikleyicisi oluşturma](https://aka.ms/cosmosdbtriggerportalfunc).
* Azure Cosmos DB portalında bir Azure Cosmos DB tetikleyicisi, aynı kaynak grubunda var olan bir Azure işlev uygulaması ekleyebilirsiniz.
* Visual Studio 2019 ' kullanarak bir Azure Cosmos DB tetikleyicisi oluşturabilirsiniz [Azure işlevleri Araçları](../azure-functions/functions-develop-vs.md):

    >[!VIDEO https://www.youtube.com/embed/iprndNsUeeg]

## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>Sunucusuz bilgi işlem için Azure işlevleri tümleştirmesi neden seçmeliyim?

Azure işlevleri, ölçeklenebilir iş ya da kısa parçalarını isteğe bağlı olarak sağlama veya altyapı yönetmenize gerek kalmadan çalıştırılabilir mantıksal birim oluşturma olanağı sağlar. Azure işlevleri'ni kullanarak, Azure Cosmos DB veritabanınıza değişikliklere yanıt vermek için tam bir uygulama oluşturmanız gerekmez, belirli görevler için yeniden kullanılabilir küçük işlevler oluşturabilirsiniz. Ayrıca, Azure Cosmos DB veri giriş veya çıkış için bir Azure işlevi gibi bir HTTP olaya yanıt olarak kullanabilirsiniz istekleri veya zamanlanmış bir tetikleyici.

Azure Cosmos DB, aşağıdaki nedenlerden dolayı sunucusuz bilgi işlem mimarisi için önerilen veritabanı şöyledir:

* **Tüm verilerinizi anında erişim**: Her bir değeri olduğundan depolanan ayrıntılı erişiminiz Azure Cosmos DB [otomatik olarak dizinleyen](index-policy.md) varsayılan olarak, tüm veri ve bu dizinleri hemen kullanılabilir hale getirir. Başka bir deyişle, sürekli olarak sorgu, güncelleştirme ve yeni öğeler eklemek için veritabanı ve Azure işlevleri aracılığıyla anında erişebilirsiniz.

* **Şemasız**. Herhangi bir Azure işlevi veri çıkışı benzersiz bir şekilde işleyebilen, bu nedenle azure Cosmos DB, şemasız - olduğu. Bu "hiçbir şey handle" yaklaşım için basit kılar oluşturan çeşitli işlevler tüm Azure Cosmos DB çıktı.

* **Ölçeklenebilir aktarım hızı**. Aktarım hızı, Azure Cosmos DB'de yukarı ve aşağı bir anında ölçeklendirilebilir. Yüzlerce veya binlerce sorgulama ve aynı kapsayıcıya yazma işlevleri varsa ölçeğini artırabilirsiniz, [RU/sn](request-units.md) yükü işlemek için. Tüm İşlevler, ayrılmış RU/sn kullanarak işlemi paralel olarak çalışabilir ve verilerinizin olması garanti [tutarlı](consistency-levels.md).

* **Genel çoğaltma**. Azure Cosmos DB veri çoğaltabilirsiniz [dünyanın farklı yerlerindeki](distribute-data-globally.md) verilerinizi kullanıcılarınızın bulunduğu konumlara yakın gecikme süresi, coğrafi azaltmak için. Olarak tüm Azure Cosmos DB sorgular ile olay temelli Tetikleyiciler verilerden veriler kullanıcıya en yakın Azure Cosmos DB'den okunur.

Verileri depolamak için Azure işlevleri ile tümleştirmek için arıyorsanız ve ayrıntılı dizin gerekmez veya ekleri ve medya dosyalarını depolamak gerekiyorsa [Azure Blob Depolama tetikleyicisi](../azure-functions/functions-bindings-storage-blob.md) daha iyi bir seçenek olabilir.

Azure işlevleri avantajları: 

* **Olay temelli**. Azure işlevleri, olay temelli ve bir değişiklik Azure Cosmos DB'den akışı dinleyebilirsiniz. Dinleme mantıksal oluşturmanız gerekmez, bunun anlamı, yalnızca için dinleme değişikliklerin takip. 

* **Sınırsız**. İşlevler, paralel ve hizmet yürütme dönüş gereksinim duyduğunuz kadar çok ayarlama. Parametrelerini ayarlayın.

* **Hızlı Görevler için iyi**. Bir olayı tetikler ve işlev tamamlandıktan hemen sonra bunları kapatır her hizmet işlevleri yeni örneklerini sanal makineleri çalıştırır. Yalnızca, işlevleriniz gerektirdikçe süre için ödeme yaparsınız.

Flow, Logic Apps, Azure işlevleri ve WebJobs uygulamanız için en iyi olup emin değilseniz bkz [Flow, Logic Apps, İşlevler ve Web işleri arasında seçim yapma](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Cosmos DB ile Azure işlevleri için gerçek yeniliklerden: 

* [Azure portalında bir Azure Cosmos DB tetikleyicisi oluşturma](https://aka.ms/cosmosdbtriggerportalfunc)
* [Bir Azure Cosmos DB giriş bağlama işlemiyle Azure işlevleri HTTP tetikleyicisi oluşturma](https://aka.ms/cosmosdbinputbind)
* [Azure Cosmos DB bağlamalar ve tetikleyiciler](../azure-functions/functions-bindings-cosmosdb.md)


 



