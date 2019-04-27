---
title: VPN ağ geçidi Klasikten Resource Manager'a geçiş | Microsoft Docs
description: Bu sayfa, VPN ağ geçidi Klasik Resource Manager'a geçiş için bir genel bakış sağlar.
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: b65b47389611bcc0e5acb3c7ebff672f72a87581
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761599"
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a>Klasik VPN Gateway'inizi Resource Manager'a geçiş
VPN ağ geçitleri artık Klasik moddan Resource Manager dağıtım modeline geçirilebilir. Daha fazla Azure Resource Manager hakkındaki [özelliklerine ve avantajlarına](../azure-resource-manager/resource-group-overview.md). Bu makalede, biz Klasik dağıtımlardan yeni Resource Manager tabanlı modele geçiş ilişkin ayrıntılı bilgi vermektedir. 

VPN ağ geçitleri, sanal ağ geçiş Klasik modelden Kaynak Yöneticisi'nin bir parçası olarak geçirilir. Bu geçiş bir kerede bir sanal ağın gerçekleştirilir. Araçlar veya geçiş için Önkoşullar açısından ek gereksinimi yoktur. Geçiş adımları, var olan sanal ağı geçiş için aynıdır ve adresinde belgelenen [Iaas kaynakları geçiş sayfası](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Geçiş sırasında hiçbir veri yolu kapalı kalma süresi yoktur ve bu nedenle mevcut iş yüklerinin geçiş sırasında şirket içi bağlantı kaybı yaşanmadan çalışmaya devam eder. VPN ağ geçidi ile ilişkili genel IP adresini, geçiş işlemi sırasında değiştirmez. Bu, geçiş tamamlandıktan sonra şirket içi yönlendiricinizi yeniden yapılandırmanız gerekmez anlamına gelir.  

Kaynak Yöneticisi'nde modeli Klasik modelden farklıdır ve sanal ağ geçitleri, yerel ağ geçitleri ve bağlantı kaynaklarını oluşur. Bunlar, VPN ağ geçidinin kendisi, şirket içi adres alanı ve ikisi arasındaki bağlantı sırasıyla temsil eden yerel site temsil eder. Geçiş tamamlandıktan sonra ağ geçitlerinizi Klasik modelde kullanılamaz ve Resource Manager modelini kullanarak sanal ağ geçitleri, yerel ağ geçitleri ve bağlantı nesneler üzerinde tüm yönetim işlemlerini gerçekleştirilmesi gerekir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
VPN bağlantısı için en sık karşılaşılan senaryolardan Klasikten Resource Manager'a geçiş tarafından ele alınmaktadır. Desteklenen senaryolar şunlardır-

* Site bağlantı noktası
* Siteden siteye bağlantı VPN ağ geçidi ile şirket içi konuma bağlı
* VPN gateways kullanan iki sanal ağ arasında VNet bağlantı
* Birden çok sanal ağlar aynı şirket içi konuma bağlı
* Çok siteli bağlantı
* Zorlamalı tünel etkin sanal ağlar

Desteklenmeyen senaryolar şunlardır-  

* Sanal ağ ile ExpressRoute ağ geçidi hem de VPN ağ geçidi şu anda desteklenmiyor.
* VM uzantıları burada şirket içi sunucular için bağlı olan geçiş senaryoları. Geçiş VPN bağlantı sınırlamaları aşağıda ayrıntılı şekilde verilmiştir.

> [!NOTE]
> Resource Manager modelinde CIDR doğrulama daha katı bir Klasik model. Geçiş yapmadan önce geçiş işlemine başlamadan önce verilen Klasik adres aralıkları için geçerli bir CIDR biçiminde uyumlu olmasını sağlamak. Tüm ortak CIDR doğrulayıcıları kullanarak CIDR doğrulanabilir. VNet veya geçirilen geçersiz CIDR aralıkları yerel siteleriyle başarısız durumda neden olur.
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a>VNet-VNet bağlantısı geçişi
Sanal ağa bağlantı, Klasik VNet, bağlı sanal ağ yerel sitesi temsilini oluşturarak ulaşılmıştı. Müşteriler, iki sanal ağ birbirine bağlanması için gereken temsil iki yerel site oluşturma geçme zorunluluğundaydı. Bu, daha sonra iki sanal ağ arasında bağlantı kurmak için IPSec tüneli kullanarak karşılık gelen sanal ağlar için bağlanmış. Bir sanal ağın adres aralığı değişiklikler, karşılık gelen yerel site gösteriminde korunmalıdır olduğundan bu modeli yönetilebilirlik zorlukları vardır. Resource Manager modelinde bu geçici çözüm artık gerekli değildir. İki sanal ağ arasındaki bağlantıyı doğrudan bağlantı kaynağında 'Vnet2Vnet' bağlantı türü kullanılarak gerçekleştirilebilir. 

![Ekran görüntüsü, VNet-VNet geçişi.](./media/vpn-gateway-migration/migration1.png)

Sanal ağ geçiş sırasında geçerli sanal ağın VPN gateway'e bağlı varlık başka bir vnet ve her iki vnet'in geçiş tamamlandıktan sonra başka bir sanal ağ ' ı temsil eden iki yerel siteler artık görür olun algılayın. Klasik modeli iki VPN ağ geçitleri, iki yerel site ve aralarında iki bağlantı iki VPN ağ geçidi ve iki tür Vnet2Vnet bağlantısı ile Resource Manager modeline dönüştürülür.

## <a name="transit-vpn-connectivity"></a>Geçiş VPN bağlantısı
Bir sanal ağın şirket içi bağlantıyı doğrudan şirket içine bağlı başka bir sanal ağa bağlanarak elde edilir, VPN ağ geçitleri bir topolojide yapılandırabilirsiniz. İlk VNet durumlarda şirket içi kaynaklara doğrudan şirket içine bağlı bağlı VNet VPN gateway'e geçiş aracılığıyla burada bağlı geçiş VPN bağlantısı budur. Bu yapılandırmayı Klasik dağıtım modelinde elde etmek için hem bağlı VNet temsil eden ön ekleri toplanmış bir yerel site oluşturmak için ihtiyacınız ve şirket içi adres alanı. Bu temsili yerel site ardından aktarım bağlantısı elde etmek için sanal ağa bağlı. Şirket içi adres aralığındaki herhangi bir değişiklik, VNet ve şirket içi bir toplamasını temsil eden yerel sitede bulundurulması olduğundan bu Klasik modeli de benzer yönetilebilirlik zorlukları vardır. Bağlı ağ geçidi yollarını şirket içi öneklerini el ile değişiklik yapılmaksızın edinebilirsiniz beri Resource Manager'ın desteklenen ağ geçidi BGP desteği sunulmasıyla yönetilebilirlik basitleştirir.

![Transit yönlendirme senaryosu ekran görüntüsü.](./media/vpn-gateway-migration/migration2.png)

Biz VNet yerel siteleri gerektirmeden sanal ağa bağlantı dönüştürme olduğundan, geçiş senaryosu dolaylı olarak şirket içine bağlı VNet için şirket içi bağlantıyı kaybeder. Geçiş tamamlandıktan sonra bağlantı kaybı aşağıdaki iki yolla azaltılabilir- 

* Bağlı olduğu birlikte ve şirket içi VPN ağ geçitleri üzerinde BGP etkinleştirin. BGP etkinleştiriliyor yollar hakkında bilgi edindiniz ve tanıtılan sanal ağ geçitleri arasında bağlantı herhangi bir yapılandırma değişikliği olmadan geri yükler. BGP seçeneği yalnızca standart ve daha yüksek SKU'larında kullanılabilir olduğunu unutmayın.
* Bir açık etkilenen vnet'ten şirket içi konumu temsil eden yerel ağ geçidi bağlantısı. Bu da oluşturun ve IPSec tüneli yapılandırmak için şirket içi yönlendiricisindeki yapılandırmasının değiştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
VPN ağ geçidi geçişi desteği hakkında daha fazla edindikten sonra Git [Iaas kaynaklarının Klasik'ten Resource Manager'a platform destekli geçişe](../virtual-machines/windows/migration-classic-resource-manager-ps.md) kullanmaya başlamak için.

