---
title: Uygulama Proxy uygulamaları - Azure Active Directory hata ayıklama | Microsoft Docs
description: Azure Active Directory (Azure AD) uygulama Proxy uygulamaları ile ilgili sorunlar hata ayıklayın.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: d0a12bde119e9dae3f950603fac4bce060bb5f91
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172273"
---
# <a name="debug-application-proxy-application-issues"></a>Uygulama proxy'si uygulaması sorunlarında hata ayıklama 

Bu makalede Azure Active Directory (Azure AD) uygulama Proxy uygulamaları ile ilgili sorunları gidermenize yardımcı olur. Uzaktan erişim için bir şirket içi web uygulamasına uygulama Proxy hizmetini kullanıyorsanız ancak uygulamaya bağlanırken sorun yaşıyorsanız, bu akış, uygulama hata ayıklamak için kullanın. 

## <a name="before-you-begin"></a>Başlamadan önce

Uygulama proxy'si ile ilgili sorunları giderirken Bağlayıcılarla başlattığınız öneririz. İlk olarak, sorun giderme akış içinde izleyin [uygulama ara sunucusu Bağlayıcısı hata ayıklama sorunları](application-proxy-debug-connectors.md) uygulama Proxy bağlayıcıları düzgün bir şekilde yapılandırıldığından emin olmak için. Hala sorun yaşıyorsanız, bir uygulamada sorun gidermek için bu makaleye dönün.  

Uygulama proxy'si hakkında daha fazla bilgi için bkz:

- [Uygulama proxy'si aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)
- [Uygulama Proxy Bağlayıcısı](application-proxy-connectors.md)
- [Yükleme ve bir bağlayıcıyı kaydetme](application-proxy-add-on-premises-application.md)
- [Uygulama proxy'si sorunlarını ve hata iletileri sorunlarını giderme](application-proxy-troubleshoot.md)

## <a name="flowchart-for-application-issues"></a>Uygulama sorunları için akış çizelgesi

Bu akış uygulamaya bağlama ile daha yaygın sorunlardan bazılarını hata ayıklama adımlarında size kılavuzluk eder. Her adım hakkında daha fazla ayrıntı için akış aşağıdaki tabloya bakın.

![Uygulama hata ayıklama adımlarını gösteren akış çizelgesi](media/application-proxy-debug-apps/application-proxy-apps-debugging-flowchart.png)

|  | Eylem | Açıklama | 
|---------|---------|---------|
|1 | Bir tarayıcı açın, uygulama erişim ve kimlik bilgilerinizi girin | Tüm kullanıcı ile ilgili hataları denetleyin ve uygulama gibi oturum açmak için kimlik bilgilerinizi kullanarak deneyin [bu kurumsal uygulama erişilemez](application-proxy-sign-in-bad-gateway-timeout-error.md). |
|2 | Uygulamaya kullanıcı atama doğrulayın | Kullanıcı hesabınıza şirket ağı içinde uygulamadan erişme izni olduğundan emin olun ve sonra içindeki adımları izleyerek uygulamasında oturum açmaya test [uygulamayı test etme](application-proxy-add-on-premises-application.md#test-the-application). Oturum açma sorunları devam ederse, bkz. [oturum açma hataları nasıl giderilir](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-troubleshoot-sign-in-errors).  |
|3 | Bir tarayıcı açın ve uygulamaya erişmeyi deneyin | Hemen bir hata ekrana gelirse, uygulama proxy'si düzgün yapılandırılıp yapılandırılmadığını denetleyin. Belirli hata iletileri hakkında daha fazla ayrıntı için bkz: [uygulama proxy'si sorunlarını giderme sorunlarını ve hata iletileri](application-proxy-troubleshoot.md).  |
|4 | Özel etki alanı ayarlarınızı kontrol edin veya hata giderme | Sayfası hiç görüntülenmezse, özel etki alanınızı düzgün yapılandırılmış inceleyerek emin [özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md).<br></br>Sayfayı yüklemez ve hata iletisi görüntülenir, başvurarak hata giderme [uygulama proxy'si sorunlarını giderme sorunlarını ve hata iletileri](application-proxy-troubleshoot.md). <br></br>Görüntülenecek hata iletisi için 20 saniyeden uzun sürerse, bağlantı sorunu olabilir. Git [hata ayıklama uygulama Proxy bağlayıcıları](application-proxy-debug-connectors.md) sorun giderme makalesi.  |
|5 | Bağlayıcı hata ayıklama için sorunları devam ederse, Git | Proxy ile bağlayıcı veya bağlayıcı ve arka uç arasında bir bağlantı sorunu olabilir. Git [hata ayıklama uygulama Proxy bağlayıcıları](application-proxy-debug-connectors.md) sorun giderme makalesi. |
|6 | Tüm kaynaklar yayımlamak, tarayıcının Geliştirici Araçları'nı denetleyin ve bağlantıları Düzelt | Tüm gerekli görüntüleri, betikleri ve stil sayfaları, uygulama için yayımlama yolu içerdiğinden emin olun. Ayrıntılar için bkz [şirket içi bir uygulamayı Azure AD'ye ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). <br></br>Tarayıcının Geliştirici Araçları (F12 araçlarındaki Internet Explorer veya Microsoft Edge) ve onay sorunları açıklandığı yayımlarken kullanmak [uygulama sayfası doğru görüntülemiyor](application-proxy-page-appearance-broken-problem.md). <br></br>Gözden geçirin, bozuk bağlantılar çözmek için Seçenekler [sayfadaki bağlantıları çalışmaz](application-proxy-page-links-broken-problem.md). |
|7 | Ağ gecikmesi denetle | Yavaş sayfa yüklenirse, ağ gecikme süresi en aza indirmek için yollar hakkında bilgi edinin [gecikme süresini azaltma konuları](application-proxy-network-topology.md#considerations-for-reducing-latency). | 
|8 | Ek sorun giderme yardımına bakın | Sorunlar devam ederse, ek sorun giderme makaleleri bulun [uygulama Proxy sorun giderme belgelerinin](application-proxy-page-appearance-broken-problem.md). |

## <a name="next-steps"></a>Sonraki adımlar


* [Ayrı ağlarda ve konumları bağlayıcı grupları kullanarak uygulama yayımlama](application-proxy-connector-groups.md)
* [Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
* [Uygulama Ara sunucusu ve bağlayıcı hatalarını giderme](application-proxy-troubleshoot.md)
* [Azure AD uygulama ara sunucusu Bağlayıcısı sessiz yükleme](application-proxy-register-connector-powershell.md)
