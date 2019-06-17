---
title: Havuz ve düğüm hataları - Azure Batch denetleyin
description: Hataları denetlemek için ve havuzları ve düğümler oluştururken kaçınma yöntemleri
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 05/28/2019
ms.topic: conceptual
ms.openlocfilehash: b0a9d04fccce7ccbacb700f7af5126c6ae05140a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66357769"
---
# <a name="check-for-pool-and-node-errors"></a>Havuz ve düğüm hataları denetleyin

Oluştururken ve Azure Batch havuzları yönetme, bazı işlemler hemen gerçekleştirilir. Ancak, bazı işlemler zaman uyumsuz ve arka planda çalıştırma. Bunlar, tamamlanması birkaç dakika sürebilir.

Hataları hemen API, CLI veya kullanıcı Arabirimi tarafından döndürüldüğünden hemen gerçekleşmesi işlemleri için hataların algılanması oldukça basittir.

Bu makalede havuzlar ve havuz düğümleri için oluşabilecek arka plan işlemleri kapsar. Bu, algılamak ve hatalarını önlemek nasıl belirtir.

## <a name="pool-errors"></a>Havuzu hataları

### <a name="resize-timeout-or-failure"></a>Yeniden boyutlandırma zaman aşımı veya hatası

Yeni bir havuz oluşturma veya mevcut bir yeniden boyutlandırma havuz, düğümlerin hedef sayısını belirtin.  İşlemi hemen tamamlanır, ancak yeni düğümlerin gerçek ayırma veya var olan düğümleri kaldırılmasını birkaç dakika sürebilir.  Yeniden boyutlandırma zaman aşımı belirtin [oluşturma](https://docs.microsoft.com/rest/api/batchservice/pool/add) veya [yeniden boyutlandırma](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API. Toplu hedef düğüm sayısı yeniden boyutlandırma zaman aşımı süresi boyunca alamazlarsa işlemi durdurur. Havuza bir kararlı durumuna girer ve raporları, hataları yeniden boyutlandırın.

[ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) özelliği en son değerlendirme için boyutlandırma zaman aşımı raporları ve oluşan hataları listeler.

Yeniden boyutlandırma zaman aşımı için yaygın nedenler şunlardır:

- Yeniden boyutlandırma zaman aşımı çok kısa
  - Çoğu durumda, 15 dakikalık varsayılan zaman aşımı ayrılan veya kaldırılacak havuz düğümleri için yeterince uzun.
  - Çok sayıda düğümü tahsis etme, yeniden boyutlandırma zaman aşımı 30 dakikaya ayarlanıyor öneririz. Örneğin, ne zaman, 1. 000'den fazla düğümleri Azure Market görüntüsünden ya da özel bir VM görüntüsü 300'den fazla düğümünden yeniden boyutlandırma.
- Yeterli çekirdek kotası
  - Bir Batch hesabı arasında tüm havuzları ayırabilirsiniz çekirdek sayısı sınırlıdır. Batch, kotasına ulaşıldı sonra düğümleri ayırma durdurur. [Artırabilirsiniz](https://docs.microsoft.com/azure/batch/batch-quota-limit) çekirdek kotası için daha fazla düğüm toplu ayırabilirsiniz.
- Yetersiz IP alt ağı, bir [havuzu, sanal ağ içinde değil](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Bir sanal ağ alt ağı yeterli olmalıdır. her istenen havuz düğümü için ayrılacak IP adresleri atanmamış. Aksi takdirde, düğümleri oluşturulamıyor.
- Kaynaklar yetersiz olduğunda bir [havuzu, sanal ağ içinde değil](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Batch hesabı ile aynı abonelikte yük dengeleyicileri genel IP'ler ve ağ güvenlik grupları gibi kaynaklar oluşturabilir. Abonelik kotaları bu kaynaklar için yeterli olduğundan emin olun.
- Büyük havuzları ile özel VM görüntüleri
  - Özel VM görüntülerini kullanmak büyük havuzlar uzun sürebilir ayırın ve yeniden boyutlandırma zaman aşımları oluşabilir.  Bkz: [sanal makine havuzu oluşturmak için özel görüntü kullanma](https://docs.microsoft.com/azure/batch/batch-custom-images) limitler ve yapılandırma konusunda öneriler için.

### <a name="automatic-scaling-failures"></a>Otomatik ölçeklendirme hataları

Bir havuzdaki düğümlerin sayısını otomatik olarak ölçeklendirmek için Azure Batch de ayarlayabilirsiniz. Parametreler için tanımladığınız [otomatik ölçeklendirme formülü bir havuz için](https://docs.microsoft.com/azure/batch/batch-automatic-scaling). Batch hizmeti formülü düzenli olarak havuzdaki düğüm sayısını değerlendirmek ve yeni bir hedef numarası ayarlamak için kullanır. Aşağıdaki türde sorunları ortaya çıkabilir:

- Otomatik ölçeklendirme değerlendirme başarısız olur.
- Sonuçta elde edilen yeniden boyutlandırma işlemi başarısız olur ve zaman aşımına uğrar.
- Otomatik ölçeklendirme formülü ile ilgili bir sorun, hatalı düğüm hedef değerleri için yol açar. Yeniden boyutlandırma çalışır veya zaman aşımına uğrar.

Kullanarak son otomatik ölçeklendirme değerlendirme hakkında bilgi alabilirsiniz [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) özelliği. Bu özellik, değerlendirme süresi, değerleri ve sonuç ve performans hataları bildirir.

[Havuz yeniden boyutlandırma tamamlama olayı](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) tüm değerlendirmeleri hakkındaki bilgileri yakalar.

### <a name="delete"></a>Sil

Düğümleri içeren bir havuzu sildiğinizde düğümler ilk Batch siler. Ardından havuzu nesneyi siler. Bu, silinecek havuz düğümleri için birkaç dakika sürebilir.

Toplu işlem kümeleri [havuzu durumu](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) için **silme** silme işlemi sırasında. Çağıran uygulama havuzunu silme işlemini kullanarak çok uzun sürüyor durumunda algılayabilir **durumu** ve **stateTransitionTime** özellikleri.

## <a name="pool-compute-node-errors"></a>Havuz işlem düğüm hataları

Çeşitli sorunları bile toplu bir havuzdaki düğümlerin başarıyla ayırır, bazı düğümlerin sağlıksız ve kullanılamaz durumda olması neden olabilir. Bu düğümler ücretleri. Kullanılamayan düğümleri için ödeme olmayan şekilde sorunlarını algılamak önemlidir.

### <a name="start-task-failure"></a>Başlangıç görevi başarısızlığı

İsteğe bağlı belirtmek isteyebilirsiniz [başlangıç görevi](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) bir havuz için. Herhangi bir görev olduğu gibi ile depolamadan indirilebilmesi için bir komut satırı ve kaynak dosyalarını kullanabilirsiniz. Başlamış sonra her düğüm için başlangıç görevi çalıştırılır. **WaitForSuccess** özelliği, toplu bir düğüme herhangi bir görevi zamanlar önce başlangıç görevinin başarıyla tamamlanana kadar bekleyip beklemeyeceğini belirtir.

Başarılı bir başlangıç görevi tamamlama, ancak başlangıç görevi başarısız için beklenecek düğümü ne yapılandırdığınız? Bu durumda, düğüm kullanılamaz ancak yine de ücreti alınmaz.

Başlangıç görevi hataları kullanarak algılayabilir [sonucu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) ve [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) özelliklerini üst düzey [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) düğüm özelliği.

Hatalı başlangıç görevi de Batch düğüm kümesi neden [durumu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) için **starttaskfailed** ayarlarsınız, **waitForSuccess** için **true**.

Herhangi bir görev olduğu gibi ile başlangıç görevi başarısız olan pek çok nedeni olabilir.  Sorunu gidermek için stdout, stderr ve herhangi başka göreve özel günlük dosyalarını denetleyin.

### <a name="application-package-download-failure"></a>Uygulama paketi yükleme hatası

Bir havuz için bir veya daha fazla uygulama paketi belirtebilirsiniz. Batch, her düğüm için belirtilen paket dosyaları indirir ve dosyaları düğümü yeniden başlatıldıktan sonra ancak zamanlanmış görevler önce genişletir. Uygulama paketleri ile birlikte bir başlangıç görevi komut satırında kullanmak için yaygındır. Örneğin, farklı bir konuma dosyaları kopyalayın veya Kurulumu çalıştırın.

Düğüm [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) özelliği indirmek ve bir uygulama paketi sıkıştırmasını açmak için bir hata bildirir. Batch düğüm durumu ayarlar **kullanılamaz**.

### <a name="container-download-failure"></a>Kapsayıcı indirme hatası

Bir havuzda bir veya daha fazla kapsayıcı başvurular belirtebilirsiniz. Batch, her düğüm için belirtilen kapsayıcıları indirir. Düğüm [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) özelliği bir kapsayıcı indirmek için bir hata bildirir ve düğüm durumu ayarlar **kullanılamaz**.

### <a name="node-in-unusable-state"></a>Düğüm kullanılamaz durumda

Azure Batch ayarlanabilir [düğüm durumu](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) için **kullanılamaz** birçok nedenden dolayı. Düğüm durumu ile kümesine **kullanılamaz**görevleri düğüme zamanlanmış olamaz, ancak yine de ücreti alınmaz.

Düğüm bir **unsuable**, olmadan [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) durumu, Batch VM ile iletişim kuramıyor, anlamına gelir. Bu durumda, Batch, sanal Makineyi kurtarmak her zaman çalışır. Toplu olmayan otomatik olarak durumlarına olsa bile, uygulama paketleri veya kapsayıcıları yüklemek için başarısız olan sanal makineleri kurtarmayı deneyecek **kullanılamaz**.

Batch düğüm nedeni belirleyebilirseniz [hataları](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) özelliği bildirir.

Ek örnekler nedenleri **kullanılamaz** düğümleri içerir:

- Özel bir VM görüntüsü geçersiz. Örneğin, görüntünün düzgün şekilde hazırlanır.

- Bir VM, bir altyapı hatası veya bir alt düzey yükseltmesi nedeniyle taşınır. Batch düğüm kurtarır.

- Bir VM görüntüsü, bunu desteklemeyen donanımda dağıtıldı. HPC dışı bir donanım üzerinde çalışan örneğin "HPC" VM görüntüsü. Örneğin, bir CentOS HPC görüntüsü çalıştırmayı denediği bir [işler için standart_d1_v2](../virtual-machines/linux/sizes-general.md#dv2-series) VM.

- Vm'leri bulunan bir [Azure sanal ağı](batch-virtual-network.md), ve trafiği anahtar bağlantı noktalarına engellendi.

### <a name="node-agent-log-files"></a>Düğüm Aracısı günlük dosyaları

Her havuz düğümü üzerinde çalışan Batch aracı işlemi, bir havuz düğümü sorun hakkında desteğe ihtiyacınız varsa yararlı olabilecek günlük dosyalarını sağlayabilir. Günlük dosyaları için bir düğüm, Batch Gezgini gibi Azure portal aracılığıyla karşıya yüklenebilir veya bir [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs). Karşıya yükleme ve günlük dosyalarını kaydetmek kullanışlıdır. Daha sonra bir düğüm veya çalışan düğümlerinin maliyetinden tasarruf etmek havuzu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı kapsamlı hata, özellikle zaman uyumsuz işlemleri için denetimi uygulamak için ayarladığınız denetleyin. En kısa sürede sorunlarını algılayıp tanılamanıza için önemli olabilir.
