---
title: Azure Güvenlik Merkezi'nde Internet'e yönelik uç noktalarla erişimi sınırlama | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **Internet'e yönelik uç noktalarla erişimi sınırlama**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: b736bb5549b7d236e746ba7b161cde79209e927b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60906595"
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Internet'e yönelik uç noktalarla erişimi sınırlama
Azure Güvenlik Merkezi, herhangi bir ağ güvenlik grupları (Nsg) varsa "tümü" kaynak IP adresinden erişime izin veren bir veya daha fazla gelen kuralları Internet'e yönelik uç noktalarla erişimi sınırlama önerir. "Herhangi bir" Access'i açıp saldırganların kaynaklarınıza erişimi sağlayabilir. Güvenlik Merkezi, gerçekten erişmesi gereken kaynak IP adresleri için erişimi kısıtlamak için aşağıdaki gelen kurallarını düzenlemenizi önerir.

Bu öneri "tümü" kaynağı var olmayan web bağlantı noktası oluşturulur.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **öneriler dikey**seçin **Internet'e yönelik uç noktalarla erişimi sınırlama**.

   ![İnternet'e yönelik uç nokta ile erişimi kısıtlama][1]
2. Bu dikey pencereyi açar **Internet'e yönelik uç noktalarla erişimi sınırlama**. Bu dikey pencereyi, olası bir güvenlik sorununa gelen kuralları ile sanal makineleri (VM'ler) listeler. Bir VM'yi seçin.

   ![Bir sanal Makineyi seçin][2]
3. **NSG** ağ güvenlik grubu bilgileri, ilgili gelen kuralları ve ilişkili VM dikey penceresinde görüntülenir. Seçin **gelen kurallarını Düzenle** bir gelen kuralı düzenlemeye devam etmek için.

   ![Ağ güvenlik grubu dikey penceresi][3]
4. Üzerinde **gelen güvenlik kuralları** dikey düzenlemek için bir gelen kuralı seçin. Bu örnekte, seçelim **AllowWeb**.

   ![Gelen güvenlik kuralları][4]

   Unutmayın, ayrıca seçebilirsiniz **varsayılan kuralları** varsayılan kuralları tarafından tüm Nsg'ler yer alan kümesini görmek için. Varsayılan kurallar silinemez ancak daha düşük bir öncelik atanmış oldukları oluşturduğunuz kurallar tarafından geçersiz kılınabilir. Daha fazla bilgi edinin [varsayılan kuralları](../virtual-network/security-overview.md#default-security-rules).

   ![Varsayılan kurallar][5]
5. Üzerinde **AllowWeb** dikey penceresinde, gelen kuralı özelliklerini düzenlemek için **kaynak** bir IP adresi veya IP adresi bloğu. Gelen kuralı özellikleri hakkında daha fazla bilgi için bkz. [NSG kuralları](../virtual-network/security-overview.md#security-rules).

   ![Gelen kuralını Düzenle][6]

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'nin önerisini "Internet'e yönelik uç nokta ile erişimi kısıtlama". uygulama nasıl oluşturulacağını gösterir Nsg'leri ve kuralları'nı etkinleştirme hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Ağ Güvenlik Grubu (NSG) Nedir?](../virtual-network/security-overview.md)
* [Bir ağ güvenlik grubu yönetme](../virtual-network/manage-network-security-group.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
