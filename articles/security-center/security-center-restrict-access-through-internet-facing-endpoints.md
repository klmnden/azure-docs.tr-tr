---
title: Azure Güvenlik Merkezi'nde Internet'e yönelik uç noktalar aracılığıyla erişimi kısıtlama | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir **Internet'e yönelik uç noktası aracılığıyla erişimi kısıtlamak**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 92906d31f4db21f37094f192dadd080e28cc6e8e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34363062"
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Internet'e yönelik uç noktalar aracılığıyla erişimi kısıtlama
Azure Güvenlik Merkezi, herhangi bir ağ güvenlik grupları (Nsg'ler) varsa, "tüm" kaynak IP adresleri erişime izin verecek bir veya daha fazla gelen kuralları Internet'e yönelik uç noktalar aracılığıyla erişimi kısıtlamak önerir. "Herhangi bir" erişim açma kaynaklarınıza erişmek saldırganlar sağlayabilir. Güvenlik Merkezi, aslında erişmesi gereken kaynak IP adresleri için erişimi kısıtlamak için bu gelen kuralları Düzenle önerir.

Bu öneri, "" kaynağı olarak içeren herhangi bir web dışı bağlantı için oluşturulur.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **öneriler dikey**seçin **Internet'e yönelik uç noktası aracılığıyla erişimi kısıtlamak**.

   ![İnternet'e yönelik uç nokta ile erişimi kısıtlama][1]
2. Bu dikey pencere açılır **Internet'e yönelik uç noktası aracılığıyla erişimi kısıtlamak**. Bu dikey potansiyel bir güvenlik sorunu oluşturmak gelen kuralları ile sanal makineleri (VM'ler) listeler. Bir VM'yi seçin.

   ![Bir VM seçin][2]
3. **NSG** ağ güvenlik grubu bilgileri, ilgili gelen kuralları ve ilişkili VM dikey penceresinde görüntülenir. Seçin **gelen kuralları Düzenle** bir gelen kuralı düzenleme ile devam etmek için.

   ![Ağ güvenlik grubu dikey penceresi][3]
4. Üzerinde **gelen güvenlik kuralları** dikey penceresinde düzenlemek için gelen kuralı seçin. Bu örnekte, şimdi seçin **AllowWeb**.

   ![Gelen güvenlik kuralları][4]

   Not, ayrıca seçebilirsiniz **varsayılan kuralları** tüm Nsg'ler tarafından bulunan varsayılan kurallar kümesini görmek için. Varsayılan kurallar silinemez ancak daha düşük bir öncelik atandığı için oluşturduğunuz kurallar tarafından kılınabilir. Daha fazla bilgi edinmek [varsayılan kuralları](../virtual-network/security-overview.md#default-security-rules).

   ![Varsayılan kurallar][5]
5. Üzerinde **AllowWeb** dikey penceresinde, gelen kuralı özelliklerini düzenlemek için **kaynak** bir IP adresi veya IP adresleri bloğu. Gelen kuralı özellikleri hakkında daha fazla bilgi için bkz: [NSG kuralları](../virtual-network/security-overview.md#security-rules).

   ![Gelen kuralını Düzenle][6]

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede "Internet'e yönelik uç nokta aracılığıyla erişimi kısıtlama". Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir Nsg'ler ve kurallarını etkinleştirme hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Ağ Güvenlik Grubu (NSG) Nedir?](../virtual-network/security-overview.md)
* [Bir ağ güvenlik grubunu yönetme](../virtual-network/manage-network-security-group.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
