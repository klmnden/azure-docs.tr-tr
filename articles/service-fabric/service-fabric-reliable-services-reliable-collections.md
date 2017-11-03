---
title: "Azure Service Fabric durum bilgisi olan Hizmetleri'ndeki güvenilir koleksiyonlara giriş | Microsoft Docs"
description: "Service Fabric durum bilgisi olan hizmetler, yüksek oranda kullanılabilir, ölçeklenebilir ve düşük gecikme süreli bulut uygulamaları yazmak etkinleştirmeniz güvenilir koleksiyonları sağlar."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d0247ba0242af05ca6dcd8049ff9116683538fa5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Azure Service Fabric durum bilgisi olan Hizmetleri'ndeki güvenilir koleksiyonlara giriş
Güvenilir koleksiyonları tek bilgisayar uygulamaları yazıyordunuz gibi sorgulamanıza yüksek oranda kullanılabilir, ölçeklenebilir ve düşük gecikme süreli bulut uygulamaları yazmak etkinleştirin. Sınıflarda **Microsoft.ServiceFabric.Data.Collections** ad alanı, durumu otomatik olarak yüksek oranda kullanılabilir hale koleksiyonları kümesini sağlar. Geliştiriciler yalnızca güvenilir koleksiyonu API'lerini program ve güvenilir çoğaltılır ve yerel durumunu yönetme koleksiyonları izin gerekir.

Güvenilir koleksiyonları ve diğer yüksek kullanılabilirlik teknolojiler (örneğin, Redis, Azure tablo hizmeti ve Azure kuyruk hizmeti) arasındaki temel farklılık, durumu yerel olarak hizmet örneği de yüksek oranda kullanılabilir yapılan sırasında saklanacağını ' dir. Bunun anlamı:

* Tüm okuma yerel, bunlar düşük gecikme sağlar ve yüksek verimlilik okur.
* Hangi içinde düşük gecikme süresi sonuçları en az sayıda ağ IOs, tüm yazma işlemlerini uygulanır ve yüksek verimlilik yazar.

![Koleksiyonları evrimi görüntüsü.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Güvenilir koleksiyonlar zorlayıcı doğal evrimi **System.Collections** sınıfları: geliştiricisi karmaşıklık artırmadan Bulut ve birden çok bilgisayar uygulamaları için tasarlanmış koleksiyonları yeni bir dizi. Bu nedenle, güvenilir koleksiyonları şunlardır:

* Çoğaltılmış: Durum değişiklikleri yüksek kullanılabilirlik için çoğaltılır.
* Kalıcı: Veri kalıcı büyük ölçekli kesintileri (örneğin, bir veri merkezi güç kesintisi) karşı dayanıklılık için diske.
* Zaman uyumsuz: API'leri zaman uyumsuz iş parçacığı GÇ yansıtılmasını zaman engellenmez emin olun.
* İşlem: bir hizmet içinde birden çok güvenilir koleksiyonları kolayca yönetilebilir API işlemleri soyutlama kullanın.

Güvenilir koleksiyonları güçlü tutarlılık mantığı uygulama durumu hakkında kolaylaştırmak için kutu dışı garanti sağlar.
Güçlü tutarlılık, tüm işlem çoğaltmalar birincil dahil olmak üzere, üzerinde bir çoğunluğu çekirdek yalnızca oturum açılmış sonra işlemeleri son işlem sağlanarak elde edilir.
Uygulamalar daha zayıf tutarlılık elde etmek için zaman uyumsuz tamamlama döndürmeden önce geri istemci/istemciye kabul.

Bir eş zamanlı koleksiyonları API'leri evrimi güvenilir koleksiyonları apı'leridir (bulunan **System.Collections.Concurrent** ad alanı):

* Zaman uyumsuz: eşzamanlı koleksiyonlarından farklı işlemler kalıcı ve çoğaltılmasını olduğundan, bir görev döndürür.
* Hayır out Parametreleri: kullanan `ConditionalValue<T>` bool ve out parametreleri yerine bir değer döndürmek için. `ConditionalValue<T>`benzer `Nullable<T>` T yapı olmasını gerektirmez, ancak.
* İşlemler: bir işlemde birden çok güvenilir koleksiyonları Grup eylemleri kullanıcıya etkinleştirmek için işlem nesnesi kullanır.

Bugün, **Microsoft.ServiceFabric.Data.Collections** üç koleksiyonları içerir:

* [Güvenilir sözlük](https://msdn.microsoft.com/library/azure/dn971511.aspx): anahtar/değer çiftlerinin çoğaltılmış, işlem ve zaman uyumsuz bir koleksiyonunu temsil eder. Benzer şekilde **ConcurrentDictionary**, anahtar ve değer herhangi bir türde olabilir.
* [Güvenilir sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx): bir çoğaltılmış, işlem ve zaman uyumsuz katı ilk çıkar (FIFO) sırayı temsil eder. Benzer şekilde **ConcurrentQueue**, değer herhangi bir türde olabilir.
* [Güvenilir eşzamanlı sıra](service-fabric-reliable-services-reliable-concurrent-queue.md): sıra yüksek verimlilik için sıralama çoğaltılmış, işlem ve zaman uyumsuz bir en iyi çaba temsil eder. Benzer şekilde **ConcurrentQueue**, değer herhangi bir türde olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir koleksiyonu kılavuzları ve önerileri](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve kilitleri](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Güvenilir durum Yöneticisi'ni ve koleksiyon dahili bileşenleri](service-fabric-reliable-services-reliable-collections-internals.md)
* Veri Yönetimi
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
