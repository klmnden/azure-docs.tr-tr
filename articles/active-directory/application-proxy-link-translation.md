---
title: "Bağlantılar ve URL'leri Azure AD uygulaması Proxy Çevir | Microsoft Docs"
description: "Azure AD uygulama proxy'si bağlayıcılar hakkında temel bilgiler yer almaktadır."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 57218346d236b376d2227e0ffaea6c6dd5ebe855
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantı yeniden yönlendirme

Azure AD uygulama proxy'si, şirket içi uygulamalarınızı Uzaktan kullanıcılara veya kendi cihazlarına kullanılabilmesini sağlar. Bazı uygulamalar, ancak, HTML biçiminde katıştırılmış yerel bağlantılar ile geliştirilmiştir. Bu bağlantılar doğru uygulama uzaktan kullanıldığında çalışmaz. Çeşitli şirket içi uygulamalar birbiriyle noktası varsa, kullanıcılarınızın ofiste olmadıklarını olduğunda çalışmaya devam için bağlantıları bekler. 

Bağlantıları aynı hem içinde hem de Şirket ağınızın dışındaki çalıştığından emin olmak için en iyi yolu, kendi iç URL'ler ile aynı olacak şekilde dış URL'ler uygulamalarınızın yapılandırmaktır. Kullanım [özel etki alanlarını](active-directory-application-proxy-custom-domains.md) varsayılan uygulama proxy etki alanı yerine şirket etki alanı adınızı sağlamak için dış URL'ler yapılandırılır.

Özel etki alanlarını kiracınızda kullanamıyorsanız, uygulama proxy'sinin bağlantı çeviri özelliği, kullanıcılarınızın nerede olursa olsun çalışma bağlantılarınızı tutar. Doğrudan iç uç noktalar veya bağlantı noktası uygulamalar varsa, bu dahili URL'leri yayımlanan dış uygulama Proxy URL'ler eşleyebilirsiniz. Bağlantı çeviri etkinleştirildiğinde ve HTML, CSS ve JavaScript etiketlerin select yayımlanan iç bağlantılar için uygulama proxy'si arar. Böylece kullanıcılarınızın kesintisiz bir deneyim almak uygulama proxy'si hizmeti bunları çevirir.

>[!NOTE]
>Bağlantı çeviri özelliğini ne olursa olsun nedeni için özel etki alanlarını uygulamalarını aynı iç ve dış URL'leri olmasını kullanamazsınız, kiracılar içindir. Bu özelliği etkinleştirmek için önce olup [Azure AD uygulama proxy'si özel etki alanlarında](active-directory-application-proxy-custom-domains.md) sizin için çalışabilir.
>
>Veya uygulama yapılandırmak gerekirse bağlantısıyla çeviri SharePoint bkz [SharePoint 2013 için diğer erişim eşleşmeleri yapılandırma](https://technet.microsoft.com/library/cc263208.aspx) eşleme bağlantıları için başka bir yaklaşım için.

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
- Bağlantılar HTML, CSS ve JavaScript içinde değil. 
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

Bu özellik, uygulamalarınız için iş yapmak için Yardım istiyoruz. Biz 30 etiketleri HTML ve CSS arayın ve hangi desteklemek için JavaScript durumlarda değerlendiriyorsanız. Çevrildiğini olmayan oluşturulan bağlantıları örneği varsa, bir kod parçacığı göndermeniz [uygulama Proxy geri bildirim](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si ile özel etki alanları kullanabilirsiniz](active-directory-application-proxy-custom-domains.md) aynı iç ve dış URL sağlamak için

[SharePoint 2013 için diğer erişim eşleşmeleri yapılandırın](https://technet.microsoft.com/library/cc263208.aspx)
