---
title: "Çoğaltmaları ve Azure Service Fabric durumlarda | Microsoft Docs"
description: "Çoğaltmaları ve örnekleri--kendi işlevi ve yaşam döngüleri anlama"
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: 
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: 4037fc869d3e26d52f33baa62c626f4621cd11f5
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="replicas-and-instances"></a>Çoğaltmaları ve örnekleri 
Bu makalede, durum bilgisi olan hizmetler ve durum bilgisi olmayan hizmetler örneklerini çoğaltmalarının yaşam döngüsüne genel bakış sunulmaktadır.

## <a name="instances-of-stateless-services"></a>Durum bilgisi olmayan hizmetler örnekleri
Durum bilgisiz hizmet örneği, küme düğümlerinden biri üzerinde çalışan hizmet mantığı kopyasıdır. Bir örnek bölüm içinde benzersiz olarak tanımlanır, **InstanceId**. Aşağıdaki şemada bir örnek yaşam döngüsü modellenir:

![Örnek yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>Inbuild (IB)
Küme Kaynak Yöneticisi'ni örneği için bir yerleştirme belirledikten sonra bu yaşam döngüsü durumuna girer. Örneğin düğümde başlatılır. Uygulama ana bilgisayarı başlatıldığında, örnek oluşturulur ve daha sonra açılır. Başlatma tamamlandıktan sonra örnek hazır duruma geçer. 

Uygulama ana bilgisayarı veya örnek için düğüm çökerse bırakılan durumuna geçiş yapar.

### <a name="ready-rd"></a>Hazır (RD)
Hazır durumda hazır ve çalışır düğümde örneğidir. Bu örnek güvenilir bir hizmet ise **RunAsync** çağrılmış. 

Uygulama ana bilgisayarı veya örnek için düğüm çökerse bırakılan durumuna geçiş yapar.

### <a name="closing-cl"></a>Kapatma (CL)
Kapatma durumda bu düğüm örneğinde kapatma işleminde Azure Service Fabric değil. Bu kapatma birçok nedenden dolayı--Örneğin, bir uygulama yükseltme, Yük Dengeleme veya Silinen hizmet olabilir. Kapatma tamamlandıktan sonra bırakılan bir duruma geçer.

### <a name="dropped-dd"></a>Bırakılan (gg)
Bırakılan durumda örneği artık düğümde çalışmıyor. Bu noktada, Service Fabric sonunda da silinir Bu örnek hakkındaki meta verileri korur.

> [!NOTE]
> Geçiş bırakılan durumuna herhangi bir durumdan kullanarak mümkündür **ForceRemove** seçeneği `Remove-ServiceFabricReplica`.
>

## <a name="replicas-of-stateful-services"></a>Durum bilgisi olan hizmetler, çoğaltmaları
Durum bilgisi olan hizmet çoğaltmasını küme düğümlerinden biri üzerinde çalışan hizmet mantığı kopyasıdır. Ayrıca, çoğaltma bu hizmetin durumunu bir kopyasını tutar. Yaşam döngüsü ve durum bilgisi olan çoğaltmaları davranışını iki ilgili kavramları açıklanmaktadır:
- Çoğaltma yaşam döngüsü
- Çoğaltma rolü

Aşağıdaki tartışma kalıcı durum bilgisi olan hizmetler açıklar. Volatile (veya bellek içi) durum bilgisi olan hizmetler için aşağı ve bırakılan durumlarını eşdeğerdir.

![Çoğaltma yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>Inbuild (IB)
Bir Inbuild çoğaltma oluşturulan veya çoğaltma kümesi katılmak için hazırlanmış bir çoğaltmadır. Çoğaltma rolü bağlı olarak farklı semantiği IB sahiptir. 

Uygulama ana bilgisayarı veya bir Inbuild çoğaltma düğümünü çökerse aşağı durumuna geçiş yapar.

   - **Birincil Inbuild çoğaltmaların**: birincil Inbuild olan bir bölüm için ilk çoğaltma. Bu çoğaltma, genellikle bölümü oluşturulduğunda gerçekleşir. Birincil bir bölümün tüm çoğaltmaları yeniden başlattığınızda çoğaltmaları ayrıca ortaya Inbuild veya olan bırakıldı.

   - **IdleSecondary Inbuild çoğaltmaların**: küme kaynak yöneticisi tarafından oluşturulan ya da yeni çoğaltmaları bunlar ya da aşağı ve eklenmesi gereken oluştu mevcut çoğaltmaları kümesine yedekleyin. Bu çoğaltmalar sağlanmış veya çoğaltma kümesi ActiveSecondary olarak katılmak ve işlemleri çekirdek bildirim içinde katılmak için önce birincil tarafından oluşturulmuş.

   - **ActiveSecondary Inbuild çoğaltmaların**: Bu durum bazı sorgular gözlenir. Bu, çoğaltma belirlendiği bir iyileştirme değiştirmeden, ancak bir yineleme oluşturulması gerekiyor olur. Çoğaltma normal durumu makine geçişleri (çoğaltma rollerinde bölümünde açıklandığı gibi) izler.

### <a name="ready-rd"></a>Hazır (RD)
Hazır bir çoğaltma çoğaltma ve çekirdek bildirim işlemlerinin katılan bir çoğaltmadır. Hazır birincil ve etkin ikincil çoğaltmalar için geçerli değil.

Uygulama ana bilgisayarı veya hazır bir çoğaltma için düğümünü çökerse aşağı durumuna geçiş yapar.

### <a name="closing-cl"></a>Kapatma (CL)
Bir çoğaltma aşağıdaki senaryolarda kapatma durumuna girer:

- **Çoğaltma için kod kapatma**: Service Fabric kod çalıştırırken bir çoğaltma için kapatmak üzere gerekebilir. Bu kapatma için birçok nedeni olabilir. Örneğin, bir uygulama, doku veya altyapı yükseltmesi nedeniyle ya da çoğaltma tarafından raporlanan bir hata nedeniyle oluşabilir. Çoğaltma tamamlanmadan kapattığınızda, çoğaltma aşağı duruma geçer. Diskte depolanan Bu çoğaltma ile ilişkili kalıcı durum temizlenmez.

- **Çoğaltma kümeden kaldırmadan**: Service Fabric kalıcı durum kaldırmak ve bir çoğaltma için kod çalıştırırken kapatmak gerekebilir. Bu nedenlerle, örneğin, Yük Dengeleme kapatılması.

### <a name="dropped-dd"></a>Bırakılan (gg)
Bırakılan durumda örneği artık düğümde çalışmıyor. Düğümde sol yok durumu yok. Bu noktada, Service Fabric sonunda da silinir Bu örnek hakkındaki meta verileri korur.

### <a name="down-d"></a>(D)
Aşağı durumda çoğaltma kod çalışmıyor, ancak bu düğümde Bu çoğaltma kalıcı durum yok. Bir çoğaltma aşağı çeşitli nedenlerle--Örneğin, aşağı, olan düğüm çoğaltma kodu, uygulama yükseltmesi ya da çoğaltma hatalarının bir kilitlenme olabilir.

Yükseltme düğümde tamamlandığında aşağı çoğaltma Service Fabric tarafından gerektiği şekilde, örneğin, açılır.

Çoğaltma rolü aşağı durumunda ilgili değildir.

### <a name="opening-op"></a>Açılış (işlem)
Service Fabric çoğaltma yeniden Getir yedekleme gerektiğinde aşağı çoğaltma açılırken durumuna girer. Örneğin, bir düğümde uygulama için bir kod yükseltme tamamlandıktan sonra bu durum olabilir. 

Uygulama ana bilgisayarı veya bir açılış çoğaltma düğümünü çökerse aşağı durumuna geçiş yapar.

Çoğaltma rolü açılış durumunda ilgili değildir.

### <a name="standby-sb"></a>Bekleme (SB)
Bekleme çoğaltma kapandı ve ardından açılmış kalıcı bir hizmetin bir çoğaltmadır. Başka bir çoğaltma (çoğaltma durumu kısmı zaten varsa ve yapı işlemi hızlıdır olduğundan) çoğaltma eklemek gerekiyorsa bu çoğaltma Service Fabric tarafından kullanılıyor olabilir. StandByReplicaKeepDuration süresi dolduktan sonra bekleme çoğaltma göz ardı edilir.

Uygulama ana bilgisayarı veya bir bekleme çoğaltma düğümünü çökerse aşağı durumuna geçiş yapar.

Çoğaltma rolü bekleme durumunda ilgili değildir.

> [!NOTE]
> Kapalı değil ya da bırakılan herhangi bir çoğaltma olarak kabul edilir *yukarı*.
>

> [!NOTE]
> Geçiş bırakılan durumuna herhangi bir durumdan kullanarak mümkündür **ForceRemove** seçeneği `Remove-ServiceFabricReplica`.
>

## <a name="replica-role"></a>Çoğaltma rolü 
Çoğaltma rolü kendi işlevini yineleme kümesindeki belirler:

- **Birincil (P)**: bir birincil çoğaltma kümesinde gerçekleştirmek için sorumlu okuma ve yazma işlemleri. 
- **ActiveSecondary (S)**: birincil sunucudan durum güncelleştirmeleri almak, bunları uygulayın ve ardından geri bildirimleri gönderin çoğaltmaların bunlar. Çoğaltma kümesinde birden fazla etkin ikincil öğe yok. Bu etkin ikincil kopya sayısını hizmet işleyebilir dayanabileceği arıza sayısını belirler.
- **IdleSecondary (ı)**: Bu çoğaltmalar birincil tarafından oluşturulmakta. Etkin ikincil yükseltilebilmesi için önce birincil sunucudan durum aldığını. 
- **Hiçbiri (N)**: Bu çoğaltmalar kopya kümesinde bir sorumluluğu yoktur.
- **Bilinmeyen (U)**: herhangi bir almadan önce bu ilk çoğaltmaların rolüdür **ChangeRole** Service Fabric API çağrısından.

Aşağıdaki diyagramda, çoğaltma rolü geçişler ve ortaya çıkabilir bazı örnek senaryolar gösterilmektedir:

![Çoğaltma rolü](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U -> Yeni bir birincil çoğaltmaya P: oluşturma.
- U -> Yeni bir boş çoğaltma I: oluşturma.
- U -> bekleme çoğaltma N: silinmesini.
- Böylece kendi onayları katkıda çekirdek etkin ikincil boşta ikinciye S: yükseltme ->.
- I birincil boşta ikincil P: yükseltme ->. Birincil olarak doğru adayı boşta ikincil bu özel yeniden yapılandırmaların altında ortaya çıkar.
- I N: silinmesini boşta ikincil çoğaltma ->.
- S -> etkin ikincil birincil P: yükseltme. Bu, yük devretme birincil ya da küme kaynak yönetici tarafından başlatılan bir birincil taşıma nedeniyle olabilir. Örneğin, bir uygulamanın yükseltmesi veya Yük Dengeleme yanıt olabilir.
- S N: silinmesini etkin ikincil çoğaltma ->.
- P S: indirgeme birincil çoğaltma ->. Bu, küme kaynak yönetici tarafından başlatılan bir birincil taşıma nedeniyle olabilir. Örneğin, bir uygulamanın yükseltmesi veya Yük Dengeleme yanıt olabilir.
- P N: silinmesini birincil çoğaltma ->.

> [!NOTE]
> Gibi daha üst düzey programlama modelleri [Reliable Actors](service-fabric-reliable-actors-introduction.md) ve [Reliable Services](service-fabric-reliable-services-introduction.md), geliştirici çoğaltma rollerden kavramı gizle. Aktör bir rol kavramı gerekli değildir. Hizmetleri, bu çoğu senaryoları için büyük ölçüde basitleştirilmiştir.
>

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramlarla ilgili daha fazla bilgi için aşağıdaki makaleye bakın:

[Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)

