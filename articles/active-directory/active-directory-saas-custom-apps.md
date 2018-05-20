---
title: Azure AD SSO uygulamaları için yapılandırma | Microsoft Docs
description: Bilgi edinmek için Self Servis SAML ve parola tabanlı SSO kullanılarak Azure Active Directory'ye uygulamalara nasıl bağlanacağını
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/20/2018
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5316cca9d3c944735355d7912a0f1e044c585001
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma
Bu makalede, Azure Active Directory Uygulama galerisinde mevcut uygulamalar için çoklu oturum açma yapılandırmak Yöneticiler sağlayan bir özellik hakkındadır *kod yazma olmadan*. Bu özellik 18 Kasım 2015 üzerinde technical preview sürümünden yayımlanmıştır ve dahil [Azure Active Directory Premium](active-directory-whatis.md). Bunun yerine özel uygulama kodu aracılığıyla Azure AD ile tümleştirme hakkında Geliştirici Kılavuzu görmek istiyorsanız bkz [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).

Azure Active Directory Uygulama galerisinde Azure Active Directory ile çoklu oturum açma biçimi desteklediği açıklandığı gibi bilinen uygulamaların bir listesini sağlar [bu makalede](manage-apps/what-is-single-sign-on.md). Siz (bir BT uzmanı veya sistem Tümleştirici kuruluşunuzdaki) bağlanmak istediğiniz uygulama bulduktan sonra çoklu oturum açmayı etkinleştirmek için Azure portalında sunulan adım adım yönergeleri izleyerek başlayabiliriz.

Müşterilerle [Azure Active Directory Premium](active-directory-whatis.md) lisans de bu ek özellikler alın:

* SAML 2.0 kimlik sağlayıcısı (SP tarafından başlatılan veya IDP başlatılan) destekleyen herhangi bir uygulama Self Servis tümleştirilmesi
* Kullanarak bir HTML tabanlı oturum açma sayfasına sahip herhangi bir web uygulaması Self Servis tümleştirilmesi [parola tabanlı SSO](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Self Servis bağlantı kullanıcı sağlamak için SCIM'yi protokolünü kullanan uygulamaların ([burada açıklanan](active-directory-scim-provisioning.md))
* Herhangi bir uygulama için bağlantılar ekleme yeteneği [Office 365 uygulama Başlatıcı](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users)

Bu yalnızca kullanan ancak henüz Azure AD uygulama galerisinde dahil edilmiş olmayan SaaS uygulamaları içerebilir, ancak Bulut veya şirket içi kontrol sunucularına kuruluşunuz tarafından dağıtılan üçüncü taraf web uygulamaları.

Bu özellikleri olarak da bilinen, *uygulama tümleştirmesi şablonları*, SAML, SCIM'yi ya da form tabanlı kimlik doğrulamasını destekleyen uygulamalar için standartlara dayalı bağlantı noktaları sağlar ve esnek seçeneklerini ve ayarlarını içerir Uygulama geniş sayısı ile uyumluluk. 

## <a name="adding-an-unlisted-application"></a>Listede bulunmayan bir uygulama ekleme
Bir uygulama tümleştirme şablonu kullanarak bir uygulama bağlanmak için Azure Active Directory yönetici hesabını kullanarak Azure portalında oturum açın ve gözatın **Active Directory > Kurumsal uygulamalar > Yeni Uygulama > Galeri olmayan uygulama** bölümünde, select **Ekle**ve ardından **Galeriden bir uygulama eklemek**.

  ![][1]

Uygulama galerisinde seçerek listede bulunmayan bir uygulama ekleyebilirsiniz **olmayan galeri uygulama** istenen uygulamanızı bulunamazsa, arama sonuçlarında gösterilen döşeme. Uygulamanız için bir ad girdikten sonra çoklu oturum açma seçenekleri ve davranışını yapılandırabilirsiniz. 

**Hızlı İpucu**: uygulama zaten uygulama galerisinde olup olmadığını denetlemek için en iyi uygulama, arama işlevini kullanın. Uygulama bulundu ve "çoklu oturum açma" açıklamasını değinmektedir, sonra uygulama zaten Federasyon çoklu oturum açma için desteklenir.

  ![][2]

Bu şekilde bir uygulama ekleme önceden tümleştirilmiş uygulama için kullanılabilen bir çok benzer bir deneyim sağlar. Başlatmak için **yapılandırma çoklu oturum açma**. Sonraki ekranda aşağıdaki bölümlerde açıklanan çoklu oturum açma, yapılandırmak için aşağıdaki üç seçenek sunar.

  ![][3]

## <a name="saml-based-sign-on"></a>SAML oturum açma tabanlı
Uygulama için SAML tabanlı kimlik doğrulaması yapılandırmak için bu seçeneği seçin. Bu uygulama desteği SAML 2.0 ve bilgi devam etmeden önce uygulamayı SAML özelliklerini kullanmak nasıl toplamanız gerekir gerektirir. Seçtikten sonra **sonraki**, uygulama için SAML uç noktaları karşılık gelen üç farklı URL'ler girmeniz istenir.

  ![][4]

Bunlar:

* **Oturum üzerinde URL'si (SP tarafından başlatılan yalnızca)** – bu uygulamaya oturum açmak için kullanıcının gider burada. Bir kullanıcı bu URL'yi gittiğinde, hizmet sağlayıcısı gerekli yeniden yönlendirme kimlik doğrulaması ve kullanıcı oturum için Azure ad ne yapacağını sonra uygulama hizmet sağlayıcı tarafından başlatılan çoklu oturum açma, gerçekleştirmek için yapılandırılmışsa. Bu alan doldurulursa, Azure AD Office 365 ve Azure AD erişim paneli uygulamayı başlatmak için bu URL'yi kullanır. Bu alan atlanmış sonra Azure AD yerine kimlik sağlayıcısı gerçekleştirmek-oturum açma, Azure AD erişim paneli Office 365 veya Azure AD çoklu oturum açma URL'si (Pano sekmesinden copyable) uygulama başlatıldığında başlatılır.
* **Veren URL'si** -veren URL'si için çoklu oturum açma yapılandırılmış uygulama benzersizce tanımlamalıdır. Bu Azure AD uygulaması olarak geri gönderdiği değerdir **İzleyici** SAML belirteci ve uygulama parametresinin doğrulamak beklenir. Bu değer olarak da görünür **varlık kimliği** uygulama tarafından sağlanan herhangi bir SAML meta veri içinde. Varlık kimliği veya İzleyici değeri ne olduğu hakkında bilgi için uygulamanın SAML belgelerine bakın. Hedef kitle URL uygulamaya döndürülen SAML belirteci görünme örneği aşağıdadır:
  
  ```
  <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
    ```

* **Yanıt URL'si** -burada SAML belirteci almak uygulama bekler yanıt URL'dir. Bu ayrıca olarak adlandırılır **onaylama tüketici Hizmeti'ni (ACS) URL**. Kendi SAML belirteci yanıt URL veya ACS URL nedir ile ilgili ayrıntılar için uygulamanın SAML belgelerine bakın.
  Bunlar girdikten sonra tıklatın **sonraki** sonraki ekrana devam etmek için. Bu ekran Azure AD'den SAML belirteci kabul etmek üzere etkinleştirmek için uygulama taraftaki yapılandırılması gerekenler hakkında bilgi sağlar.

  * Tıklayın **Lıtware yapılandırma**.
  
    ![][5]

Hangi değerlerin zorunludur uygulamaya bağlı olarak farklılık gösterir, böylece Ayrıntılar için uygulamanın SAML belgelerine bakın. **Oturum açma** ve **oturum kapatma** hizmet hem de çözmek için Azure AD Örneğinize SAML istek işleme uç noktadır aynı uç URL'si. Veren URL'si "uygulamaya verilen veren" SAML belirteci içinde görünen değerdir. 

Uygulamanızı yapılandırıldıktan sonra tıklatın **sonraki** düğmesine ve ardından **tam** iletişim kutusunu kapatmak için. 

## <a name="assigning-users-and-groups-to-your-saml-application"></a>Kullanıcılar ve gruplar SAML uygulamanıza atama
Ardından, uygulamanızın Azure AD SAML tabanlı kimlik sağlayıcısı olarak kullanmak üzere yapılandırıldıktan sonra test neredeyse hazırsınız. Güvenlik denetimi olarak Azure AD, Azure AD kullanarak erişim verildi sürece uygulamasına oturum vermeden bir belirteç yayınlamayacaktır. Kullanıcılar, doğrudan veya bir üyesi olan bir grubu aracılığıyla erişim verilebilir. 

Bir kullanıcı veya grup uygulamanıza atamak için tıklatın **Kullanıcıları Ata** düğmesi. Kullanıcı veya atayın ve ardından istediğiniz grubu seçin **atamak** düğmesi.

  ![][6]

Bir kullanıcı atamak için kullanıcı yanı sıra bu uygulamayı kullanıcının erişim panelinde görünmesi için bir kutucuğa neden bir belirteç vermek Azure AD izin verir. Kullanıcı Office 365 kullanıyorsa, bir uygulama kutucuğu Office 365 uygulama Başlatıcı de görüntülenir. 

Kullanarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **Yapılandır** uygulama sekmesinde. 

### <a name="customizing-the-claims-issued-in-the-saml-token"></a>SAML belirtecinde verilen talepler özelleştiriliyor
Kullanıcı uygulamaya doğruladığında, Azure AD bilgileri (veya talep) içeren uygulamaya bir SAML belirtecine benzersiz olarak tanımlayan kullanıcı hakkında gönderin. Varsayılan olarak bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir. 

Görüntülemek veya SAML belirteci altındaki uygulamaya gönderilen talep Düzenle **öznitelikleri** sekmesi.

  ![][7]

Neden ihtiyacınız olabilecek SAML belirtecinde verilen talepler düzenlemek için iki olası nedeni vardır: •Hedef uygulama yazıldı talep URI'ler farklı kümesi gerektiren veya talep değerleri •Your uygulama dağıtılan NameIdentifier talep gerektirir şekilde Azure Active Directory'de depolanan kullanıcı adı (AKA kullanıcı asıl adı) dışında bir şey olabilir. 

Ekleme ve bu senaryolar için talepleri düzenleme hakkında daha fazla bilgi için bu denetleyin [talep özelleştirme makale](active-directory-saml-claims-customization.md). 

### <a name="testing-the-saml-application"></a>SAML uygulamayı test etme
SAML URL'leri ve sertifika Azure AD ve uygulama yapılandırıldıktan sonra kullanıcılar veya gruplar Azure, uygulamaya atanan ve talepleri gözden ve gerekirse düzenlenebilir, sonra kullanıcı uygulamayı imzalamak hazırdır. 

Test etmek için yalnızca oturum-içine Azure AD erişim paneli adresindeki https://myapps.microsoft.com uygulamaya atanmış bir kullanıcı hesabı kullanarak ve uygulamanın çoklu oturum açma kapatma işlemini kazandırın kutucuğa tıklayın. Alternatif olarak, doğrudan uygulama ve oturum açma için oturum açma URL'si buradan gözatabilirsiniz. 

Hata ayıklama ipuçları için bkz [nasıl SAML tabanlı çoklu oturum açma uygulamalarda hata ayıklamak makalede](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Parola çoklu oturum açma
Yapılandırmak için bu seçeneği belirleyin [parola tabanlı çoklu oturum açma](manage-apps/what-is-single-sign-on.md) oturum açma bir HTML sayfası olduğu bir web uygulaması. SSO, parola tabanlı ya da gibi kullanıcı erişimi ve Kimlik Federasyonu Desteği web uygulamalarına parolaları yönetmek vaulting, parola sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak için gereken yere senaryoları için de yararlıdır. 

Seçtikten sonra **sonraki**, uygulamanın web tabanlı oturum açma sayfasının URL'sini girmeniz istenir. Bu kullanıcı adı ve parola giriş alanları içeren sayfa olması gerektiğini unutmayın. Girilen sonra Azure AD oturum açma sayfasına Giriş bir kullanıcı adı ve parola girişi için ayrıştırmak için bir işlem başlatır. İşlem başarılı olmazsa, sonra da, el ile alanları yakalamanıza olanak sağlayacak bir tarayıcı uzantısı (Internet Explorer, Chrome veya Firefox gerektirir) yükleme bir alternatif işleminde size rehberlik eder.

Oturum açma sayfası yakalandıktan sonra kullanıcılar ve gruplar atanabilir ve kimlik bilgisi ilkeleri yalnızca normal gibi ayarlanabilir [parola SSO uygulamaları](manage-apps/what-is-single-sign-on.md).

Not: Kullanılarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **yapılandırma** uygulama sekmesinde. 

## <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma
Kuruluşunuzun Azure AD erişim paneli veya Office 365 portalında bir uygulamaya bir bağlantı eklemek için bu seçeneği belirleyin. Bu Azure Active Directory Federasyon Hizmetleri (veya başka bir Federasyon Hizmeti) şu anda kullandığınız özel web uygulamaları için bağlantı eklemek için kimlik doğrulaması için Azure AD yerine kullanabilirsiniz. Veya belirli SharePoint sayfaları veya yalnızca kullanıcı erişimi panoları görünmesini istediğiniz diğer web sayfaları için ayrıntılı bağlantılar ekleyebilirsiniz. 

Seçtikten sonra **sonraki**, bağlantı sağlamak için uygulama URL'sini girmeniz istenir. Tamamlandığında, kullanıcılar ve gruplar görünmesi ve uygulamanın neden uygulamaya atanabilir [Office 365 uygulama Başlatıcı](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) olan kullanıcılar için.

Not: Kullanılarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **yapılandırma** uygulama sekmesinde.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştirme](active-directory-saml-claims-customization.md)
* [Sorun giderme SAML tabanlı çoklu oturum açma](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png