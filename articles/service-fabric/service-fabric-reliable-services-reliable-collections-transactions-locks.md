---
title: İşlemler ve kilit modları azure'da Service Fabric güvenilir koleksiyonlar | Microsoft Docs
description: Azure Service Fabric güvenilir durum Yöneticisi ve güvenilir koleksiyonlar işlemleri ve kilitleme.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: aljo
ms.openlocfilehash: 9785a09a3ac3e119507b4ac28075d887c7edc619
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60774072"
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>İşlemler ve Azure Service Fabric Reliable Collections kilit modları

## <a name="transaction"></a>İşlem
Bir işlem, tek bir mantıksal birim iş olarak gerçekleştirilen işlemlerin sayısına bir dizidir.
Bir işlem aşağıdaki ACID özellikleri göstermesi gerekir. (bkz: https://technet.microsoft.com/library/ms190612)
* **Kararlılık**: Bir işlem çalışmanın bir atomik birim olması gerekir. Diğer bir deyişle, tüm veri değişiklikleri gerçekleştirilen ya da bunların hiçbiri gerçekleştirilir.
* **Tutarlılık**: İşlem tamamlandığında, tüm verileri tutarlı bir durumda bırakmanız gerekir. Tüm iç veri yapılarını işlem sonunda doğru olması gerekir.
* **Yalıtım**: Eşzamanlı işlem tarafından yapılan değişiklikler diğer eşzamanlı işlemler tarafından yapılan değişiklikler gelen yalıtılmış olması gerekir. Bir ITransaction içinde bir işlem için kullanılan yalıtım düzeyi, işlemi gerçekleştiren IReliableState tarafından belirlenir.
* **Dayanıklılık**: İşlem tamamlandıktan sonra etkilerini sistemde kalıcı olarak yerdesiniz demektir. Değişikliklerin bir sistem hatası durumunda bile kalıcı hale getirin.

### <a name="isolation-levels"></a>Yalıtım düzeyleri
İstediğiniz işlemin diğer işlemler tarafından yapılan değişiklikleri gelen yalıtılmış olmalıdır ne ölçüde yalıtılacağını yalıtım düzeyini tanımlar.
Güvenilir koleksiyonlar desteklenen iki yalıtım düzeyi vardır:

* **Tekrarlanabilir okuma**: Deyimleri değiştirilmiş, ancak diğer işlemler tarafından henüz uygulanmamış veri okunamıyor ve başka bir işlem geçerli işlem bitene kadar geçerli işlem tarafından okunan veri değiştirebilirsiniz belirtir. Daha fazla ayrıntı için [ https://msdn.microsoft.com/library/ms173763.aspx ](https://msdn.microsoft.com/library/ms173763.aspx).
* **Anlık Görüntü**: Herhangi bir deyimle bir işlem tarafından okunan veri işlem başlangıcında var olan verileri işlemsel olarak tutarlı sürümü olduğunu belirtir.
  İşlem, yalnızca işlem başlamadan önce kaydedilmiş veri değişiklikleri tanıyabilirsiniz.
  Geçerli işlem başladıktan sonra diğer işlemler tarafından yapılan değişiklikleri geçerli işlem sırasında yürütülen deyimleri için görünür değildir.
  Bir işlemde deyimleri bir işlemin başında yeterdir taahhüt edilen verilerin bir anlık görüntüsünü almak için gibi etkisidir.
  Güvenilir koleksiyonlar genelinde tutarlı anlık görüntüleri.
  Daha fazla ayrıntı için [ https://msdn.microsoft.com/library/ms173763.aspx ](https://msdn.microsoft.com/library/ms173763.aspx).

Güvenilir koleksiyonlar, otomatik olarak zaman hareketin oluşturma işlemi ve çoğaltmanın rolü, bağlı olarak belirli bir okuma işlemi için kullanılacak yalıtım düzeyini seçin.
Güvenilir bir sözlük ve kuyruk işlemleri için yalıtım düzeyi varsayılan değerlerini gösteren tabloyu aşağıdadır.

| İşlem \ rolü | Birincil | İkincil |
| --- |:--- |:--- |
| Tek varlık okuma |Tekrarlanabilir okuma |Anlık Görüntü |
| Numaralandırma, sayısı |Anlık Görüntü |Anlık Görüntü |

> [!NOTE]
> Tek varlık işlemleri ortak verilebilir `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Güvenilir bir sözlük ve güvenilir sıranın hem okuma bilgisayarınızı Yazar destekler.
Diğer bir deyişle, bir işlem içinde herhangi bir yazma için aynı işlem ait aşağıdaki okuma için görünür olur.

## <a name="locks"></a>Kilitler
Kilitleme güvenilir koleksiyonlar titiz tüm işlemleri uygular iki aşama: bir işlem edinilen bir iptal veya bir işleme ile işlem sonlanana kadar kilitleri serbest bırakmaz.

Güvenilir bir sözlük kilitleme tüm tek varlık işlemler için satır düzeyinde kullanır.
Eşzamanlılık katı işlem FIFO özelliği için kapalı güvenilir kuyruk arasında denge kurar.
Güvenilir kuyruğu kullanan bir işlem ile izin verme işlemi düzeyi kilitleri `TryPeekAsync` ve/veya `TryDequeueAsync` ve bir işlem ile `EnqueueAsync` birer güncelleştirir.
Olmadığını unutmayın FIFO, korumak için bir `TryPeekAsync` veya `TryDequeueAsync` hiç olmadığı kadar gözlemler güvenilir sıranın boş olduğundan, ayrıca kilitleyecek `EnqueueAsync`.

Yazma işlemlerinin her zaman özel kilit alıyor.
Okuma işlemleri için kilitleme, birkaç etkene bağlıdır.
Anlık görüntü yalıtımı kullanılarak gerçekleştirilen okuma işlemi kilitsiz ' dir.
Varsayılan olarak herhangi bir tekrarlanabilir okuma işlemi paylaşılan kilit alır.
Ancak, tekrarlanabilir okuma destekleyen tüm okuma işlemi için kullanıcı paylaşılan kilit yerine bir güncelleştirme kilidi isteyebilir.
Bir güncelleştirme kilidi birden çok işlem sonraki bir zamanda potansiyel güncelleştirmeleri için kaynakları kilitleme geldiğinde gerçekleşen kilitlenme ortak bir tür önlemek için kullanılan bir asimetrik kilidi var.

Kilit uyumluluk matrisi aşağıdaki tabloda bulunabilir:

| İstek \ verildi | None | Paylaşılan | Güncelleştirme | Özel |
| --- |:--- |:--- |:--- |:--- |
| Paylaşılan |Çakışma yok |Çakışma yok |Çakışma |Çakışma |
| Güncelleştirme |Çakışma yok |Çakışma yok |Çakışma |Çakışma |
| Özel |Çakışma yok |Çakışma |Çakışma |Çakışma |

Güvenilir koleksiyonlar API'lerde zaman aşımı bağımsız değişkeni, kilitlenme algılaması için kullanılır.
Örneğin, iki işlemleri (T1 ve T2) okumasına ve güncelleştirmesine K1 deniyorsunuz.
Her ikisi de paylaşılan kilit sonunda kilitlenme için mümkün olmasıdır.
Bu durumda, bir veya iki işlem zaman aşımına uğrar.

Bu kilitlenme senaryo, bir güncelleştirme kilidi kilitlenmeleri nasıl engelleyebilir, harika bir örnektir.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Reliable Services bildirimleri](service-fabric-reliable-services-notifications.md)
* [Reliable Services yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir durum Yöneticisi'ni yapılandırma](service-fabric-reliable-services-configuration.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

