---
title: Azure Site Recovery ile Azure Traffic Manager | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure Traffic Manager kullanmayı açıklar
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 6c77cd43231d4596535c11564313a0fe90633cdb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60947819"
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Site Recovery ile Azure Traffic Manager

Azure Traffic Manager, uygulama uç noktalar genelinde trafiğinin dağıtımını denetlemenizi sağlar. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Traffic Manager trafik yönlendirme yöntemi ve sistem durumu uç nokta göre en uygun uç noktaya, istemci istekleri yönlendirmek için etki alanı adı sistemi (DNS) kullanır. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](../traffic-manager/traffic-manager-monitoring.md) sunar. İstemciler, seçili uç noktaya doğrudan bağlanır. Traffic Manager, bir ara sunucu veya bir ağ geçidi değildir ve istemci ile hizmet arasında geçen trafiği görmez.

Bu makalede, Azure Site Recovery'nin güçlü bir olağanüstü durum kurtarma ve geçiş özellikleri sayesinde Azure Traffic izleyicinin akıllı yönlendirme nasıl birleştirebilirsiniz açıklanır.

## <a name="on-premises-to-azure-failover"></a>Şirket içi Azure yük devretme

İlk senaryo için göz önünde bulundurun **şirketi** kendi şirket içi ortamda çalışan kendi uygulama altyapısı vardır. İş sürekliliği ve uyumluluk nedenleriyle **şirketi** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

**Şirketi** uygulamaları genel uç noktaları ile çalışıyorsa ve trafiği Azure'a olağanüstü durum olayında sorunsuz bir şekilde yönlendirmek için sağlayabilmek istemektedir. [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) şirket bir kolayca bu yük devretme deseni uygulamak için Azure Traffic Manager trafik yönlendirme yöntemine sağlar.

Kurulumu aşağıdaki gibidir:
- **Şirketi** oluşturur bir [Traffic Manager profili](../traffic-manager/traffic-manager-create-profile.md).
- Yararlanarak **öncelik** yönlendirme yöntemini **şirketi** iki uç nokta – oluşturur **birincil** şirket içi ve **yük devretme** Azure için. **Birincil** öncelik 1 atanır ve **yük devretme** öncelik 2 atanır.
- Bu yana **birincil** uç nokta Azure dışında barındırılan, uç nokta olarak oluşturulan bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç noktası.
- Herhangi bir sanal makine ya da yük devretme öncesinde çalışan uygulamalar, Azure Site Recovery ile Azure site yok. Bu nedenle, **yük devretme** uç nokta olarak oluşturulan ayrıca bir **dış** uç noktası.
- Uç noktanın ilişkili en yüksek önceliğe sahip olduğundan varsayılan olarak, kullanıcı trafiğini şirket içi uygulamaya yönlendirilir. Hiçbir trafik, Azure if yönlendirilir **birincil** sağlıklı uç noktadır.

![Şirket içi-Azure'a yük devretmeden önce](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

Olağanüstü bir durumda, şirket bir tetikleyebilirsiniz bir [yük devretme](site-recovery-failover.md) azure'a ve azure'da kendi uygulamalarını kurtarın. Azure Traffic Manager algıladığında, **birincil** uç nokta sağlıklı olup artık, otomatik olarak kullanan **yük devretme** uç nokta DNS yanıtı ve kullanıcılar üzerinde kurtarılır uygulama bağlanır Azure.

![Şirket içi-Azure'a yük devretme sonrasında](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

İş gereksinimlerine bağlı olarak **şirketi** daha yüksek bir seçim yapabilirsiniz veya daha düşük [yoklama sıklığı](../traffic-manager/traffic-manager-monitoring.md) şirket içi azure'a olağanüstü durum olay arasında geçiş yapma ve kullanıcılar için çok az kesinti emin olun.

Olağanüstü durum içerdiğinde **şirketi** azure'dan şirket içi ortamın olabilir ([VMware](vmware-azure-failback.md) veya [Hyper-V](hyper-v-azure-failback.md)) Azure Site Recovery kullanarak. Artık, Traffic Manager algıladığında, **birincil** sağlıklı yeniden olan uç nokta, otomatik olarak kullanan **birincil** DNS yanıtları uç noktası.

## <a name="on-premises-to-azure-migration"></a>Şirket içinden Azure'a geçiş

Olağanüstü durum kurtarma ek olarak, Azure Site Recovery ayrıca sağlar [azure'a geçişini](migrate-overview.md). Azure Site Recovery'nin güçlü test yük devretme özellikleri kullanarak, müşterilerin kendi şirket içi ortamınızı etkilemeden azure'da uygulama performansını değerlendirebilirsiniz. Ve müşterilerin geçirmeye hazır olduğunuzda, bunlar geçmek ve yavaş yavaş ölçeği seçin veya tüm iş yükleri geçişini birlikte seçebilirsiniz.

Azure Traffic Manager'ın [ağırlıklı](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) yönlendirme yöntemi, çoğu şirket içi ortama yönlendirirken bazı bölümlerinin gelen Azure trafiği yönlendirmek için kullanılabilir. Bu yaklaşım daha fazlasını iş yüklerinizi Azure'a geçirirken Azure'a atanan ağırlığı artırma devam ederken ölçek performansı değerlendirmeniz yardımcı olabilir.

Örneğin, **Şirket B** aşamalardaki kendi uygulama ortamı bazıları şirket rest korurken taşıma, geçirme seçer. Çoğu ortam, şirket içinde olduğunda ilk aşamaları sırasında şirket içi ortama daha büyük bir ağırlık atanır. Traffic manager kullanılabilir uç noktaları için atanan ağırlıklara göre bir uç nokta döndürür.

![Şirket içi-Azure'a geçiş](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Geçiş sırasında her iki bitiş noktası etkin olan ve trafiğin çoğu, şirket içi ortama yönlendirilir. Geçiş devam ettikçe daha büyük bir ağırlık azure'da uç noktasına atanabilir ve son olarak, şirket içi uç nokta geçiş sonrasında devre dışı bırakılmış olabilir.

## <a name="azure-to-azure-failover"></a>Azure'dan Azure'a yük devretme

Bu örnekte, göz önünde bulundurun **şirket C** Azure çalışan kendi uygulama altyapısı vardır. İş sürekliliği ve uyumluluk nedenleriyle **şirket C** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

**Şirket C** uygulamaları genel uç noktaları ile çalışıyorsa ve sorunsuz bir şekilde farklı bir Azure bölgesine olağanüstü durum olay trafiği yönlendirmek için sağlayabilmek istemektedir. [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafik yönlendirme yöntemine sağlayan **şirket C** kolayca bu yük devretme deseni uygulamak için.

Kurulumu aşağıdaki gibidir:
- **Şirket C** oluşturur bir [Traffic Manager profili](../traffic-manager/traffic-manager-create-profile.md).
- Yararlanarak **öncelik** yönlendirme yöntemini **şirket C** iki uç nokta – oluşturur **birincil** Kaynak bölgesi (Azure Doğu Asya) için ve **Yükdevretme** kurtarma bölgesi (Azure Güneydoğu Asya) için. **Birincil** öncelik 1 atanır ve **yük devretme** öncelik 2 atanır.
- Bu yana **birincil** uç nokta Azure'da barındırılan, uç nokta olarak olabilir bir [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) uç noktası.
- Azure Site Recovery ile Azure site recovery, sanal makine ya da yük devretme öncesinde çalışan uygulamalar yok. Bu nedenle, **yük devretme** uç nokta olarak oluşturulabilir bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç noktası.
- Uç noktanın ilişkili en yüksek önceliğe sahip olduğundan, varsayılan olarak, kullanıcı trafiğini kaynak bölge (Doğu Asya) uygulamaya yönlendirilir. Hiçbir trafik durumunda kurtarma bölgeye yönlendirilir **birincil** sağlıklı uç noktadır.

![Azure-Azure yük devretmeden önce](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

Bir olağanüstü durum olayına **şirket C** tetikleyebilirsiniz bir [yük devretme](azure-to-azure-tutorial-failover-failback.md) ve uygulamaları Azure bölgesine kurtarma kurtarın. Azure Traffic Manager, birincil uç noktaya artık sağlam olmadığını algıladığında, otomatik olarak kullanan **yük devretme** Kurtarılan bir Azure bölgesine (Kurtarma üzerinde uygulama uç nokta DNS yanıtı ve kullanıcılar bağlanır Güneydoğu Asya).

![Azure-Azure yük devretmeden sonra](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

İş gereksinimlerine bağlı olarak **şirket C** daha yüksek bir seçim yapabilirsiniz veya daha düşük [yoklama sıklığı](../traffic-manager/traffic-manager-monitoring.md) kaynak ve kurtarma bölgeler arasında geçiş yapabilir ve kullanıcılar için çok az kesinti emin olun.

Olağanüstü durum içerdiğinde **şirket C** Azure bölgesi kurtarma'dan Azure Site Recovery ile Azure bölgesi kaynak veritabanını yeniden çalıştırabilirsiniz. Artık, Traffic Manager algıladığında, **birincil** sağlıklı yeniden olan uç nokta, otomatik olarak kullanan **birincil** DNS yanıtları uç noktası.

## <a name="protecting-multi-region-enterprise-applications"></a>Çok bölgeli Kurumsal uygulamaları koruma

Küresel kuruluşlar genellikle bölgesel gereksinimlerini karşılamak için uygulamalarına konuşma tarzına uyarlayarak müşteri deneyimini geliştirin. Yerelleştirme ve gecikme süresini azaltma bölgeler arasında bölünmüş uygulama altyapısı açabilir. Kuruluşların belirli alanlarda bölgesel veri yasalar tarafından bağlanmış ve bir bölümü uygulama altyapılarını bölgesel sınırlar içinde yalıtmak seçin.  

Bir örnek düşünelim burada **şirket D** Almanya ve dünyanın kalanı ayrı olarak sunmak için kendi uygulama uç bölündü. **Şirket D** Azure Traffic Manager'ın yararlanan [Geographic](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) bunu ayarlamak için yönlendirme yöntemi. Almanya kaynaklı kaynaklanan tüm trafik yönlendirilir **uç nokta 1** ve Almanya kaynaklanan tüm trafik yönlendirilir **uç nokta 2**.

Bu kurulum ile ilgili sorun ise **uç nokta 1** herhangi bir nedenle çalışmayı durdurduysa trafiğinin herhangi bir yeniden yönlendirme var. **uç nokta 2**. Almanya kaynaklı kaynaklanan trafik için yönlendirilebilir eder **uç nokta 1** uç noktasının durumunu bağımsız olarak, Alman kullanıcıların erişimi olmadan bırakarak **şirket D**ait uygulama. Benzer şekilde, varsa **uç nokta 2** çevrimdışı trafiğinin herhangi bir yeniden yönlendirme var. gider **uç nokta 1**.

![Önce çok bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Bu sorunla çalışmasını önlemek ve uygulama dayanıklılığı sağlamak için **şirket D** kullanan [iç içe Traffic Manager profillerini](../traffic-manager/traffic-manager-nested-profiles.md) Azure Site Recovery ile. Bir iç içe geçmiş profil kurulumunda trafik için tekil uç noktalarını, ancak bunun yerine diğer Traffic Manager profillerine yönlendirilir değil. Bu kurulum şeklini aşağıda verilmiştir:
- Tek tek uç noktaları ile coğrafi yönlendirme kullanan yerine **şirket D** Traffic Manager profilleri ile coğrafi yönlendirme kullanır.
- Her alt Traffic Manager profili kullanan **öncelik** birincil ve kurtarma uç noktası ile yönlendirme, bu nedenle iç içe **öncelik** içinde yönlendirme **Geographic** yönlendirme.
- Uygulama dayanıklılığı etkinleştirmek için Azure Site Recovery'ye yük devretme durumunda bir olağanüstü durum olay tabanlı bir kurtarma bölgeye her iş yükü dağıtım kullanır.
- Traffic Manager üst DNS sorgu aldığında, ilgili alt sorgu kullanılabilir uç nokta ile yanıt veren bir Traffic Manager için yönlendirilir.

![Sonra da çok bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Örneğin, Almanya Orta, uç nokta başarısız olursa, uygulamanın hızla Almanya Kuzeydoğu kurtarılabilir. Yeni uç nokta, kullanıcılar için en düşük kapalı kalma süresi ile Almanya kaynaklı kaynaklanan trafiğini işler. Benzer şekilde bir uç nokta kesinti Batı Avrupa'da Kuzey Avrupa, uygulama iş yükünü kullanılabilir uç nokta için DNS yönlendiren Azure Traffic Manager işleme ile kurtarma tarafından işlenebilir.

Yukarıdaki Kurulum, gereken sayıda bölge ve uç nokta birleşim içerecek şekilde genişletilebilir. Traffic Manager, iç içe geçmiş profilleri en fazla 10 düzeylerine izin verir ve iç içe geçmiş yapılandırması içindeki döngülere izin vermez.

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma süresi hedefi (RTO) konuları

Birçok kuruluşta, ayrı bir ekip veya kuruluş dışındaki birine ekleme veya DNS kayıtlarını değiştirme işlenir. Bu, DNS kayıtları çok zor değiştirme görevini sağlar. DNS kayıtlarını güncelleştirmeye yönelik diğer takımlar veya DNS altyapı yönetimiyle kuruluşlar tarafından harcanan süre kuruluştan kuruluşa değişir ve uygulamanın RTO etkiler.

Traffic Manager yararlanarak frontload DNS güncelleştirmeleri için gereken çalışmayı kullanabilirsiniz. Gerçek bir yük devretme sırasında herhangi bir el ile veya betikle eylemi gereklidir. Bu yaklaşım bir olağanüstü durum olay maliyetli zaman DNS değişikliği hataların önlenmesiyle yanı sıra hızlı geçiş (ve bu nedenle azaltmayı RTO) yardımcı olur. Traffic Manager ile aksi takdirde ayrı olarak yönetilen zorunda bile yeniden çalışma adım otomatikleştirildi.

Doğru ayarlama [yoklama aralığı](../traffic-manager/traffic-manager-monitoring.md) temel veya hızlı aralıklı durum denetimleri önemli ölçüde RTO yük devretme sırasında uygulayabileceğinizi ve kullanıcılar için kapalı kalma süresini azaltın.

Ayrıca, yaşam süresi (TTL) değeri Traffic Manager profili için DNS zaman iyileştirebilirsiniz. TTL kendisi için bir DNS girişi bir istemci tarafından önbelleğe değerdir. Bir kaydı için DNS iki kez TTL aralığında sorgulanması değil. Her bir DNS kaydı ile ilişkilendirilmiş bir TTL sahiptir. Bu değer azaltma, Traffic Manager için daha fazla DNS sorgularının sonuçlarını ancak RTO kesintiler daha hızlı bir şekilde bularak azaltabilir.

Yetkili DNS sunucusu ile istemci arasında DNS Çözümleyicileri sayısı artarsa istemci tarafından yaşadı TTL de artırmaz. DNS Çözümleyicileri 'TTL saymaya' ve yalnızca kayıt önbelleğe ilişkili olduğundan, geçen süreyi gösteren bir TTL değeri geçirin. Bu zincirdeki DNS Çözümleyicileri sayısından bağımsız olarak TTL sonra DNS kaydını istemcide yenilenir sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Traffic Manager hakkında daha fazla bilgi edinin [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).
- Daha fazla bilgi edinin [iç içe Traffic Manager profillerini](../traffic-manager/traffic-manager-nested-profiles.md).
- Daha fazla bilgi edinin [uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md).
- Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
