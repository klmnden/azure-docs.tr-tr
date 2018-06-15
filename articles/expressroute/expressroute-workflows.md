---
title: Bir expressroute bağlantı hattı yapılandırmak için iş akışları | Microsoft Docs
description: Bu sayfada, expressroute bağlantı hattı ve eşlemeleri yapılandırmak için iş akışları açıklanmaktadır
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: ''
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: cba1b2cfee379e7d2b079bcb3089981ef1044d66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23850780"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Devre sağlama ve devre durumları için ExpressRoute iş akışları
Bu sayfa, sağlama ve yüksek düzeyde configuration iş akışlarını yönlendirme hizmeti açıklanmaktadır.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Aşağıdaki şekil ve ilgili adımlarda, sağlanan bir expressroute bağlantı hattı olması için izlemeniz gereken görevleri uçtan uca gösterin. 

1. Bir expressroute bağlantı hattı yapılandırmak için PowerShell kullanın. ' Ndaki yönergeleri izleyin [oluşturma ExpressRoute bağlantı hatları](expressroute-howto-circuit-classic.md) daha fazla ayrıntı için makale.
2. Hizmet sağlayıcısı sipariş bağlantısı. Bu işlem değişir. Bağlantı sipariş konusunda daha fazla ayrıntı için bağlantı sağlayıcınıza başvurun.
3. Bağlantı hattı başarıyla sağlama durumu PowerShell aracılığıyla expressroute bağlantı hattı doğrulayarak sağlandığından emin olun. 
4. Yönlendirme etki alanlarını yapılandırın. Bağlantı sağlayıcınızdan sizin için Katman 3 yönetiyorsa hattı için yönlendirmeyi yapılandırır. Bağlantı sağlayıcınız yalnızca Katman 2 Hizmetleri sunuyorsa, açıklanan yönergeleri başına yönlendirme yapılandırmalısınız [yönlendirme gereksinimleri](expressroute-routing.md) ve [yönlendirme yapılandırması](expressroute-howto-routing-classic.md) sayfaları.
   
   * Azure özel eşlemeyi etkinleştirmesini - Bu eşleme VM'ler için Bağlan / bulut sanal ağlar içinde dağıtılan Hizmetleri etkinleştirmeniz gerekir.
   * Azure ortak eşleme enable - genel IP adresleri üzerinde barındırılan Azure hizmetlerine bağlanmak isterseniz, Azure ortak eşleme etkinleştirmeniz gerekir. Bu, Azure özel eşleme için varsayılan yönlendirme etkinleştirmek seçmiş olmanız durumunda Azure kaynaklarına erişmek için gerekli değildir.
   * Microsoft eşlemesi - etkinleştirmek, bu erişim Office 365 ve Dynamics 365 etkinleştirmeniz gerekir. 
     
     > [!IMPORTANT]
     > Ayrı bir proxy sunucu kullan / olandan Microsoft'a bağlanmak için sınır kullanmak için Internet sağlanmalıdır. Hem ExpressRoute hem de Internet için aynı sınır kullanarak asimetrik yönlendirme neden ve ağınıza bağlantı kesintileri neden.
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. Sanal ağları bağlama ExpressRoute bağlantı hatları için - expressroute bağlantı hattına sanal ağlara bağlayabilirsiniz. Yönergeleri izleyerek [sanal ağlara bağlamak için](expressroute-howto-linkvnet-arm.md) bağlantı hattınız için. Bu sanal ağlar ya da expressroute bağlantı hattı aynı Azure abonelik olabilir veya farklı bir abonelikte olabilir.

## <a name="expressroute-circuit-provisioning-states"></a>Expressroute bağlantı hattı durumları sağlama
Her expressroute bağlantı hattı iki durum vardır:

* Hizmet Sağlayıcısı sağlama durumu
* Durum

Durum Microsoft'un sağlama durumunu temsil eder. Bir expressroute bağlantı hattı oluşturduğunuzda, bu özellik etkin olarak ayarlanır

Bağlantı Sağlayıcısı sağlama durumu bağlantı sağlayıcısı tarafında durumunu temsil eder. Ya da olabilir *NotProvisioned*, *sağlama*, veya *hazırlandı*. Expressroute bağlantı hattı, onu kullanabilmek hazırlandı durumunda olması gerekir.

### <a name="possible-states-of-an-expressroute-circuit"></a>Olası bir expressroute bağlantı hattı durumları
Bu bölümde bir expressroute bağlantı hattı için olası durumlar çıkışı listelenir.

**Oluşturma sırasında**

Expressroute bağlantı hattı oluşturmak için PowerShell cmdlet'ini çalıştırın hemen sonra expressroute bağlantı hattı şu duruma görürsünüz.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Bağlantı sağlayıcı devre sağlama sürecinde olduğunda**

Şu duruma expressroute bağlantı hattı bağlantı sağlayıcı hizmet anahtarı geçirmek ve sağlama işlemini başlattığınız hemen görürsünüz.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Bağlantı sağlayıcı sağlama işlemini zaman tamamlandı**

Bağlantı sağlayıcı sağlama işlemi tamamlandıktan hemen sonra expressroute bağlantı hattı şu duruma görürsünüz.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Sağlanan ve yalnızca durum bağlantı hattı onu kullanabilmek için kullanılabilir etkin olur. Bir katman 2 sağlayıcısı kullanıyorsanız, yalnızca bu durumda olduğunda, hattı için yönlendirmeyi yapılandırabilirsiniz.

**Bağlantı sağlayıcı bağlantı hattı zaman sağlamayı kaldırma**

Expressroute bağlantı hattı yetkisini kaldırma için hizmet sağlayıcısı istenen hizmet sağlayıcısı sağlamanın kaldırılması işlemini tamamladıktan sonra aşağıdaki durumuna ayarlamak hattı görürsünüz.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Gerekli ya da devreyi silmek için PowerShell cmdlet'lerini çalıştırın yeniden etkinleştirmeyi seçebilirsiniz.  

> [!IMPORTANT]
> Çalıştırırsanız ServiceProviderProvisioningState sağlamada devreyi silmek için PowerShell cmdlet veya hazırlandı işlemi başarısız olur. Lütfen expressroute bağlantı hattı ilk yetkisini kaldırma için bağlantı sağlayıcınıza çalışır ve bağlantı hattı silin. Microsoft devreyi silmek için PowerShell cmdlet çalıştırılıncaya bağlantı hattı faturalandırmak devam eder.
> 
> 

## <a name="routing-session-configuration-state"></a>Yönlendirme oturum yapılandırma durumu
Sağlama durumu BGP, BGP oturumu Microsoft edge etkinleştirilirse bilmenizi sağlar. Durum eşlemeyi kullanabilmek için etkinleştirilmelidir.

Özellikle Microsoft eşlemesi için BGP oturum durumunu denetlemek önemlidir. Sağlama durumu BGP yanı sıra yok adlı başka bir duruma *tanıtılan genel ön ekten durumu*. Tanıtılan genel ön ekten durumu olmalıdır *yapılandırılmış* durum hem yukarı BGP oturumu ve uçtan uca çalışmak için yönlendirme. 

Tanıtılan genel ön eki durumu ayarlanırsa bir *gerekli doğrulama* durumunda, BGP oturumu etkin değil, tanıtılan önekler herhangi bir kayıt defterlerini yönlendirme AS numarası eşleşmediğinden. 

> [!IMPORTANT]
> Tanıtılan genel ön ek durum ise *el ile doğrulama* durum ile bir destek bileti açmanız [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ve boyunca ile ilişkili Otonom sistem numarası tanıtılan IP adreslerini kendi kanıt sağlar.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)

