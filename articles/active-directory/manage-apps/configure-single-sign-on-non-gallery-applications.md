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
ms.date: 01/08/2019
ms.author: mimart
ms.reviewer: asmalser,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f55de2d081f12f1bddf7e68a795133c4b394823
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65824997"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-microsoft-identity-platform"></a>Galeri dışı uygulamalar için çoklu oturum açma Microsoft kimlik platformu yapılandırın.

Bu makalede, çoklu oturum açma uygulamaları yok için Microsoft kimlik platformu app Galerisi'nde yapılandırma olanağı sağlayan bir özellik hakkındadır *kod yazmadan*.

Bunun yerine özel uygulamalar kod aracılığıyla Azure AD ile tümleştirme hakkında Geliştirici Kılavuzu için arıyorsanız, bkz. [Azure AD için kimlik doğrulama senaryoları](../develop/authentication-scenarios.md).

Microsoft kimlik platformu uygulama galerisinde bir formu Microsoft kimlik platformu ile çoklu oturum açmayı destekleyecek şekilde açıklandığı gibi bilinen uygulamaların bir listesini sağlar [bu makalede](what-is-single-sign-on.md). (Bir BT uzmanı veya sistem entegratörü, kuruluşunuzdaki) gibi bağlanmak istediğiniz uygulamayı bulduğunuzda, çoklu oturum açmayı etkinleştirmek için Azure Portalı'nda sunulan adım adım yönergeleri izleyerek başlayabilirsiniz.

Bu özellikler ayrıca, lisans sözleşmenize göre kullanılabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/).

- Self Servis tümleştirme gibi modern bir protokolünü kullanan bir uygulamanın [Openıd Connect/OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) , kullanıcıların kimliklerini doğrulamak ve belirteçleri almak için [Microsoft Graph](https://graph.microsoft.com).
- SAML 2.0 kimlik sağlayıcısı (SP tarafından başlatılan veya IDP tarafından başlatılan) destekleyen herhangi bir uygulamanın Self Servis tümleştirme
- Self Servis tümleştirme kullanarak bir HTML tabanlı oturum açma sayfası olan herhangi bir web uygulaması [parola tabanlı SSO](what-is-single-sign-on.md#password-based-sso)
- Self Servis bağlantı kullanıcı sağlama için SCIM'yi protokolünü kullanan uygulamalar ([burada açıklanan](use-scim-to-provision-users-and-groups.md))
- Herhangi bir uygulama için bağlantılar ekleme yeteneği [Office 365 uygulama başlatıcısında](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](what-is-single-sign-on.md#linked-sign-on)

Bu yalnızca kullanan ancak henüz için Azure AD uygulama Galerisi konaklarınızda yok SaaS uygulamaları içerebilir, ancak kuruluşunuzun Bulut veya şirket içi denetim sunucuları dağıtılan üçüncü taraf web uygulamaları.

Olarak da bilinen bu yeteneklerin *uygulama tümleştirme şablonları*, SAML, SCM veya form tabanlı kimlik doğrulamasını destekleyen uygulamalar için standartlara dayalı bağlantı noktaları belirtin ve esnek seçenekler ve ayarlar için içerir uygulamaların kapsamlı bir dizi ile uyumluluk.

## <a name="adding-an-unlisted-application"></a>Listede bulunmayan bir uygulama eklendiğinde

Microsoft Identity platform uygulamaları kaydetmek için iki mekanizma sağlar.

Modern bir protokol gibi kullanan bir uygulamayı [Openıd Connect/OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) kullanarak kayıtlı, kullanıcıların kimliklerini doğrulamak için [uygulama kayıt portalı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-register-an-app).

Diğer tüm türleri kullanan uygulamaların kayıt [desteklenen kimlik doğrulama mekanizmaları](what-is-single-sign-on.md) gibi [SAML](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol) protokolünü kullan **kurumsal uygulamalar** dikey penceresine kendileri ile Microsoft kimlik platformu bağlanın.

Bir uygulama tümleştirme şablonu kullanarak bir uygulamaya bağlanmak için Microsoft kimlik platformu yönetici hesabınızı kullanarak Azure portalında oturum açın. Gözat **Active Directory > Kurumsal uygulamalar > Yeni Uygulama > galeri dışı uygulama** bölümünden **Ekle**, ardından **Galeriden bir uygulama ekleyin** .

![Uygulama ekleme](./media/configure-single-sign-on-non-gallery-applications/customapp1.png)

App Galerisi'nde listelenmeyen bir uygulamayı seçerek ekleyebilirsiniz **galeri dışı uygulama** istenen uygulama bulunamazsa, arama sonuçlarında gösterilen bir kutucuk. Uygulamanız için bir ad girdikten sonra çoklu oturum açma seçenekleri ve davranışını yapılandırabilirsiniz.

> [!TIP]
> En iyi uygulama, uygulama galerisinde bulunan uygulama zaten var olup olmadığını kontrol etmek için arama işlevini kullanın. Uygulama bulunamadı ve çoklu oturum açma açıklamasını bahsetmeleri, ardından uygulamayı zaten Federasyon çoklu oturum açma için desteklenir.

![Arama](./media/configure-single-sign-on-non-gallery-applications/customapp2.png)

Bu şekilde bir uygulama eklendiğinde, önceden tümleştirilmiş uygulamalar için kullanılabilir bir benzer bir deneyim sağlar. Başlamak için seçim **yapılandırma çoklu oturum açma** veya tıkladığınızda **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde. Sonraki ekranda, çoklu oturum açmayı yapılandırma seçeneklerini sunar. Seçenekler, bu makalenin sonraki bölümlerinde açıklanmıştır.
  
![Yapılandırma seçenekleri](./media/configure-single-sign-on-non-gallery-applications/customapp3.png)

## <a name="saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma

Uygulama SAML tabanlı kimlik doğrulamasını yapılandırmak için bu seçeneği belirleyin. Bu, uygulamanın SAML 2.0 desteği gerektirir. Devam etmeden önce uygulamanın SAML işlevlerini nasıl kullanacağınız hakkında bilgi toplamak. Uygulama ve Azure AD arasında çoklu oturum açmayı yapılandırmak için aşağıdaki bölümleri tamamlayın.

### <a name="enter-basic-saml-configuration"></a>Temel bir SAML yapılandırma girin

Azure ad kurulumu için temel bir SAML yapılandırma girin. El ile değerleri girin veya alanın değerini ayıklamak için bir meta veri dosyasını karşıya yükleyin.

  ![Litware etki alanı ve URL'ler](./media/configure-single-sign-on-non-gallery-applications/customapp4.png)

- **Üzerinde oturum URL'si (SP tarafından başlatılan yalnızca)** – burada kullanıcının bu uygulamada oturum gider. Bir kullanıcı bu URL'ye gittiğinde, hizmet sağlayıcısı gerekli yönlendirme Azure AD kullanıcısıyla oturum açma ve kimlik doğrulamasını yapmak için ne yapacağını sonra uygulama hizmet sağlayıcısı tarafından başlatılan çoklu oturum açma, gerçekleştirmek için yapılandırılmışsa. Bu alan doldurulursa, Azure AD Office 365 ve Azure AD erişim paneli uygulamasını başlatmak için bu URL'yi kullanır. Bu alan atlanırsa sonra Azure AD kimlik sağlayıcısı yerine gerçekleştirecek-oturum açma (Pano sekmesinden kopyalanabilir) Azure AD çoklu oturum açma URL'si veya Office 365, Azure AD erişim paneli uygulama başlatıldığında başlattı.
- **Tanımlayıcı** -uygulama için çoklu oturum açma yapılandırılmış benzersiz şekilde tanımlamalıdır. Bu değer uygulama tarafından gönderilen AuthRequest (SAML isteği) veren öğesi olarak bulabilirsiniz. Bu değer de görünür **varlık kimliği** uygulama tarafından sağlanan herhangi bir SAML meta veri içinde. Varlık kimliği veya hedef kitle değeri nedir ilişkin ayrıntılar için uygulamanın SAML belgelerine bakın. 

    Azure AD uygulama tarafından gönderilen SAML isteğindeki veren ve tanımlayıcı nasıl görüneceğini gösteren bir örnek verilmiştir:

    ```XML
    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>
    ```

- **Yanıt URL'si** -burada SAML belirteci almak uygulamanın beklediği yanıt URL'dir. Bu onaylama tüketici hizmeti (ACS) URL'si olarak da adlandırılır. Kendi SAML belirteci yanıt URL'si veya ACS URL'si nedir ilişkin ayrıntılar için uygulamanın SAML belgelerine bakın. 

    Birden çok replyURLs yapılandırmak için aşağıdaki PowerShell betiğini kullanabilirsiniz.

    ```powershell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Daha fazla bilgi için [Microsoft kimlik Platformu (Azure AD) destekleyen SAML 2.0 kimlik doğrulama istekleri ve yanıtları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML)


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Gözden geçirmek veya SAML belirtecinde verilen talepleri özelleştirme

Bir kullanıcı uygulama kimliğini doğruladığında, Azure AD bilgileri (veya talep) içeren uygulamaya bir SAML belirteç benzersiz olarak tanımlayan bir kullanıcı hakkında sorun. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir. 

Görüntüleyebilir veya altındaki uygulamaya SAML belirtecindeki gönderilen talep Düzenle **öznitelikleri** sekmesi.

  ![Öznitelikler](./media/configure-single-sign-on-non-gallery-applications/customapp7.png)

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir neden iki nedeni vardır:

- Uygulamayı farklı bir URI'leri talep kümesi gerektirir veya talep değerleri hedefine yazıldı.
- Uygulamanızı Microsoft kimlik platformu içinde depolanan kullanıcı adı (diğer bir deyişle, kullanıcı asıl adı) dışında bir şey olacak şekilde NameIdentifier talebini gerektirdiği şekilde dağıtıldı.

Daha fazla bilgi için [kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](./../develop/../develop/active-directory-saml-claims-customization.md). 

### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Gözden geçirme sertifika sona erme verilerini, durum ve e-posta bildirimi

Bir galeride veya galeri dışı bir uygulama oluşturduğunuzda, Azure AD, oluşturma tarihinden itibaren 3 yıllık bir sona erme tarihi olan bir uygulamaya özgü sertifika oluşturur. Azure AD arasında güven ayarlamak için bu sertifika gerekir ve uygulama. Sertifika biçimi hakkında daha fazla bilgi için uygulamanın SAML belgelerine bakın. 

Azure AD'den sertifikanın Base64 veya ham biçiminde indirebilirsiniz. Ayrıca, sertifika uygulama meta veri XML dosyasını indirme veya uygulama Federasyon meta verileri URL'sini kullanarak elde edebilirsiniz.

  ![Sertifika](./media/configure-single-sign-on-non-gallery-applications/certificate.png)

Sertifikaya sahip olduğunu doğrulayın:

- İstenen sona erme tarihi. En fazla 3 yıl için sona erme tarihi yapılandırabilirsiniz.
- Etkin durumu. Etkin olmayan durum ise, durum etkin olarak değiştirin. Durumu değiştirmek için denetleme **etkin** ve yapılandırmayı kaydedin. 
- Doğru bildirim e-postası. Etkin sertifika sona erme tarihi olduğunda, Azure AD, bu alanda yapılandırılan e-posta adresine bir bildirim gönderir.  

Daha fazla bilgi için [Federasyon çoklu oturum açma, Microsoft kimlik platformu için sertifikaları Yönet](manage-certificates-for-federated-single-sign-on.md).

### <a name="set-up-target-application"></a>Hedef uygulama ayarlama

Uygulama için çoklu oturum açmayı yapılandırmak için uygulamanın belgeleri bulun. Belgeleri bulmak için SAML tabanlı oturum açma yapılandırması sayfanın sonuna kaydırın ve ardından **yapılandırma \<uygulama adı >**. 

Gereken değerleri uygulamaya göre değişir. Ayrıntılar için uygulamanın SAML belgelerine bakın. Oturum açma ve oturum kapatma hizmeti URL'si hem de Azure AD Örneğiniz için SAML isteği işleme uç noktadır aynı uç çözümleyin. SAML varlık kimliği uygulamaya verilen SAML belirteci veren olarak görünen değerdir.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Kullanıcılar ve gruplar SAML uygulamanıza atayın

Ardından, uygulamanızın bir SAML tabanlı kimlik sağlayıcısı olarak Azure AD'yi kullanacak şekilde yapılandırıldıktan sonra test etmek neredeyse hazırsınız. Güvenlik denetimi gibi Azure AD, Azure AD kullanıcı için erişim vermedikçe uygulamasına oturum açmasına izin vererek bir belirteç yayınlamayacaktır. Kullanıcılar, doğrudan veya bir grup üyeliği aracılığıyla erişim verilebilir. 

Uygulamanız için bir kullanıcı veya grup atamak için tıklatın **kullanıcıları atama** düğmesi. Kullanıcı veya grup atayın ve ardından istediğiniz seçin **atama** düğmesi.

  ![Kullanıcıları atama](./media/configure-single-sign-on-non-gallery-applications/customapp6.png)

Kullanıcı atama, kullanıcı için bir belirteç vermek Azure AD izin verir. Bu uygulama, kullanıcının erişim panelinde görünmesi için bir kutucuk neden olur. Kullanıcının Office 365 kullanıyorsa, uygulama kutucuğu de Office 365 uygulama başlatıcısında görünecek. 

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde. 


### <a name="test-the-saml-application"></a>SAML uygulamayı test etme

SAML uygulama test etmeden önce Azure AD ile uygulama ayarlayın ve gerekir kullanıcıları veya grupları uygulamaya atanmış. SAML uygulamayı test etmek için bkz: [SAML tabanlı çoklu oturum açma için Microsoft kimlik platformu uygulamalarında hata ayıklama](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Parola çoklu oturum açma

Yapılandırmak için bu seçeneği belirleyin [parola tabanlı çoklu oturum açma](what-is-single-sign-on.md) bir HTML oturum açma sayfası olan bir web uygulaması için. Parola tabanlı SSO de denir gibi kullanıcı erişim ve Kimlik Federasyonu Desteği web uygulamaları için parolaları yönetmek vaulting, parola sağlar. Burada, kuruluşunuzun sosyal medya uygulaması hesaplara gibi tek bir hesabı paylaşmak için birkaç kullanıcı gerektiği senaryolar için de yararlıdır. 

Seçtikten sonra **sonraki**, uygulamanın web tabanlı oturum açma sayfasının URL'sini girmeniz istenir. Bu kullanıcı adı ve parola giriş alanlarını içeren sayfasında olması gerektiğini unutmayın. Giriş yapıldığında, Azure AD oturum açma sayfasına Giriş bir kullanıcı adı ve parola girişi için ayrıştırmak için bir işlem başlatır. İşlem başarılı olmazsa, ardından bunu, el ile alanlarını Yakala sağlayacak tarayıcı uzantısı (Internet Explorer, Chrome ve Firefox gerektirir) yükleme bir alternatif sürecinde size yol gösterir.

Oturum açma sayfası yakalanan, kullanıcılar ve gruplar atanabilir ve kimlik bilgisi ilkeleri, yalnızca normal gibi ayarlanabilir sonra [parola SSO uygulamaları](what-is-single-sign-on.md).

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde. 
>

## <a name="existing-single-sign-on"></a>Varolan çoklu oturum açma
Uygulamanın, kuruluşunuzun Azure AD erişim paneli veya Office 365 portalına bir bağlantı eklemek için bu seçeneği belirleyin. Bu bağlantıları şu anda Active Directory Federasyon Hizmetleri (veya başka bir Federasyon Hizmeti) kullanan özel web Apps'e eklemek yerine Azure AD kimlik doğrulaması için kullanabilirsiniz. Veya belirli SharePoint sayfaları veya yalnızca kullanıcı erişim panellerinde görünmesi istiyorsanız diğer web sayfaları için ayrıntılı bağlantılar ekleyebilirsiniz. 

Seçtikten sonra **sonraki**, bağlamak için uygulamanın URL'sini girmeniz istenir. Tamamlandığında, kullanıcıları ve grupları görünen uygulamanın uygulama atanabilir [Office 365 uygulama başlatıcısında](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](end-user-experiences.md) yönetmeyecek olan kullanıcılar için.

> [!NOTE] 
> Kullanarak uygulama için bir kutucuk logosu karşıya yüklediğiniz **karşıya Logo** düğmesini **yapılandırma** uygulama için sekmesinde. 
>

## <a name="related-articles"></a>İlgili makaleler

- [Önceden tümleştirilmiş uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md)
- [SAML tabanlı çoklu oturum açma sorunlarını giderme](../develop/howto-v1-debug-saml-sso-issues.md)
- [Microsoft kimlik Platformu (Azure Active Directory geliştiricileri için](https://aka.ms/aaddev)
