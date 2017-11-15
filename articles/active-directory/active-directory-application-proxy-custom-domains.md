---
title: "Azure AD uygulama proxy'si özel etki alanlarında | Microsoft Docs"
description: "Uygulama için URL'yi, kullanıcılarınıza eriştiği bakılmaksızın aynı böylece Azure AD uygulama proxy'si özel etki alanlarında yönetin."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: f254f4959c66aeb3eec522f31e0d8a6780358188
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si özel etki alanları ile çalışma

Azure Active Directory Uygulama proxy'si aracılığıyla uygulama yayımladığınızda, kullanıcılarınız uzaktan çalışırken gitmek için bir dış URL oluşturun. Varsayılan etki alanını bu URL'yi alır *yourtenant.msappproxy.net*. Gider ve Kiracı adlı bir uygulama yayımladıysanız dış URL'yi https://expenses-contoso.msappproxy.net olacaktır sonra Örneğin, Contoso adlı. Kendi etki alanı adınızı kullanmak istiyorsanız, uygulamanız için özel bir etki alanı yapılandırın. 

Mümkün olduğunda, uygulamalarınız için özel etki alanları ayarlamanızı öneririz. Özel etki alanlarını avantajlarından bazıları şunlardır:

- İçinde veya ağınızın dışındaki çalıştıkları olup olmadığını, kullanıcılarınızın uygulamaya aynı URL'ye sahip alabilirsiniz.
- Tüm uygulamalar aynı iç ve dış URL'ler varsa, başka bir işaret eden bir uygulama bağlantılar, kurumsal ağ dışından bile çalışmaya devam. 
- Marka bilgilerinizi denetlemek ve istediğiniz URL'leri oluşturun. 


## <a name="configure-a-custom-domain"></a>Özel bir etki alanı yapılandırma

### <a name="prerequisites"></a>Önkoşullar

Özel bir etki alanı yapılandırmadan önce hazırlanan aşağıdaki gereksinimlere sahip olduğundan emin olun: 
- A [Azure Active Directory'ye eklenen etki alanını doğruladıysanız](active-directory-domains-add-azure-portal.md).
- Bir PFX dosyası biçiminde etki alanı için özel bir sertifika. 
- Bir şirket içi uygulama [uygulaması Proxy üzerinden yayımlanan](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Özel etki alanınızı yapılandırın

Bu üç gereksinimleri hazır olduğunda, özel etki alanı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** ve yönetmek istediğiniz uygulamayı seçin.
3. Seçin **uygulama proxy'si**. 
4. Dış URL alanında açılır listeden özel etki alanınızı seçmek için kullanın. Listede etki alanınızı görmüyorsanız, ardından bunu henüz doğrulanmış kurmadı. 
5. Seçin **Kaydet**
5. **Sertifika** hale devre dışı bırakıldı alan etkin. Bu alanı seçin. 

   ![Bir sertifikayı karşıya yüklemek için tıklayın](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Bu etki alanı için bir sertifika Karşıya zaten sertifika alanındaki sertifika bilgilerini görüntüler. 

6. PFX sertifikasını karşıya yükleyin ve sertifikanın parolasını girin. 
7. Seçin **kaydetmek** yaptığınız değişiklikleri kaydetmek için. 
8. Ekleme bir [DNS kaydı](../dns/dns-operations-recordsets-portal.md) yeni dış URL msappproxy.net etki alanına yeniden yönlendirir. 

>[!TIP] 
>Özel etki alanı başına bir sertifika karşıya yeterlidir. Bir sertifika karşıya yükledikten sonra yeni bir uygulama yayımlama ve DNS kaydı dışında ek yapılandırma gerekmez, özel etki alanı seçebilirsiniz. 

## <a name="manage-certificates"></a>Sertifikaları yönetme

### <a name="certificate-format"></a>Sertifika biçimi
Sertifika imza yöntemleri sınırlaması yoktur. Tüm Eliptik Eğri Şifrelemesi (ECC), konu alternatif adı (SAN) ve diğer ortak sertifika türleri desteklenir. 

İstenen dış URL'yi joker eşleştiği sürece bir joker sertifikası kullanabilirsiniz. 

### <a name="changing-the-domain"></a>Etki alanını değiştirme
Tüm doğrulanmış etki alanları, uygulamanız için dış URL'yi açılır listesinde görünür. Etki alanını değiştirmek için yalnızca bu alan uygulama için güncelleştirin. İstediğiniz etki alanının listede yoksa [doğrulanmış bir etki alanına eklemek](active-directory-domains-add-azure-portal.md). İlişkili bir sertifikanız henüz, sertifika eklemek için 5-7 adımları bir etki alanı seçerseniz. Ardından, yeni dış URL'yi yeniden yönlendirmek için DNS kaydı güncelleştirdiğinizden emin olun. 

### <a name="certificate-management"></a>Sertifika yönetimi
Bir dış ana bilgisayarda uygulamaları paylaşmak sürece birden çok uygulama için aynı sertifikayı kullanabilirsiniz. 

Bir sertifikanın süresi dolduğunda, portal üzerinden başka bir sertifikayı karşıya yüklemek için bildiren bir uyarı alın. Sertifika iptal edilirse, kullanıcılarınızın uygulama erişirken bir güvenlik uyarısı görebilirsiniz. Biz için sertifikalar iptal denetimlerini yerine getirmiyor.  Belirli bir uygulamada bir sertifikayı güncelleştirmek için uygulamaya gidin ve yeni sertifikayı karşıya yüklemek için yayımlanan uygulamalar özel etki alanlarını yapılandırma için 5-7 adımları izleyin. Eski sertifikayı diğer uygulamalar tarafından kullanılmadığından, otomatik olarak silinir. 

Şu anda sertifikalar ilgili uygulamalar bağlamında yönetmeniz gereken tüm sertifika yönetimi tek tek uygulama sayfaları olduğundan. 

## <a name="next-steps"></a>Sonraki adımlar
* [Çoklu oturum açmayı etkinleştir](active-directory-application-proxy-sso-using-kcd.md) yayımlanan uygulamalarınızı Azure AD kimlik doğrulamasına sahip.
* [Koşullu erişimi etkinleştirme](application-proxy-enable-remote-access-sharepoint.md) , yayımlanan uygulamalar için.
* [Azure AD ile özel etki alanı adınızı ekleme](active-directory-domains-add-azure-portal.md)


