---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Sanal WAN otomatik ölçeklenebilir daldan dala bağlantı hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/10/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 54cf6c356ec4bb51b123e48c52c5beebc990e59d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009979"
---
# <a name="what-is-azure-virtual-wan-preview"></a>Azure Sanal WAN nedir? (Önizleme)

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu işlem el ile yapılabildiği gibi Sanal WAN iş ortağı üzerinden tercih edilen sağlayıcı cihazları kullanılarak da yapılabilir. Tercih edilen sağlayıcı cihazlarının kullanılması, size kullanım kolaylığı, basitleştirilmiş bağlantı ve yapılandırma yönetimi sağlar. Azure WAN yerleşik panosu, zaman kazanmanıza yardımcı olabilecek anında sorun giderme içgörüleri sağlar ve geniş ölçekli Siteden Siteye bağlantıyı görüntülemek için kolay bir yol sunar.

> [!IMPORTANT]
> Azure Sanal WAN şu anda yönetilen genel önizleme aşamasındadır. Sanal WAN'yi kullanmak için [Önizlemeye kaydolmanız](#enroll) gerekir.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Azure ve Azure dışı iş yüklerinizin ağ bağlantısına hızlı bir bakış sağlanır. Sanal WAN şu avantajları sunar:

* **Merkezde ve uçta tümleşik bağlantı çözümleri:** Sanal WAN iş ortağı çözümleri de dahil olmak üzere çeşitli kaynaklardan şirket içi sitelerle Azure hub'ı arasında Siteden Siteye yapılandırmasını ve bağlantısını otomatik hale getirin.
* **Otomatik uç kurulumu ve yapılandırması:** Sanal ağlarınızı ve iş yüklerinizi Azure hub'ına rahatça bağlayın.
* **Sezgisel sorun giderme:** Azure'un içinde uçta uca akışı görebilir ve bu bilgiyi kullanarak gerekli eylemleri gerçekleştirebilirsiniz.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

## <a name="vendor"></a>Sanal WAN iş ortağıyla çalışma

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, Azure Hizmet Sorumlusu kullanarak Site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’ye bir Siteden Siteye bağlantısı (iki etkin tünel) kurulur. IKEv2’yi desteklemek için Azure’a dal (VPN/SDWAN) denetleyicisi gerekir. BGP isteğe bağlıdır.

## <a name="resources"></a>Sanal WAN kaynakları

Uçtan uca sanal WAN'yi yapılandırmak için şu kaynakları oluşturursunuz:

* **virtualWAN:** virtualWAN kaynağı Azure ağınızdaki sanal bir katmanı temsil eder ve birden fazla kaynağı içeren bir koleksiyondur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Sanal WAN’daki Sanal Hub'lar birbiriyle iletişim kurmaz.

* **Site:** vpnsite olarak bilinen site kaynağı şirket içi VPN cihazını ve bu cihazın ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

* **Hub:** Sanal hub Microsoft tarafından yönetilen bir sanal ağdır. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir. Azure portalı kullanarak bir hub oluşturduğunuzda, otomatik olarak bir sanal hub VNet ve vpngateway sanal hub'ı oluşturulur.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, Sanal WAN kullanırken şirket içi sitenizden doğrudan Sanal Ağınıza bir Siteden Siteye bağlantı oluşturmazsınız. Bunun yerine, hub'a Siteden Siteye bağlantısı oluşturursunuz. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır. 

* **Hub sanal ağ bağlantısı:** Hub'ı sorunsuzca sanal ağınıza bağlamak için bir hub sanal ağ bağlantı kaynağı kullanılır. Şu anda, yalnızca aynı hub bölgesi içinde yer alan sanal ağlara bağlanabilirsiniz.

##<a name="enroll"></a>Önizlemeye kaydolma

Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmeniz gerekir. Aksi halde portalda Sanal WAN ile çalışamazsınız. Kaydetmek için, <azurevirtualwan@microsoft.com> adresine abonelik kimliğinizi içeren bir e-posta gönderin. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız.

## <a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="feedback"></a>Önizleme geri bildirimi

Geri bildirimleriniz bizim için önemlidir. Sanal WAN ile ilgili sorunları bildirmek veya geri bildirim (olumlu veya olumsuz) sağlamak için lütfen <azurevirtualwan@microsoft.com> adresine e-posta gönderin. Şirketinizin adını konu satırına “[ ]” içinde yazın. Sorun bildiriyorsanız abonelik kimliğinizi de eklemeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN kullanarak Siteden Siteye bağlantı oluşturmak için, [Sanal WAN iş ortağı](https://aka.ms/virtualwan) üzerinden ilerleyebilir veya bağlantıyı el ile oluşturabilirsiniz. Bağlantıyı el ile oluşturmak için bkz. [Sanal WAN kullanarak Siteden Siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md).