---
title: Parola SSO için yapılandırılmış bir Azure AD galeri uygulaması için oturum açarken sorun | Microsoft Docs
description: Parola çoklu oturum açma için yapılandırılmış Azure AD galeri uygulaması ile ilgili sorunları giderme
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 742df882fb64e09ff63ef2eceb5514ca070dc227
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190316"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>Oturum açma sorunları ile SSO için yapılandırılmış bir Azure AD galeri uygulaması

Erişim paneli web tabanlı bir portaldır. Ancak, kullanıcılar Azure Active Directory (Azure AD) iş veya Okul hesapları için iznine sahip oldukları bulut tabanlı uygulamalara erişmek için etkinleştirir. Azure AD sürümleri olan kullanıcıların, Self Servis grup ve erişim paneli üzerinden uygulama yönetimi özellikleri de kullanabilirsiniz.

Erişim paneli, Azure Portalı'ndan ayrıdır. Kullanıcıların erişim paneli kullanmak için Azure aboneliği gerekmez.

Parola tabanlı çoklu oturum açma (SSO) erişim panelinde kullanmak için erişim paneli uzantısı tarayıcınızda yüklenmesi gerekir. Parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde uzantısı otomatik olarak indirir.

## <a name="browser-requirements-for-access-panel"></a>Erişim paneli tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi.

Aşağıdaki tarayıcılar, parola tabanlı SSO destekler:

- Internet Explorer 8, 9, 10 ve 11 Windows 7 veya üzeri

- Chrome Windows 7 ve daha sonra veya MacOS x veya sonrası

- Firefox 26,0 veya Windows XP SP2 veya üzeri veya Mac OS X 10.6 veya daha üstü

>[!NOTE]
>Parola tabanlı SSO uzantısı kullanılabilir hale gelir Microsoft Edge, Windows 10 için Microsoft Edge tarayıcı uzantıları eklenen için destek.

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükle

Şu adımları uygulayın:

1. Açık [erişim paneli](https://myapps.microsoft.com) Desteklenen tarayıcılar ve Azure AD'de bir kullanıcı olarak oturum açın.

2. Parola SSO etkin bir uygulama erişim panelinde seçin.

3. İstendiğinde, seçin **Şimdi Yükle**.

4. Tarayıcınıza bağlı bir indirme bağlantısı için yönlendirilirsiniz. Seçin **Ekle** tarayıcı uzantısı yüklenecek.

5. İstenirse, seçin **etkinleştirme** veya **izin**.

6. Yükleme işleminden sonra tarayıcınızı yeniden başlatın.

7.  Erişim paneli oturum açın ve parola SSO etkin uygulamalarınızı başlatırsanız bakın.

Ayrıca doğrudan uzantıları Chrome ve Firefox için bu bağlantıları indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir Grup İlkesi ayarlama

Uzaktan erişim paneli uzantısını Internet Explorer için kullanıcılarınızın makinelerde yüklemenize olanak sağlayan bir Grup İlkesi ayarlayabilirsiniz.

Önkoşullar şunlardır:

-   [Active Directory etki alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) , ayarlanmalıdır ve kullanıcılarınızın makineleri etki alanınıza katılmalıdır.

-   Grup İlkesi nesnesi (GPO) düzenleyin "ayarlarını Düzenle" izni var. Varsayılan olarak, şu güvenlik gruplarının üyeleri bu izne sahiptir: Etki alanı yöneticileri, kuruluş yöneticileri ve Grup İlkesi Oluşturucu Sahipleri. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Grup İlkesi yapılandırmak ve kullanıcılara dağıtmak için bkz: [Internet Explorer için Grup İlkesi kullanarak erişim panelinde uzantısını dağıtmak nasıl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy).

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Erişim paneli Internet Explorer'da sorunlarını giderme

Bir tanılama aracı ve uzantısını yapılandırmak üzere yönergeleri erişmek için bkz: [erişim paneli uzantısını Internet Explorer için sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Bir Azure AD galeri uygulaması için parola SSO yapılandırma

Bir Azure AD galeri uygulaması yapılandırmak için bu işlemleri yapmanız gerekir:

-   Azure AD galeri uygulaması ekleme
-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-app-for-password-sso)
-   [Uygulamaya kullanıcı atama](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>Azure AD galeri uygulaması ekleme

Şu adımları uygulayın:

1. Açık [Azure portalında](https://portal.azure.com) ve bir genel yönetici veya ortak yönetici olarak oturum

2. Seçin **tüm hizmetleri** Azure AD uzantısı'nı açmak için sol taraftaki gezinti bölmesinin üst.

3. Tür **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory**.

4. Seçin **kurumsal uygulamalar** Azure AD Gezinti bölmesinden.

5. Seçin **Ekle** sağ alt köşesindeki **kurumsal uygulamalar** bölmesi.

6. İçinde **Galeriden Ekle** bölümünde, bir uygulama adı yazın **bir ad girin** kutusu.

7. SSO için yapılandırmak istediğiniz uygulamayı seçin.

8. *İsteğe bağlı:* Uygulama eklemeden önce adını değiştirebilirsiniz **adı** kutusu.

9. Tıklayın **Ekle** seçerek uygulamayı ekleyin.

   Kısa bir gecikmenin ardından, uygulamanın yapılandırma bölmesi görmek mümkün olacaktır.

### <a name="configure-the-app-for-password-sso"></a>Parola SSO için uygulamayı yapılandırma

Şu adımları uygulayın:

1. Açık [Azure portalında](https://portal.azure.com/) ve bir genel yönetici veya ortak yönetici olarak oturum

2. Seçin **tüm hizmetleri** Azure AD uzantısı'nı açmak için sol taraftaki gezinti bölmesinin üst.

3. Tür **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory**.

4. Seçin **kurumsal uygulamalar** Azure AD Gezinti bölmesinde.

5. Seçin **tüm uygulamaları** , uygulamaların bir listesini görüntülemek için.

   > [!NOTE]
   > İstediğiniz uygulamayı göremiyorsanız, kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini**. Ayarlama **Göster** seçeneği "Tüm uygulamalara."

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra seçin **çoklu oturum açma** uygulamasının sol tarafındaki bölmede.

8. Seçin **parola tabanlı oturum açma** modu.

9. Uygulamaya kullanıcı atama.

10. Ayrıca, kullanıcılar için kimlik bilgilerini sağlayabilirsiniz. (Aksi takdirde, kullanıcılar uygulama başlatma sırasında kimlik bilgilerini girmeniz istenir.) Bunu yapmak için kullanıcıların satırları seçin. Ardından **kimlik bilgilerini güncelleştirme** ve kullanıcı adları ve Parolaları girin.

### <a name="assign-users-to-the-app"></a>Uygulamaya kullanıcı atama

Kullanıcıları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Açık [Azure portalında](https://portal.azure.com/) ve genel yönetici olarak oturum açın

2. Seçin **tüm hizmetleri** içinde Azure AD uzantısı'nı açmak için sol taraftaki gezinti kolaylaştırın.

3. Tür **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory**.

4. Seçin **kurumsal uygulamalar** Azure AD Gezinti bölmesinde.

5. Seçin **tüm uygulamaları** uygulamalarınızın bir listesini görüntülemek için.

   > [!NOTE]
   > İstediğiniz uygulamayı göremiyorsanız, kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini**. Ayarlama **Göster** seçeneği "Tüm uygulamalara."

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra seçin **kullanıcılar ve gruplar** uygulamanın sol tarafındaki gezinti bölmesinden.

8. Seçin **Ekle** en üstündeki **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. Seçin **kullanıcılar ve gruplar** içinde **atama Ekle** bölmesi.

10. İçinde **adına veya e-posta adresine göre arama** kutusuna tam adını yazın veya e-posta adresi atamak istediğiniz kullanıcı.

11. Listede bir kullanıcıyı üzerine gelin. Kullanıcının profil fotoğrafı ya da bu kullanıcıyı eklemek logosu yanındaki onay kutusunu işaretleyin **seçili** listesi.

12. *İsteğe bağlı:* Başka bir kullanıcı eklemek için başka bir ad veya e-posta adresi yazın **adına veya e-posta adresine göre arama** kutusuna ve bu kullanıcıyı eklemek için onay kutusunu seçip **seçili** listesi.

13. Kullanıcılar seçmeyi tamamladığınızda, tıklayın **seçin** kullanıcı ve uygulamaya atanan gruplar listesi eklemek için.

14. *İsteğe bağlı:* Tıklayın **rolü Seç** içinde **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Seçin **atama** Seçili kullanıcılar için uygulama atamak için.

    Kısa bir gecikmenin ardından kullanıcıları bu uygulamaları erişim panelinden erişmek mümkün olacaktır.

## <a name="request-support"></a>Destek isteği 
SSO'yu ayarlama ve kullanıcı atama bir hata iletisi alırsanız, bir destek bileti açın. Aşağıdaki bilgileri mümkün olduğunca çok şunlardır:

-   Bağıntı hata kimliği
-   UPN (kullanıcı e-posta adresi)
-   Kiracı kimliği
-   Tarayıcı türü
-   Saat dilimi ve hata gerçekleştiğinde saat/zaman çerçevesi
-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
