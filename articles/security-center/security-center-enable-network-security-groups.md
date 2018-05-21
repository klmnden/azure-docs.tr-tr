---
title: Ağ güvenlik grupları Azure Güvenlik Merkezi'nde etkinleştirme | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir **etkinleştirmek ağ güvenlik grupları**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 3c0ad4a0e1a5f4f2fd6def4f29599e2e55eb1a9d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Ağ güvenlik grupları Azure Güvenlik Merkezi'nde etkinleştir
Azure Güvenlik Merkezi bir zaten etkin değilse, bir ağ güvenlik grubu (NSG) etkinleştirmenizi önerir. Nsg'ler izin veren veya bir sanal ağ üzerindeki VM örneklerinize ağ trafiğinin reddeden erişim denetimi listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, tekil bir VM trafik kısıtlanabilir başka bir NSG doğrudan bu VM ilişkilendirerek. Daha fazla bilgi edinmek için [bir ağ güvenlik grubu (NSG) nedir?](../virtual-network/security-overview.md)

Nsg'ler etkin yoksa Güvenlik Merkezi iki önerileri size gösterir: alt ağları ve ağ güvenlik grupları sanal makinelerde etkinleştir üzerinde ağ güvenlik grupları etkinleştirin. Hangi düzeyinde, alt ağ veya Nsg'ler uygulamak için VM seçin.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **etkinleştirmek ağ güvenlik grupları** alt ağlarda veya sanal makinelerde.
   ![Ağ Güvenlik Gruplarını Etkinleştirme][1]
2. Bu dikey pencere açılır **eksik ağ güvenlik gruplarını yapılandırma** alt ağlar için veya seçtiğiniz öneri bağlı olarak sanal makineler için. Bir alt ağ ya da bir NSG yapılandırmak için bir sanal makineyi seçin.

   ![Alt ağı için NSG yapılandırma][2]

   ![VM için NSG yapılandırma][3]
3. Üzerinde **ağ güvenlik grubunu seçme** dikey penceresinde, varolan bir NSG veya seçin **Yeni Oluştur** bir NSG oluşturmak için.

   ![Ağ güvenlik grubu seç][4]

Bir NSG'yi oluşturursanız, adımları [Azure portalını kullanarak Nsg'ler yönetme](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) bir NSG oluşturmak ve güvenlik kuralları ayarlamak için.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalenin alt ağlar ya da sanal makineleri için Güvenlik Merkezi öneri "Ağ güvenlik grupları'ı etkinleştir" uygulamak nasıl oluşturulacağını gösterir. Nsg'ler etkinleştirme hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Ağ Güvenlik Grubu (NSG) Nedir?](../virtual-network/security-overview.md)
* [Bir ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
