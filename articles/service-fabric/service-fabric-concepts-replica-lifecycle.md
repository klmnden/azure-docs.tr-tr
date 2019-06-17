---
title: Çoğaltmalar ve örnekler Azure Service fabric'te | Microsoft Docs
description: Çoğaltmalar ve örnekler--kendi işlevi ve yaşam döngüleri anlama
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: ''
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: 7f8638365b40395a5dd82457c40e5c15209ba1a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60882427"
---
# <a name="replicas-and-instances"></a>Çoğaltmalar ve örnekler 
Bu makalede, durum bilgisi olan hizmetler ve durum bilgisi olmayan hizmetler örneklerini kopyasının yaşam döngüsüne genel bir bakış sağlar.

## <a name="instances-of-stateless-services"></a>Durum bilgisi olmayan hizmetler örnekleri
Küme düğümlerinden biri üzerinde çalışan hizmet mantığı bir kopyasını bir durum bilgisi olmayan hizmet örneğidir. Bir örnek bölüm içindeki benzersiz olarak tanımlanır, **InstanceId**. Aşağıdaki diyagramda yaşam döngüsü örneği modellenmiştir:

![Örnek yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>Inbuild (IB)
Küme Kaynak Yöneticisi örneği için bir yerleştirme belirledikten sonra bu yaşam döngüsü durumuna girer. Örneğin düğümde başlatılır. Uygulama konağı başlatıldığında, örneği oluşturulmuş ve ardından açılır. Başlangıç tamamlandıktan sonra örnek hazır duruma geçer. 

Bu örnek için düğüm ve uygulama konağı çökerse, bırakılan durumuna geçer.

### <a name="ready-rd"></a>(RD) hazır
Hazır durumda çalışmaya düğümde örneğidir. Bu örnek, bir güvenilir hizmet ise **RunAsync** çağrılmış. 

Bu örnek için düğüm ve uygulama konağı çökerse, bırakılan durumuna geçer.

### <a name="closing-cl"></a>Kapatma (CI)
Kapatma, Azure Service Fabric bu düğüm örneğinde kapatma işleminde durumdadır. Bu kapatma nedeniyle birçok nedenden Örneğin, bir uygulama yükseltmesi, Yük Dengeleme veya Silinen hizmet olabilir. Kapatma tamamlandıktan sonra bırakılan durumuna geçer.

### <a name="dropped-dd"></a>Bırakılan (gg)
Bırakılan durumda örneği artık düğümde çalışmıyor. Bu noktada, Service Fabric sonunda de silinir Bu örneği hakkındaki meta veriler tutar.

> [!NOTE]
> Geçiş bırakılan durumuna herhangi bir durumdan kullanarak mümkündür **ForceRemove** seçeneğini `Remove-ServiceFabricReplica`.
>

## <a name="replicas-of-stateful-services"></a>Durum bilgisi olan hizmetler çoğaltmaları
Durum bilgisi olan hizmet çoğaltmasını küme düğümlerinden biri üzerinde çalışan hizmet mantıksal bir kopyasıdır. Ayrıca, çoğaltma, hizmet durumunun bir kopyasını tutar. İki ilgili kavramları yaşam döngüsü ve durum bilgisi olan yinelemeler davranışını açıklar:
- Çoğaltma yaşam döngüsü
- Çoğaltma rolü

Kalıcı durum bilgisi olan hizmetler aşağıdaki tartışma açıklar. Volatile (veya bellek içi) durum bilgisi olan hizmetler için aşağı ve bırakılan durumları eşdeğerdir.

![Çoğaltma yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>Inbuild (IB)
Inbuild çoğaltma oluşturulan veya çoğaltma kümesine katılmak için hazırlanmış bir çoğaltmadır. Çoğaltma rolü bağlı olarak IB farklı semantiğe sahip. 

Uygulama konağı veya düğüm Inbuild çoğaltma için bir çökme gerçekleşirse aşağı durumuna geçer.

   - **Birincil Inbuild çoğaltmaların**: Bir bölüm için ilk çoğaltmaları birincil Inbuild var. Bu yineleme, genellikle bölüm oluşturulduğunda gerçekleşir. Birincil çoğaltmaları ayrıca tüm bölüm çoğaltmaları yeniden başlattığınızda ortaya Inbuild veya olan bırakıldı.

   - **IdleSecondary Inbuild çoğaltmaların**: Bunlar küme kaynak yöneticisi tarafından oluşturulan yeni çoğaltmalar veya kapanmış mevcut çoğaltmaları ve kümesine eklenmesi gerekir. Bu çoğaltmaların çekirdek değeri oluşturulmuş veya ActiveSecondary olarak çoğaltma kümesine katılın ve işlem çekirdek bildirim içinde katılmak önce birincil tarafından oluşturulmuş.

   - **ActiveSecondary Inbuild çoğaltmaların**: Bu durum, bazı sorgularda dikkate alınır. Bu, çoğaltma ayarlandığı bir iyileştirme değişmiyor, ancak bir yineleme derlenmesi gerekir olur. Çoğaltma normal durumu makine geçişleri (çoğaltma rolü bölümünde anlatıldığı gibi) izler.

### <a name="ready-rd"></a>(RD) hazır
Hazır bir çoğaltma çoğaltma ve çekirdek bildirim işlemlerinin yer aldığı bir yinelemedir. Birincil ve etkin ikincil çoğaltmalara hazır durumunda geçerlidir.

Uygulama konağı veya düğüm hazır bir çoğaltma için bir çökme gerçekleşirse aşağı durumuna geçer.

### <a name="closing-cl"></a>Kapatma (CI)
Bir çoğaltma, aşağıdaki senaryolarda kapatma durumuna girer:

- **Çoğaltma için kod kapatma**: Service Fabric, bir çoğaltma için çalışan kod kapatmanız gerekebilir. Bu kapatma için birçok neden olabilir. Örneğin, bir uygulama, doku veya altyapı yükseltmesi nedeniyle ya da çoğaltma raporlanan bir hata nedeniyle oluşabilir. Çoğaltma, çoğaltma tamamlanmadan kapattığınızda aşağı duruma geçer. Kalıcı durum diskte depolanan Bu çoğaltma ile ilişkili temizlenmez.

- **Çoğaltma kümeden kaldırma**: Service Fabric kalıcı durum kaldırın ve bir yineleme için çalışan kod kapatmanız gerekebilir. Bu kapatma çeşitli nedenlerle, örneğin, Yük Dengeleme olabilir.

### <a name="dropped-dd"></a>Bırakılan (gg)
Bırakılan durumda örneği artık düğümde çalışmıyor. Düğümde sol yok durumu yoktur. Bu noktada, Service Fabric sonunda de silinir Bu örneği hakkındaki meta veriler tutar.

### <a name="down-d"></a>(D)
Aşağı durumunda çoğaltma kod çalışmadığı, ancak bu yineleme için kalıcı durum o düğümde yok. Bir çoğaltma aşağı çeşitli nedenlerle--örneğin aşağı olan düğüm çoğaltma kod, bir uygulama yükseltmesi ya da çoğaltma hatalarının bir kilitlenme olabilir.

Düğümde Yükseltme tamamlandığında aşağı çoğaltma Service Fabric tarafından gerektiği şekilde, örneğin, açılır.

Çoğaltma rolü aşağı durumunda ilgili değildir.

### <a name="opening-op"></a>Açılış (işlem)
Service Fabric çoğaltma tekrar Getir yedekleme gerektiğinde aşağı çoğaltma açılış durumuna girer. Örneğin, bir düğümünde uygulama için bir kod yükseltme tamamlandıktan sonra bu durum olabilir. 

Uygulama konağı veya düğüm açılış çoğaltma için bir çökme gerçekleşirse aşağı durumuna geçer.

Çoğaltma rolü açılış durumunda ilgili değildir.

### <a name="standby-sb"></a>Bekleme (SB)
Bekleme kapandı ve ardından açılan bir kalıcı hizmet çoğaltmasını çoğaltmasıdır. Başka bir çoğaltmaya (çoğaltma durumu bölümü zaten varsa ve yapı işlemi hızlıdır çünkü) çoğaltma eklemeniz gerekiyorsa bu çoğaltma Service Fabric tarafından kullanılıyor olabilir. StandByReplicaKeepDuration süresi dolduktan sonra bekleme çoğaltmayı göz ardı edilir.

Uygulama konağı veya düğüm bekleme çoğaltma için bir çökme gerçekleşirse aşağı durumuna geçer.

Çoğaltma rolü bekleme durumunda ilgili değildir.

> [!NOTE]
> Aşağı değil veya bırakılan herhangi bir çoğaltma olarak kabul edilir *yukarı*.
>

> [!NOTE]
> Geçiş bırakılan durumuna herhangi bir durumdan kullanarak mümkündür **ForceRemove** seçeneğini `Remove-ServiceFabricReplica`.
>

## <a name="replica-role"></a>Çoğaltma rolü 
Çoğaltmanın rolü, onun çoğaltma kümesine işlevinde belirler:

- **Birincil (P)** : Bir birincil çoğaltma kümesindeki yapmaktan sorumlu olan okuma ve yazma işlemleri. 
- **ActiveSecondary (S)** : Birincil durum güncelleştirmeleri almak, bunları uygulamak ve daha sonra geri bildirimleri göndermek çoğaltmaları şunlardır. Kopya kümesinde birden çok etkin ikincil veritabanı vardır. Bu etkin ikincil veritabanı hizmeti üstesinden hataların sayısını belirler.
- **(I) IdleSecondary**: Bu çoğaltmaların birincil tarafından oluşturulur. Etkin ikincil yükseltilebilmesi durumu birincilden alıyorsunuz. 
- **Hiçbiri (N)** : Bu çoğaltmaların kopya kümesinde bir sorumluluğu yoktur.
- **Bilinmeyen (U)** : Tüm almadan önce bir çoğaltmasının ilk rol budur **ChangeRole** Service fabric'teki API çağrısı.

Aşağıdaki diyagramda, çoğaltma rolü geçişi ve ortaya çıkabilir bazı örnek senaryolar gösterilmektedir:

![Çoğaltma rolü](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U -&GT; P: Yeni bir birincil çoğaltmaya oluşturma.
- U -&GT; I: Yeni bir boş çoğaltma oluşturma.
- U -&GT; N: Bir yedek kopya silme işlemi.
- S: BEN -&GT; Yükseltme boşta, böylece onun bir onayları doğru çekirdek katkıda ikincil etkin ikincil.
- BEN P: -&GT; Birincil boşta ikincil yükseltme. Özel yeniden yapılandırmalar altında boşta ikincil birincil olarak doğru aday olduğunda bu durum oluşabilir.
- BEN N: -&GT; Boşta ikincil çoğaltmayı silme işlemi.
- S -> P: Birincil etkin ikincil yükseltme. Bu, yük devretme birincil ya da küme kaynak yöneticisi tarafından başlatılan bir birincil taşıma nedeniyle olabilir. Örneğin, bir uygulama yükseltmesi veya Yük Dengeleme yanıt olabilir.
- S -&GT; N: Etkin ikincil çoğaltmayı silme işlemi.
- S: P -&GT; Birincil çoğaltma indirgenmesi. Bu, küme kaynak yöneticisi tarafından başlatılan bir birincil taşıma nedeniyle olabilir. Örneğin, bir uygulama yükseltmesi veya Yük Dengeleme yanıt olabilir.
- P -&GT; N: Birincil çoğaltmayı silme işlemi.

> [!NOTE]
> Gibi daha üst düzey programlama modelleri [Reliable Actors](service-fabric-reliable-actors-introduction.md) ve [Reliable Services](service-fabric-reliable-services-introduction.md), çoğaltma rolleri ' geliştiriciden kavramını gizle. Aktör bir rol kavramı gerekli değildir. Hizmetlerinde, çoğu senaryo için büyük ölçüde basitleştirilmiştir.
>

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için şu makaleye bakın:

[Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)

