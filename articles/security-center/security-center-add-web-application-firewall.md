---
title: Azure Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerinin uygulanması gösterilmektedir **bir web uygulaması güvenlik duvarı ekleme** ve **uygulama korumasını sonlandırma**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: 63852ccab842f11f30bcbe695206fedf72931911
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60706293"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme
Azure Güvenlik Merkezi bir web uygulaması Güvenlik Duvarı (WAF) eklemek, bir Microsoft iş ortağından, web uygulamalarınızın güvenliğini sağlamak için önerebilir. Bu belge, bu öneriyi uygulamak nasıl bir örnek üzerinden açıklanmaktadır.

Bir WAF öneri, bir giden açık web bağlantı noktaları (80,443) ile ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP için (örnek düzeyi IP veya yük dengeli IP) gösterilir.

Güvenlik Merkezi, sanal makinelerde ve üzerinde dış App Service ortamları (altında dağıtılan ASE) web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlama önerir [yalıtılmış](https://azure.microsoft.com/pricing/details/app-service/windows/) hizmet planı. Yalıtılmış plan, uygulamalarınızı gizli ve adanmış bir Azure ortamında barındırır. Şirket içi ağınızla güvenli bir bağlantıya ya da daha fazla performans ve ölçeğe gerek duyan uygulamalar için idealdir. Uygulamanıza ek olarak yalıtılmış bir ortamda olan uygulamanızın yük dengeleyici dış IP adresi olmalıdır. ASE hakkında daha fazla bilgi için bkz: [App Service ortamı belgeleri](../app-service/environment/intro.md).

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belgede, adım adım bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. Altında **önerileri**seçin **güvenli web uygulaması Güvenlik Duvarı'nı kullanarak web uygulaması**.
   ![Web uygulamasını güvenli hale getirme][1]
2. Altında **web uygulaması Güvenlik Duvarı'nı kullanarak web uygulamalarınızı güvenli**, bir web uygulamasını seçin. **Bir Web uygulaması güvenlik duvarı ekleme** açılır.
   ![Web uygulaması güvenlik duvarı ekleme][2]
3. Varsa mevcut bir web uygulaması güvenlik duvarı kullanmayı seçebilirsiniz veya yeni bir tane oluşturabilirsiniz. Bu örnekte, kullanılabilir mevcut hiçbir Waf'ler şekilde bir WAF oluştururuz.
4. Bir WAF oluşturmak için tümleştirilmiş iş ortaklarının listeden bir çözüm seçin. Bu örnekte, seçiyoruz **Barracuda Web uygulaması güvenlik duvarı**.
5. **Barracuda Web uygulaması güvenlik duvarı** iş ortağı çözümü hakkında bilgi sağlayan açılır. **Oluştur**’u seçin.

   ![Güvenlik Duvarı bilgileri dikey penceresi][3]

6. **Yeni Web uygulaması güvenlik duvarı** yapabileceğiniz açıldığında **VM Yapılandırması** sağlayın ve adımlarını **WAF bilgileri**. Seçin **VM Yapılandırması**.
7. Altında **VM Yapılandırması**, WAF çalışan sanal makineyi çalıştırmak için gereken bilgileri girin.

   ![VM yapılandırması][4]
   
8. Geri dönüp **yeni Web uygulaması güvenlik duvarı** seçip **WAF bilgileri**. Altında **WAF bilgileri**, WAF yapılandırın. 7. adım yapılandırmanıza olanak tanır, WAF sağlama WAF çalıştırır ve 8. adım bir sanal makine sağlar.

## <a name="finalize-application-protection"></a>Uygulama korumasını sonlandırma
1. Geri dönüp **önerileri**. Adlı bir WAF oluşturduktan sonra yeni bir girişin üretildiği **uygulama korumasını sonlandırma**. Bu giriş, uygulama koruyabilmesi WAF Azure sanal ağ içinde yukarı gerçekten bağlama işlemini tamamlamak ihtiyacınız bilmenizi sağlar.

   ![Uygulama korumasını sonlandırma][5]

2. Seçin **uygulama korumasını sonlandırma**. Yeni bir dikey pencere açılır. Yönlendirdi akışı olmasını gerektiren bir web uygulaması olduğunu görebilirsiniz.
3. Web uygulaması'nı seçin. Web uygulaması güvenlik duvarı kurulumunu sonlandırılıyor için adımları size bir dikey pencere açılır. Adımları tamamlayın ve ardından **trafiği kısıtlamak**. Güvenlik Merkezi kablolama yukarı sonra sizin için halleder.

   ![Trafiği kısıtlama][6]

> [!NOTE]
> Var olan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulaması Güvenlik Merkezi'nde koruyabilirsiniz.
>
>

Bu WAF günlükleri artık tamamen tümleştirilmiştir. Güvenlik Merkezi otomatik olarak toplama ve bu önemli güvenlik uyarıları, görmenizi sağlayabilir, böylece günlüklerini analiz başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Güvenlik Merkezi önerisini "Add bir web uygulaması." uygulama nasıl oluşturulacağını gösterir Bir web uygulaması güvenlik duvarı yapılandırma hakkında daha fazla bilgi için şunlara bakın:

* [App Service Ortamı için Web Uygulaması Güvenlik Duvarı (WAF) Yapılandırma](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
