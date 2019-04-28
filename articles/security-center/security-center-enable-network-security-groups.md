---
title: Azure Güvenlik Merkezi'nde ağ güvenlik gruplarını etkinleştirme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **ağ güvenlik gruplarını etkinleştirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 14b7cc8f8162574380ca21ac515af8b7d3d5ded9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60911471"
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde ağ güvenlik gruplarını etkinleştirme
Azure Güvenlik Merkezi bir zaten etkin değilse, bir ağ güvenlik grubu (NSG) etkinleştirmenizi önerir. Nsg'ler izin veren veya bir sanal ağ, sanal makine örneklerine trafiği reddeden erişim denetimi listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, tekil bir VM trafik kısıtlanabilir NSG'yi doğrudan bu VM ile ilişkilendirme tarafından daha fazla. Daha fazla bilgi edinmek için [bir ağ güvenlik grubu (NSG) nedir?](../virtual-network/security-overview.md)

Nsg'ler etkin değilse, Güvenlik Merkezi, iki önerileri sunar: Ağ güvenlik grupları, sanal makinelerde alt ağlar ve ağ güvenlik gruplarını etkinleştirme etkinleştirin. Hangi düzeyi, alt ağ veya Nsg uygulamak için VM seçin.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **ağ güvenlik gruplarını etkinleştirme** alt ağlar veya sanal makinelerde.
   ![Ağ Güvenlik Gruplarını Etkinleştirme][1]
2. Bu dikey pencereyi açar **eksik ağ güvenlik gruplarını yapılandırma** alt ağlar için veya seçtiğiniz öneri bağlı olarak sanal makineler için. Bir alt ağ veya NSG yapılandırmak için bir sanal makine seçin.

   ![NSG için alt ağ yapılandırın][2]

   ![NSG, VM için yapılandırma][3]
3. Üzerinde **ağ güvenlik grubunu seçme** dikey penceresinde var olan bir NSG seçin ya da seçin **Yeni Oluştur** bir NSG oluşturmak için.

   ![Ağ güvenlik grubu seç][4]

Bir NSG oluşturursanız, adımları [bir ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md) NSG oluşturun ve güvenlik kuralları ayarlayın.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede "Ağ güvenlik gruplarını etkinleştir" alt ağlar ve sanal makineler için Güvenlik Merkezi önerinin nasıl uygulanacağını gösterdi. Nsg'ler etkinleştirme hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Ağ Güvenlik Grubu (NSG) Nedir?](../virtual-network/security-overview.md)
* [Bir ağ güvenlik grubu yönetme](../virtual-network/manage-network-security-group.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
