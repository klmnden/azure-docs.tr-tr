---
title: Uygulama Ara sunucusu bağlayıcıları - Azure Active Directory hata ayıklama | Microsoft Docs
description: Azure Active Directory (Azure AD) uygulama Proxy bağlayıcıları ile ilgili sorunlar hata ayıklayın.
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
ms.openlocfilehash: c3088ae777fe1a64be218105d36fdb9e01d7b798
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172243"
---
# <a name="debug-application-proxy-connector-issues"></a>Uygulama Ara sunucusu Bağlayıcısı sorunlarında hata ayıklama 

Bu makalede Azure Active Directory (Azure AD) uygulama Proxy bağlayıcıları ile ilgili sorunları gidermenize yardımcı olur. Uzaktan erişim için bir şirket içi web uygulamasına uygulama Proxy hizmetini kullanıyorsanız ancak uygulamaya bağlanırken sorun yaşıyorsanız, bu akış Bağlayıcısı hata ayıklamak için kullanın. 

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, uygulama ara sunucusu Bağlayıcısı'nı yüklediyseniz ve soruna sahip olduğunu varsayar. Uygulama Ara sunucusu sorunlarını giderme, uygulama ara sunucusu bağlayıcıları düzgün yapılandırılıp yapılandırılmadığını belirlemek için sorun giderme bu akışla başlayın öneririz. Sorun giderme flow'da uygulamaya bağlanırken sorun yaşamaya devam ediyorsanız izleyin [hata ayıklama uygulama proxy'si uygulama sorunlarını](application-proxy-debug-apps.md).  


Uygulama Ara sunucusu ve onun bağlayıcıları kullanma hakkında daha fazla bilgi için bkz:

- [Uygulama proxy'si aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)
- [Uygulama Proxy Bağlayıcısı](application-proxy-connectors.md)
- [Yükleme ve bir bağlayıcıyı kaydetme](application-proxy-add-on-premises-application.md)
- [Uygulama proxy'si sorunlarını ve hata iletileri sorunlarını giderme](application-proxy-troubleshoot.md)

## <a name="flowchart-for-connector-issues"></a>Bağlayıcı sorunları için akış çizelgesi

Bu akış Bağlayıcısı daha yaygın sorunlardan bazılarını hata ayıklama adımlarında size kılavuzluk eder. Her adım hakkında daha fazla ayrıntı için akış aşağıdaki tabloya bakın.

![Bağlayıcı hata ayıklama için adımları gösteren akış çizelgesi](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

|  | Eylem | Açıklama | 
|---------|---------|---------|
|1 | Uygulamaya atanmış bağlayıcı grubu bulma | Büyük olasılıkla birden çok sunucu üzerinde yüklü bir bağlayıcı vardır, bu durumda bağlayıcıları olmalıdır [bağlayıcı gruplara atanan](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups). Bağlayıcı grupları hakkında daha fazla bilgi için bkz: [ayrı ağlar ve konumları bağlayıcı grupları kullanarak uygulama yayımlama](application-proxy-connector-groups.md). |
|2 | Bağlayıcıyı yükleyin ve bir grup atayabilir | Yüklü bir bağlayıcı yoksa bkz [yükleme ve kaydetme bağlayıcı](application-proxy-add-on-premises-application.md#install-and-register-a-connector).<br></br>Bağlayıcı bir gruba atanmamış olup [bağlayıcı bir gruba atamak](application-proxy-connector-groups.md#create-connector-groups).<br></br>Uygulama için bir bağlayıcı grubu atanmamış olup [bir bağlayıcı grubu uygulamaya atama](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups).|
|3 | Bağlayıcı sunucu üzerinde bir bağlantı noktası testi Çalıştır | Bağlayıcı sunucusunda bir bağlantı noktası test çalıştırmak [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) veya diğer bağlantı sınama aracı 443 ve 80 numaralı bağlantı noktalarını açık olup olmadığını denetlemek için.|
|4 | Etki alanları ve bağlantı noktalarını yapılandırma | [Etki alanları ve bağlantı noktası doğru şekilde yapılandırıldığından emin olun](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) bağlayıcısının düzgün çalışması için belirli bağlantı noktalarının açık olması gerekir ve vardır URL'leri sunucunuza erişebilir olmalıdır. |
|5 | Arka uç proxy kullanımda olup olmadığını denetleyin | Bağlayıcıları kullanarak arka uç proxy sunucuları veya bunları atlama, denetleyin. Ayrıntılar için bkz [Bağlayıcısı proxy sorunları gidermenize ve hizmet bağlantı sorunları](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues). |
|6 | Bağlayıcı ve arka uç proxy kullanmak için güncelleştirici güncelleştirme | Arka uç Ara sunucu kullanımdaysa, aynı proxy Bağlayıcısı'nı kullandığından emin olmak isteyeceksiniz. Sorun giderme ve proxy sunucuları ile çalışmak için bağlayıcı yapılandırma hakkında daha fazla ayrıntı için bkz. [iş mevcut şirket içi proxy sunucuları](application-proxy-configure-connectors-with-proxy-servers.md). |
|7 | Bağlayıcı sunucusundaki İç URL uygulamanın yükleme | Bağlayıcı sunucu üzerinde uygulamanın İç URL yükleyin. |
|8 | İç ağ bağlantısını kontrol edin | Bu hata ayıklama akışı tanılayamıyorsanız olan iç ağınızda bir bağlantı sorunu var. Uygulamanın çalışması dahili olarak bağlayıcıları için erişilebilir olmalıdır. Etkinleştirebilir ve açıklandığı gibi Bağlayıcısı olay günlüklerini görüntüleyin [uygulama Proxy bağlayıcıları](application-proxy-connectors.md#under-the-hood). |
|9 | Arka uçta zaman aşımı değerini uzatın | İçinde **ek ayarlar** uygulamanız için değiştirme **arka uç uygulama zaman aşımı** ayarını **uzun**. Bkz: [şirket içi bir uygulamayı Azure AD'ye ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). |
|10 | Sorunlar devam ederse, belirli bir akış sorunları, gözden geçirme uygulama ve akışlar hata ayıklama SSO hedef | Kullanım [hata ayıklama uygulama proxy'si uygulama sorunlarını](application-proxy-debug-apps.md) akışla ilgili sorunları giderme. |

## <a name="next-steps"></a>Sonraki adımlar


* [Ayrı ağlarda ve konumları bağlayıcı grupları kullanarak uygulama yayımlama](application-proxy-connector-groups.md)
* [Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
* [Uygulama Ara sunucusu ve bağlayıcı hatalarını giderme](application-proxy-troubleshoot.md)
* [Azure AD uygulama ara sunucusu Bağlayıcısı sessiz yükleme](application-proxy-register-connector-powershell.md)
