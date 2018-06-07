---
title: Azure Active Directory Uygulama proxy'si joker uygulamalarda | Microsoft Docs
description: Azure Active Directory Uygulama proxy'si joker uygulamalarını kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: bf50c63b351c711b2b6fc5091109289635036513
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589517"
---
# <a name="wildcard-applications-in-the-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si joker uygulamaları 

Azure Active Directory'de çok sayıda şirket içi yapılandırma (Azure AD), uygulamaları hızla yönetilemeyen büyüyebilir ve bunların çoğu, aynı ayarları gerekiyorsa yapılandırma hataları gereksiz risklerini de beraberinde getirir. İle [Azure AD uygulama proxy'si](manage-apps/application-proxy.md), yayımlama ve aynı anda birçok uygulamaları yönetmek için joker karakter uygulama yayımlama kullanarak bu sorunu gidermek. Bu, olanak sağlayan bir çözüme.

-   Yönetim yükünü basitleştirir
-   Olası yapılandırma hataları sayısını azaltın
-   Daha fazla kaynaklarına güvenle erişmesini etkinleştirin

Bu makalede, ortamınızda joker uygulama yayımlamayı yapılandırmak için gereken bilgileri sağlar.


## <a name="create-a-wildcard-application"></a>Joker karakter uygulaması oluşturma 

Aynı yapılandırmaya sahip bir uygulama grubu varsa bir joker karakter (*) uygulaması oluşturabilirsiniz. Olası bir joker uygulama uygulamaları aşağıdaki ayarları paylaşımı adaylar:

- Bunlara erişimi olan kullanıcı grubu
- SSO yöntemi
- Erişim Protokolü (http, https)

Hem, iç ve dış URL aşağıdaki biçimde olması durumunda joker karakterlerle uygulamalar yayımlayabilmeniz için:

> http (s) :/ / *. \<etki alanı\>

Örneğin: `http(s)://*.adventure-works.com`. İç ve dış URL'ler farklı etki alanlarında, en iyi uygulama olarak kullanabilirsiniz, ancak bunlar aynı olmalıdır. URL'lerden birini bir joker karakter yoksa, uygulama yayımlarken, bir hata görürsünüz.

Farklı yapılandırma ayarlarıyla ek uygulamalarınız varsa, bu özel durumlar için joker karakter kümesi varsayılanlarını geçersiz kılmak için ayrı uygulamalar olarak yayımlamanız gerekir. Joker karakter olmayan uygulamalar joker uygulamalar yerine her zaman önceliklidir. Yapılandırma açısından bakıldığında, bunlar "yalnızca" Normal uygulamalardır.

Joker uygulama oluşturma temel aynı [uygulama yayımlama akışını](manage-apps/application-proxy-publish-azure-portal.md) diğer tüm uygulamalar için kullanılabilir. Tek fark, URL'ler ve büyük olasılıkla SSO yapılandırma içinde bir joker karakter dahil edilir.


## <a name="prerequisites"></a>Önkoşullar

### <a name="custom-domains"></a>Özel etki alanları

Sırada [özel etki alanlarını](manage-apps/application-proxy-configure-custom-domain.md) olan joker karakter uygulamalar için bir önkoşul oldukları diğer tüm uygulamaları için isteğe bağlıdır. Özel etki alanları oluşturma şunları gerektirir:

1. Azure içinde doğrulanmış bir etki alanı oluşturma 
2. Uygulama proxy'niz PFX biçiminde bir SSL sertifikası yükleyin.

Oluşturmayı planladığınız uygulama eşleştirmek için joker karakter sertifika kullanmayı düşünmeniz gerekir. Alternatif olarak, yalnızca belirli uygulamaları listeleyen sertifika kullanabilirsiniz. Bu durumda, yalnızca sertifikasında listelenen uygulamalar bu joker uygulama erişilebilir.

Güvenlik nedeniyle, bu sabit bir gereksinimdir ve biz joker karakterler özel bir etki alanı için dış URL kullanamazsınız uygulamaları desteklemez.

### <a name="dns-updates"></a>DNS güncelleştirmeleri

Özel etki alanı kullanırken, bir DNS girişi için dış URL ile bir CNAME kaydı oluşturmanız gerekir (örneğin, `*.adventure-works.com`) uygulama proxy endpoint dış URL'sine yönlendiren. Joker karakter uygulamalar için CNAME kaydı ilgili dış URL'ler işaret etmesi gerekir:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

CNAME düzgün yapılandırılmış, kullanabileceğiniz onaylamak için [nslookup](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) hedef uç noktalardan biri, örneğin, üzerinde `expenses.adventure-works.com`.  Yanıtınız zaten belirtilen diğer ad içermelidir (`<yourAADTenantId>.tenant.runtime.msappproxy.net`).


## <a name="considerations"></a>Dikkat edilmesi gerekenler


### <a name="accepted-formats"></a>Kabul edilen biçimler

Joker karakter uygulamalar için **İç URL** olarak biçimlendirilmelidir `http(s)://*.<domain>`. 

![AppID](./media/active-directory-application-proxy-wildcard\22.png)


Yapılandırdığınızda bir **dış URL**, aşağıdaki biçimi kullanın: `https://*.<custom domain>` 

![AppID](./media/active-directory-application-proxy-wildcard\21.png)

Joker karakter, birden fazla joker karakterler veya diğer regex dizeleri diğer konumlarını desteklenmez ve hataları neden oluyor.


### <a name="excluding-applications-from-the-wildcard"></a>Joker karakter ' uygulamaları hariç

Bir uygulama tarafından joker uygulamasından dışlayabilirsiniz

- Normal uygulama olarak özel uygulama yayımlama 
- DNS ayarlarınız aracılığıyla belirli uygulamalar için yalnızca bir joker karakter etkinleştirme  


Normal uygulama olarak bir uygulama yayımlama bir joker karakter bir uygulama dışlamak için tercih edilen yöntemdir. Dışlanan uygulamalar, özel durumlar baştan olan uygulanmasını sağlamak için joker karakter uygulamaları önce yayımlamanız gerekir. En belirli bir uygulamayı her zaman – olarak yayımlanan uygulama öncelikli olur `budgets.finance.adventure-works.com` uygulama önceliklidir `*.finance.adventure-works.com`, hangi sırayla önceliklidir uygulama `*.adventure-works.com`. 

Ayrıca, DNS Yönetimi aracılığıyla belirli uygulamalar için yalnızca çalışmak için joker karakter sınırlayabilirsiniz. En iyi uygulama, yapılandırdığınız dış URL'nin biçimi ile eşleşen ve bir joker karakter içeren bir CNAME girişi oluşturmanız gerekir. Ancak, joker karakterler için bunun yerine belirli bir uygulama URL'leri işaret edebilir. Örneğin, yerine, `*.adventure-works.com`, noktası `hr.adventure-works.com`, `expenses.adventure-works.com` ve `travel.adventure-works.com individually` için `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`. 

Bu seçeneği kullanırsanız, başka bir CNAME giriş değeri etmeniz `AppId.domain`, örneğin, `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`gelin de aynı konumu. Bulabileceğiniz **AppID** joker uygulama, uygulama özellikleri sayfasında:

![AppID](./media/active-directory-application-proxy-wildcard\01.png)



### <a name="setting-the-homepage-url-for-the-myapps-panel"></a>Giriş sayfası URL'si MyApps paneli için ayarlama

Joker uygulama sadece bir parçasında ile temsil edilen [MyApps Masası](https://myapps.microsoft.com). Varsayılan olarak bu kutucuğu gizlenir. Döşeme göstermek ve belirli bir sayfada kullanıcılar kara sağlamak için:

1. İçin yönergeleri izleyin [ayarlanırken bir giriş sayfası URL'si](manage-apps/application-proxy-configure-custom-home-page.md).
2. Ayarlama **Göster uygulama** için **doğru** uygulama özellikleri sayfasında.

### <a name="kerberos-constrained-delegation"></a>Kısıtlı Kerberos temsilcisi seçme

Kullanan uygulamalar için [kerberos Kısıtlı temsilci (KCD) SSO yöntemi olarak](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md), SSO yöntemi de bir joker karakter gerekebilir için SPN listelenen. Örneğin, SPN olabilir: `HTTP/*.adventure-works.com`. Yine de, arka uç sunucuları üzerinde yapılandırılmış tek tek SPN'ler olması gerekiyorsa (örneğin, `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`).



## <a name="scenario-1-general-wildcard-application"></a>Senaryo 1: Genel joker uygulama

Bu senaryoda, yayımlamak istediğiniz üç farklı uygulamalar vardır:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

Üç uygulama:

- Tüm kullanıcılar tarafından kullanılır
- Kullanım *tümleşik Windows kimlik doğrulaması* 
- Aynı özelliklere sahip


Özetlenen adımları kullanarak joker uygulama yayımlayabilirsiniz [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md). Bu senaryo varsayar:

- Şu kimlikli Kiracı: `000aa000-11b1-2ccc-d333-4444eee4444e` 

- Doğrulanmış bir etki alanı adında `adventure-works.com` yapılandırıldı.

- A **CNAME** işaret girişi `*.adventure-works.com` için `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net` oluşturuldu.

Aşağıdaki [adımları belgelenen](manage-apps/application-proxy-publish-azure-portal.md), kiracınızda yeni bir uygulama proxy uygulaması oluşturun. Bu örnekte, aşağıdaki alanlarda joker karakter kullanılabilir:

- İç URL'si:

    ![İç URL](./media/active-directory-application-proxy-wildcard\42.png)


- Dış URL:

    ![Dış URL](./media/active-directory-application-proxy-wildcard\43.png)

 
- İç uygulama SPN: 

    ![SPN yapılandırma](./media/active-directory-application-proxy-wildcard\44.png)


Joker uygulama yayımlayarak, artık üç uygulamalarınız için kullanılan URL'leri giderek erişebilirsiniz (örneğin, `travel.adventure-works.com`).

Yapılandırma aşağıdaki yapısını uygular:

![AppID](./media/active-directory-application-proxy-wildcard\05.png)

| Renk | Açıklama |
| ---   | ---         |
| Mavi  | Uygulamaları açıkça yayımlanan ve Azure Portalı'nda görünür. |
| Gri  | Uygulamaları üst uygulama üzerinden erişilebilir olabilir. |




## <a name="scenario-2-general-wildcard-application-with-exception"></a>Senaryo 2: Genel joker uygulama özel durum ile

Bu senaryoda, ayrıca üç genel için başka bir uygulama, uygulamanız `finance.adventure-works.com`, yalnızca olacağı Finans bölme erişilebilir. Geçerli uygulama yapısıyla Finans uygulamanız tüm çalışanlar tarafından ve joker uygulama aracılığıyla erişilebilir olacaktır. Bunu değiştirmek için uygulamanızın, joker karakter daha kısıtlayıcı izinlerle ayrı bir uygulama olarak Finans yapılandırarak dışlayın.



CNAME kayıtları işaret var olduğundan emin olmanız gerekir `finance.adventure-works.com` uygulama için uygulama proxy'si sayfasında belirtilen uygulama belirli uç. Bu senaryo için `finance.adventure-works.com` işaret `https://finance-awcycles.msappproxy.net/`. 

Aşağıdaki [adımları belgelenen](manage-apps/application-proxy-publish-azure-portal.md), bu senaryo aşağıdaki ayarları gerektirir:


- İçinde **İç URL**, ayarladığınız **Finans** yerine bir joker karakter. 

    ![İç URL](./media/active-directory-application-proxy-wildcard\52.png)

- İçinde **dış URL**, ayarladığınız **Finans** yerine bir joker karakter. 

    ![Dış URL](./media/active-directory-application-proxy-wildcard\53.png)

- İç uygulama ayarladığınız SPN **Finans** yerine bir joker karakter.

    ![SPN yapılandırma](./media/active-directory-application-proxy-wildcard\54.png)


Bu yapılandırma aşağıdaki senaryoyu uygular:

![Senaryo](./media/active-directory-application-proxy-wildcard\09.png)

Çünkü `finance.adventure-works.com` daha fazla belirli bir URL olduğunu `*.adventure-works.com`, öncelik kazanır. Kullanıcılar için gezinme `finance.adventure-works.com` Finans kaynakları uygulamada belirtilen deneyim sahibi. Bu durumda, yalnızca finans çalışanlar erişebilir `finance.adventure-works.com`.

Birden çok uygulama için finans yayımlanan varsa ve sahip olduğunuz `finance.adventure-works.com` doğrulanmış bir etki alanı başka bir joker uygulama yayımlayabilirsiniz `*.finance.adventure-works.com`. Bu genel daha fazla belirli olduğundan `*.adventure-works.com`, bir kullanıcı bir uygulama Finans etki alanındaki erişirse öncelik kazanır.


## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla bilgi için:

- **Özel etki alanlarını**, bkz: [Azure AD uygulama proxy'si özel etki alanları ile çalışma](manage-apps/application-proxy-configure-custom-domain.md).

- **Uygulamaları yayımlama**, bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md)


