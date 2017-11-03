---
title: "Service Fabric güvenilir koleksiyonları işlemleri ve Azure kilit modu | Microsoft Docs"
description: "Azure Service Fabric güvenilir durum Yöneticisi ve güvenilir koleksiyonları işlemleri ve kilitleme."
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
ms.openlocfilehash: 3452473f5b2f86d29e46339c997193bc6403736a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>İşlemler ve Azure Service Fabric güvenilir koleksiyonları kilit modu

## <a name="transaction"></a>İşlem
Bir işlem, tek bir mantıksal birim iş olarak gerçekleştirilen işlemler dizisidir.
Bir işlem aşağıdaki ACID özellikleri göstermesi gerekir. (bkz: https://technet.microsoft.com/en-us/library/ms190612)
* **Kararlılık**: atomik bir iş birimine bir işlem olmalıdır. Diğer bir deyişle, tüm veri değişiklikleri gerçekleştirilen ya da bunların hiçbiri gerçekleştirilir.
* **Tutarlılık**: tamamlandığında, bir işlem tüm verileri tutarlı bir durumda bırakmanız gerekir. Tüm iç veri yapılarını işlemin sonunda doğru olması gerekir.
* **Yalıtım**: eşzamanlı işlemler tarafından yapılan değişiklikler diğer eşzamanlı işlemler tarafından yapılan değişiklikleri üzerinden olmalıdır. Bir ITransaction içinde bir işlem için kullanılan yalıtım düzeyi, işlemi gerçekleştiren IReliableState tarafından belirlenir.
* **Dayanıklılık**: bir işlem tamamlandıktan sonra etkileri kalıcı olarak sistemde karşılandığından. Değişikliklerin bile bir sistem hatası durumunda kalır.

### <a name="isolation-levels"></a>Yalıtım düzeyi
Yalıtım düzeyi için işlem diğer işlemler tarafından yapılan değişiklikler üzerinden olmalıdır derece tanımlar.
Güvenilir koleksiyonlarda desteklenen iki yalıtım düzeyi vardır:

* **Yinelenebilir Okuma**: deyimleri değiştirildi, ancak henüz diğer işlemler tarafından kaydedilen veri okunamıyor ve başka bir işlem geçerli işlem kadar geçerli işlem tarafından okunur verileri değiştirebilir belirtir tamamlanır. Daha fazla ayrıntı için bkz: [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
* **Anlık Görüntü**: bir işlemde herhangi bir deyim tarafından okunan veriler işlem başlangıcında var olan verileri işlemsel olarak tutarlı sürümü olduğunu belirtir.
  İşlem yalnızca işlem başlamadan önce kaydedilmiş veri değişiklikleri tanıyabilirsiniz.
  Geçerli işlem başladıktan sonra diğer işlemler tarafından yapılan veri değişikliklerinin geçerli işlem yürütülürken deyimleri görünür değildir.
  İşlem başlangıcında var gibi bir işlem deyimlerinde kaydedilen verilerin bir anlık görüntüsünü alırsanız gibi etkisidir.
  Anlık görüntüler güvenilir koleksiyonlar genelinde tutarlı değil.
  Daha fazla ayrıntı için bkz: [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Güvenilir koleksiyonları otomatik olarak hareket oluşturma zamanında işlemi ve çoğaltma rolü bağlı olarak belirli bir okuma işlemi için kullanılacak yalıtım düzeyini seçin.
Güvenilir sözlük ve sıra işlemleri için yalıtım düzeyi varsayılan değerlerini gösterir tablosu aşağıdadır.

| İşlem \ rolü | Birincil | İkincil |
| --- |:--- |:--- |
| Tek varlık okuma |Yinelenebilir Okuma |Anlık Görüntü |
| Numaralandırma, sayısı |Anlık Görüntü |Anlık Görüntü |

> [!NOTE]
> Tek varlık işlemleri için ortak örnekler `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Sözlüğün güvenilir ve güvenilir sıranın okuma bilgisayarınızı Yazar destekler.
Diğer bir deyişle, bir işlem içinde her yazma aynı işlem ait aşağıdaki okuma görünür olacaktır.

## <a name="locks"></a>Kilitler
Kilitleme güvenilir koleksiyonlarında tüm işlemleri uygulama sıkı iki aşama: bir işlem alınan bir durdurma veya bir yürütme ile işlem sonlanana kadar kilitleri bırakmaz.

Güvenilir sözlüğü satır düzeyinde tüm tek bir varlık işlemler için kilitleme kullanır.
Eşzamanlılık katı işlem FIFO özelliği için devre dışı güvenilir sıra trades.
Güvenilir kuyruğu kullanan bir işlemle izin verme işlemi düzeyi kilitleri `TryPeekAsync` ve/veya `TryDequeueAsync` ve bir işlemle `EnqueueAsync` birer birer.
FIFO, korumak için unutmayın bir `TryPeekAsync` veya `TryDequeueAsync` hiç gözlemleyen güvenilir sıranın boş olduğundan, ayrıca kilitleyecek `EnqueueAsync`.

Yazma işlemlerinin her zaman özel kilit alıyor.
Okuma işlemleri için kilitleme, birkaç etkene bağlıdır.
Anlık görüntü yalıtımı kullanılarak gerçekleştirilen okuma işlemi kilidi serbest ' dir.
Varsayılan olarak tüm yinelenebilir okuma işlemi paylaşılan kilitleri alır.
Ancak, yinelenebilir okuma destekleyen tüm okuma işlemi için kullanıcı bir güncelleştirme kilidi paylaşılan kilit yerine isteyebilir.
Bir güncelleştirme kilidi birden çok işlem kaynakları daha sonraki bir zamanda potansiyel güncelleştirmeleri kilitlediğinizde gerçekleşen kilitlenme ortak biçimi önlemek için kullanılan bir asimetrik kilidi ' dir.

Kilit uyumluluk matrisi aşağıdaki tabloda bulunabilir:

| İstek \ verildi | None | Paylaşılan | Güncelleştirme | Özel |
| --- |:--- |:--- |:--- |:--- |
| Paylaşılan |Çakışma |Çakışma |Çakışma |Çakışma |
| Güncelleştirme |Çakışma |Çakışma |Çakışma |Çakışma |
| Özel |Çakışma |Çakışma |Çakışma |Çakışma |

Güvenilir koleksiyonları API'leri zaman aşımı değişkeninde kilitlenme algılama için kullanılır.
Örneğin, iki işlemleri (T1 ve T2) okuma ve güncelleştirme K1 deniyorsunuz.
Her ikisi de paylaşılan kilit sahip end için kilitlenme için mümkündür.
Bu durumda, bir veya iki işlem zaman aşımına uğrar.

Bu kilitlenme senaryo, bir güncelleştirme kilidi kilitlenmeleri nasıl engelleyebilirsiniz, harika bir örnektir.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Güvenilir hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
* [Güvenilir hizmetler yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

