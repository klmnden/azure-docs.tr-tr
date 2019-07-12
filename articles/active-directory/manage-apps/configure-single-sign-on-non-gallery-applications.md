---
title: Çoklu oturum açma - galeri dışı uygulamalar - Microsoft kimlik platformu | Microsoft Docs
description: Microsoft kimlik Platformu (Azure AD) içinde galeri dışı uygulamalar için çoklu oturum açma (SSO) yapılandırma
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: a72cb7bc7feeba984d568a0465d4f23a494496e8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807644"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-microsoft-identity-platform"></a>Galeri dışı uygulamalar için çoklu oturum açma Microsoft kimlik platformu yapılandırın.

Bu makalede, Microsoft kimlik platformu app Galerisi'nde eksik uygulamalar için çoklu oturum açmayı yapılandırmak Yöneticiler sağlayan bir özellik hakkındadır *kod yazmadan*.

Bunun yerine özel uygulamalar kod aracılığıyla Azure AD ile tümleştirme hakkında Geliştirici Kılavuzu için arıyorsanız, bkz. [Azure AD için kimlik doğrulama senaryoları](../develop/authentication-scenarios.md).

Microsoft kimlik platformu uygulama galerisinde bir formu Microsoft kimlik platformu ile çoklu oturum açmayı destekleyecek şekilde açıklandığı gibi bilinen uygulamaların bir listesini sağlar [bu makalede](what-is-single-sign-on.md). (Bir BT uzmanı veya sistem entegratörü, kuruluşunuzdaki) gibi bağlanmak istediğiniz uygulamayı bulduğunuzda, çoklu oturum açmayı etkinleştirmek için Azure Portalı'nda sunulan adım adım yönergeleri izleyerek başlayabilirsiniz.

Aşağıdaki özellikleri de, lisans sözleşmenize göre sunulur. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/).

- Self Servis tümleştirme gibi modern bir protokolünü kullanan bir uygulamanın [Openıd Connect/OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) , kullanıcıların kimliklerini doğrulamak ve belirteçleri almak için [Microsoft Graph](https://graph.microsoft.com).
- Self Servis tümleştirme destekleyen herhangi bir uygulamanın [güvenlik onaylama işlemi biçimlendirme dili (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) kimlik sağlayıcısı (SP tarafından başlatılan veya IDP tarafından başlatılan)
- Self Servis tümleştirme kullanarak bir HTML tabanlı oturum açma sayfası olan herhangi bir web uygulaması [parola tabanlı SSO](what-is-single-sign-on.md#password-based-sso)
- Self Servis bağlantı kullanan uygulamaların [sistemi için kullanıcı sağlama için etki alanları arası Kimlik Yönetimi (SCIM) Protokolü](use-scim-to-provision-users-and-groups.md)
- Herhangi bir uygulama için bağlantılar ekleme yeteneği [Office 365 uygulama başlatıcısında](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](what-is-single-sign-on.md#linked-sign-on)

Hiç kimse eklenen Azure AD uygulama Galerisi henüz uygulamaya sahip olsa bile bu özellikler, kullandığınız hizmet (SaaS) uygulaması olarak Self Servis tümleştirme yazılım içerebilir. Self Servis tümleştirme Bulut veya şirket içi denetim sunucularına kuruluşunuz tarafından dağıtılan üçüncü taraf web uygulamasının başka bir özelliktir.

Olarak da bilinen *uygulama tümleştirme şablonları*, bu özellikler, SAML, SCM veya form tabanlı kimlik doğrulamasını destekleyen uygulamalar için standartlara dayalı bağlantı noktaları sağlar. Esnek seçenekler ve ayarlar uygulamalarını geniş kapsamlı bir dizi ile uyumluluk için özellikleri içerir.

## <a name="adding-an-unlisted-application"></a>Listede bulunmayan bir uygulama eklendiğinde

Microsoft kimlik platformu uygulamaları kaydetmek için iki mekanizma sağlar.

Modern bir protokol gibi kullanan bir uygulamayı [Openıd Connect/OAuth](../develop/active-directory-v2-protocols.md) kullanarak kayıtlı, kullanıcıların kimliklerini doğrulamak için [uygulama kayıt portalı](../develop/quickstart-register-app.md).

Diğer tüm türleri kullanarak uygulamaları kaydetmek için [desteklenen kimlik doğrulama mekanizmaları](what-is-single-sign-on.md)gibi [SAML](../develop/single-sign-on-saml-protocol.md) protokolü, kullanın **kurumsal uygulamalar** bağlanmak için dikey penceresi Microsoft kimlik platformu kendileriyle.

Bir uygulama tümleştirme şablonu kullanarak listelenmemiş uygulamaya bağlanmak için bu adımları uygulayın:

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/) Microsoft kimlik platformu yönetici hesabınızı kullanarak.
1. Seçin **kurumsal uygulamalar** > **yeni uygulama**.
1. (İsteğe bağlı ancak önerilir) İçinde **Galeriden Ekle** arama kutusuna, uygulamanın görünen adı girin. Uygulama arama sonuçlarında görüntülenirse, seçin ve bu yordamın geri kalanını atlayın.
1. Seçin **galeri dışı uygulama**. **Kendi uygulamanızı ekleyin** sayfası görüntülenir.

   ![Kendi uygulama Sayfası Ekle gösterir](./media/configure-single-sign-on-non-gallery-applications/add-your-own-application.png)

1. Yeni uygulamanızın görünen adı girin.
1. **Add (Ekle)** seçeneğini belirleyin.

Bu şekilde uygulamaya eklediğinizde, önceden tümleştirilmiş uygulamalar için kullanılabilir bir benzer bir deneyim sağlar. İlk seçin **çoklu oturum açma** uygulamanın kenar çubuğundan. Sonraki sayfaya (**tek bir oturum açma yönteminizi seçmeniz**) SSO yapılandırma seçeneklerini sunar:

- **SAML**
- **Parola tabanlı**
- **Bağlı**

![Bir çoklu oturum açma yöntemi sayfası seçin gösterir](./media/configure-single-sign-on-non-gallery-applications/select-a-single-sign-on-method.png)

Bu seçenekler hakkında daha fazla bilgi için bu makalenin aşağıdaki bölümlere bakın.

## <a name="saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma

Seçin **SAML** SAML tabanlı kimlik doğrulaması için uygulama yapılandırma seçeneği. (Bu seçenek, uygulamanın SAML 2.0 desteği gerektirir.) **Yukarı çoklu oturum açma SAML ile ayarlanmış** sayfası görüntülenir.

![Çoklu oturum açma SAML sayfası ile Ayarla gösterir](./media/configure-single-sign-on-non-gallery-applications/set-up-single-sign-on-with-saml.png)

Bu sayfa, beş farklı başlıkları var:

| Başlık numarası | Başlık adı | Bu başlık bir özeti için bkz: |
| --- | --- | --- |
| 1\. | **Temel bir SAML yapılandırma** | [Temel bir SAML yapılandırma girin](#enter-basic-saml-configuration) |
| 2 | **Kullanıcı öznitelikleri ve talepler** | [Gözden geçirmek veya SAML belirtecinde verilen talepleri özelleştirme](#review-or-customize-the-claims-issued-in-the-saml-token) |
| 3 | **SAML imzalama sertifikası** | [Gözden geçirme sertifika sona erme verilerini, durum ve e-posta bildirimi](#review-certificate-expiration-data-status-and-email-notification) |
| 4 | **Ayarlanan \<uygulama adı >** | [Hedef uygulama ayarlama](#set-up-target-application) |
| 5 | **Çoklu oturum açma ile test \<uygulama adı >** | [SAML uygulamayı test etme](#test-the-saml-application) |

Artık devam etmeden önce uygulamanın SAML işlevlerini nasıl kullanacağınız hakkında bilgi toplayın. Uygulama ve Azure AD arasında SSO yapılandırmak için aşağıdaki bölümleri tamamlayın.

### <a name="enter-basic-saml-configuration"></a>Temel bir SAML yapılandırma girin

Azure ad kurulumu için şuraya gidin: **temel SAML yapılandırma** başlık ve seçin, **Düzenle** simgesi (Kalem). El ile değerleri girin veya alanın değerini ayıklamak için bir meta veri dosyasını karşıya yükleyin.

![Temel bir SAML yapılandırma sayfasını gösterir](./media/configure-single-sign-on-non-gallery-applications/basic-saml-configuration.png)

Aşağıdaki iki alan gereklidir:

- **Tanımlayıcı**. Bu değer, benzersiz bir şekilde çoklu oturum açma için yapılandırılmış uygulama tanımlamanız gerekir. Bu değer olarak bulabilirsiniz **veren** öğesinde **AuthnRequest** (SAML isteği), uygulama tarafından gönderilen. Bu değer de görünür **varlık kimliği** uygulama tarafından sağlanan herhangi bir SAML meta veri içinde. Ne hakkında ayrıntılı bilgi için uygulamanın SAML belgelere bakın, **varlık kimliği** veya **İzleyici** değerdir.

  Aşağıdaki kodda gösterildiği nasıl **tanımlayıcı** veya **veren** uygulamayı Azure AD'ye gönderir SAML isteğinde görünür:

  ```xml
  <samlp:AuthnRequest
  xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
  ID="id6c1c178c166d486687be4aaf5e482730"
  Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
  </samlp:AuthnRequest>
  ```

- **Yanıt URL'si**. Yanıt URL'si, SAML belirteci almak burada uygulama bekliyor. ' dir. Bu URL, onay belgesi tüketici hizmeti (ACS) URL'si da bilinir. Kendi SAML belirteci yanıt URL'si veya ACS URL'si nedir ilişkin ayrıntılar için uygulamanın SAML belgelerine bakın.

  Birden çok yanıt URL'leri yapılandırmak için aşağıdaki PowerShell betiğini kullanabilirsiniz.

  ```powershell
  $sp = Get-AzureADServicePrincipal -SearchString "<Exact app name>"
  $app = Get-AzureADApplication -SearchString "<Exact app name>"
  $urllist = New-Object "System.Collections.Generic.List[String]"
  $urllist.Add("<reply URL 1>")
  $urllist.Add("<reply URL 2>")
  $urllist.Add("<reply URL 3>")
  Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls $urllist
  Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls $urllist
  ```

Aşağıdaki üç alanı isteğe bağlıdır:

- **Oturum açma URL'si (SP tarafından başlatılan yalnızca)** . Bu değer, burada kullanıcının bu uygulamada oturum gider gösterir. SP tarafından başlatılan SSO'yu uygulamanın gerçekleştirdiği, bir kullanıcı bu URL'ye gittiğinde ardından SP gerekli yönlendirme Azure AD kullanıcısıyla oturum açma ve kimlik doğrulamasını yapmak için yapar. Bu alan belirtilmesi, Azure AD, Office 365 ve Azure AD erişim paneli uygulamayı başlatmak için bu URL'yi kullanır. Bu alan atlarsanız, Azure AD yerine IDP tarafından başlatılan oturum açmayı Office 365, Azure AD erişim paneli ya da Azure AD SSO URL'sini uygulama başlatma sırasında gerçekleştirir (hangi, kopyalayabilirsiniz **Pano** sayfası).

- **Geçiş durumu**. SAML kimlik doğrulamasından sonra kullanıcıların yönlendirileceği uygulama istemek için geçiş durumu belirtebilirsiniz. Değer bir URL veya URL genellikle olan kullanıcılar, uygulama içinde belirli bir konuma alan yolu.

- **Oturum kapatma URL'si**. Bu değer, uygulamaya SAML oturum kapatma yanıt göndermek için kullanılır.

Daha fazla bilgi için [çoklu oturum açma SAML Protokolü](../develop/single-sign-on-saml-protocol.md).

### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Gözden geçirmek veya SAML belirtecinde verilen talepleri özelleştirme

Bir kullanıcı uygulama kimliğini doğruladığında, Azure AD uygulama SAML belirteci bilgilerini (veya talep) ile benzersiz olarak tanımlayan bir kullanıcı hakkında verir. Varsayılan olarak, bu bilgiler kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir.

Görüntülemek veya uygulamaya SAML belirtecindeki gönderilen talepleri düzenlemek için:

- Git **kullanıcı öznitelikleri ve talepler** seçin ve başlık **Düzenle** simgesi. **Kullanıcı öznitelikleri ve talepler** sayfası görüntülenir.

![Kullanıcı öznitelikleri ve talepler sayfasını gösterir](./media/configure-single-sign-on-non-gallery-applications/user-attributes-and-claims.png)

İki nedenden dolayı SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir:

- Farklı uygulama gerektiriyorsa URI'ler talep veya değerleri.
- Uygulamanızın gerektirdiği **ad tanımlayıcı değeri** Microsoft kimlik platformu içinde depolanan kullanıcı adı (diğer adıyla kullanıcı asıl adı) dışında bir şey olacak talep.

Daha fazla bilgi için [nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md).

### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Gözden geçirme sertifika sona erme verilerini, durum ve e-posta bildirimi

Bir galeride veya galeri dışı bir uygulama oluşturduğunuzda, Azure AD'ye üç yıl, oluşturma tarihinden itibaren süresi dolar ve uygulama özel bir sertifika oluşturur. Azure AD arasında güven ayarlamak için bu sertifika gerekir ve uygulama. Sertifika biçimi hakkında daha fazla bilgi için uygulamanın SAML belgelerine bakın.

Azure AD'den etkin Sertifikayı Base64 veya ham biçiminde ana doğrudan indirebileceğiniz **yukarı çoklu oturum açma SAML ile ayarlayın** sayfası. Alternatif olarak, etkin sertifikayı uygulama meta veri XML dosyasını indirme veya uygulama Federasyon meta verileri URL'sini kullanarak elde edebilirsiniz.

Görüntüleme, oluşturma veya sertifikalarınızı (etkin veya devre dışı) indirmek için Git **SAML imzalama sertifikası** seçin ve başlık **Düzenle** simgesi. **SAML imzalama sertifikası** görünür.

![SAML imzalama sertifikası sayfasını gösterir](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)

Sertifikaya sahip olduğunu doğrulayın:

- *İstenen sona erme tarihi.* Üç yıla kadar sona erme tarihini geleceğe yapılandırabilirsiniz.
- *İstenen sertifika için etkin bir durumu.* Durum ise **Inactive**, durumunu değiştir **etkin**. Durumu değiştirmek için istenen sertifikanın satır sağ tıklayıp **sertifika etkin hale getirin**.
- *Algoritma ve doğru imzalama seçeneği.*
- *Doğru bildirim e-posta adresi.* Etkin sertifika sona erme tarihi olduğunda, Azure AD, bu alanda yapılandırılan e-posta adresine bir bildirim gönderir.  

Daha fazla bilgi için [Federasyon çoklu oturum açma için sertifikaları Yönet](manage-certificates-for-federated-single-sign-on.md) ve [gelişmiş sertifika imzalama seçenekleri SAML belirtecindeki](certificate-signing-options.md).

### <a name="set-up-target-application"></a>Hedef uygulama ayarlama

SSO için uygulamayı yapılandırmak için uygulamanın belgeleri bulun. Belgeleri bulmak için Git **ayarlanan \<uygulama adı >** seçin ve başlık **görüntülemek hakkında adım adım yönergeler**. Belgeleri görünür **yapılandırma oturum açma** sayfası. Bu sayfayı dolduran içinde kılavuzları **oturum açma URL'si**, **Azure AD tanımlayıcısı**, ve **oturum kapatma URL'si** değerler **ayarlanan \<uygulama adı >** başlığı.

Gereken değerleri uygulamaya göre değişir. Ayrıntılar için uygulamanın SAML belgelerine bakın. **Oturum açma URL'si** ve **oturum kapatma URL'si** hem çözümlemek için Azure AD örneğiniz SAML isteği işleme uç noktadır aynı uç değerleri. **Azure AD tanımlayıcısı** değeri **veren** SAML belirtecinde verilen uygulamaya.

### <a name="assign-users-and-groups-to-your-saml-application"></a>Kullanıcılar ve gruplar SAML uygulamanıza atayın

Uygulamanızı bir SAML tabanlı kimlik sağlayıcısı olarak Azure AD'yi kullanacak şekilde yapılandırdıktan sonra test etmek neredeyse hazırsınız. Bir güvenlik denetimi olarak Azure AD, yalnızca Azure AD, kullanıcıya erişim izni vermiştir, uygulamaya oturum açmasına izin vererek bir belirteç verir. Kullanıcılar, doğrudan veya bir grup üyeliği aracılığıyla erişim elde.

Uygulamanız için yeni bir kullanıcı veya grup atamak için:

1. Uygulama Kenar çubuğunda seçin **kullanıcılar ve gruplar**. **\<Uygulama adı >-Kullanıcılar ve gruplar** sayfası görüntülenirse, atanan kullanıcılar ve gruplar geçerli listesini gösterir.
1. Seçin **kullanıcı ekleme**. **Atamaları Ekle** sayfası görüntülenir.
1. Seçin **kullanıcılar ve gruplar (\<numarası > Seçili)** . **Kullanıcılar ve gruplar** sayfası görüntülenirse, mevcut kullanıcıları ve grupları listesini gösteren.
1. Kullanıcı veya Grup listesinden atamak istediğiniz bulmak için tür veya kaydırma.
1. Her bir kullanıcı veya grubu ekleyin ve ardından istediğiniz seçin **seçin** düğmesi. **Kullanıcılar ve gruplar** sayfası kaybolur.
1. İçinde **atamaları Ekle** sayfasında **atama**. **\<Uygulama adı >-Kullanıcılar ve gruplar** listesinde gösterilen ek kullanıcılar sayfası görüntülenir.

   ![Uygulama kullanıcıları ve grupları sayfasını gösterir](./media/configure-single-sign-on-non-gallery-applications/application-users-and-groups.png)

Bu listeden şunları yapabilirsiniz:

- Bir kullanıcıyı kaldırın.
- Kendi rolleri düzenleyin.
- Böylece kullanıcı kullanıcının erişim paneli içinden uygulamada kimlik doğrulaması gerçekleştirebilir (kullanıcı adı ve parola) kimlik bilgilerini güncelleştirin.

Düzen ya da aynı anda birden çok kullanıcıları veya grupları kaldırın.

Kullanıcı atama, kullanıcı bir belirteç vermek için Azure AD'ye verir. Bu uygulama, kullanıcının erişim panelinde görünmesi için bir kutucuk neden olur. Kullanıcının Office 365 kullanılıyorsa bir uygulama kutucuk, ayrıca Office 365 uygulama başlatıcısında görünür.

> [!NOTE]
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde.

### <a name="test-the-saml-application"></a>SAML uygulamayı test etme

SAML uygulamayı test etme önce gerekir zaten Azure AD ile uygulama ayarlayın ve kullanıcıları veya grupları uygulamaya atanmış. SAML uygulamayı test etmek için seçin **çoklu oturum açma**, size döndüren **SAML tabanlı oturum açma** sayfası. (Farklı bir SSO yöntemi etkili olduysa, seçin **değiştirme tek oturum açma modları** > **SAML** çok.) Ardından **ile çoklu oturum açmayı Test \<uygulama adı >** başlığı seçin **Test**. Daha fazla bilgi için [hata ayıklama SAML tabanlı çoklu oturum açma için Azure Active Directory'de uygulamalar](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Parola çoklu oturum açma

Yapılandırmak için bu seçeneği belirleyin [parola tabanlı çoklu oturum açma](what-is-single-sign-on.md) bir HTML oturum açma sayfası ile bir web uygulaması için. Parola tabanlı SSO de denir gibi kullanıcı erişim ve Kimlik Federasyonu Desteği web uygulamaları için parolaları yönetmek vaulting, parola sağlar. Burada, kuruluşunuzun sosyal medya uygulaması hesaplara gibi tek bir hesabı paylaşmak için birkaç kullanıcı gerektiği senaryolar için de yararlıdır.

Seçtikten sonra **parola tabanlı**, uygulamanın web tabanlı oturum açma sayfasının URL'sini girmeniz istenir.

![Oturum açma URL'sini girin ve oturum açma URL'si sayfasına gösterir](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

Ardından bu adımları uygulayın:

1. URL'yi girin. Bu dize, kullanıcı adı giriş alanını içeren sayfasında olmalıdır.
1. **Kaydet**’i seçin. Azure AD oturum açma sayfasına Giriş bir kullanıcı adı ve parola girişi için ayrıştırmak çalışır.
1. Azure AD girişimi başarısız ayrıştırma, seçin **yapılandırma \<uygulama adı > Parola çoklu oturum açma ayarları** görüntülenecek **yapılandırma oturum açma** sayfası. (Deneme başarılı olursa, bu yordamın kalan kısmını atlayabilirsiniz.)
1. Seçin **oturum açma alanlarını el ile algılama**. Oturum açma alanlarının el ile algılama ek yönergeler görünür.

   ![Parola tabanlı çoklu oturum açma, el ile yapılandırma](./media/configure-single-sign-on-non-gallery-applications/password-configure-sign-on.png)

1. Seçin **oturum açma alanlarını Yakala**. Bir yakalama durumu sayfası iletisini gösteren yeni bir sekmede açılır **meta veri yakalama şu anda devam eden**.
1. Varsa **erişim paneli uzantısı için gereken** kutusu yeni bir sekmede görünür seçin **Şimdi Yükle** yüklemek için **My Apps güvenli oturum açma uzantısı** tarayıcı uzantısı. (Tarayıcı uzantısı, Microsoft Edge, Chrome ve Firefox gerektirir.) Yüklemeyi, başlatma, uzantıyı etkinleştirmek ve yakalama durumu sayfayı yenileyin.

   Tarayıcı uzantısı ardından girilen URL görüntüler başka bir sekme açılır.

1. Girilen URL ile bir sekmede, oturum açma sürecinden geçer. Kullanıcı adı ve parola alanları doldurun ve oturum açmayı deneyin. (Doğru parolayı sağlamanız gerekmez.)

   Bir komut istemi yakalanan oturum açma alanlarını kaydetmenizi ister.

1. **Tamam**’ı seçin. Sekme kapatır, tarayıcı uzantısı iletinin yakalama durumu sayfasını güncelleştirir **uygulaması için meta verileri güncelleştirildi**ve ayrıca kapanır sekmesinde bu tarayıcı.
1. Azure AD'de **yapılandırma oturum açma** sayfasında **Tamam, ı uygulamada başarıyla oturum açabildi**.
1. **Tamam**’ı seçin.

Oturum açma sayfası yakalama sonra kullanıcılar ve gruplar atayabilir ve normal gibi kimlik bilgisi ilkeleri ayarlayabilirsiniz [parola SSO uygulamaları](what-is-single-sign-on.md).

> [!NOTE]
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde.

## <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma

Uygulama, kuruluşunuzun Azure AD erişim paneli veya Office 365 portalındaki bir bağlantı eklemek için bu seçeneği belirleyin. Azure AD kimlik doğrulaması yerine bağlantılar şu anda Active Directory Federasyon Hizmetleri (veya başka bir Federasyon Hizmeti) kullanan özel web uygulamalarınıza eklemek için bu yöntemi kullanabilirsiniz. Veya belirli SharePoint sayfaları veya yalnızca kullanıcı erişim panellerinde görünmesi istiyorsanız diğer web sayfaları için ayrıntılı bağlantılar ekleyebilirsiniz.

Seçtikten sonra **bağlı**, bağlamak için uygulamanın URL'sini girmeniz istenir. URL'yi yazın ve seçin **Kaydet**. Uygulamanın görünen uygulama kullanıcıları ve grupları atayabilir [Office 365 uygulama başlatıcısında](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](end-user-experiences.md) yönetmeyecek olan kullanıcılar için.

> [!NOTE]
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde.

## <a name="related-articles"></a>İlgili makaleler

- [Nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md)
- [Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama](../develop/howto-v1-debug-saml-sso-issues.md)
- [Microsoft kimlik Platformu (eski adıyla Azure Active Directory geliştiriciler için)](../develop/index.yml)
