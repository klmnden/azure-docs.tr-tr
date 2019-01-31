---
title: Azure Cosmos DB değişiklik akışı ile Azure işlevleri'ni kullanma
description: Kullanım Azure Cosmos DB değişiklik akışı ile Azure işlevleri
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 93cd93b40c142d504c52f08f9005d082fb5a2a20
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469490"
---
# <a name="trigger-azure-functions-from-azure-cosmos-db"></a>Azure işlevleri Azure Cosmos DB tetikleyicisi

Azure işlevleri'ni kullanıyorsanız, eklemek için değişiklik akışını bağlanmak için en kolay yolu olan bir [Azure Cosmos DB tetikleyicisi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger) Azure işlevleri uygulamanıza. Bir Azure işlev uygulaması Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için Cosmos kapsayıcıyı seçin ve kapsayıcıdaki bir şey değiştiğinde işlevi tetiklenir.

Azure işlevleri Portalı'nda Tetikleyiciler oluşturulabilir veya Azure Cosmos DB portalında veya program aracılığıyla. Daha fazla bilgi için [Azure Cosmos DB ile Azure işlevleri'ni kullanarak sunucusuz veritabanı computing](serverless-computing-database.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region"></a>Belirli bir bölgeden okumak için Azure işlevleri nasıl yapılandırabilirim?

Tanımlamak mümkündür [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations) bölgelerin bir listesini belirtmek için Azure Cosmos DB tetikleyicisi kullanırken. Tetikleyici, tercih edilen bölgelerden okuma yapmak için ConnectionPolicy özelleştirdiğiniz sırada aynıdır. İdeal olarak, Azure işlevleri dağıtıldığı en yakın bölgeden okumak istediğiniz.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Azure işlevleri'nde toplu varsayılan boyutu nedir?

Azure işlevleri'nin her çağırma için 100 öğe varsayılan boyutudur. Ancak, bu sayı function.json dosyasında yapılandırılabilir. İşte tam [yapılandırma seçeneklerinin listesi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Yerel olarak geliştiriyorsanız local.settings.json dosyasında uygulama ayarlarını güncelleştirin.

### <a name="i-am-monitoring-a-container-and-reading-its-change-feed-however-i-dont-get-all-the-inserted-documents-some-items-are-missing"></a>Kapsayıcı izleme ve kendi değişiklik akışı okuma, ancak alamıyorum eklenen tüm belgelerin, bazı öğeler eksik?

Aynı kira kapsayıcısı ile aynı kapsayıcı okuma diğer Azure işlev olduğundan emin olun. Eksik belgeleri, diğer Azure da aynı kira kullanıyorsanız işlevleri tarafından işlenir.

Bu nedenle, aynı değişiklik akışını okumak için birden çok Azure işlevleri oluşturuyorsanız bunlar farklı kira kapsayıcılar kullanma veya aynı kapsayıcıyı paylaşan için "leasePrefix" yapılandırmasını kullanın gerekir. Ancak, değişiklik akışı işlemci kitaplığı kullandığınızda Azure işlevinizi birden çok örneğini başlayabilir ve SDK Belgeleri otomatik olarak farklı örnekleri arasında böler.

### <a name="azure-cosmos-item-is-updated-every-second-and-i-dont-get-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Azure Cosmos öğesi saniyede güncelleştirilir ve tüm değişiklikleri Azure işlevleri'nde değişiklik akışını dinleme alamıyorum?

Azure işlevleri, değişiklik değişiklikler için sürekli olarak 5 saniye ile bir varsayılan en fazla gecikme akışı yoklar. İşlev okumak için bekleyen bir değişiklik varsa veya varsa bekleyen değişiklikleri tetikleyici uygulandıktan sonra bunları hemen okur. Ancak hiçbir bekleyen değişiklik varsa, işlev 5 saniye ve daha fazla değişiklikler için yoklama.

Belgenizi yeni değişikliklerini yoklamak için tetikleyici geçen aynı aralığa birden çok değişiklik alırsa, belge ve Ara bir en son sürümünü alabilirsiniz.

Değişiklik akışı 5 saniyeden kısa bir süre için yoklama istiyorsanız, örneğin, her saniye için yoklama süresi "feedPollDelay" yapılandırmak, bkz: [yapılandırmanın tamamı](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations). Varsayılan değer 5000 milisaniye cinsinden tanımlanır. 1 saniyeden az mümkündür, ancak daha fazla CPU belleği'ni kullanmaya başlar çünkü önerilmez için yoklanıyor.

### <a name="can-multiple-azure-functions-read-one-containers-change-feed"></a>Birden çok Azure işlevleri, bir kapsayıcının değişiklik akışı okuyabilir miyim?

Evet. Birden çok Azure işlevleri aynı kapsayıcının değişiklik akışı okuyabilirsiniz. Ancak, Azure işlevleri "tanımlanan bir ayrı leaseCollectionPrefix" olması gerekir.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-in-a-batch-of-10-documents-and-i-get-an-error-at-seventh-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-document-ie-seventh-document-in-my-next-feed"></a>İşleme 10 belgeleri toplu Azure işlevleri'ni kullanarak değişiklik akışı ve yedinci belgeye bir hata alıyorum. Bu durumda son üç belgeleri nasıl işleme başarısız belge (yani, yedinci belge) sonraki akışınıza github'da işlenmez?

Hatayı işlemek için kodunuzu try-catch bloğu ile sarmalayın ve belgelerin, liste üzerinde yineleme, her yineleme kendi try-catch bloğu içinde sarmalamak için önerilen Düzen olur. Hata catch ve belge bir sıra (edilemeyen) koyun ve mantık hatası üretilen belgelerle tanımlayabilirsiniz. 200 belge toplu ve başarısız, tek bir belge varsa bu yöntemle, hemen tüm batch throw gerekmez.

Durumda, hata, Kontrol noktasına geri başına geri değil başka bu belgeleri değişiklik akışı'ndan almaya devam. Unutmayın, değişiklik akışı tutar, bu nedenle belgelerin son son ek görüntüsü belgedeki önceki anlık görüntüye kaybedebilir. değişiklik akışı yalnızca bir belgenin son sürümünü tutar ve arasında diğer işlemleri gelir ve belgeyi değiştirme.

Kod çözme tutmak gibi eski ileti sırası üzerinde hiçbir belge yakında bulabilirsiniz. Azure işlevleri, değişiklik akışı sistem tarafından otomatik olarak çağrılır ve denetim noktası Azure işlevi tarafından dahili olarak korunur. Denetim noktası geri alma ve her yönüyle denetlemek istiyorsanız, kullanarak değişiklik akışı işlemci SDK düşünmelisiniz.

### <a name="are-there-any-extra-costs-for-using-the-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyicisi kullanmak için ek maliyetler var mı?

Azure Cosmos DB tetikleyicisi, değişiklik akışı işlemci kitaplığı dahili olarak yararlanır. Bu nedenle, durum ve kısmi kontrol noktaları korumak için kira koleksiyon olarak adlandırılan, ek bir koleksiyonu gerektirir. Bu durum yönetimi, dinamik olarak ölçeklendirin ve, Azure işlevleri'ni durdurun ve daha sonra işleme devam etmek istemeniz durumunda devam edebilmek için gereklidir. Daha fazla bilgi için bkz. [değişiklik ile çalışma konusunda akışı işlemci Kitaplığı](change-feed-processor.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışı genel bakış](change-feed.md)
* [Değişiklik akışını okumak için yollar](read-change-feed.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
* [Değişiklik ile çalışma konusunda akışı işlemci kitaplığı](change-feed-processor.md)
* [Azure Cosmos DB ile Azure işlevleri'ni kullanarak sunucusuz veritabanı bilgi işlem](serverless-computing-database.md)
