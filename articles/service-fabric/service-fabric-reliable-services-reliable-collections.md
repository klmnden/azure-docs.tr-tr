---
title: Azure Service Fabric durum bilgisi olan hizmetler güvenilir koleksiyonlar giriş | Microsoft Docs
description: Service Fabric durum bilgisi olan hizmetler yüksek oranda kullanılabilir, ölçeklenebilir ve düşük gecikme süreli bulut uygulamaları yazmanıza olanak tanıyan güvenilir koleksiyonlar sağlar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 1/3/2019
ms.author: aljo
ms.openlocfilehash: 4ed76b207db4712058b5524cd1b31fd65b0e19a4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664421"
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Azure Service Fabric durum bilgisi olan hizmetler güvenilir koleksiyonlar giriş

Güvenilir koleksiyonlar, tek bir bilgisayar uygulamaları için yazar gibi sorgulamanıza yüksek oranda kullanılabilir, ölçeklenebilir ve düşük gecikme süreli bulut uygulamaları yazmak etkinleştirin. Sınıflarda **Microsoft.ServiceFabric.Data.Collections** ad alanı, durum otomatik olarak yüksek oranda kullanılabilir yap koleksiyonları kümesi sağlar. Güvenilir koleksiyonlar çoğaltılır ve yerel durumunu yönetme ve güvenilir koleksiyon API'leri yalnızca program geliştiriciler gerekir.

Güvenilir koleksiyonlar ve diğer yüksek kullanılabilirlik teknolojilerinin (örneğin, Redis, Azure tablo hizmeti ve Azure kuyruk hizmeti) arasındaki temel fark, durumu yerel olarak hizmet örneğinde ayrıca yüksek oranda kullanılabilir yapılan sırasında tutulduğundan emin olan. Bunun anlamı:

* Tüm okuma yerel, düşük gecikme süresine sonuçlanır ve yüksek performanslı okur.
* Tüm yazma işlemlerini düşük gecikme süresi sonuçları en düşük ağ IOs sayısı uygulanır ve yüksek performanslı yazar.

![Koleksiyonları evrimi görüntüsü.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Güvenilir koleksiyonlar düşünülebilir doğal gelişimi **System.Collections** sınıfları: yeni bir karmaşıklık artırmadan cloud ve çoklu bilgisayar uygulamaları için tasarlanmış koleksiyonları Geliştirici. Bu nedenle, güvenilir koleksiyonlar şunlardır:

* Çoğaltılmış: Durum değişiklikleri, yüksek kullanılabilirlik için çoğaltılır.
* Kalıcı: Kalıcı verileri diske büyük ölçekli kesintiler (örneğin, bir veri merkezinde güç kesintisi) karşı dayanıklılık için.
* Yazma kalıcı ve çoğaltılan olduğundan geçici ReliableDictionary, ReliableQueue veya bellekteki verileri yalnızca devam eden diğer güvenilir koleksiyon oluşturulamıyor.
* Zaman uyumsuz: API zaman uyumsuz iş parçacıkları, g/ç yansıtılmasını olduğunda engellenmediğinden emin olun.
* İşlem: Bir hizmet birden çok güvenilir koleksiyon bir kolayca yönetebilirsiniz API'lerini hareketlerinin soyutlama kullanır.

Güvenilir koleksiyonlar, mantık uygulaması durumu hakkında daha kolay hale getirmek için kullanıma hazır güçlü tutarlılık garantileri sağlar.
Yalnızca bir Çoğunluk çekirdeği birincil dahil olmak üzere, tüm işlem açılmış sonra işlemeler son işlem sağlayarak güçlü tutarlılık sağlanır.
Uygulamaları zayıf tutarlılık elde etmek için zaman uyumsuz tamamlama döndürmeden önce geri istemci/istek sahibine kabul.

Güvenilir koleksiyonlar API'leri bir eş zamanlı koleksiyonlar API'leri aşamasıdır (bulunan **System.Collections.Concurrent** ad alanı):

* Zaman uyumsuz: Eş zamanlı koleksiyonlarından farklı işlemler çoğaltılan ve kalıcı olduğundan, bir görev döndürür.
* Out parametreleri yok: Kullanan `ConditionalValue<T>` döndürülecek bir `bool` ve dış parametrelerin yerine bir değer. `ConditionalValue<T>` benzer `Nullable<T>` T bir yapısı olması gerekmez ancak.
* İşlemler: Kullanıcı grubu eylemleri bir işlemde birden çok güvenilir koleksiyonlar için etkinleştirmek için bir işlem nesnesini kullanır.

Bugün, **Microsoft.ServiceFabric.Data.Collections** üç koleksiyonu içerir:

* [Güvenilir bir sözlük](https://msdn.microsoft.com/library/azure/dn971511.aspx): Çoğaltılan, işlemsel ve zaman uyumsuz bir anahtar/değer çifti koleksiyonunu temsil eder. Benzer şekilde **ConcurrentDictionary**, hem anahtar hem de değer herhangi bir türde olabilir.
* [Güvenilir kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx): Bir çoğaltılmış, işlemsel ve zaman uyumsuz katı ilk giren ilk çıkar (FIFO) sırayı temsil eder. Benzer şekilde **ConcurrentQueue**, değer herhangi bir türde olabilir.
* [Güvenilir eşzamanlı kuyruk](service-fabric-reliable-services-reliable-concurrent-queue.md): Yüksek aktarım hızı için kuyruk sıralama çoğaltılan, işlemsel ve zaman uyumsuz bir en iyi çaba temsil eder. Benzer şekilde **ConcurrentQueue**, değer herhangi bir türde olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenilir koleksiyon ile ilgili Kılavuzlar ve öneriler](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri yönetme
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimleri](service-fabric-reliable-services-notifications.md)
  * [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir durum Yöneticisi'ni yapılandırma](service-fabric-reliable-services-configuration.md)
* Diğerleri
  * [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
