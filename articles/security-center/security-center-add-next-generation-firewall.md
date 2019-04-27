---
title: Azure Güvenlik Merkezi'nde yeni nesil güvenlik duvarı ekleme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerinin uygulanması gösterilmektedir **yeni nesil güvenlik duvarı ekleme** ve **trafiği yalnızca NGFW aracılığıyla yönlendirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 731102037b596091b80fbdfa02a8ff3c111b556e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707083"
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde yeni nesil güvenlik duvarı ekleme
Azure Güvenlik Merkezi, yeni nesil Güvenlik Duvarı (NGFW) eklemeniz, bir Microsoft iş ortağından, güvenlik korumaları artırmak için önerebilir. Bu belgede Bunu yapmak nasıl bir örnek sizi yönlendirir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **yeni nesil güvenlik duvarı ekleme**.
   ![Yeni Nesil Güvenlik Duvarı ekleme][1]
2. İçinde **yeni nesil güvenlik duvarı ekleme** dikey penceresinde, bir uç nokta seçin.
   ![Bir uç nokta seçin][2]
3. İkinci **yeni nesil güvenlik duvarı ekleme** dikey penceresi açılır. Varolan bir çözümü varsa kullanmayı seçebilirsiniz veya yeni bir tane oluşturabilirsiniz. Bu örnekte, kullanılabilir var olan çözüm şekilde bir NGFW oluştururuz.
   ![Yeni nesil güvenlik duvarı oluşturma][3]
4. Bir NGFW oluşturmak için tümleştirilmiş iş ortaklarının listeden bir çözüm seçin. Bu örnekte, seçiyoruz **denetim noktası**.
   ![Yeni nesil güvenlik duvarı çözümü seçin][4]
5. **Denetim noktası** dikey penceresi açılır iş ortağı çözümü hakkında bilgi sağlar. Seçin **Oluştur** bilgi dikey penceresinde.
   ![Güvenlik Duvarı bilgileri dikey penceresi][5]
6. **Sanal makine oluşturma** dikey penceresi açılır. Burada bir sanal NGFW'nun çalışan makinesini (VM) dönmesi için gerekli bilgileri girebilirsiniz. Adımları ve gerekli NGFW bilgileri sağlayın. Uygulamak için Tamam'ı seçin.
   ![NGFW çalıştırmak için sanal makine oluşturma][6]

## <a name="route-traffic-through-ngfw-only"></a>Trafiği yalnızca NGFW aracılığıyla yönlendirme
Geri dönüp **önerileri** dikey penceresi. Adlı Güvenlik Merkezi aracılığıyla bir NGFW eklendikten sonra yeni bir girişin üretildiği **trafiği yalnızca NGFW aracılığıyla yönlendirme**. Bu öneri, yalnızca Güvenlik Merkezi aracılığıyla, NGFW yüklediyseniz oluşturulur. Internet'e yönelik uç noktaları varsa, Güvenlik Merkezi, NGFW aracılığıyla sanal makinenize gelen trafiği zorlamak ağ güvenlik grubu kurallarını yapılandırma önerir.

1. İçinde **öneriler dikey**seçin **trafiği yalnızca NGFW aracılığıyla yönlendirme**.
   ![Trafiği yalnızca NGFW aracılığıyla yönlendirme][7]
2. Bu dikey pencereyi açar **trafiği yalnızca NGFW aracılığıyla yönlendirme**, trafiği yönlendirebilen Vm'leri listeler. Listeden bir VM seçin.
   ![Bir sanal Makineyi seçin][8]
3. Seçili VM için bir dikey pencere açılır ve ilgili gelen kuralları görüntüler. Bir açıklama olası sonraki adımlarla ilgili daha fazla bilgi sağlar. Seçin **gelen kurallarını Düzenle** bir gelen kuralı düzenlemeye devam etmek için. Beklenti olan **kaynak** ayarlı değil **herhangi** NGFW'nun ile bağlantılı Internet'e yönelik uç noktalar için. Gelen kuralı özellikleri hakkında daha fazla bilgi için bkz. [güvenlik kuralları](../virtual-network/security-overview.md#security-rules).
   ![Erişimi sınırlamak için kurallar Yapılandır][9]
   ![gelen kuralı Düzenle][10]

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi önerisini "Yeni nesil güvenlik duvarı Ekle." uygulama nasıl oluşturulacağını gösterir NGFWs ve denetim noktası iş ortağı çözümü hakkında daha fazla bilgi için şunlara bakın:

* [Yeni nesil güvenlik duvarı](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Denetim noktası vSEC](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/checkpoint.vsec)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) --güvenlik ilkelerinin nasıl yapılandırılacağını öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
