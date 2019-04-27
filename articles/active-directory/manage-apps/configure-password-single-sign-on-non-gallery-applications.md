---
title: Parola çoklu oturum açma için Galeri dışı applicationn yapılandırma | Microsoft Docs
description: Azure AD uygulama Galerisi'nde listelenmeyen olduğunda özel bir galeri dışı uygulama güvenli parola tabanlı çoklu oturum açma için yapılandırma
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c77ac32bd6079a8583cc6ea91cb8e214e5b31ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60442737"
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Azure AD uygulama Galerisi içinde bulunan seçenekler ek olarak, ekleme seçeneğiniz de bir **galeri dışı uygulama** uygulamayı değil listelendiğinde vardır. Bu özelliği kullanarak, kuruluşunuzda zaten herhangi bir uygulama veya kullanıyor olabileceğiniz herhangi bir üçüncü taraf uygulama olmayan bir satıcıdan ekleyebileceğiniz zaten parçası [Azure AD uygulama Galerisi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Galeri dışı bir uygulama ekledikten sonra bu uygulamanın kullandığı seçerek çoklu oturum açma yöntemi daha sonra yapılandırabilirsiniz **çoklu oturum açma** Gezinti öğesi üzerinde bir kurumsal uygulamada [Azureportalı](https://portal.azure.com/).

Çoklu oturum açma yöntemleri kullanabileceğiniz biri [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) seçeneği. İle **galeri dışı bir uygulama eklemek** deneyimi, bir HTML tabanlı bir kullanıcı adı işleyen herhangi bir uygulama tümleştirilebilir ve önceden tümleştirilmiş uygulamalar kümemizdeki içinde olsa bile alan, parolayı girin.

Bu işlemin çalıştığı kullanıcı adı ve parola giriş alanlarını otomatik olarak algıla, bunları güvenli bir şekilde depolamak için belirli uygulama örneğinizi olanak sağlayan erişim paneli uzantısı'nın bir parçası olan teknolojiyi değiştirilene bir sayfa yoludur. Bir kullanıcı bu uygulamayı uygulama erişim panelinde gittiğinde ardından güvenli bir şekilde kullanıcı adları ve parolalar için bu alanları yeniden yürütün.

Bu uygulama herhangi bir türden Azure AD'ye hızlı bir şekilde tümleştirmek kullanmaya başlamak için harika bir yoludur ve sağlar:

-   Tümleştirme **dünyanın herhangi bir uygulamada** , HTML kullanıcı adı ve parola giriş alanını işleyen sürede Azure AD kiracınız ile

-   Etkinleştirme **kullanıcılarınız için çoklu oturum açmayı** güvenli bir şekilde depolamak ve kullanıcı adları ve parolalar uygulamanın yeniden yürüterek Azure AD ile entegre ettik

-   **Otomatik Algıla giriş** alanlar için herhangi bir uygulama ve otomatik algılamayı bulmaz durumunda erişim paneli tarayıcı uzantısını kullanırken bu alanları el ile algılama olanak sağlar

-   **Çoklu oturum açma alanı gerektiren uygulamalar desteği** oturum açmak için yalnızca kullanıcı adı ve parola alanları gerektiren uygulamalar için

-   **Etiketleri özelleştirme** kullanıcılarınızın bakın kullanıcı adı ve parola giriş alanlarının [uygulama erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) kimlik bilgilerini girmeleri zaman

-   İzin, **kullanıcılar** bunlar metin üzerinde el ile tüm var olan uygulama hesaplarını için kendi kullanıcı adları ve parolalar sağlamak için [uygulama erişim paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   İzin bir **iş grubunun üyesi** kullanıcı adları ve parolaları kullanarak bir kullanıcıya atanmış belirtmek için [Self Servis uygulama erişimini](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) özelliği

-   İzin bir **yönetici** kullanıcı adları ve parolalar uygulamaya kullanıcı atama, kimlik bilgilerini güncelleştirme özelliğini kullanarak bir kullanıcıya atanmış belirtmek için

-   İzin bir **yönetici** belirtmek için paylaşılan kullanıcı adı veya parola güncelleştirme kimlik bilgilerini kullanarak bir kişi grubu tarafından kullanılan özellik ne zaman [uygulamaya grup atama](#assign-an-application-to-a-group-directly)

Aşağıdaki bölümde, nasıl olanak sağlayabileceğiniz açıklanmaktadır [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) kullanarak eklediğiniz herhangi bir uygulama için **galeri dışı bir uygulama eklemek** karşılaşırsınız.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   [Galeri dışı bir uygulama ekleyin](#add-a-non-gallery-application)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

-   Bir kullanıcının veya grubun uygulamaya atama

    -   [Bir uygulamaya doğrudan bir kullanıcı atama](#assign-a-user-to-an-application-directly)

    -   [Uygulama bir gruba doğrudan atama](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleyin

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  tıklayın **galeri dışı uygulama**.

7.  Uygulamanızda adını **adı** metin. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

## <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Girin **oturum açma URL'si**. Bu URL, burada kullanıcıların kullanıcı adını ve oturum açmak için parola girin kullanılır. Oturum açma alanlarını URL'SİNDE görünür olduğundan emin olun.

10. Uygulamaya kullanıcı atama.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.


## <a name="assign-a-user-to-an-application-directly"></a>Bir uygulamaya doğrudan bir kullanıcı atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Açmak için **atama Ekle** bölmesinde tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

## <a name="assign-an-application-to-a-group-directly"></a>Uygulama bir gruba doğrudan atama

Bir veya daha fazla grup, doğrudan uygulamaya atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Açmak için **atama Ekle** bölmesinde tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla Grup Ekle**, başka bir tür **tam grup adı** içine **adına veya e-posta adresine göre arama** arama kutusuna ve bu gruba eklemek için onay kutusuna tıklayın için **seçili** listesi.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatması mümkün.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
