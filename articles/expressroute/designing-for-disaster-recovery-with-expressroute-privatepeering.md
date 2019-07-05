---
title: Azure ExpressRoute ile olağanüstü durum kurtarma için tasarlama | Microsoft Docs
description: Bu sayfa, Azure Expressroute'u kullanırken, olağanüstü durum kurtarma için Mimari öneriler sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 05/25/2019
ms.author: rambala
ms.openlocfilehash: cf2b4e385de901254fde3c3d3e807feda98d5b41
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466065"
---
# <a name="designing-for-disaster-recovery-with-expressroute-private-peering"></a>ExpressRoute özel eşlemesi ile olağanüstü durum kurtarma için tasarlama

ExpressRoute, taşıyıcı Microsoft kaynakları sınıf özel ağ bağlantısı sağlamak yüksek kullanılabilirlik için tasarlanmıştır. Diğer bir deyişle, hiçbir tek Microsoft ağı içerisinde ExpressRoute yolunda hata noktası yoktur. Bir ExpressRoute bağlantı hattı kullanılabilirliği en üst düzeye çıkarmak için tasarım konuları, bkz: [ExpressRoute ile yüksek kullanılabilirlik için tasarlama][HA].

Ancak ayırdığınız Murphy'nın popüler adage--*herhangi bir şey yanlış gidebilir, götürür*--göz önünde bulundurarak, bu makaledeki bize tarafından tek bir ExpressRoute bağlantı hattı kullanılarak çözülebilir hataları geçebilen çözümler üzerinde odaklanın. Diğer bir deyişle, bu makalede, coğrafi olarak yedekli bir ExpressRoute bağlantı hatları kullanarak olağanüstü durum kurtarma için sağlam bir arka uç ağ bağlantısı oluşturmak için ağ altyapısı konuları halinde bize arayın.

## <a name="need-for-redundant-connectivity-solution"></a>Yedekli bağlantı çözümü gereksinimi

Olasılıklar ve burada (ağ hizmeti sağlayıcıları, müşteri veya diğer bulut hizmeti sağlayıcıları, Microsoft, bunun olması) tüm bir bölgesel hizmet düzeyi düşürülmüş örnekleri vardır. Bu tür bölgesel geniş hizmet etkisi kök nedenini doğal calamity içerir. Bu nedenle, iş sürekliliği ve görev açısından kritik uygulamalar için olağanüstü durum kurtarma planlaması önemlidir.   

İster görev açısından kritik uygulamalarınızı bir Azure bölgesi veya şirket içinde veya başka bir yerde çalıştırdığınız bağımsız olarak, başka bir Azure bölgesine yük devretme siteniz olarak kullanabilirsiniz. Aşağıdaki makaleler adresleri olağanüstü durum kurtarma uygulamalardan ve ön uç erişimi Perspektifler:

- [Kurumsal ölçekte olağanüstü durum kurtarma][Enterprise DR]
- [Azure Site Recovery ile işletmelerde olağanüstü durum kurtarma][SMB DR]

Görev açısından kritik işlemler için şirket içi ağınız ile Microsoft arasındaki ExpressRoute bağlantısı üzerinde güveniyorsanız, olağanüstü durum kurtarma planınızı ayrıca coğrafi olarak yedekli ağ bağlantısı içermelidir. 

## <a name="challenges-of-using-multiple-expressroute-circuits"></a>Birden çok ExpressRoute bağlantı hattına kullanmanın zorlukları

Birden fazla bağlantıyla ağlar aynı kümesini bağlantı, ağlar arasında paralel yollar sunar. Düzgün şekilde tasarlanmış, asimetrik yönlendirme için yol açabilecek yolları paralel. Durum bilgisi olan varlıkları (örneğin, NAT, güvenlik duvarı) yolu varsa, asimetrik yönlendirme, trafik akışını engelleyebilir.  Genellikle, ExpressRoute özel eşleme yolundan, NAT veya güvenlik duvarları gibi durum bilgisi olan varlıklar arasında gelen olmaz. Bu nedenle, ExpressRoute özel eşlemesi üzerinden asimetrik yönlendirme mutlaka trafik akışını engellemez.
 
Ancak, veya değil, durum bilgisi olan varlıklar olmasına bakılmaksızın, coğrafi olarak yedekli paralel yolları arasında trafiğin yükünü dengelemenizi yüklerseniz tutarsız ağ performansı karşılaşırsınız. Bu makalede, şimdi bu sorunları gidermek üzere nasıl ele almaktadır.

## <a name="small-to-medium-on-premises-network-considerations"></a>Küçük ve orta şirket içi ağ konuları

Aşağıdaki diyagramda gösterildiği örnek ağ düşünelim. Bu örnekte bir Contoso şirket içi konum ve bir Azure bölgesinde Contoso'nun VNet arasında coğrafi olarak yedekli ExpressRoute bağlantı kurulur. Diyagramdaki yeşil düz çizgi (ExpressRoute 1) aracılığıyla tercih edilen yol gösterir ve noktalı bir gelebilmek yolu (ExpressRoute 2) aracılığıyla temsil eder.

[![1]][1]

Olağanüstü durum kurtarma için ExpressRoute bağlantısını tasarlarken dikkate almanız gerekir:

- coğrafi olarak yedekli bir ExpressRoute bağlantı hatları kullanma
- farklı ExpressRoute bağlantı hattı için farklı hizmet sağlayıcısı ağ kullanma
- Her ExpressRoute bağlantı hattı için tasarlama [yüksek kullanılabilirlik][HA]
- Müşteri ağ üzerindeki farklı bir konumda farklı ExpressRoute bağlantı hattı sonlandırılıyor

Varsayılan olarak, yolları tüm ExpressRoute yolları üzerinde aynı şekilde tanıtmak varsa Azure olacak bağlı şirket trafik yükünü dengele eşit maliyetli çoklu yol (ECMP) yönlendirmesi kullanarak tüm ExpressRoute yollarda.

Ancak, coğrafi olarak yedekli ExpressRoute bağlantı hatları ile farklı ağ yollarını (özellikle de ağ gecikmesi) ile göz önünde bulundurarak farklı ağ performanslarını gerçekleştirilecek ihtiyacımız var. Normal işlem sırasında daha tutarlı ağ performansı elde etmek için en düşük gecikme sunan ExpressRoute bağlantı hattını tercih isteyebilirsiniz.

Bir ExpressRoute bağlantı hattı üzerinden başka bir (verimliliğini sırasına göre listelenir) aşağıdaki tekniklerden birini kullanarak tercih ettiğiniz Azure'a etkileyebilir:

- diğer bir ExpressRoute devreden kıyasla tercih edilen ExpressRoute bağlantı hattı üzerinden daha belirli bir yolun tanıtılması
- daha yüksek bağlantı ağırlığına tercih edilen ExpressRoute bağlantı hattına sanal ağa bağlanan bağlantı yapılandırma
- uzun AS (yol başına gibi) yolu ile daha az tercih edilen ExpressRoute bağlantı hattı üzerinden yolların tanıtılması

### <a name="more-specific-route"></a>Daha belirli bir yol

Aşağıdaki diyagramda daha belirli yönlendirme Tanıtımı kullanarak konularında ExpressRoute yol seçimi gösterilmektedir. Resimli örnekte, Contoso şirket içi/24 IP aralığı tercih edilen yol (ExpressRoute 1) yoluyla iki /25 adres aralıkları ve gelebilmek yolu (ExpressRoute 2) aracılığıyla /24 olarak bildirilir.

[![2]][2]

/24 için karşılaştırıldığında /25 daha belirli olduğundan, Azure ExpressRoute 1'de normal durumu aracılığıyla 10.1.11.0/24 hedefleyen trafiği gönderir. Sonra her iki bağlantı ExpressRoute 1 kesilirse VNet yalnızca ExpressRoute 2 aracılığıyla 10.1.11.0/24 Yol Duyurusu bakın; ve bu nedenle, bu hata durumunda bekleme devre kullanılır.

### <a name="connection-weight"></a>Bağlantı ağırlığı

Aşağıdaki ekran görüntüsünde, Azure portal aracılığıyla bir ExpressRoute bağlantısı ağırlığını yapılandırma gösterilmektedir.

[![3]][3]

Aşağıdaki diyagramda, bağlantı ağırlığına kullanarak konularında ExpressRoute yol seçimi gösterilmektedir. Varsayılan bağlantı ağırlığına 0'dır. Aşağıdaki örnekte, bağlantı için ExpressRoute 1 değerini 100 yapılandırılır. Sanal ağ, bir sanal ağa birden fazla ExpressRoute bağlantı hattı tanıtılan bir rota öneki aldığında, en yüksek ağırlığı bağlantıyla tercih eder.

[![4]][4]

Sonra her iki bağlantı ExpressRoute 1 kesilirse VNet yalnızca ExpressRoute 2 aracılığıyla 10.1.11.0/24 Yol Duyurusu bakın; ve bu nedenle, bu hata durumunda bekleme devre kullanılır.

### <a name="as-path-prepend"></a>Yol olarak önüne ekleyin

Aşağıdaki diyagramda yolu önüne ekleyin kullanarak konularında ExpressRoute yol seçimi gösterilmektedir. Diyagramda, Yol Duyurusu 1 ExpressRoute üzerinden eBGP varsayılan davranışını belirtir. ExpressRoute 2 üzerinden rota tanıtım üzerinde şirket içi ağın ASN ayrıca rotaya ait yolu olarak eklenir. Aynı yol eBGP rota seçim işlemi başına birden fazla ExpressRoute devrenizin aracılığıyla alındığında VNet rotası kısa yol olarak tercih edebilir. 

[![5]][5]

Daha sonra her iki bağlantı ExpressRoute 1 kesilirse VNet yalnızca ExpressRoute 2 aracılığıyla 10.1.11.0/24 Yol Duyurusu bakın. Consequentially, uzun bir yol olarak önemli hale gelir. Bu nedenle, bekleme devre başarısız durumda kullanılır.

Azure ExpressRoute birini diğer tercih edilmesi etkilemek tekniklerden birini kullanarak, aynı zamanda şirket içi ağ sağlamak ihtiyacınız asimetrik akışlar önlemek için trafik Azure bağlı için de aynı ExpressRoute yolu tercih. Genellikle, yerel tercih değeri, şirket içi ağ diğer bir ExpressRoute bağlantı hattını tercih etkilemek için kullanılır. Yerel tercih bir iç BGP'yi (iBGP) unsurdur. Yerel tercih değeri en yüksek BGP yolu tercih edilir.

> [!IMPORTANT]
> Belirli bir ExpressRoute bağlantı hatları Beklemede kullandığınızda, etkin bir şekilde yönetmek ve yük devretme işlemi düzenli aralıklarla test gerekir. 
> 

## <a name="large-distributed-enterprise-network"></a>Dağıtılmış büyük bir kurumsal ağ

Dağıtılmış büyük bir kurumsal ağ varsa, büyük olasılıkla birden fazla ExpressRoute devreniz vardır. Bu bölümde, ek gelebilmek devreler gerek kalmadan aktif-aktif ExpressRoute bağlantı hatları kullanarak olağanüstü durum kurtarma tasarım nasıl görelim. 

Aşağıdaki diyagramda gösterildiği örnek düşünelim. Örnekte, Contoso iki içeren iki Contoso Iaas dağıtımını iki farklı Azure bölgelerindeki iki farklı eşleme konumları ExpressRoute işlem hatları aracılığıyla şirket içi konumlara bağlı. 

[![6]][6]

Biz nasıl olağanüstü durum kurtarma Mimar, çapraz bölge konumu (Bölge1/region2 location2/location1 için) trafiği nasıl yönlendirileceğini üzerinde bir etkisi yoktur. Farklı çapraz bölgeyi trafiği yönlendiren iki farklı olağanüstü durum mimarileri düşünelim.

### <a name="scenario-1"></a>Senaryo 1

Bir Azure bölgesi ve şirket içi ağ arasındaki tüm trafik akışı gibi kararlı durumda yerel ExpressRoute bağlantı hattı üzerinden ilk senaryoda şimdi olağanüstü durum kurtarma tasarlayın. Yerel ExpressRoute bağlantı hattı başarısız olursa, ardından uzak ExpressRoute devresine Azure arasındaki tüm trafik akışları için kullanılır ve şirket içinde ağ.

Senaryo 1, aşağıdaki diyagramda gösterilmiştir. Diyagramda, yeşil satırlar VNet1 ve şirket içi ağlar arasındaki trafik akışını yollarını belirtin. Mavi satırlar VNet2 ve şirket içi ağlar arasındaki trafik akışını yollarını gösterir. Düz çizgiler kararlı durumdaki istediğiniz yolu belirtin ve trafik kararlı bir duruma trafik akışını yürüten karşılık gelen ExpressRoute bağlantı hattı başarısız yolunda kesikli satırları gösterir. 

[![7]][7]

Vnet trafiği şirket içi ağ için yerel eşleme konumu ExpressRoute bağlantı bağlı tercih edilmesi etkilemek için bağlantı ağırlığına kullanarak senaryosunu mimari. Eksiksiz çözümü için simetrik ters trafik akışını sağlamak gerekir. Yerel tercih iBGP oturum (şirket içi tarafında sona erdirilir ExpressRoute bağlantı hatları üzerinde), BGP yönlendiriciler arasında bir ExpressRoute bağlantı hattını tercih için kullanabilirsiniz. Çözüm, aşağıdaki diyagramda gösterilmiştir. 

[![8]][8]

### <a name="scenario-2"></a>Senaryo 2

Senaryo 2, aşağıdaki diyagramda gösterilmiştir. Diyagramda, yeşil satırlar VNet1 ve şirket içi ağlar arasındaki trafik akışını yollarını belirtin. Mavi satırlar VNet2 ve şirket içi ağlar arasındaki trafik akışını yollarını gösterir. Kararlı durum içinde (diyagramdaki düz çizgiler), tüm sanal ağlar ve şirket içi konumlar arasındaki trafik Microsoft omurga çoğunlukla akış ve arasında bağlantısı üzerinden şirket içi konumlara yalnızca (noktalı satırlarında hata durumu Diyagram), bir ExpressRoute.

[![9]][9]

Çözüm, aşağıdaki diyagramda gösterilmiştir. Gösterildiği gibi ya da senaryo mimari daha belirli yönlendirme (1. seçenek) veya AS yolu kullanarak önüne ekleyin (seçenek 2) sanal ağ yolu seçimini etkilemek için. İlişkili Azure trafiği için şirket içi ağ rotası seçimini etkilemek için daha az olarak tercih şirket içi konumunuz arasında bağlantısı yapılandırma. İç bağlantı bağlantıyı tercih olarak yapılandırdığınız Howe şirket ağında kullanılan yönlendirme protokolü bağlıdır. İBGP veya ölçüm IGP (OSPF veya olduğu olduğu) ile birlikte yerel tercihini kullanabilirsiniz.

[![10]][10]


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ExpressRoute bağlantı hattı özel eşleme bağlantısı olağanüstü durum kurtarma için tasarlama konularını ele almıştık. Aşağıdaki makaleler adresleri olağanüstü durum kurtarma uygulamalardan ve ön uç erişimi Perspektifler:

- [Kurumsal ölçekte olağanüstü durum kurtarma][Enterprise DR]
- [Azure Site Recovery ile işletmelerde olağanüstü durum kurtarma][SMB DR]

<!--Image References-->
[1]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/one-region.png "küçük ve orta boyutta şirket içi ağ konuları"
[2]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/specificroute.png "daha belirli yollar kullanılarak yol seçimi etkileme"
[3]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/configure-weight.png "bağlantı ağırlığına Azure portal aracılığıyla yapılandırma"
[4]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/connectionweight.png "bağlantı ağırlığına kullanarak yol seçimi etkileme"
[5]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/aspath.png "yolu olarak kullanarak yol seçimi etkileyen önüne ekleyin"
[6]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region.png "büyük dağıtılmış şirket içi ağ konuları"
[7]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch1.png "Senaryo 1"
[8]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol1.png "etkin-etkin ExpressRoute devrelerini Çözüm 1"
[9]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch2.png "Senaryo 2"
[10]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol2.png "etkin-etkin ExpressRoute devrelerini Çözüm 2"

<!--Link References-->
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[Enterprise DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-enterprise-scale-dr/
[SMB DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-smb-azure-site-recovery/
[con wgt]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-assign-a-high-weight-to-local-connection
[AS Path Pre]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending





