---
title: Tanılama ve Azure Cosmos DB tetikleyicisi Azure işlevleri'nde kullanırken sorunlarını giderme
description: Yaygın sorunlar, geçici çözümler ve Azure işlevleri ile Azure Cosmos DB tetikleyicisi kullanırken tanılama adımları
author: ealsur
ms.service: cosmos-db
ms.date: 04/16/2019
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: e8f0b9c8bf1bfb846f13306f58bcb1721ed6b422
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510536"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-trigger-in-azure-functions"></a>Tanılama ve Azure Cosmos DB tetikleyicisi Azure işlevleri'nde kullanırken sorunlarını giderme

Bu makalede kullanırken sık karşılaşılan sorunları ve geçici çözümler tanılama adımları kapsar [Azure Cosmos DB tetikleyicisi](change-feed-functions.md) Azure işlevleri ile.

## <a name="dependencies"></a>Bağımlılıklar

Azure Cosmos DB tetikleyicisini ve bağlamalarını uzantı paketleri temel Azure işlevleri çalışma zamanı üzerinde bağlıdır. Düzeltmeleri ve karşılaşabileceğiniz olası sorunları gidermeye yeni özellikler içerebilir gibi her zaman bu paketlerin güncelleştirilmesi sakla:

* Azure işlevler V2 için bkz: [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB).
* Azure işlevleri V1 için bkz: [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Çalışma zamanı belirtildiği zaman, bu makalede açıkça belirtilmediği sürece her zaman Azure işlevler V2'ye başvuracaktır.

## <a name="consume-the-azure-cosmos-db-sdk-independently"></a>Bağımsız olarak Azure Cosmos DB SDK'sını kullanma

Uzantı paketinin temel işlevselliğini bağlamaları ve Azure Cosmos DB tetikleyicisi için destek sağlamaktır. Ayrıca [Azure Cosmos DB .NET SDK'sı](sql-api-sdk-dotnet-core.md), Azure Cosmos DB ile tetikleyicisini ve bağlamalarını kullanmadan program aracılığıyla etkileşim kurmak istiyorsanız yararlı olduğu.

Varsa Azure Cosmos DB SDK'sını kullanmak istediğiniz başka bir NuGet paketi başvurusu projenize eklemeyin emin olun. Bunun yerine, **Azure işlevleri uzantı paketi çözmek SDK başvurusu izin**. Bağlamalar ve tetikleyici listesinden Azure Cosmos DB SDK'sını kullanma

Ayrıca, kendi örneğini el ile oluşturuyorsanız [Azure Cosmos DB SDK istemcisi](./sql-api-sdk-dotnet-core.md), istemci yalnızca bir örneği olan desenini izlemelidir [bir Singleton deseni yaklaşımı kullanarak](../azure-functions/manage-connections.md#documentclient-code-example-c) . Bu işlem, işlemlerinizin içinde olası yuva sorunlarından kurtulursunuz.

## <a name="common-scenarios-and-workarounds"></a>Yaygın senaryolar ve geçici çözümler

### <a name="azure-function-fails-with-error-message-collection-doesnt-exist"></a>Azure işlevi başarısız olduğunda hata iletisi koleksiyonuyla yok

Azure işlevi, hata iletisi ile başarısız oluyor "ya da kaynak koleksiyonu 'koleksiyon-adı' ('veritabanı adı' veritabanında) veya 'collection2-name' ('Veritabanı2-name' veritabanında) kira koleksiyonu yok. Her iki koleksiyon dinleyici başlamadan önce mevcut olması gerekir. Otomatik olarak kira koleksiyonu oluşturmak için 'CreateLeaseCollectionIfNotExists' 'true' olarak Ayarla"

Bu, bir veya tetikleyici çalışması gerekli Azure Cosmos kapsayıcıları her ikisi de mevcut değil veya Azure işlevi için erişilebilir durumda olmadığı anlamına gelir. **Hangi Azure Cosmos veritabanı hata bildirir ve kapsayıcılar olan aranırken tetikleyici** yapılandırmanızı temel alarak.

1. Doğrulama `ConnectionStringSetting` özniteliği ve BT'nin **başvuran Azure işlev uygulamanızda var olan bir ayar**. Bu öznitelik değeri, bağlantı dizesi, ancak yapılandırma ayarının adı olmamalıdır.
2. Doğrulayın `databaseName` ve `collectionName` Azure Cosmos hesabınızdaki mevcut. Otomatik değer değişimi kullanıyorsanız (kullanarak `%settingName%` desenleri), Azure işlev uygulamanızda ayarının adı var olduğundan emin olun.
3. Belirtmezseniz bir `LeaseCollectionName/leaseCollectionName`, "kiralamalar" varsayılandır. Bu tür kapsayıcı var olduğunu doğrulayın. İsteğe bağlı olarak ayarlayabileceğiniz `CreateLeaseCollectionIfNotExists` Tetikleyicinize özniteliğinde `true` otomatik olarak oluşturmak için.
4. Doğrulayın, [Azure Cosmos hesabın güvenlik duvarı yapılandırması](how-to-configure-firewall.md) , olmadığını görmek için görmek için Azure işlevi engellemediğinden.

### <a name="azure-function-fails-to-start-with-shared-throughput-collection-should-have-a-partition-key"></a>Azure işlev "paylaşılan aktarım hızı koleksiyon bölüm anahtarı olmalıdır" ile başlaması başarısız olur

Azure Cosmos DB uzantısı önceki sürümlerini kullanarak içinde oluşturulmuş bir kira kapsayıcı desteklememektedir bir [paylaşılan aktarım hızı veritabanı](./set-throughput.md#set-throughput-on-a-database). Bu sorunu çözmek için güncelleştirme [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) en son sürümünü almak için uzantı.

### <a name="azure-function-fails-to-start-with-the-lease-collection-if-partitioned-must-have-partition-key-equal-to-id"></a>Azure işlev "kira koleksiyonu bölümlenmiş, bölüm anahtarı kimliğine eşit olmalıdır." ile başlaması başarısız olur

Bu hata, geçerli kiraları kapsayıcınızı bölümlenmiş, ancak bölüm anahtar yolu değil anlamına gelir `/id`. Bu sorunu çözmek için kira kapsayıcısı ile yeniden oluşturmanız gerekir `/id` bölüm anahtarı olarak.

### <a name="you-see-a-value-cannot-be-null-parameter-name-o-in-your-azure-functions-logs-when-you-try-to-run-the-trigger"></a>Gördüğünüz bir "değer null olamaz. Parametre adı: o "tetikleyici çalıştırmayı denediğinizde, Azure işlevleri günlüklerinde

Azure portalını kullanıyorsanız ve seçmeyi deneyin. Bu sorunu görünür **çalıştırma** tetikleyiciyi kullanan bir Azure işlevi incelerken, ekranda düğmesi. Tetikleyici gerektirmez başlatmak için Çalıştır'ı seçmeniz için bir Azure işlevi dağıtıldığında otomatik olarak başlatılacak. Azure portalında Azure işlevinin günlük akışı denetleyin, yalnızca izlenen kapsayıcınıza gidin ve bazı yeni öğeler eklemek istiyorsanız, tetikleyici yürütülürken otomatik olarak görürsünüz.

### <a name="my-changes-take-too-long-be-received"></a>Çok uzun My değişiklikleri Al alınması

Bu senaryo, birden çok nedeni olabilir ve bunların tümünde iade edilmelidir:

1. Azure işlevinizi, Azure Cosmos hesabınızla aynı bölgede dağıtılır? İçin en iyi ağ gecikme süresi, hem Azure işlevi hem de Azure Cosmos hesabınızı aynı Azure bölgesinde birlikte.
2. Değişiklikleri, Azure Cosmos kapsayıcısında sürekli veya ara sıra olay meydana gelir?
İkincisi ise depolanmakta olan değişiklikleri ve bunları çekme Azure işlevi arasındaki bir gecikme olabilir. Tetikleyici değişiklikler Azure Cosmos kapsayıcınızda denetler ve bekleyen hiçbiri okunacak bulduğunda dahili olarak, bu süre (varsayılan 5 saniye) yapılandırılabilir bir süre (yüksek RU kullanımını önlemek) yeni değişiklikler iade etmeden önce uyku olmasıdır. Aracılığıyla bu bekleme süresini yapılandırabilirsiniz `FeedPollDelay/feedPollDelay` ayarı [yapılandırma](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) , tetikleyicisinin (milisaniye olarak değeri beklenir).
3. Azure Cosmos kapsayıcınızı olabilir [oranı sınırlı](./request-units.md).
4. Kullanabileceğiniz `PreferredLocations` tetikleyicinizin bir özel tercih edilen bağlantı düzenini tanımlamak için Azure bölgelerinin virgülle ayrılmış bir listesini belirtmek için özniteliği.

### <a name="some-changes-are-missing-in-my-trigger"></a>Bazı değişiklikler my tetikleyici yok

Bazı Azure Cosmos kapsayıcınızı gerçekleşen değişiklikler Azure işlevi tarafından alınmakta olan değil olduğunu fark ederseniz, gerçekleşmesi gereken bir ilk araştırma adım yoktur.

Azure işlevinizi değişiklikleri aldığında, bu genellikle işler ve isteğe bağlı olarak olabilir, başka bir hedefe sonucu Gönder. Eksik değişiklikleri incelerken emin olun **hangi değişikliklerin alımı noktada alındığından ölçü** (Azure işlevi başladığında) hedefte değil.

Bazı değişiklikler hedefte eksik olduğunda, bu, değişiklikleri alınan sonra Azure işlevi yürütme sırasında gerçekleşen işlemleri bazı hata olduğu anlamına gelebilir.

Bu senaryoda en iyi eylem planını eklemektir `try/catch blocks` öğelerinin belirli bir alt kümesi için herhangi bir hata algılama ve bunları uygun şekilde işlemek için değişiklikleri işleme döngü içindeydi ve kodunuzda (başka bir depolama için başka şirketlerde Analiz veya yeniden deneme). 

> [!NOTE]
> Azure Cosmos DB tetikleyicisi, kod yürütülürken işlenmeyen bir özel durum oluştuysa varsayılan olarak, toplu değişiklikler yeniden olmaz. Bu değişiklikler hedefte ulaşmasını değil neden olduğundan bunları işlemekte başarısız olan anlamına gelir.

İse, bulma, bazı değişiklikler, tetikleyici tarafından hiç alınmadı, en sık karşılaşılan bir senaryodur olan **başka bir Azure işlevi çalışan**. Azure'da dağıtılan başka bir Azure işlevi olabilir veya yerel olarak Geliştirici makine çalıştıran bir Azure işlevi olan **tam olarak aynı yapılandırmayı** (aynı izlenen ve kira kapsayıcılar) ve bu Azure işlevi çalarak bir Azure işlevinizi işlenecek beklediğiniz değişiklikleri alt kümesi.

Ayrıca, senaryoya doğrulandı, kaç Azure işlev uygulaması örnekleri biliyor varsa çalışıyor. Kira kapsayıcınızı inceleyin ve öğeleri içinde ayrık değerlerini sayısını `Owner` bunları özelliğinde işlev uygulamanızın örnek sayısına eşit olmalıdır. Bilinen bir Azure işlev uygulaması örnekleri değerinden daha fazla sahipleri varsa, bu ek sahipleri bir "değişiklikleri çalarak" olduğunu gösterir.

Bu durum geçici çözüm kolay bir yolu olan uygulamak için bir `LeaseCollectionPrefix/leaseCollectionPrefix` işlevinize yeni/farklı bir değerle veya alternatif olarak, yeni bir kira kapsayıcı ile test edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure işlevleri için izlemeyi etkinleştir](../azure-functions/functions-monitoring.md)
* [Azure Cosmos DB .NET SDK'sı sorunlarını giderme](./troubleshoot-dot-net-sdk.md)