---
title: "Performansı ve ölçeği dayanıklı işlevlerinde - Azure"
description: "Azure işlevleri dayanıklı işlevleri uzantısı için giriş."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 10ce74097388a0283797e4692126c5039e8d4dd0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Performansı ve ölçeği dayanıklı işlevlerinde (Azure işlevleri)

Performans ve ölçeklenebilirlik iyileştirmek için benzersiz ölçeklendirme özelliklerini anlamak önemlidir [dayanıklı işlevleri](durable-functions-overview.md).

Ölçek davranışlarını anlamak için bazı dayanıklı işlevler tarafından kullanılan temel Azure depolama sağlayıcısı ayrıntılarını anlamak zorunda.

## <a name="history-table"></a>Geçmiş tablosu

Geçmiş tablosu tüm orchestration örnekleri için geçmiş olaylarını içeren bir Azure Storage tablodur. Örnekleri çalıştırmak gibi yeni satırlar bu tabloya eklenir. Bu tablonun bölüm anahtarı orchestration örneği Kimliğinden türetilir. Bu değerleri çoğu durumda, rastgele da Azure Storage iç bölüm en iyi dağıtım sağlar.

## <a name="internal-queue-triggers"></a>İç queue Tetikleyicileri

Orchestrator işlevler ve etkinlik işlevleri hem işlevi uygulamanın varsayılan depolama hesabı iç sıralarda tarafından tetiklenir. Dayanıklı işlevleri kuyruklarda iki tür vardır: **denetim sırası** ve **çalışma öğesi kuyruk**.

### <a name="the-work-item-queue"></a>Çalışma öğesi sırası

Bir çalışma öğesi kuyruk dayanıklı işlevleri görev hub başına yoktur. Bu temel bir sıra ve diğer benzer şekilde davranır `queueTrigger` Azure işlevlerinde sırası. Bu sıra durum bilgisiz tetiklemek için kullanılan *etkinlik işlevleri*. Ne zaman dayanıklı işlevleri uygulama çıkışı birden çok VM, tüm iş öğesi sırasından iş edinmeye rekabet bu VM'ler için ölçeklendirir.

### <a name="control-queues"></a>Denetim kuyrukların

*Denetim sırası* daha basit çalışma öğesi kuyruk daha karmaşık değil. Durum bilgisi olan orchestrator işlevleri tetiklemek için kullanılır. Orchestrator işlevi örnekleri durum bilgisi olan teklileri olduğundan, VM'ler üzerindeki yük dağıtmak için bir rakip tüketici modeli kullanmak mümkün değil. Bunun yerine, orchestrator iletileri, yük dengeli arasında birden çok denetim sıralar. Sonraki bölümlerde daha fazla ayrıntı bu.

Denetim kuyruklar çeşitli orchestration yaşam döngüsü ileti türlerini içerir. Örnekler [orchestrator denetim iletileri](durable-functions-instance-management.md), etkinlik işlevi *yanıt* iletileri ve Zamanlayıcı iletileri.

## <a name="orchestrator-scale-out"></a>Orchestrator genişletme

Etkinlik işlevleri, durum bilgisiz ve otomatik olarak VM'ler ekleyerek ölçeğini. Orchestrator, diğer yandan işlevlerdir *bölümlenmiş* arasında bir veya daha fazla denetim sıralar. Denetim sıraların sayısı sabittir ve yük oluşturmaya başlamak sonra değiştirilemez.

(Genellikle üzerinde farklı VM'ler) birden çok işlev konak örneklerine ölçeğini, her bir örnek bir kilit denetim sıraları birinde alır. Bu kilit orchestration örneği aynı anda yalnızca tek bir VM üzerinde çalışır sağlar. Bu görev hub üç denetim kuyruklarla yapılandırılırsa, orchestration örnekleri olarak en fazla üç VM'ler üzerindeki yük dengeli olabileceği anlamına gelir. Etkinlik işlevi yürütme kapasiteyi artırmak için ek sanal makineleri eklenebilir.  Ancak, ek kaynaklar orchestrator işlevleri çalıştırmak için kullanılmayacak.

Aşağıdaki diyagramda, Azure işlevleri konak genişletilmiş bir ortamda depolama varlıkları ile nasıl etkileşim kurduğu gösterilmektedir.

![Ölçek diyagramı](media/durable-functions-perf-and-scale/scale-diagram.png)

Gördüğünüz gibi tüm VM'ler iletileri iş öğesi sırasına rekabet. Ancak, yalnızca üç VM'ler denetim sıralardan iletileri elde edebilir ve tek bir denetim sırası her VM kilitler.

Orchestration'ın örnek kimliği karşı bir dahili karma işlevi çalıştırarak denetim sıra örneklerinde dağıtılmış Orchestration örnekleri Örnek kimlikleri otomatik olarak oluşturulan ve rastgele varsayılan örnekleri tüm kullanılabilir denetim kuyrukta dengelendiğini sağlar. Geçerli desteklenen denetim sıra bölümleri varsayılan sayısıdır **4**.

> [!NOTE]
> Bölüm sayısı Azure işlevlerinde yapılandırmak şu anda olası değil. [Bu yapılandırma seçeneği desteklemek üzere iş izlenen](https://github.com/Azure/azure-functions-durable-extension/issues/73).

Genel olarak, orchestrator işlevleri basit olacak şekilde tasarlanmıştır ve bilgi işlem gücü çok gerekli değildir. Bu nedenle, çok sayıda büyük verimi almak için Denetim sıra bölüm oluşturmak gerekli değildir. Bunun yerine, en ağır iş sonsuz Genişletilebilir durum bilgisiz etkinlik işlevlerde yapılır.

## <a name="auto-scale"></a>Otomatik Ölçeklendirme

Tüm Azure tüketim planda çalışan işlevlerine ile dayanıklı işlevleri otomatik ölçek aracılığıyla destek gibi [Azure işlevlerini ölçeklendirme-denetleyicisi](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). Ölçek denetleyicisi ekleme veya VM kaynakları uygun şekilde kaldırma iş öğesi sırası uzunluğu ve her denetim sıraların izler. Denetim sırası uzunlukları zaman içerisinde arttığını, Ölçek denetleyicisi denetim sıra bölüm sayısı ulaşana kadar örnekleri ekleme devam eder. Çalışma öğesi kuyruk uzunluğu zaman içerisinde arttığını, Ölçek denetleyicisi denetim sıra bölüm sayısı bağımsız olarak yük eşleştirebilirsiniz kadar VM kaynakları ekleme devam eder.

## <a name="thread-usage"></a>İş parçacığı kullanımı

Orchestrator işlevleri tek bir iş parçacığı üzerinde yürütülür. Bu, orchestrator işlevi yürütme kararlı olduğundan emin olmak için gereklidir. Bu durum dikkate alınarak, hiçbir zaman orchestrator işlev iş parçacığı engelleme ya da işlemleri dönmesini (hangi çeşitli nedenlerden dolayı yasaklanmış) g/ç gibi görevleri ile gereksiz yere meşgul saklanacak önemlidir. Herhangi bir iş engellemek, g/ç gerektirebilir veya birden çok iş parçacığı etkinliği işlevlerini taşınması gereken.

Etkinlik işlevleri hepsi aynı davranışları normal sıra tetiklemeli işlevleri olarak var. Bu, bunlar güvenli bir şekilde g/ç yapmak için CPU yoğunluklu işlemlerini yürütmek ve birden çok iş parçacığı kullanma anlamına gelir. Etkinlik Tetikleyicileri durum bilgisiz olduğundan, bunlar serbestçe çıkışı VM'ler sınırsız bir sayıya ölçeklendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevleri uzantısı ve örnekleri yükleyin](durable-functions-install.md)
