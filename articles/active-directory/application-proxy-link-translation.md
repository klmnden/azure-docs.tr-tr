---
title: Bağlantılar ve URL'leri Azure AD uygulaması Proxy Çevir | Microsoft Docs
description: Azure AD uygulama proxy'si bağlayıcılar hakkında temel bilgiler yer almaktadır.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5009266dc2cbea360ef9c5dfa69fc2c13225d8cd
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantı yeniden yönlendirme

Azure AD uygulama proxy'si, şirket içi uygulamalarınızı Uzaktan kullanıcılara veya kendi cihazlarına kullanılabilmesini sağlar. Bazı uygulamalar, ancak, HTML biçiminde katıştırılmış yerel bağlantılar ile geliştirilmiştir. Bu bağlantılar doğru uygulama uzaktan kullanıldığında çalışmaz. Çeşitli şirket içi uygulamalar birbiriyle noktası varsa, kullanıcılarınızın ofiste olmadıklarını olduğunda çalışmaya devam için bağlantıları bekler. 

Bağlantıları aynı hem içinde hem de Şirket ağınızın dışındaki çalıştığından emin olmak için en iyi yolu, kendi iç URL'ler ile aynı olacak şekilde dış URL'ler uygulamalarınızın yapılandırmaktır. Kullanım [özel etki alanlarını](active-directory-application-proxy-custom-domains.md) varsayılan uygulama proxy etki alanı yerine şirket etki alanı adınızı sağlamak için dış URL'ler yapılandırılır.


Özel etki alanlarını kiracınızda kullanamıyorsanız, bu işlevselliği sağlamak için birkaç seçeneğiniz vardır. Bunların tümü de özel etki alanları ve gerekirse diğer çözümleri yapılandırabilmek için özel etki alanlarını ve birbirlerine ile uyumlu değildir. 

**Seçenek 1: yönetilen tarayıcı kullanmanız** – Bu çözüm yalnızca önerilir veya kullanıcıların Intune yönetilen tarayıcı uygulamaya erişim gerektiren planlıyorsanız geçerlidir. Tüm yayımlanan URL'ler işleyecek. 

**Seçenek 2: MyApps bu uzantıyı kullanan** – kullanıcılar bir istemci-tarafı tarayıcı uzantısı yüklemek bu çözüm gerektiriyor, ancak tüm yayımlanan URL'ler ve en popüler tarayıcılarla çalışır işleyecek. 

**Seçenek 3: bağlantı çeviri ayarı kullanmak** – kullanıcılara görünmezdir bir yönetici yan ayar budur. Ancak, HTML ve CSS URL'lerinde yalnızca işleyecek. JavaScript oluşturulan sabit kodlanmış Dahili URL (örneğin) çalışmaz.  

Bu üç özellik, kullanıcılarınızın nerede olursa olsun çalışma bağlantılarınızı tutun. Doğrudan iç uç noktalar veya bağlantı noktası uygulamalar varsa, bu dahili URL'leri yayımlanan dış uygulama Proxy URL'ler eşleyebilirsiniz. 

 
> [!NOTE]
> Son seçenek ne olursa olsun nedeni için özel etki alanlarını uygulamalarını aynı iç ve dış URL'leri olmasını kullanamazsınız, kiracılar içindir. Bu özelliği etkinleştirmek için önce olup [Azure AD uygulama proxy'si özel etki alanlarında](active-directory-application-proxy-custom-domains.md) sizin için çalışabilir. 

>Veya uygulama yapılandırmak gerekirse bağlantısıyla çeviri SharePoint bkz [SharePoint 2013 için diğer erişim eşleşmeleri yapılandırma](https://technet.microsoft.com/library/cc263208.aspx) eşleme bağlantıları için başka bir yaklaşım için. 

 
### <a name="option-1-intune-managed-browser-integration"></a>Seçenek 1: Tarayıcı tümleştirme Intune yönetilen 

Uygulama ve içerik daha iyi korumak için Intune yönetilen tarayıcı kullanabilirsiniz. Bu çözümü kullanmak için kullanıcılar erişim Intune yönetilen tarayıcı üzerinden uygulama gerektiren/öneri gerekir. Uygulama Ara sunucusu ile yayımlanan tüm iç URL'leri yönetilen tarayıcı tarafından tanınmıyor ve karşılık gelen bir dış URL'ye yeniden yönlendirildi. Bu, tüm sabit kodlanmış iç URL'leri çalışır ve bir kullanıcı tarayıcıya gider ve doğrudan İç URL türleri, kullanıcı uzak olsa bile, çalıştığı sağlar.  

Bu seçeneği yapılandırmak da dahil olmak üzere daha fazla bilgi için lütfen bkz [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser) belgeleri.  

### <a name="option-2-myapps-browser-extension"></a>Seçenek 2: MyApps tarayıcı uzantısı 

MyApps tarayıcı uzantısı ile uygulama ara sunucusu ile yayımlanan tüm iç URL'leri uzantısı tarafından tanınmıyor ve karşılık gelen bir dış URL'ye yeniden yönlendirildi. Bu, tüm sabit kodlanmış iç URL'leri çalışır ve bir kullanıcı tarayıcının adres çubuğuna gider ve doğrudan İç URL türleri, kullanıcı uzak olsa bile, çalıştığı sağlar.  

Bu özelliği kullanmak için kullanıcının uzantısını indirin ve oturum açmanız gerekir. Yöneticiler veya kullanıcılar için gerekli herhangi bir yapılandırma yoktur. 

 

### <a name="option-3-link-translation-setting"></a>Seçenek 3: Bağlantı çeviri ayarı 

Bağlantı çeviri etkinleştirildiğinde, uygulama proxy'si hizmeti HTML ve CSS yayımlanan iç bağlantıları için arar ve kesintisiz bir deneyim kullanıcılarınız alır şekilde çevirir. 



## <a name="how-link-translation-works"></a>Çeviri works nasıl bağlantı

Kimlik doğrulamasından sonra proxy sunucunun kullanıcı için uygulama verilerini başarılı olduğunda uygulama proxy'si uygulama sabit kodlanmış bağlantılar için tarar ve dış URL'ler yayımlanan kendi ilgili ile değiştirir.

Uygulama proxy'si uygulamaları UTF-8'de kodlandığını varsayar. Bu durumda değilse kodlama türü bir http yanıt üstbilgisi gibi belirtin `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Hangi bağlantıların etkilenen?

Bağlantı çeviri özelliği yalnızca bir uygulama gövdesinde kod etiketleri olan bağlantılar arar. Uygulama proxy'si tanımlama bilgilerini veya URL'leri üstbilgilerinde çevirme için ayrı bir özellik vardır. 

Şirket içi uygulamalara iç bağlantıları iki genel türleri şunlardır:

- **Göreli iç bağlantıları** bir yerel dosya yapısında bir paylaşılan kaynak noktasına ister `/claims/claims.html`. Bu bağlantılar otomatik olarak uygulama proxy'si aracılığıyla yayımlanır ve ile veya olmadan bağlantı çeviri çalışmaya devam uygulamaları çalışın. 
- **Sabit kodlanmış iç bağlantılar** gibi diğer şirket içi uygulamalara `http://expenses` ya da dosyalar gibi yayımlanan `http://expenses/logo.jpg`. Bağlantı çeviri özelliğini sabit kodlanmış iç bağlantıları çalışır ve bunları uzak kullanıcıların geçmeniz URL'lere işaret edecek şekilde değiştirir.

### <a name="how-do-apps-link-to-each-other"></a>Nasıl uygulamaları birbirine bağlamak?

Böylece uygulama başına düzeyinde kullanıcı deneyimi üzerinde denetiminiz bağlantı çeviri her uygulama için etkinleştirilir. Bir uygulama için bağlantı çeviri bağlantıları istediğinizde Aç *gelen* çevrilecek, bu uygulama olmayan bağlantıları *için* bu uygulama. 

Örneğin, uygulama tüm diğer için bağlantı Proxy üzerinden yayımlanan üç uygulama olduğunu varsayalım: avantajları, gider ve seyahat. Dördüncü bir uygulama, uygulama proxy'si aracılığıyla yayımlanan değil geri bildirim yoktur.

Bağlantı çevirisi avantajları uygulama için etkinleştirdiğinizde, giderleri ve seyahat bağlantılar dış URL'ler bu uygulamalarda yönlendirilir, ancak hiçbir dış URL olduğundan geri bildirim bağlantısını yeniden yönlendirilen değil. Bağlantı çeviri iki uygulamalarla için etkin değil çünkü bağlantılardan gider ve seyahat geri avantajları çalışmıyor.

![Avantajları bağlanan bağlantı çeviri etkinleştirilmişse, diğer uygulamalar](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Hangi bağlantıların çevrilen değil mi?

Performans ve güvenliği iyileştirmek için bazı bağlantılar çevrilen değil:

- Bağlantılar kod etiketleri içinde değil. 
- Bağlantılar HTML veya CSS biçiminde değil. 
- İç bağlantılar diğer programlardan açıldı. Gönderilen e-posta veya anlık ileti veya diğer belgelerde bulunan bağlantılar çevrilmiş olmaz. Kullanıcıların dış URL'sine gitmek için bilmeniz gerekir.

Şu iki senaryodan biri desteklemeniz gerekiyorsa, aynı iç ve dış URL yerine bağlantı çeviri kullanın.  

## <a name="enable-link-translation"></a>Bağlantı çeviri etkinleştir

Bağlantı çevirisi ile çalışmaya başlama düğmesi olarak kadar kolaydır:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** > yönetmek istediğiniz uygulamayı seçin >  **Uygulama proxy'si**.
3. Kapatma **Çevir URL'leri uygulama gövdesindeki** için **Evet**.

   ![Uygulama gövdesinde URL'leri çevirmek için Evet'i seçin](./media/application-proxy-link-translation/select_yes.png).
4. Seçin **kaydetmek** değişikliklerinizi uygulamak için.

Kullanıcıların bu uygulamayı eriştiğinizde, artık, proxy otomatik olarak uygulama proxy'si aracılığıyla Kiracı'yayımlandı iç URL'ler için tarar.

## <a name="send-feedback"></a>Geri bildirim gönderin

Bu özellik, uygulamalarınız için iş yapmak için Yardım istiyoruz. 30 etiketleri HTML ve CSS arayın. Çevrildiğini olmayan oluşturulan bağlantıları örneği varsa, bir kod parçacığı göndermeniz [uygulama Proxy geri bildirim](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si ile özel etki alanları kullanabilirsiniz](active-directory-application-proxy-custom-domains.md) aynı iç ve dış URL sağlamak için

[SharePoint 2013 için diğer erişim eşleşmeleri yapılandırın](https://technet.microsoft.com/library/cc263208.aspx)
