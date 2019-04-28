---
title: Azure sanal ağı (Önizleme) için IPv6'ya genel bakış
titlesuffix: Azure Virtual Network
description: Azure sanal ağında IPv6 uç noktaları ve veri yolları IPv6 açıklaması.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 0ec650880a45f6383b24b5ac810fc2ee745806b7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62131037"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>Azure sanal ağ için IPv6 nedir? (Önizleme)

IPv6 için Azure sanal ağ (VNET), Azure uygulamalarını barındırmak için bir sanal ağdaki ve için hem Internet'ten IPv6 ve IPv4 bağlantı sağlar. Ortak IPv4 adreslerinin tükenmesi nedeniyle, taşınabilirlik ve nesnelerin interneti (IOT) için yeni ağlar genellikle IPv6 oluşturulur. ISS ve mobil ağ için IPv6 dönüştürülmekte bile uzun kurdu. Yalnızca IPv4 Hizmetleri kendilerini gerçek bir dezavantajı mevcut ve Gelişmekte olan pazarlarda bulabilirsiniz. Azure'da barındırılan hizmetler, hem mevcut IPv4 ve bu yeni IPv6 aygıtları ve ağlar ile kolayca bağlanın küresel olarak kullanılabilir, ikili yığın Hizmetleri ile bu teknoloji boşluğu geçirmek çift yığın IPv4/IPv6 bağlantısı sağlar.

Azure'nın özgün IPv6 bağlantısı, Azure'da barındırılan uygulamalar için ikili yığın (IPv4/IPv6) Internet bağlantısı sağlamak kolaylaştırır. Gelen ve giden kurulan bağlantılar için yük dengeli IPv6 bağlantısı ile sanal makinelerinin basit dağıtım için sağlar. Bu özellik hala kullanılabilir ve daha fazla bilgi edinilebilir [burada](../load-balancer/load-balancer-ipv6-overview.md).
Azure sanal ağ için IPv6, çok daha tam özellikli tam IPv6 çözüm mimarileri, Azure'da dağıtılması etkinleştirme-olur.

> [!Important]
> Azure sanal ağ için IPv6, şu anda genel Önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Aşağıdaki diyagram, azure'da basit ikili yığın (IPv4/IPv6) dağıtımı gösterir:

![IPv6 ağ dağıtım diyagramı](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Avantajlar

Azure sanal ağ IPv6 avantajları:

- Yardımcı olur, büyüyen içine Azure'da barındırılan uygulamalarınız mobil ve nesnelerin interneti pazarlara erişim ağını genişletin.
- İkili yığın IPv4/IPv6 VM'ler, en yüksek hizmet dağıtım esnekliği sunar. Tek hizmet örneği, hem IPv4 hem de IPv6 özellikli Internet istemcileri ile bağlanabilir.
- Uzun kurulan ve kararlı Azure VM Internet üzerinde yapılar IPv6 bağlantısı.
- Açıkça dağıtımınızda talebinde bulunduğu sırada yalnızca IPv6 Internet bağlantısı kurulduktan sonra varsayılan olarak güvenli hale getirin.

## <a name="capabilities"></a>Özellikler

Sanal makineler için IPv6 desteği aşağıdakileri içerir:

- Azure müşterileri, müşterilere uygulamalarını ihtiyaçlarını veya kendi şirket içi IP alanına sorunsuzca tümleştirin, kendi IPv6 sanal ağ adres alanı tanımlayabilirsiniz.
- İkili yığın (IPv4 ve IPv6) sanal ağlar ile çift yığın alt ağlar, uygulamaların hem IPv4 hem de IPv6 kaynaklara kendi sanal ağına veya - Internet'e bağlanmasına olanak sağlar.
- Ağ güvenlik grupları için kaynaklarınızı IPv6 kuralları ile koruma
- IPv6 trafiği sanal ağınızdaki ile kullanıcı tanımlı yollar - özellikle uygulamanızın genişletmek için ağ sanal Gereçleri ne yönlendirilmesini özelleştirin.
- Azure DNS IPv6 Genel IP'ler için AAAA kayıt desteği içeren dayanıklı, ölçeklenebilir uygulamalar oluşturmak için IPv6 yük dengeleyici desteği.
- Yerinde yükseltme mevcut yalnızca IPv4 dağıtımlarında kolayca IPv6 bağlantısı ekleyin.
- Yükleme ile sanal makine ölçek kümeleri için otomatik ölçeklendirme çift yığın uygulamalar oluşturun.

## <a name="limitations"></a>Sınırlamalar
Azure sanal ağ için IPv6 önizleme sürümünü aşağıdaki sınırlamalara sahiptir:
- Azure sanal ağı (Önizleme) için IPv6, kullanılabilir tüm genel Azure bölgelerinde, ancak yalnızca içinde genel Azure - kamu bulutlarında.   
- IPv6 dağıtımı için Azure Powershell ve komut satırı arabirimi (CLI) kullanarak sanal ağ için IPv6 tam destek ve (örnekler ile) belgeler sahip ancak yalnızca kadar ancak tüm IPv6 Yapılandırması için görüntülemek için Önizleme portalı desteği sınırlıdır.
- NSG akış günlükleri için Ağ İzleyicisi Önizleme desteği sınırlıdır ve ağ paketi yakalar.
- Önizleme için Yük Dengeleme desteği için temel yük dengeleyici başlangıçta sınırlı olur.
- Örnek düzeyi genel IP'ler (doğrudan bir VM'de genel IP'ler) Önizleme'de desteklenmez.  
- Sanal Ağ eşlemesi (Bölgesel veya genel) Önizleme'de desteklenmez. 

## <a name="pricing"></a>Fiyatlandırma

IPv6 Azure kaynaklarını ve bant genişliği, IPv4 ile aynı fiyatlar üzerinden ücretlendirilir. IPv6 için ek veya bunlardan farklı ücretlendirme yoktur. Fiyatlandırma hakkında ayrıntılı bilgi bulabilirsiniz [genel IP adresleri](https://azure.microsoft.com/pricing/details/ip-addresses/), [ağ bant genişliği](https://azure.microsoft.com/pricing/details/bandwidth/), veya [yük dengeleyici](https://azure.microsoft.com/pricing/details/load-balancer/).

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure PowerShell kullanarak bir IPv6 ikili yığını uygulaması dağıtma](virtual-network-ipv4-ipv6-dual-stack-powershell.md).
- Bilgi edinmek için nasıl [Azure CLI kullanarak bir IPv6 ikili yığını uygulaması dağıtma](virtual-network-ipv4-ipv6-dual-stack-cli.md).
