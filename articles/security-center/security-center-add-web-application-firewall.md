---
title: Azure Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi önerileri uygulamak gösterilmiştir **bir web uygulaması güvenlik duvarı ekleme** ve **uygulama korumayı Sonlandır**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2018
ms.author: terrylan
ms.openlocfilehash: e28a1f6b865dae3abe2cb9dfac2921c6a2034491
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203534"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme
Azure Güvenlik Merkezi bir Microsoft iş ortağı web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) ekleme önerebilir. Bu belgede bu öneriyi konusunda bir örnek size yol gösterir.

WAF öneri açık gelen web bağlantı noktaları (80,443) içeren bir ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP'si için (örnek düzeyinde IP veya yük dengeli IP) gösterilir.

Güvenlik Merkezi önerir sanal makinelerde ve üzerinde dış App Service ortamları (altında dağıtılan ana), web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlamak [Isolated](https://azure.microsoft.com/pricing/details/app-service/windows/) hizmet planı. Yalıtılmış plan, uygulamalarınızı gizli ve adanmış bir Azure ortamında barındırır. Şirket içi ağınızla güvenli bir bağlantıya ya da daha fazla performans ve ölçeğe gerek duyan uygulamalar için idealdir. Yalıtılmış bir ortamda olan uygulamanızı ek olarak, uygulamanızın yük dengeleyici dış IP adresi olmalıdır. Ana hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı belgeleri](../app-service/environment/intro.md).

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belge hakkında adım adım kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. Altında **önerileri**seçin **güvenli web uygulaması Güvenlik Duvarı'nı kullanarak web uygulamasına**.
   ![Güvenli web uygulaması][1]
2. Altında **web uygulaması Güvenlik Duvarı'nı kullanarak web uygulamalarınızı güvenli**, bir web uygulaması seçin. **Bir Web uygulaması güvenlik duvarı ekleme** açar.
   ![Web uygulaması güvenlik duvarı ekleme][2]
3. Var olan bir web uygulaması güvenlik duvarı varsa kullanmayı seçebilirsiniz veya yeni bir tane oluşturabilirsiniz. Bu örnekte, yok hiçbir varolan WAFs bir WAF oluşturuyoruz şekilde.
4. Bir WAF oluşturmak için bir çözüm tümleşik ortakları listesinden seçin. Bu örnekte, biz seçin **Barracuda Web uygulaması güvenlik duvarı**.
5. **Barracuda Web uygulaması güvenlik duvarı** iş ortağı çözümü hakkında bilgi sağlayan açar. **Oluştur**’u seçin.

   ![Güvenlik Duvarı bilgileri dikey penceresi][3]

6. **Yeni Web uygulaması güvenlik duvarı** açar, burada gerçekleştirebilirsiniz **VM Yapılandırması** sağlamak ve adımlarını **WAF bilgi**. Seçin **VM Yapılandırması**.
7. Altında **VM Yapılandırması**, WAF çalıştıran sanal makineyi dönmesi için gereken bilgileri girin.
   ![VM yapılandırması][4]
8. Geri dönüp **yeni Web uygulaması güvenlik duvarı** seçip **WAF bilgi**. Altında **WAF bilgi**, WAF yapılandırın. Adım 7 yapılandırmanıza olanak sağlayan WAF çalıştırır ve adım 8 sanal makine WAF sağlamanıza imkan sağlar.

## <a name="finalize-application-protection"></a>Uygulama korumasını sonlandırma
1. Geri dönüp **önerileri**. Adlı WAF oluşturduktan sonra yeni bir giriş oluşturulan **uygulama korumayı Sonlandır**. Bu giriş, uygulama koruyabilmeniz için gerçekten Azure sanal ağ içinde WAF yukarı bağlantı kabloları işlemini tamamlamak gereken bilmenizi sağlar.

   ![Uygulama korumasını sonlandırma][5]

2. Seçin **uygulama korumayı Sonlandır**. Yeni bir dikey pencere açılır. Yönlendirdi trafik olması gerekiyorsa bir web uygulaması olduğunu görebilirsiniz.
3. Web uygulaması seçin. Web uygulaması güvenlik duvarı Kurulumu Tamamlanıyor için adımları sağlayan bir dikey pencere açılır. Adımları tamamlayın ve ardından **trafiği kısıtlamak**. Güvenlik Merkezi kablolama yukarı ardından sizin için yapar.

   ![Trafiği kısıtlama][6]

> [!NOTE]
> Varolan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulamasına Güvenlik Merkezi'nde koruyabilirsiniz.
>
>

Bu WAF günlüklerinden artık tam olarak tümleşiktir. Güvenlik Merkezi otomatik olarak toplamak ve böylece bu önemli güvenlik uyarıları için yüzey günlüklerini analiz başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Güvenlik Merkezi öneri "Ekleme bir web uygulaması." uygulamak nasıl oluşturulacağını gösterir Bir web uygulaması Güvenlik Duvarı'nı yapılandırma hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Bir Web uygulaması Güvenlik Duvarı (WAF) için uygulama hizmeti ortamını yapılandırma](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
