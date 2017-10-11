---
title: "Azure Güvenlik Merkezi'nde yeni nesil güvenlik duvarı ekleme | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi önerileri uygulamak gösterilmiştir ** bir sonraki nesil güvenlik duvarı ** ekleyin ve ** yalnızca rota traffice NGFW aracılığıyla **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 30589d0a943517c03394a3aae7c03c8094e78c1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde yeni nesil güvenlik duvarı ekleme
Azure Güvenlik Merkezi bir Microsoft iş ortağı güvenlik korumaları artırmak için yeni nesil Güvenlik Duvarı (NGFW) eklediğiniz önerebilir. Bu belge, bunun nasıl yapılacağı örneği aracılığıyla açıklanmaktadır.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **yeni nesil güvenlik duvarı ekleme**.
   ![Yeni Nesil Güvenlik Duvarı ekleme][1]
2. İçinde **yeni nesil güvenlik duvarı ekleme** dikey penceresinde, bir uç nokta seçin.
   ![Bir uç nokta seçin][2]
3. İkinci bir **yeni nesil güvenlik duvarı ekleme** dikey pencere açılır. Varsa varolan bir çözümü kullanmayı seçebilirsiniz veya yeni bir tane oluşturabilirsiniz. Bu örnekte, yok varolan çözüm bir NGFW oluşturuyoruz şekilde.
   ![Yeni nesil güvenlik duvarı oluşturma][3]
4. Bir NGFW oluşturmak için bir çözüm tümleşik ortakları listesinden seçin. Bu örnekte, biz seçin **Check Point**.
   ![Yeni nesil güvenlik duvarı çözümü seçin][4]
5. **Check Point** dikey pencere açılır iş ortağı çözümü hakkında bilgi sağlar. Seçin **oluşturma** bilgi dikey penceresinde.
   ![Güvenlik Duvarı bilgileri dikey penceresi][5]
6. **Sanal makine oluşturma** dikey pencere açılır. Buraya bir NGFW'nun çalıştıran sanal makineyi (VM) dönmesi için gerekli bilgileri girebilirsiniz. Aşağıdaki adımları uygulayın ve gerekli NGFW bilgileri sağlayın. Uygulamak için Tamam'ı seçin.
   ![NGFW çalıştırmak için sanal makine oluşturma][6]

## <a name="route-traffic-through-ngfw-only"></a>Trafiği yalnızca NGFW aracılığıyla yönlendir
Geri dönüp **önerileri** dikey. Güvenlik Merkezi, adlı aracılığıyla bir NGFW eklendikten sonra yeni bir giriş oluşturulan **rota yalnızca trafiği NGFW aracılığıyla**. Bu öneri, yalnızca Güvenlik Merkezi aracılığıyla, NGFW yüklediyseniz oluşturulur. Internet'e yönelik uç noktaları varsa, Güvenlik Merkezi, NGFW aracılığıyla, VM'ye gelen trafiği zorla ağ güvenlik grubu kurallarını yapılandırmak önerir.

1. İçinde **öneriler dikey**seçin **rota yalnızca trafiği NGFW aracılığıyla**.
   ![Trafiği yalnızca NGFW aracılığıyla yönlendirme][7]
2. Bu dikey pencere açılır **rota yalnızca trafiği NGFW aracılığıyla**, trafiği yönlendirebilir sanal makineleri listeler. VM listeden seçin.
   ![Bir VM seçin][8]
3. İlgili gelen kuralları görüntüleme seçili VM için bir dikey pencere açılır. Bir açıklama olası sonraki adımlar hakkında daha fazla bilgi sağlar. Seçin **gelen kuralları Düzenle** bir gelen kuralı düzenleme ile devam etmek için. Beklentisi **kaynak** ayarlanmazsa **herhangi** NGFW'nun ile bağlantılı Internet'e yönelik uç noktalar için. Gelen kuralı özellikleri hakkında daha fazla bilgi için bkz: [NSG kuralları](../virtual-network/virtual-networks-nsg.md#nsg-rules).
   ![Erişimi sınırlamak için kurallar Yapılandır][9]
   ![düzenleme gelen kuralı][10]

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi öneri "Ekleme yeni nesil güvenlik duvarı." uygulamak nasıl oluşturulacağını gösterir NGFWs ve denetim noktası iş ortağı çözümü hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Yeni nesil güvenlik duvarı](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Denetim noktası vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) --güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

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
