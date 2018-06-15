---
title: Azure Site Recovery ile Azure trafik Yöneticisi | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure Traffic Manager kullanmayı açıklar
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: d5b8887d4013f688cd20a0b2e4f6c0dbd5bdc9b6
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34071368"
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Site Recovery ile Azure Traffic Manager

Azure trafik Yöneticisi, uygulama uç noktalar arasında trafiğinin dağıtımını denetlemenize olanak sağlar. Bir uç nokta içinde veya Azure dışında barındırılan tüm Internet'e hizmetidir.

Trafik yöneticisi etki alanı adı sistemi (DNS) istemci isteklerini trafik yönlendirme yöntemini ve uç noktaları durumunu göre en uygun uç noktasına yönlendirmek için kullanır. Trafik Yöneticisi sağlayan bir dizi [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](../traffic-manager/traffic-manager-monitoring.md) farklı uygulama gereksinimlerini ve otomatik yük devretme modelleri uyacak şekilde. İstemcileri seçili uç noktasına doğrudan bağlanın. Trafik Yöneticisi bir proxy veya ağ geçidi değildir ve istemci ile hizmet arasında geçen trafiğine görmez.

Bu makalede, Azure Site Recovery'nin güçlü olağanüstü durum kurtarma ve geçiş yeteneklerine ile Azure trafik monitörün akıllı yönlendirme nasıl birleştirebileceğiniz açıklanmaktadır.

## <a name="on-premises-to-azure-failover"></a>Şirket içi Azure yük devretme

İlk senaryo için göz önünde bulundurun **Şirket A** kendi şirket içi ortamda çalışan kendi uygulama altyapısı vardır. İş devamlılığı ve uyumluluk nedenleriyle **Şirket A** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

**Şirket A** ortak uç ile çalışan uygulamalar ve sorunsuz bir şekilde trafiği için Azure bir olağanüstü yeniden yönlendirme istemektedir. [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafik yönlendirme yöntemini Azure trafik Yöneticisi'nde şirket kolayca bu yük devretme desen uygulamak için bir izin verir.

Kurulum aşağıdaki gibidir:
- **Şirket A** oluşturur bir [trafik Yöneticisi profili](../traffic-manager/traffic-manager-create-profile.md).
- Kullanılarak **öncelik** yönlendirme yöntemini **Şirket A** iki uç nokta – oluşturur **birincil** şirket içi ve **yük devretme** Azure için. **Birincil** öncelik 1 atanır ve **yük devretme** öncelik 2 atanır.
- Bu yana **birincil** endpoint Azure dışında barındırılan, uç nokta olarak oluşturulan bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç noktası.
- Azure Site Recovery ile herhangi bir sanal makineler veya yük devretme önce çalışan uygulamalar Azure site yok. Bu nedenle, **yük devretme** uç noktası olarak oluşturulan ayrıca bir **dış** uç noktası.
- Varsayılan olarak, bu uç ile ilişkili en yüksek önceliğe sahip olduğundan kullanıcı trafiğinin şirket içi uygulamaya yönlendirilir. Hiçbir trafik Azure if yönlendirilmiş **birincil** uç nokta sağlam.

![Şirket içi-Azure'a yük devretme önce](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

Bir olağanüstü, Şirket A tetikleyebilir bir [yük devretme](site-recovery-failover.md) Azure ve Azure üzerinde kendi uygulamalarına kurtarın. Azure Traffic Manager algıladığında, **birincil** uç noktası artık sağlıklı, otomatik olarak kullanır **yük devretme** uç noktası DNS yanıtının ve kullanıcıların bağlanın kurtarılan uygulama Azure.

![Şirket içi-Azure'a yük devretme sonrasında](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

İş gereksinimlerinize bağlı olarak **Şirket A** daha yüksek seçebilirsiniz veya daha düşük [yoklama sıklığı](../traffic-manager/traffic-manager-monitoring.md) şirket içi Azure bir olağanüstü arasında geçiş yapma ve kullanıcılar için en düşük kapalı kalma emin olun.

Olağanüstü durum içerdiğinde **Şirket A** azure'dan kendi şirket içi ortamına olabilir ([VMware](vmware-azure-failback.md) veya [Hyper-V](hyper-v-azure-failback.md)) Azure Site RECOVERY'yi kullanarak. Şimdi, trafik Yöneticisi algıladığında, **birincil** uç noktası sağlıklı yeniden, otomatik olarak kullanan **birincil** DNS yanıtları içindeki uç nokta.

## <a name="on-premises-to-azure-migration"></a>Şirket içi Azure geçiş

Olağanüstü durum kurtarma yanı sıra Azure Site Recovery ayrıca sağlar [Azure geçişler](migrate-overview.md). Azure Site Recovery'nin güçlü test yük devretme yetenekleri kullanarak, müşterilerin kendi şirket içi ortamına etkilemeden Azure uygulama performansına değerlendirebilirsiniz. Ve müşteriler geçirmeye hazır olduğunuzda, bunlar tüm iş yüklerinin birlikte geçirmek veya geçiş ve kademeli olarak ölçekleme seçmek seçebilirsiniz.

Azure Traffic Manager'ın [Weighted](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) yönlendirme yöntemi, şirket içi ortamına çoğunluğu yönlendirerek sırasında Azure gelen trafiğin bir kısmını yönlendirmek için kullanılabilir. Bu yaklaşım, daha fazla bilgi ve iş yüklerinizin daha fazla bilgi için Azure geçirirken Azure'a atanan ağırlığı artırma devam ederken ölçek performansını değerlendirmek yardımcı olabilir.

Örneğin, **Şirket B** bazı uygulama ortamı içi rest korurken taşımayı aşamaları geçirmek seçer. Ortamının çoğu şirket içi, ilk aşamaları sırasında daha büyük bir ağırlık şirket içi ortama atanır. Trafik Yöneticisi kullanılabilir Uç noktalara atanan ağırlığı dayalı bir uç nokta döndürür.

![Şirket içi-Azure'a geçiş](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Geçiş sırasında her iki uç noktaları etkindir ve trafiğin çoğu şirket içi ortama yönlendirilir. Geçiş devam ettikçe daha büyük bir ağırlık Azure ile ilgili uç noktasına atanabilir ve şirket içi uç noktası devre dışı bırakılan post geçiş son olabilir.

## <a name="azure-to-azure-failover"></a>Azure için Azure yük devretme

Bu örnekte, göz önünde bulundurun **şirket C** Azure çalışan kendi uygulama altyapısı vardır. İş devamlılığı ve uyumluluk nedenleriyle **şirket C** uygulamaları korumak için Azure Site Recovery kullanmaya karar verir.

**Şirket C** ortak uç ile çalışan uygulamalar ve sorunsuz bir olağanüstü farklı Azure bölgesinde trafiği yönlendirmek istemektedir. [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafik yönlendirme yöntemini sağlar **şirket C** kolayca bu yük devretme desen uygulamak için.

Kurulum aşağıdaki gibidir:
- **Şirket C** oluşturur bir [trafik Yöneticisi profili](../traffic-manager/traffic-manager-create-profile.md).
- Kullanılarak **öncelik** yönlendirme yöntemini **şirket C** iki uç nokta – oluşturur **birincil** kaynak bölge (Azure Doğu Asya) için ve **yük devretme** kurtarma bölgesinin (Azure Güneydoğu Asya). **Birincil** öncelik 1 atanır ve **yük devretme** öncelik 2 atanır.
- Bu yana **birincil** endpoint Azure'da barındırılan, uç nokta olarak olabilir bir [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) uç noktası.
- Azure Site Recovery ile herhangi bir sanal makineler veya yük devretme önce çalışan uygulamalar Azure site kurtarma sahip değil. Bu nedenle, **yük devretme** uç noktası olarak oluşturulabilir bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç noktası.
- Bu uç ile ilişkili en yüksek önceliğe sahip olarak varsayılan olarak, kullanıcı trafiğinin Kaynak bölgesi (Doğu Asya) uygulamaya yönlendirilir. Hiçbir trafik durumunda kurtarma bölgesine yönlendirilir **birincil** uç nokta sağlam.

![Azure Azure yük devretme önce](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

Bir olağanüstü durum olay **şirket C** tetikleyebilir bir [yük devretme](azure-to-azure-tutorial-failover-failback.md) ve kendi uygulamalarına Azure bölgesi kurtarma kurtarın. Azure Traffic Manager birincil uç noktası artık sağlıklı olduğunu algılarsa, otomatik olarak kullanan **yük devretme** uç noktası DNS yanıtının ve kullanıcıların bağlanın kurtarılan Azure bölgesi (Kurtarma üzerinde uygulama Güneydoğu Asya).

![Azure Azure yük devretme sonrasında](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

İş gereksinimlerinize bağlı olarak **şirket C** daha yüksek seçebilirsiniz veya daha düşük [yoklama sıklığı](../traffic-manager/traffic-manager-monitoring.md) kaynak ve kurtarma bölgeler arasında geçiş yapın ve kullanıcılar için en düşük kapalı kalma emin olun.

Olağanüstü durum içerdiğinde **şirket C** kurtarma'dan Azure bölgesine geri dönme Azure Site Kurtarma'yı kullanarak Azure bölgesi kaynağına olabilir. Şimdi, trafik Yöneticisi algıladığında, **birincil** uç noktası sağlıklı yeniden, otomatik olarak kullanan **birincil** DNS yanıtları içindeki uç nokta.

## <a name="protecting-multi-region-enterprise-applications"></a>Bölgeli Kurumsal uygulamaları koruma

Genel kuruluşların genellikle bölgesel gereksinimlerini karşılamak üzere uygulamalarını uyarlama tarafından müşteri deneyimini geliştirir. Uygulama altyapısı bölgeler arasında bölme yerelleştirme ve gecikme süresi azaltma neden olabilir. Şirketler Ayrıca belirli alanlarda bölgesel veri yasalarına bağlı olan ve bir bölümü uygulama altyapılarını bölgesel sınırları içinde yalıtmak seçin.  

Şimdi bir örneği göz önünde bulundurun nerede **şirket D** Almanya ve dünya geri kalanı ayrı olarak hizmet vermek için uygulama uç noktalarını bölündü. **Şirket D** Azure Traffic Manager'ın yararlanan [Geographic](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) bunu ayarlamak için yönlendirme yöntemi. Almanya tüm trafiği yönlendirildiği **uç nokta 1** ve Almanya dışında tüm trafiği yönlendirildiği **uç nokta 2**.

Bu kurulum sorun bu ise **uç nokta 1** herhangi bir nedenle çalışmamaya başlıyor hiçbir yeniden yönlendirme trafiği için yoktur **uç nokta 2**. Almanya trafiği devam için yönlendirilmesine **uç nokta 1** uç noktası sistem durumu ne olursa olsun, Almanca kullanıcıların erişim olmadan bırakarak **şirket D**'s uygulama. Benzer şekilde, varsa **uç nokta 2** çevrimdışı hiçbir yeniden yönlendirme trafiği için yoktur gider **uç nokta 1**.

![Önce bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Bu sorunla çalışmasını önlemek ve uygulama dayanıklılık sağlamak için **şirket D** kullanan [iç içe trafik Yöneticisi profillerine](../traffic-manager/traffic-manager-nested-profiles.md) Azure Site Recovery ile. Bir iç içe profil kurulumunda trafiği ayrı uç noktaları için bunun yerine diğer trafik Yöneticisi profillerine yönlendirilir değil. Bu kurulum şeklini aşağıda verilmiştir:
- Coğrafi tek tek uç ile yönlendirme kullanılarak yerine **şirket D** coğrafi trafik Yöneticisi profilleri ile yönlendirme kullanır.
- Her alt trafik Yöneticisi profili kullanan **öncelik** birincil ve kurtarma bitiş noktası ile yönlendirme, bu nedenle iç içe **öncelik** içinde yönlendirme **Geographic** yönlendirme.
- Uygulama dayanıklılığı etkinleştirmek için yük devretme durumunda olağanüstü durum olay tabanlı bir kurtarma bölge için Azure Site Recovery her iş yükü dağıtım kullanır.
- Trafik Yöneticisi üst DNS sorgu aldığında, ilgili alt sorgu kullanılabilir uç nokta ile yanıtlar trafik Yöneticisi için yönlendirilir.

![Sonra bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Örneğin, Almanya merkezi uç başarısız olursa, uygulama hızlı bir şekilde Almanya Kuzeydoğu kurtarılabilir. Yeni uç nokta Almanya kullanıcılar için en az kapalı kalma süresi ile trafiği işler. Benzer şekilde bir uç nokta kesinti Batı Avrupa'da Kuzey Avrupa için uygulama iş yükü için kullanılabilir uç nokta DNS yönlendiren Azure Traffic Manager işleme ile kurtarma tarafından işlenebilir.

Yukarıdaki Kurulum, gereken sayıda bölge ve uç nokta birleşim içerecek şekilde genişletilebilir. Trafik Yöneticisi iç içe profil en fazla 10 düzeyleri sağlar ve iç içe geçmiş yapılandırma içinde döngülere izin vermez.

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma süresi hedefi (RTO) değerlendirmeleri

Çoğu kuruluşta, ekleme veya DNS kayıtlarını değiştirme ayrı bir ekip veya kuruluş dışındaki bir kişi tarafından işlenir. Bu, çok zor DNS kayıtlarını değiştirme görevini kolaylaştırır. DNS kayıtlarını güncelleştirmek için diğer ekip veya kuruluşlar DNS altyapısı yönetme tarafından harcanan süre kuruluştan kuruluşa değişir ve uygulama RTO etkiler.

Trafik Yöneticisi yararlanarak frontload DNS güncelleştirmeleri için gereken iş olabilir. Herhangi bir el ile veya komut dosyası eylemi gerçek yük devretme sırasında gereklidir. Bu yaklaşımın bir olağanüstü maliyetli zaman DNS değişiklik hatalarını önleme yanı sıra hızlı geçiş (ve bu nedenle azaltmayı RTO) yardımcı olur. Trafik Yöneticisi ile bile geri dönme adım, aksi takdirde ayrı olarak yönetilen gerekirdi otomatik hale getirilmiştir.

Doğru ayarı [yoklama aralığı](../traffic-manager/traffic-manager-monitoring.md) temel veya hızlı aralığı sistem durumu denetimleri önemli ölçüde RTO yük devretme sırasında hale getirip kullanıcılar için kapalı kalma süresini kısaltabilir.

Trafik Yöneticisi profili için yaşam süresi (TTL) değerine DNS süresi ayrıca iyileştirebilirsiniz. TTL kendisi için bir DNS girişi istemci tarafından önbelleğe değerdir. Bir kaydı için DNS iki kez TTL aralık içinde sorgulanmasını değil. Kendisiyle ilişkili bir TTL her DNS kaydı yok. Bu değer azaltma trafik Yöneticisi için daha fazla DNS sorgularının sonuçlarını ancak RTO kesintilerine karşı daha hızlı bularak azaltabilir.

DNS Çözümleyicileri yetkili DNS sunucusu ile istemci arasında sayısı artarsa istemci tarafından yaşadı TTL ayrıca artırmaz. DNS Çözümleyicileri 'TTL sayımını ' ve yalnızca kayıt önbelleğe ilişkili olduğundan, geçen süre yansıtan bir TTL değerini geçirin. Bu DNS Çözümleyicileri sayısını zincirindeki yedeklemiş TTL sonra DNS kaydı istemcide yenilenir sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Trafik Yöneticisi hakkında daha fazla bilgi [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).
- Daha fazla bilgi edinmek [iç içe trafik Yöneticisi profillerine](../traffic-manager/traffic-manager-nested-profiles.md).
- Daha fazla bilgi edinmek [uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md).
- Daha fazla bilgi edinmek [kurtarma planlarına](site-recovery-create-recovery-plans.md) uygulama yük devretmeyi otomatikleştirmek için.
