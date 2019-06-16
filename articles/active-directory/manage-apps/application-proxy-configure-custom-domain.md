---
title: Azure AD uygulama proxy'sinde özel etki alanları | Microsoft Docs
description: Böylece uygulama URL'sini kullanıcılarınız, eriştiği ne olursa olsun aynı olan Azure AD uygulama proxy'sinde özel etki alanlarını yönetin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bae13de156d502cdd731005d460641ca452448d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108669"
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Azure AD uygulama proxy'sinde özel etki alanları ile çalışma

Azure Active Directory Uygulama proxy'si aracılığıyla uygulama yayımladığınızda, kullanıcılarınız uzaktan çalışırken gitmek için bir dış URL oluşturun. Varsayılan etki alanı bu URL'yi alır *yourtenant.msappproxy.net*. Örneğin, yayımladığınız uygulama adlı masrafları ve kiracınız Contoso adlı, sonra dış URL şu şekilde olacaktır `https://expenses-contoso.msappproxy.net`. Kendi etki alanı adınızı kullanmak istiyorsanız, uygulamanız için özel bir etki alanı yapılandırın. 

Mümkün olduğunda, uygulamalarınız için özel etki alanları ayarlamanızı öneririz. Özel etki alanları avantajlarından bazıları şunlardır:

- İçinde veya dışında ağınıza çalışıyor olmanızdan kullanıcılarınızın uygulama ile aynı URL'yi elde edebilirsiniz.
- Tüm uygulamalar aynı iç ve dış URL'ler varsa, başka bir işaret eden bir uygulama bağlantılar, kurumsal ağ dışından bile çalışmaya devam. 
- Markanızın denetlemek ve istediğiniz URL'leri oluşturun. 


## <a name="configure-a-custom-domain"></a>Özel etki alanı yapılandırma

### <a name="prerequisites"></a>Önkoşullar

Özel bir etki alanını yapılandırmadan önce hazırlanmış aşağıdaki gereksinimlere sahip olduğunuzdan emin olun: 
- A [doğrulanmamış etki alanını Azure Active Directory'ye eklenen](../fundamentals/add-custom-domain.md).
- Bir PFX dosyası biçiminde etki alanı için özel bir sertifika. 
- Şirket içi uygulama [uygulama proxy'si aracılığıyla yayımlandığından](application-proxy-add-on-premises-application.md).

### <a name="configure-your-custom-domain"></a>Özel etki alanınızı yapılandırın

Bu üç gereksinimleri hazır olduğunda, özel etki alanı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** ve yönetmek istediğiniz uygulamayı seçin.
3. Seçin **uygulama proxy'si**. 
4. Dış URL alanına özel etki alanınızı seçmek için açılan listeyi kullanın. Listede etki alanınızı görmüyorsanız, ardından da henüz doğrulanmış edilmemiş. 
5. Seçin **Kaydet**
5. **Sertifika** olur devre dışı bırakılmış bir alan etkin. Bu alanı seçin. 

   ![Bir sertifikayı karşıya yüklemek için tıklayın](./media/application-proxy-configure-custom-domain/certificate.png)

   Bu etki alanı için bir sertifika zaten karşıya sertifika alanı sertifika bilgilerini görüntüler. 

6. PFX sertifikasını karşıya yükleyin ve sertifikanın parolasını girin. 
7. Seçin **Kaydet** yaptığınız değişiklikleri kaydedin. 
8. Ekleme bir [DNS kaydı](../../dns/dns-operations-recordsets-portal.md) yeni dış URL msappproxy.net etki alanına yeniden yönlendirir.
9. DNS kaydı doğru şekilde kullanarak yapılandırıldığından emin olun [nslookup](https://social.technet.microsoft.com/wiki/contents/articles/29184.nslookup-for-beginners.aspx) , dış URL erişilebilir olduğundan ve diğer ad olarak msapproxy.net etki alanı gösterilir görmek için komutu.

>[!TIP] 
>Özel etki alanı başına bir sertifikayı karşıya yüklemek yeterlidir. Sertifika karşıya yükledikten sonra yeni bir uygulama yayımlama ve DNS kaydı dışında ek yapılandırma gerekmez, özel etki alanını seçebilirsiniz. 

## <a name="manage-certificates"></a>Sertifikaları yönetme

### <a name="certificate-format"></a>Sertifika biçimi
Sertifika imza yöntemler konusunda bir kısıtlama yoktur. Tüm Eliptik Eğri Şifrelemesi (ECC), konu alternatif adı (SAN) ve diğer ortak sertifika türleri desteklenir. 

İstenen dış URL'yi joker karakter eşleşmesi şartıyla, bir joker sertifikası kullanabilirsiniz.

Kendi ortak anahtar altyapısı (PKI) nedeniyle güvenlik konuları tarafından verilen bir sertifika kullanamazsınız.

### <a name="changing-the-domain"></a>Etki alanını değiştirme
Tüm doğrulanmış etki alanları, uygulamanız için dış URL aşağı açılan listede görünür. Etki alanını değiştirmek için bu alan için uygulamayı güncelleştirmeniz yeterlidir. İstediğiniz etki alanının listesinde değilse [doğrulanmış bir etki alanı ekleme](../fundamentals/add-custom-domain.md). İlişkili bir sertifikanız henüz, sertifika eklemek için 5-7 adımları bir etki alanı seçtiğinizde. Ardından, yeni harici URL'den yönlendirmek için DNS kaydı güncelleştirdiğinizden emin olun. 

### <a name="certificate-management"></a>Sertifika yönetimi
Bir dış konak uygulamaları paylaşmak sürece birden çok uygulama için aynı sertifikayı kullanabilirsiniz. 

Bir sertifikanın süresi dolduğunda, portal üzerinden başka bir sertifikayı karşıya yüklemek için bildiren bir uyarı alırsınız. Sertifika iptal edilirse, kullanıcılarınızın uygulamaya erişirken bir güvenlik uyarısı görebilirsiniz. Biz için sertifikalar iptal denetimlerini yerine getirmiyor.  Belirli bir uygulama için bir sertifikayı güncelleştirmek için uygulamaya gidin ve özel etki alanlarını yapılandırma yeni sertifikayı karşıya yüklemek için yayımlanan uygulamalar için 5-7 adımları izleyin. Eski sertifika diğer uygulamalar tarafından kullanılmayan, otomatik olarak silinir. 

Şu anda sertifikalar ilgili uygulamalar bağlamında yönetmeniz gereken tüm sertifika yönetimi tek tek uygulama sayfaları olduğundan. 

## <a name="next-steps"></a>Sonraki adımlar
* [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md) yayımlanan uygulamalarınıza Azure AD kimlik doğrulamasıyla.
* [Koşullu erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md) , yayımlanan uygulamalar için.
* [Özel etki alanı adınızı Azure AD'ye ekleme](../fundamentals/add-custom-domain.md)


