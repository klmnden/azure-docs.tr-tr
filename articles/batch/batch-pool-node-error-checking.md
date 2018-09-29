---
title: Batch havuzu ve düğüm hatalarını denetleme
description: Hataları denetlemek için ve havuzları ve düğümler oluştururken önlemek yapma
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: bc09089fa9984e9042166938da19499afca21509
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435224"
---
# <a name="checking-for-pool-and-node-errors"></a>Havuz ve düğüm hatalarını denetleme

Batch havuzlarını yönetme, hemen gerçekleşmeye işlemler vardır ve vardır zaman uyumsuz oluştururken ve anında, olmayan işlemler arka planda çalışmaya ve tamamlanması birkaç dakika sürebilir.

Tüm hataları hemen API, CLI veya kullanıcı Arabirimi tarafından döndürülecek gibi hemen gerçekleşmesi işlemleri için hataların algılanması, basittir.

Bu makalede havuzlar ve havuz düğümleri için gerçekleştirilebildiği arka plan işlemleri kapsar: hataları nasıl algılanabilir ve hataların nasıl önlenebilir belirtir.

## <a name="pool-errors"></a>Havuzu hataları

### <a name="resize-timeout-or-failure"></a>Yeniden boyutlandırma zaman aşımı veya hatası

Yeni bir havuz oluşturma veya var olan bir havuzu, düğümlerin hedef sayısı yeniden boyutlandırma zaman belirtilir.  İşlem hemen tamamlanır, ancak yeni düğümlerin gerçek ayırma veya var olan düğümleri kaldırma işlemi arka planda ne birkaç dakika olabilir üzerinden gerçekleşir.  Yeniden boyutlandırma zaman aşımı belirtilir [oluşturma](https://docs.microsoft.com/rest/api/batchservice/pool/add) veya [yeniden boyutlandırma](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API'si - düğümlerin hedef sayısını yeniden boyutlandırma zaman aşımı süresi içinde aldıysanız sonra işlemi durdurur kararlı bir duruma geçmeden havuzu ve yeniden boyutlandırma hatası sahip.

Yeniden boyutlandırma zaman aşımı tarafından bildirilen [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) gerçekleşen bir veya daha fazla hataları listeler Son Değerlendirme özelliği.

Yeniden boyutlandırma zaman aşımı için yaygın nedenler şunlardır:
- Yeniden boyutlandırma zaman aşımı çok kısa
  - Varsayılan zaman aşımı 15, ayrılmış veya kaldırılacak havuz düğümleri için normalde bol zaman olduğu dakikadır.
  - Çok sayıda (1000'den düğüm bir Market görüntüsünden) veya özel bir görüntüden 300'den düğümlerinin düğümleri havuzu oluşturma veya yeniden boyutlandırma sırasında ayırırken 30 dakikalık zaman aşımı tavsiye edilir.
- Yeterli çekirdek kotası
  - Bir batch hesabı, çekirdek sayısı için bir kota sahip tüm havuzlardaki ayrılmış.  Bu kotasına ulaşıldı sonra batch düğüm ayrılamadı.  Çekirdek kotası [artırılabilir](https://docs.microsoft.com/azure/batch/batch-quota-limit) ayrılacak daha fazla düğüm etkinleştirmek için.
- Yetersiz IP alt ağı, bir [havuzu, sanal ağ içinde değil](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Bir sanal ağ alt ağı yeterli olmalıdır. istenen tüm havuz düğümleri için ayrılacak IP adresleri atanmamış, aksi takdirde düğümleri oluşturulamıyor.
- Kaynaklar yetersiz olduğunda bir [havuzu, sanal ağ içinde değil](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Yük dengeleyicileri, genel IP'ler ve Nsg'ler gibi kaynakları, Batch hesabı oluşturmak için kullanılan abonelik oluşturulur.  Abonelik kotaları bu kaynaklar için yeterli olmalıdır.
- Büyük havuzlar için özel bir VM görüntüsü kullanma
  - Özel görüntüleri kullanarak büyük havuzları ayırmak ve zaman aşımları yeniden boyutlandırmak için daha uzun sürer ortaya çıkabilir.  Limitler ve yapılandırma önerileri sağlanan bir [belirli bir makaleye](https://docs.microsoft.com/azure/batch/batch-custom-images). 

### <a name="auto-scale-failures"></a>Otomatik ölçeklendirme hataları

Açıkça bir havuz için düğümlerin hedef sayısını havuzu oluşturma veya yeniden boyutlandırma ayarlamak yerine, bir havuzdaki düğümlerin sayısını otomatik olarak ölçeklendirilebilir.  Bir [otomatik ölçeklendirme formülü için bir havuz oluşturulabilir](https://docs.microsoft.com/azure/batch/batch-automatic-scaling), hangi değerlendirilir normal yapılandırılabilir bir aralıkta havuz için düğümlerin hedef sayısını ayarlamak için.  Aşağıdaki türde sorunları oluşabilir ve algılandı:

- Otomatik ölçek değerlendirme başarısız olabilir.
- Sonuçta elde edilen yeniden boyutlandırma işlemi başarısız olabilir ve zaman aşımı.
- Yanlış düğümü hedef değerleri için önde gelen çalışma veya zaman aşımına uğruyor yeniden boyutlandırma ile otomatik ölçeklendirme formülü ile ilgili bir sorun olabilir.

Son Otomatik ölçek değerlendirme hakkında bilgi kullanılarak elde edilir [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) değerlendirme, değerleri ve deneme ve değerlendirme gerçekleştiriliyor herhangi bir hata sonucu zamanında raporları özelliği.

Tüm değerlendirmeleri hakkında bilgi tarafından yakalanan otomatik olarak bir [havuz yeniden boyutlandırma tamamlama olayı](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event).

### <a name="delete"></a>Sil

Bir havuzda düğümleri olan bir havuz ilk olarak, silinen düğüm işlemi sonuçlarında silip varsayılarak havuzu nesnesinin kendisi tarafından izlenen.  Bu, silinecek havuz düğümleri için birkaç dakika sürebilir.

[Havuzu durumu](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) 'silme işlemi sırasında siliniyor' ayarlayın.  Çağıran uygulama havuzunu silme 'state' ve 'stateTransitionTime' özelliklerini kullanarak çok uzun sürüyor durumunda algılayabilir.

## <a name="pool-compute-node-errors"></a>Havuz işlem düğüm hataları

Düğüm başarıyla bir havuzda ayrılabilir, ancak çeşitli sorunlar sistem durumu kötü olan düğümler için sağlama ve kullanılabilir değil.  Bir havuzu düğümler atandıktan sonra bunlara ücret uygulanmaz ve kullanılamaz düğümleri için ödeme yapmaktan kaçınmak üzere sorunlarını algılamak bu nedenle önemlidir.

### <a name="start-task-failure"></a>Başlangıç görevi başarısızlığı

İsteğe bağlı [başlangıç görevi](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) bir havuz için belirtilebilir.  Herhangi bir görev olduğu gibi ile komut satırını ve depolamadan indirilebilmesi için kaynak dosyaları belirtilebilir.  Başlangıç görevi havuz için belirtilmiş ancak her düğümü yeniden başlatıldıktan sonra her bir düğümde - başlangıç görevi çalıştırırsanız çalıştırın.  Daha fazla özelliği [başlangıç görevi](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask), 'waitForSuccess' Batch başlangıç görevi, herhangi bir düğüm görevler zamanlanmadan önce başarıyla tamamlanması için beklemesi gerekip gerekmediğini belirtir.

Başlangıç görevi başarısız ve başarılı tamamlanması için beklenecek belirtilen başlangıç görevi yapılandırma, düğüm kullanılamaz ve yine de ücret uygulanabilir.

Başlangıç görevi hataları kullanarak algılanamıyor [sonucu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) ve [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) özelliklerini üst düzey [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) düğüm özelliği.

Hatalı başlangıç görevi, düğüme ayrıca müşteri adayları [durumu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) 'waitForSuccess' ayarlarsanız 'starttaskfailed' ancak ayarlanan 'true'.

Herhangi bir görev olduğu gibi ile başlangıç görevi başarısız olan pek çok nedeni olabilir.  Sorunu gidermek için stdout, stderr ve herhangi ek görev özgü günlük dosyaları denetlenmelidir.

### <a name="application-package-download-failure"></a>Uygulama paketi yükleme hatası

Bir veya daha fazla uygulama paketi isteğe bağlı olarak her düğüme yüklenen belirtilen paket dosyaları için bir havuz, belirtilen ve düğümü yeniden başlatıldıktan sonra ancak zamanlanmış görevler önce sıkıştırılmamış.  Farklı bir konuma dosyaları kopyalayın veya Kurulum, örneğin çalıştırmak için uygulama paketleri ile birlikte bir başlangıç görevi komut satırı kullanımı yaygındır.

Düğümün indirmek ve bir uygulama paketi sıkıştırmasını açmak için bir hata bildirilir [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) özelliği.  Düğüm durumu 'kullanılamaz' ayarlanır.

### <a name="node-in-unusable-state"></a>Düğüm kullanılamaz durumda

[Düğüm durumu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) çeşitli nedenlerle 'kullanılamaz' olarak ayarlanabilir.  'Kullanılamaz,' görevleri düğüme zamanlanamaz ancak düğüm yine de ücret uygulanabilir.

Batch her zaman kullanılamaz düğümleri kurtarmayı deneyecek, ancak kurtarma olabilir veya nedenine bağlı olarak mümkün olmayabilir.

Neden belirlenebilir olduğunda, bu düğüm tarafından raporlanır [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) özelliği.

Bazı diğer örnekleri 'kullanılamaz' düğümleri nedenleri:

- Geçersiz özel görüntü; doğru örneğin hazır değil.
- Altyapı hatası veya taşınan temel alınan VM önde gelen düşük düzeyli yükseltme; Batch düğüm kurtarır.

### <a name="node-agent-log-files"></a>Düğüm Aracısı günlük dosyaları

Bir havuz düğümü sorunu ilgili desteğe başvurmanız gerekirse, her bir havuz düğümü üzerinde çalışan Batch aracı işleminin günlük dosyalarından alınabilir.  Azure portalı üzerinden Batch Gezgini, bir düğüm için günlük dosyalarını karşıya yüklenebilir veya bir [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs).  Karşıya yükleme ve günlük dosyalarını kaydetme düğümü olarak son derece kullanışlı olabilir veya havuz çalışan düğümlerinin maliyetinden tasarruf etmek silinebilir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı sorunları hızla tespit ve tanı koydu kapsamlı hata, özellikle zaman uyumsuz işlemler için denetimi gerçekleştirdiğini emin olun.
