---
title: Uygulama erişim paneli tarayıcı uzantısını - Azure'ı yükleme | Microsoft Docs
description: Erişim paneli tarayıcı uzantısını yükleme sırasında karşılaşılan yaygın hataları düzeltin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/4/2018
ms.author: celested
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 216568ac43c8e1b04c91d9a8f611a0ceb2e430af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60294200"
---
# <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli web tabanlı bir portaldır. Bir iş veya Okul hesabı Azure Active Directory'de (Azure AD), görüntülemek ve bir Azure AD Yöneticisi erişim izni vermiştir, bulut tabanlı uygulamalarda başlatmak için erişim paneli kullanabilirsiniz. 

Azure AD sürümleri kullanıyorsanız, Self Servis grup ve uygulama yönetimi özelliklerinin erişim paneli ile kullanabilirsiniz. 

Erişim paneli, Azure Portalı'ndan ayrıdır. Azure aboneliğinin olmasını gerektirmez.

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından CSS etkin olduğundan ve erişim paneli JavaScript destekleyen bir tarayıcı gerektirir. Uygulamalara erişim panelinde parola tabanlı SSO ile oturum açmanız, erişim paneli uzantısını tarayıcınızda yüklü olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde otomatik olarak indirilir.

Parola tabanlı SSO için aşağıdaki tarayıcılardan herhangi birini kullanabilirsiniz:

- **Microsoft Edge**: Windows 10 Anniversary Edition veya sonrası. 
- **Chrome**: Windows 7 ve daha sonra ve MacOS x veya sonrası.
- **26,0 veya üzeri Firefox**: Windows XP SP2 veya sonraki sürümlerde ve Mac OS X 10.6 veya daha sonra.

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdakileri yapın:

1.  Desteklenen tarayıcılar her birinde açın [erişim paneli](https://myapps.microsoft.com)ve Azure AD hesabı bir kullanıcı olarak oturum açın.

2.  Bir parola tabanlı SSO uygulaması'nı seçin.

3.  İstendiğinde, seçin **Şimdi Yükle**.  
    İndirme bağlantısını için seçilen tarayıcınızın yönlendirilir. 
    
4.  **Add (Ekle)** seçeneğini belirleyin.

5.  Ya da istendiğinde **etkinleştirme** veya **izin** uzantısı.

6.  Yükleme tamamlandıktan sonra tarayıcınızı yeniden başlatın.

7.  Erişim paneli ve onay için parola tabanlı SSO uygulamalarınızı başlayıp başlamayacağını görmek için oturum açın.

Uzantı, Chrome ve aşağıdaki sitelerden doğrudan Microsoft Edge için de indirebilirsiniz:

- [Chrome uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Microsoft Edge uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Kullanım uygulamalarım güvenli oturum açma uzantısı
* My Apps URL dışında kullanıyorsanız `https://myapps.microsoft.com`, aşağıdakileri yaparak, varsayılan URL yapılandırın:
   1. Siz *değil* uzantı açtığınız uzantı simgesine sağ tıklayın.
   2. Menüsünde **My Apps URL**.
   3. Varsayılan URL'yi seçin.
   4. Genişletme simgesini seçin.
   5. Uzantı oturum açmak için seçin **kullanmaya başlamak oturum**.

* Bir uygulamaya doğrudan bir tarayıcıdan oturum açmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra ona seçerek oturum **kullanmaya başlamak oturum**.
   2. Uygulama oturum açma URL'si ile oturum açın.  
       Oturum açma URL'si genellikle oturum açma formunu görüntüleyen bir uygulama URL'dir.
      Uzantı, durumunu değiştirin ve parola kullanılabilir olduğunu bilmesini sağlar.
   3. Oturum açmak için genişletme simgesini seçin.

* Uzantı bir uygulamayı başlatmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra ona seçerek oturum **kullanmaya başlamak oturum**.
   2. Alt menüsünü açmak için genişletme simgesini seçin.
   3. Uygulamalarım portalında kullanılabilir olan uygulama arayın.
   4. Arama sonuçları listesinde uygulamayı seçin.  
       Son üç uygulamaları kullandığınız görüntülenen **kısa süre önce kullanılan** kısayol listesi.
       
* Şirket içi URL'leri uzaktayken kullanmak için aşağıdakileri yapın:
    1. [Uygulama Ara sunucusu yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) kiracınıza
    2. [Uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) ve uygulama proxy'si aracılığıyla uygulama URL'si
    3. Uzantıyı yüklemek ve ona oturum açma'yı seçerek kullanmaya başlamak için oturum açın
    4. Artık uzaktan çalışırken bile şirket içi URL'sine göz atabilirsiniz

> [!NOTE]
> Yukarıdaki seçeneklerden yalnızca Microsoft Edge, Chrome ve Firefox için kullanılabilir.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir Grup İlkesi ayarlama

Uzaktan erişim paneli uzantısını Internet Explorer için kullanıcılarınızın makineler yüklemenize olanak sağlayan bir Grup İlkesi ayarlayabilirsiniz.

Bir Grup İlkesi ayarlama önce şunlardan emin olun:

-   Ayarlamış olduğunuz [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineleri etki alanınıza katılmış.

-   Grup İlkesi nesnesi (GPO) düzenleyin için olmalıdır *ayarlarını Düzenle* izinleri. Varsayılan olarak, aşağıdaki güvenlik gruplarının bir üyesi için bu izin verilir: etki alanı yöneticileri, kuruluş yöneticileri ve Grup İlkesi Oluşturucu Sahipleri.

Grup İlkesi yapılandırma ve kullanıcılara dağıtma hakkında adım adım yönergeler için bkz: [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Erişim paneli uzantısını Internet Explorer'da sorunlarını giderme

Bir tanılama aracı ve Internet Explorer uzantısı yapılandırma hakkında bilgi erişmek için bkz: [erişim paneli uzantısını Internet Explorer için sorun giderme](manage-access-panel-browser-extension.md).

> [!NOTE]
> Internet Explorer üzerinde sınırlı desteği ve yeni yazılım güncelleştirmeleri artık almaz. Microsoft Edge, önerilen tarayıcısıdır.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Yukarıdaki adımlar sorunu çözmezse

Varsa aşağıdaki bilgileri ile bir destek bileti açın:

-   Bağıntı hata kimliği
-   UPN (kullanıcı e-posta adresi)
-   Kiracı kimliği
-   Tarayıcı türü
-   Saat dilimi ve saat veya hatanın oluştuğu andaki zaman çerçevesi
-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](what-is-single-sign-on.md)
