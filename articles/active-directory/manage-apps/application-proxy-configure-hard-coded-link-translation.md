---
title: Bağlantılar ve URL'leri Azure AD uygulaması Proxy Çevir | Microsoft Docs
description: Azure AD uygulama ara sunucusu bağlayıcıları ile ilgili temel bilgileri kapsar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2949559542759cadf90d329bc50b352998b3eb7e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437754"
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantıları yeniden yönlendirin

Azure AD uygulama proxy'si, şirket içi uygulamalarınızı uzak kullanıcılar veya kendi cihazlarını kullanılabilmesini sağlar. Bazı uygulamalar, ancak HTML'de gömülü yerel bağlantılar ile geliştirilmiştir. Bu bağlantıları, uygulama uzaktan kullanıldığında düzgün çalışmaz. Birbirine noktası çeşitli şirket içi uygulamalara sahip olduğunuzda, kullanıcılarınızın ofisinde bulunmasa çalışmaya devam etmek için bağlantıları bekler. 

Bağlantılar aynı hem iç hem de Şirket ağınızın dışında çalıştığından emin olmak için en iyi yolu, dış URL'leri uygulamalarınızı kendi iç URL ile aynı olacak şekilde yapılandırmaktır. Kullanım [özel etki alanları](application-proxy-configure-custom-domain.md) varsayılan uygulama ara sunucusu etki alanı yerine şirket etki alanı adınızı sağlamak için dış URL'leri yapılandırmak için.


Özel etki alanları kiracınızda kullanamıyorsanız, bu işlevi sağlamak için birkaç seçenek vardır. Bunların tümü aynı zamanda özel etki alanları ve gerekirse diğer çözümleri yapılandırabilmek için özel etki alanları ve birbiriyle uyumlu değildir. 

**1. seçenek: Managed Browser kullanmasını** – Bu çözüm yalnızca önerilir veya kullanıcılar Intune yönetilen tarayıcı uygulamaya erişim gerektiren düşünüyorsanız geçerlidir. Bu, tüm yayımlanan URL'ler işleyecektir. 

**2. seçenek: MyApps uzantısının kullanılması** – bir istemci-tarafı tarayıcı uzantısını yüklemek kullanıcıların bu çözümü gerektirir, ancak tüm yayımlanan URL'ler ve en popüler tarayıcılarla çalışır işleyecek. 

**Seçenek 3: Bağlantı çeviri ayarı kullanmak** – kullanıcılarına görünür hale gelen bir yönetici yan ayar budur. Ancak, yalnızca HTML ve CSS URL'lerinde işleyeceği. Oluşturulan JavaScript iç URL'leri sabit kodlanmış (örneğin) işe yaramaz.  

Bu üç özellik, kullanıcılarınızın nerede olursa olsun çalışma bağlantılarınızı tutun. Doğrudan iç uç nokta veya bağlantı noktası uygulamalar varsa, bu iç URL'leri yayımlanan dış uygulama ara sunucusu URL'leri eşleyebilirsiniz. 

 
> [!NOTE]
> Son seçenek ne olursa olsun nedeni için özel etki alanları uygulamalarını aynı iç ve dış URL'leri için kullanamazsınız, kiracılar için kullanılabilir. Bu özelliği etkinleştirmeden önce bkz. [Azure AD uygulama proxy'sinde özel etki alanları](application-proxy-configure-custom-domain.md) sizin için çalışabilir. 
> 
> Veya uygulamayı yapılandırmak gerekirse bağlantıyla çeviri SharePoint için bkz. [SharePoint 2013 için alternatif erişim eşlemelerini yapılandırma](https://technet.microsoft.com/library/cc263208.aspx) eşleme bağlantıları için başka bir yaklaşım. 

 
### <a name="option-1-intune-managed-browser-integration"></a>1. seçenek: Intune yönetilen tarayıcı tümleştirme 

Daha fazla uygulama ve içerik korumak için Intune Managed Browser'ı kullanabilirsiniz. Bu çözümü kullanmak için kullanıcı erişimi Intune Managed Browser üzerinden uygulama gerektir/öneri gerekir. Uygulama Ara sunucusu ile yayımlanan tüm iç URL'lerin Managed Browser tarafından tanınan ve karşılık gelen dış URL'ye yeniden yönlendirilen. Bu, tüm sabit kodlanmış iç URL'leri çalışır ve bir kullanıcı, tarayıcıya gider ve doğrudan İç URL türleri kullanıcı uzak olsa bile çalıştığını sağlar.  

Bu seçenek, yapılandırma dahil olmak üzere daha fazla bilgi için lütfen bkz [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser) belgeleri.  

### <a name="option-2-myapps-browser-extension"></a>2. seçenek: MyApps tarayıcı uzantısı 

MyApps tarayıcı uzantısı ile uygulama ara sunucusu ile yayımlanan tüm iç URL'leri uzantısı tarafından tanınan ve karşılık gelen bir dış URL'ye yeniden yönlendirildi. Bu, tüm sabit kodlanmış iç URL'leri çalışır ve bir kullanıcı, tarayıcının adres çubuğuna gider ve doğrudan İç URL türleri kullanıcı uzak olsa bile çalıştığını sağlar.  

Bu özelliği kullanmak için kullanıcının uzantısını indirin ve oturum açmış olmanız gerekir. Yöneticiler veya kullanıcılar için gerekli herhangi bir yapılandırma yoktur. 

 

### <a name="option-3-link-translation-setting"></a>Seçenek 3: Bağlantı çeviri ayarı 

Bağlantı çeviri etkinleştirildiğinde, uygulama proxy'si hizmeti HTML ve CSS yayımlanan iç bağlantıları arar ve kullanıcılarınız kesintisiz bir deneyim elde şekilde çevirir. 



## <a name="how-link-translation-works"></a>Çeviri works nasıl bağlantı

Kimlik doğrulamasından sonra proxy sunucunun kullanıcı için uygulama verilerinin başarılı olduğunda uygulama proxy'si sabit kodlanmış bağlantı uygulama tarar ve URL'lere yayımlanan kendi ilgili, değiştirir.

Uygulama proxy'si uygulamaları UTF-8 olarak kodlanmış olduğu varsayılır. Durum bu değilse, kodlama türü bir http yanıt üst bilgisinde, gibi belirtmek `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Hangi bağlantıları etkilenir?

Bağlantı çevirisi özelliğini yalnızca bir uygulamanın gövdesindeki kod etiketlerindeki bağlantıları arar. Uygulama proxy'si üst bilgilerinde tanımlama veya URL'leri çevirme için ayrı bir özellik vardır. 

İç bağlantı şirket içi uygulamalarda yaygın iki tür vardır:

- **İç göreli bağlantıları** bir yerel dosya yapısı içinde bir paylaşılan kaynak noktasına gibi sağladığı `/claims/claims.html`. Bu bağlantılar otomatik olarak uygulama proxy'si aracılığıyla yayımlandığından ve ile veya olmadan bağlantı çeviri devam uygulamalar çalışır. 
- **Sabit kodlanmış iç bağlantıları** gibi diğer şirket içi uygulamalara `http://expenses` ya da dosyalar gibi yayımlanan `http://expenses/logo.jpg`. Bağlantı çevirisi özelliğini sabit kodlanmış iç bağlantılarında çalışır ve bunları üzerinden geçmek üzere uzak kullanıcıların gereken dış URL'leri işaret edecek şekilde değiştirir.

Uygulama proxy'si için ekleme bağlantısı çeviri destekler HTML kod etiketlerinin tam listesi:
* a
* Ses
* temel
* Düğme
* div
* Katıştır
* Formu
* Çerçeve
* HEAD
* html
* iframe
* görüntü
* giriş
* bağlantı
* MenuItem
* Meta
* object
* script
* source
* İzleme
* video

Ayrıca, CSS içinde URL özniteliğini de çevrilir.

### <a name="how-do-apps-link-to-each-other"></a>Uygulamaları birbirleriyle nasıl bağlantı kurarım?

Böylece uygulama başına düzeyinde kullanıcı deneyimi üzerinde denetime sahip bağlantı çeviri her uygulama için etkinleştirilir. Bir uygulama için bağlantı çeviri bağlantılar istediğinizde Aç *gelen* değil çevrilmesi için bu uygulama bağlantılar *için* bu uygulama. 

Örneğin, uygulama tüm birbirine bağlamasını Proxy üzerinden yayımlanan üç uygulama olduğunu varsayalım: Avantajları, giderleri ve seyahat. Dördüncü bir uygulama, uygulama proxy'si aracılığıyla yayımlandığından değilse geri bildirim yoktur.

Bağlantı çeviri avantajları uygulama için etkinleştirdiğinizde, giderleri ve seyahat bağlanan dış URL'leri bu uygulamalarda yönlendirilir, ancak hiçbir dış URL olduğundan geri bildirim bağlantısını yeniden yönlendirilmediği. Bağlantı çeviri bu iki uygulama için etkin değil çünkü geri avantajları için giderleri ve seyahat bağlantıları çalışmaz.

![Avantajları bağlanan bağlantı çeviri etkinleştirildiğinde diğer uygulamalar](./media/application-proxy-configure-hard-coded-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Hangi bağlantıları çevrilmemiş?

Performans ve güvenliğini geliştirmek için bazı bağlantılar çevrilmiş değildir:

- Bağlantılar kod etiketleri içinde değil. 
- HTML veya CSS bağlantılar. 
- URL kodlanmış biçimindeki bağlantıları.
- İç bağlantı başka programlar tarafından açılmış. E-posta ya da anlık ileti gönderilen veya diğer belgelerin içinde bulunan bağlantılar çevrilmiş olmaz. Kullanıcılar için dış URL'yi Git bilmeniz gerekir.

Bu iki senaryodan biri desteklemeniz gerekiyorsa, aynı iç ve dış URL'leri bağlantı çeviri yerine kullanın.  

## <a name="enable-link-translation"></a>Bağlantı çeviri etkinleştir

Bağlantı çeviri ile çalışmaya başlama, bir düğmeye tıklatmak kadar kolaydır:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** > yönetmek istediğiniz uygulamayı seçin >  **Uygulama proxy'si**.
3. Kapatma **uygulama gövdesi URL'leri çevir** için **Evet**.

   ![Uygulama gövdesinde URL'leri çevirme için Evet'i seçin](./media/application-proxy-configure-hard-coded-link-translation/select_yes.png)
4. Seçin **Kaydet** yaptığınız değişiklikleri uygulamak için.

Kullanıcılarınızın bu uygulamaya eriştiğinde, artık proxy otomatik olarak kiracınızda uygulama proxy'si aracılığıyla yayımlanan iç URL'ler için tarar.

## <a name="send-feedback"></a>Seslenme iber

Bu özellik tüm uygulamalar için çalışır hale getirmek için yardımınıza istiyoruz. 30'dan etiketleri HTML ve CSS arayın. Bir örnek çevrildiğini olmayan oluşturulan bağlantı varsa, bir kod parçacığı için gönderme [uygulama Proxy geri bildirimi](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Sonraki adımlar
[Özel etki alanları ile Azure AD uygulama proxy'si kullanın](application-proxy-configure-custom-domain.md) aynı iç ve dış URL'sini sağlamak için

[SharePoint 2013 için alternatif erişim eşlemelerini yapılandırma](https://technet.microsoft.com/library/cc263208.aspx)
