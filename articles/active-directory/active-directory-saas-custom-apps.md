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
ms.openlocfilehash: 96f6e2cc9b137de48a1d186739a8c76e2f1d18c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34590129"
---
# <a name="configure-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Çoklu oturum açma Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için yapılandırma

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
Bir uygulama tümleştirme şablonu kullanarak bir uygulamaya bağlanmak için Azure Active Directory yönetici hesabı kullanarak Azure portalında oturum açın. Göz atın **Active Directory > Kurumsal uygulamalar > Yeni Uygulama > olmayan galeri uygulama** bölümünde, select **Ekle**ve ardından **Galeriden bir uygulama ekleme** .

  ![](./media/active-directory-saas-custom-apps/customapp1.png)

Uygulama galerisinde seçerek listede bulunmayan bir uygulama ekleyebilirsiniz **olmayan galeri uygulama** istenen uygulamanızı bulunamazsa, arama sonuçlarında gösterilen döşeme. Uygulamanız için bir ad girdikten sonra çoklu oturum açma seçenekleri ve davranışını yapılandırabilirsiniz. 

**Hızlı İpucu**: uygulama zaten uygulama galerisinde olup olmadığını denetlemek için en iyi uygulama, arama işlevini kullanın. Uygulama bulundu ve çoklu oturum açma açıklamasını değinmektedir, sonra uygulama zaten Federasyon çoklu oturum açma için desteklenir.

  ![](./media/active-directory-saas-custom-apps/customapp2.png)

Bu şekilde bir uygulama ekleme önceden tümleştirilmiş uygulama için kullanılabilen bir benzer bir deneyim sağlar. Başlatmak için **yapılandırma çoklu oturum açma** veya tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde. Sonraki ekranda, çoklu oturum açma yapılandırma seçeneklerini gösterir. Seçenekler, bu makalenin sonraki bölümlerinde açıklanmıştır.
  
![](./media/active-directory-saas-custom-apps/customapp3.png)

## <a name="saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma
Uygulama için SAML tabanlı kimlik doğrulaması yapılandırmak için bu seçeneği seçin. Bu uygulama SAML 2.0 desteği gerektirir. Devam etmeden önce uygulamayı SAML özelliklerini kullanma hakkında bilgi toplamanız gerekir. Çoklu oturum açma uygulama ile Azure AD arasındaki yapılandırmak için aşağıdaki bölümleri tamamlayın.

### <a name="enter-basic-saml-configuration"></a>Temel SAML yapılandırması girin

Azure AD ayarlamak için temel SAML yapılandırması girin. El ile değerleri girin veya alanın değerini ayıklamak için bir meta veri dosyasını karşıya yükleyin.

  ![Lıtware etki alanı ve URL'leri](./media/active-directory-saas-custom-apps/customapp4.png)

- **Oturum üzerinde URL'si (SP tarafından başlatılan yalnızca)** – bu uygulamaya oturum açmak için kullanıcının gider burada. Bir kullanıcı bu URL'yi gittiğinde, hizmet sağlayıcısı gerekli yeniden yönlendirme kimlik doğrulaması ve kullanıcı oturum için Azure ad ne yapacağını sonra uygulama hizmet sağlayıcı tarafından başlatılan çoklu oturum açma, gerçekleştirmek için yapılandırılmışsa. Bu alan doldurulursa, Azure AD Office 365 ve Azure AD erişim paneli uygulamayı başlatmak için bu URL'yi kullanır. Bu alan atlanmış sonra Azure AD yerine kimlik sağlayıcısı gerçekleştirmek-oturum açma, Azure AD erişim paneli Office 365 veya Azure AD çoklu oturum açma URL'si (Pano sekmesinden copyable) uygulama başlatıldığında başlatılır.
- **Tanımlayıcı** -için çoklu oturum açma yapılandırılmış uygulama benzersizce tanımlamalıdır. Uygulama tarafından gönderilen bu değeri (SAML isteğinde) AuthRequest veren öğe olarak bulabilirsiniz. Bu değer olarak da görünür **varlık kimliği** uygulama tarafından sağlanan herhangi bir SAML meta veri içinde. Varlık kimliği veya İzleyici değeri ne olduğu hakkında bilgi için uygulamanın SAML belgelerine bakın. 

    Azure ad uygulama tarafından gönderilen SAML isteğinde tanımlayıcısı veya sertifikayı verenin nasıl göründüğünü bir örnek verilmiştir:

    ```
    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>
    ```

- **Yanıt URL'si** -burada SAML belirteci almak uygulama bekler yanıt URL'dir. Bu aynı zamanda onaylama işlemi tüketici Hizmeti'ni (ACS) URL'si olarak adlandırılır. Kendi SAML belirteci yanıt URL veya ACS URL nedir ile ilgili ayrıntılar için uygulamanın SAML belgelerine bakın. 

    Birden çok replyURLs yapılandırmak için aşağıdaki PowerShell betiğini kullanabilirsiniz.

    ```PowerShell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Daha fazla bilgi için bkz: [Azure Active Directory (Azure AD) destekleyen SAML 2.0 kimlik doğrulama istekleri ve yanıtları](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML)


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>İnceleme veya SAML belirtecinde verilen talepler özelleştirme

Kullanıcı uygulamaya doğruladığında, Azure AD bilgileri (veya talep) içeren uygulamaya bir SAML belirtecine benzersiz olarak tanımlayan kullanıcı hakkında gönderin. Varsayılan olarak bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir. 

Görüntülemek veya SAML belirteci altındaki uygulamaya gönderilen talep Düzenle **öznitelikleri** sekmesi.

  ![](./media/active-directory-saas-custom-apps/customapp7.png)

Neden SAML belirtecinde verilen talepler düzenlemek için gerekebilecek iki nedeni vardır:

- Uygulama, bir talep URI'ler farklı kümesi gerektiren veya talep değerleri için yazılmış.
- Uygulamanız Azure Active Directory'de depolanan kullanıcı adı (AKA kullanıcı asıl adı) dışında bir şey olması NameIdentifier talep gerektiren bir şekilde dağıtıldı. 

Daha fazla bilgi için bkz: [kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor](./develop/active-directory-saml-claims-customization.md). 



### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Gözden geçirme sertifika sona erme veri, durum ve e-posta bildirimi

Bir galeri ya da olmayan galeri bir uygulama oluşturduğunuzda, Azure AD 3 yıl oluşturulma tarihi, sona erme tarihi olan bir uygulamaya özgü sertifika oluşturacak. Azure AD arasında güven ayarlamak için bu sertifikaya gereksinim ve uygulama. Sertifika biçimi hakkında daha fazla bilgi için uygulamanın SAML belgelerine bakın. 

Azure AD'den Base64 veya ham biçiminde sertifika yükleyebilirsiniz. Ayrıca, uygulama meta verileri XML dosyası yükleyerek veya uygulama Federasyon meta veri URL'sini kullanarak sertifika edinebilirsiniz.

  ![Sertifika](./media/active-directory-saas-custom-apps/certificate.png)

Sertifika olduğunu doğrulayın:

- İstenen sona erme tarihi. En fazla 3 yıl sona erme tarihini yapılandırabilirsiniz.
- Etkin durumu. Durum Etkin değilse, durum etkin olarak değiştirin. Durumu değiştirmek için denetleme **etkin** ve yapılandırmayı kaydedin. 
- Doğru bildirim e-postası. Etkin sertifikanın sona erme tarihini olduğunda, Azure AD Bu alanda yapılandırılan e-posta adresine bir bildirim gönderir.  

Daha fazla bilgi için bkz: [yönetmek Federasyon tek oturum açma için Azure Active Directory'de sertifikaları](active-directory-sso-certs.md).

### <a name="set-up-target-application"></a>Hedef uygulama ayarlama

Uygulama için çoklu oturum açma yapılandırmak için uygulamanın belgelerine bulun. Belgeleri bulmak için SAML tabanlı oturum açma yapılandırma sayfası sonuna kaydırın ve tıklayın **yapılandırma <application name>** . 

Gereken değerleri uygulamanın göre farklılık gösterir. Ayrıntılar için uygulamanın SAML belgelerine bakın. Oturum açma ve oturum kapatma hizmeti URL'si her ikisi de Azure AD Örneğiniz için SAML istek işleme uç noktadır aynı uç çözümleyin. SAML varlık kimliği veren uygulamaya verilen SAML belirtecinde görünen değerdir.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Kullanıcılar ve gruplar SAML uygulamanıza atayın

Ardından, uygulamanızın Azure AD SAML tabanlı kimlik sağlayıcısı olarak kullanmak üzere yapılandırıldıktan sonra test neredeyse hazırsınız. Güvenlik denetimi olarak Azure AD, bir Azure AD kullanıcıya erişim verildi sürece uygulamasına oturum arkasından bir belirteç yayınlamayacaktır. Kullanıcılar, doğrudan veya bir grup üyeliği aracılığıyla erişim verilebilir. 

Bir kullanıcı veya grup uygulamanıza atamak için tıklatın **Kullanıcıları Ata** düğmesi. Kullanıcı veya atayın ve ardından istediğiniz grubu seçin **atamak** düğmesi.

  ![](./media/active-directory-saas-custom-apps/customapp6.png)

Bir kullanıcı atama kullanıcı için bir belirteç vermek Azure AD izin verir. Bu uygulamayı kullanıcının erişim panelinde görünmesi için bir kutucuğa neden olur. Kullanıcı Office 365 kullanıyorsa, bir uygulama kutucuğu Office 365 uygulama Başlatıcı de görüntülenir. 

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **Yapılandır** uygulama sekmesinde. 


### <a name="test-the-saml-application"></a>SAML uygulamayı test etme

SAML uygulama test etmeden önce Azure AD ile uygulama ayarlama ve gerekir kullanıcılar veya gruplar uygulamaya atanan.

  ![Test Etme](./media/active-directory-saas-custom-apps/testing.png)

Çoklu oturum açma sayfasından tıklayın **Test SAML ayarları** etki alanı ve URL'ler bölümünün altında. Bu uygulamayı test etme hakkında yönergeler içeren bir içerik bölmesi açılır.

1. Uygulama için oturum açın. Uygulama hizmeti sağlayıcısı tarafından başlatılan çoklu oturum açma yapılandırılmışsa, burada oturum açma işlemi başlatabilirsiniz çoklu oturum açma URL'sine yönlendirilir. Uygulama kimlik sağlayıcısı tarafından başlatılan çoklu oturum açma yapılandırılmışsa, daha sonra uygulamaya açan.
2.  Oturum açma şirket sayfanızda herhangi bir hata görürseniz, hata kopyalayın ve Azure AD çoklu oturum açma içerik bölmesinde test geri dönün. Hata kutusuna yapıştırın ve tıklayın **alma çözüm adımları**. Hata uygulamanın sayfasında ise, uygulamanın satıcısına başvurun ve yapılandırmanızı değerleri doğrulamak için Azure AD temel paylaşmak gerekir. 
3.  Hataya göre Azure AD sorunun nasıl çözümleneceği hakkında belirli adımlar sağlar.

Daha fazla bilgi için bkz: [SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-saml-debugging/?WT.mc_id=DMC_AAD_?WT.mc_id=UI_AAD_Configure_NonGalleryApps)


## <a name="password-single-sign-on"></a>Parola çoklu oturum açma

Yapılandırmak için bu seçeneği belirleyin [parola tabanlı çoklu oturum açma](manage-apps/what-is-single-sign-on.md) oturum açma bir HTML sayfası olduğu bir web uygulaması. SSO, parola tabanlı ya da gibi kullanıcı erişimi ve Kimlik Federasyonu Desteği web uygulamalarına parolaları yönetmek vaulting, parola sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak için gereken yere senaryoları için de yararlıdır. 

Seçtikten sonra **sonraki**, uygulamanın web tabanlı oturum açma sayfasının URL'sini girmeniz istenir. Bu kullanıcı adı ve parola giriş alanları içeren sayfa olması gerektiğini unutmayın. Girilen sonra Azure AD oturum açma sayfasına Giriş bir kullanıcı adı ve parola girişi için ayrıştırmak için bir işlem başlatır. İşlem başarılı olmazsa, sonra da, el ile alanları yakalamanıza olanak sağlayacak bir tarayıcı uzantısı (Internet Explorer, Chrome veya Firefox gerektirir) yükleme bir alternatif işleminde size rehberlik eder.

Oturum açma sayfası yakalandıktan sonra kullanıcılar ve gruplar atanabilir ve kimlik bilgisi ilkeleri yalnızca normal gibi ayarlanabilir [parola SSO uygulamaları](manage-apps/what-is-single-sign-on.md).

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **Yapılandır** uygulama sekmesinde. 
>

## <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma
Kuruluşunuzun Azure AD erişim paneli veya Office 365 portalında bir uygulamaya bir bağlantı eklemek için bu seçeneği belirleyin. Bu Azure Active Directory Federasyon Hizmetleri (veya başka bir Federasyon Hizmeti) şu anda kullandığınız özel web uygulamaları için bağlantı eklemek için kimlik doğrulaması için Azure AD yerine kullanabilirsiniz. Veya belirli SharePoint sayfaları veya yalnızca kullanıcı erişimi panoları görünmesini istediğiniz diğer web sayfaları için ayrıntılı bağlantılar ekleyebilirsiniz. 

Seçtikten sonra **sonraki**, bağlantı sağlamak için uygulama URL'sini girmeniz istenir. Tamamlandığında, kullanıcılar ve gruplar görünmesi ve uygulamanın neden uygulamaya atanabilir [Office 365 uygulama Başlatıcı](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) olan kullanıcılar için.

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu yükleyebilirsiniz **karşıya logosu** düğmesini **Yapılandır** uygulama sekmesinde. 
>

## <a name="related-articles"></a>İlgili makaleler

- [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
- [Önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştirme](active-directory-saml-claims-customization.md)
- [Sorun giderme SAML tabanlı çoklu oturum açma](active-directory-saml-debugging.md)

