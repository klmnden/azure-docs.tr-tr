---
title: Microsoft Azure, Oracle bulut altyapısıyla tümleştirme | Microsoft Docs
description: Veritabanları Oracle bulut altyapısı (OCI) ile Microsoft Azure üzerinde çalışan Oracle uygulamalarını tümleştirme çözümleri hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: ''
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: 5a60e41d3195c0f7d88fd3ba14336d693d2f528e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446674"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Oracle uygulama çözümlerini Microsoft Azure'da ve Oracle bulut altyapısı (Önizleme) ile tümleştirme

Microsoft ve Oracle düşük gecikme süreli, yüksek aktarım hızı Bulutlar arası bağlantı sağlamak için her iki bulut en iyi şekilde yararlanmak sağlayarak iş ortaklığı kurdu. 

Bu Bulutlar arası bağlantı kullanarak, veritabanı katmanı Oracle bulut altyapısı (OCI) üzerinde çalıştırmak için çok katmanlı bir uygulama ve uygulama ve diğer Katmanlar Microsoft Azure üzerinde bölümleyebilirsiniz. Tüm çözüm yığın tek bir bulutta çalıştırmaya benzer deneyimidir. 

> [!IMPORTANT]
> Bu Bulutlar arası şu anda önizlemededir ve bazı özelliktir [sınırlamalar uygulanır](#preview-limitations). Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

Tamamen Azure altyapı üzerinde Oracle çözümleri dağıtırken ilgileniyorsanız bkz [Oracle VM görüntüsü ve dağıtım Microsoft Azure üzerinde](oracle-vm-solutions.md).

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bulutlar arası bağlantı, Azure sanal makinelerinde barındırılan veritabanı hizmetlerinde OCI avantajların tadını sırasında Oracle'nın sektör lideri uygulamaları ve kendi özel uygulamalarınızı çalıştırmanız için bir çözüm sağlar. 

Bulutlar arası yapılandırmasında çalıştırabileceğiniz uygulamalar şunlardır:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Oracle perakende uygulamaları
* Oracle Hyperion Finans Yönetimi

Aşağıdaki diyagramda bağlı çözüme üst düzey bir genel bakış ' dir. Kolaylık olması için yalnızca uygulama katmanı ve veri katmanı diyagramda gösterilmektedir. Uygulama Mimarisi bağlı olarak, Azure'da bir web katmanı gibi ek katmanları çözümünüzü dahil olabilir. Daha fazla bilgi için aşağıdaki bölümlere bakın.

![Azure OCI çözümüne genel bakış](media/oracle-oci-overview/crosscloud.png)

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Bulutlar arası bağlantı önizlemesinde, Azure Doğu ABD (myresourcegroup) bölgesi ve OCI Ashburn (ABD-ashburn-1) bölge ile sınırlıdır.

## <a name="networking"></a>Ağ

Kurumsal müşteriler, genellikle farklılaştır ve çeşitli iş ve işletimsel nedeniyle birden çok bulut üzerinden iş yükleri dağıtmak seçin. Farklılaştır için müşterilerin internet, IPSec VPN veya şirket içi ağınız üzerinden bulut sağlayıcısının doğrudan bir bağlantı çözümüdür kullanarak bulut ağlarındaki bağlantı. Bulut ağlarındaki interconnecting önemli bir yatırım zamanı, para, tasarım, tedarik, yükleme, test ve işlemleri gerektirebilir. 

Bu müşteri sorunları gidermek üzere, Oracle ve Microsoft tümleşik bir çoklu bulut deneyimi etkinleştirdiniz. Bulutlar arası ağlar arasında bağlantı kurarak oluşturulduğunda bir [ExpressRoute](../../../expressroute/expressroute-introduction.md) devre ile Microsoft azure'da bir [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) bağlantı hattı OCI içinde. Bu bağlantı, bir Azure ExpressRoute eşleme konumuna yakınlığını veya aynı konumda eşleme OCI FastConnect olduğu mümkündür. Bu kurulum, bir ara hizmet sağlayıcısı gerek kalmadan iki bulut arasındaki güvenli ve hızlı bağlantı sağlar.

Özel IP adresi alanını çakışmaması şartıyla FastConnect ExpressRoute kullanarak, azure'da bir sanal ağ OCI, bir bulut sanal ağ ile müşteriler eşleyebilirsiniz. İki ağ eşlemesi, sanal ağdaki her ikisi de aynı ağda gibi bir kaynak OCI bulut sanal ağ arasında iletişim kurmak için bir kaynak sağlar.

## <a name="network-security"></a>Ağ güvenliği

Ağ güvenliği herhangi bir kurumsal uygulamanın önemli bir bileşendir ve bu çok bulut çözümü için önemlidir. ExpressRoute ve FastConnect üzerinden giden tüm trafiği, özel bir ağ üzerinden geçirir. Bu yapılandırma, bir Azure sanal ağı ve bir Oracle bulut sanal ağ arasında güvenli iletişim için sağlar. Azure'daki tüm sanal makineler için genel bir IP adresini belirtmeniz gerekmez. Benzer şekilde, bir internet ağ geçidi OCI gerekmez. Tüm iletişimi makineler özel IP adresi gerçekleşir.

Ayrıca, ayarlayabilirsiniz [güvenlik listelerini](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) OCI bulut sanal ağ ve güvenlik kuralları (Azure'a bağlı [ağ güvenlik grupları](../../../virtual-network/security-overview.md)). Bu kurallar sanal ağlarda bulunan makineler arasında akan trafiği denetlemek için kullanın. Ağ güvenlik kuralları, bir makine düzeyinde, bir alt ağ düzeyinde yanı sıra sanal ağ düzeyinde eklenebilir.
 
## <a name="identity"></a>Kimlik

Kimlik, Oracle ile Microsoft arasındaki iş ortaklığı temel yapı taşları biridir. Önemli iş tümleştirmek için yapıldığını [Oracle kimlik bulut hizmeti](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) (IDCS) ile [Azure Active Directory](../../../active-directory/index.yml) (Azure AD). Azure AD Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Bu, kullanıcılarınızın oturum açın ve çeşitli kaynaklara yardımcı olur. Azure AD, kullanıcıları ve izinlerini yönetmenize olanak sağlar.

Şu anda, bu tümleştirme, Azure Active Directory olan bir merkezi konumda yönetmenize olanak sağlar. Azure AD dizinindeki herhangi bir değişiklik karşılık gelen bir Oracle dizin ile senkrondur ve çoklu oturum açma için Bulutlar arası Oracle çözümleri için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama bir [Bulutlar arası ağ](configure-azure-oci-networking.md) OCI ile Azure arasındaki. 

Daha fazla bilgi ve teknik incelemeler OCI hakkında bkz [Oracle bulut](https://docs.cloud.oracle.com/iaas/Content/home.htm) belgeleri.
