---
title: "Parola tek oturum açma için Galeri olmayan applicationn yapılandırma | Microsoft Docs"
description: "Azure AD uygulama galerisinde listelenmeyen zaman güvenli parola tabanlı çoklu oturum açma için özel bir galeri olmayan uygulama yapılandırma"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri olmayan uygulama için yapılandırma

Azure AD uygulama galerisinde içinde bulunan seçenekleri yanı sıra ekleme seçeneğiniz de bir **galeri olmayan uygulama** zaman istediğiniz uygulama listelenmemiş vardır. Bu özelliği kullanarak, kuruluşunuzda zaten herhangi bir uygulama ya da kullanıyor olabileceğiniz herhangi bir üçüncü taraf uygulama olmayan bir satıcıdan ekleyebileceğiniz zaten parçası [Azure AD uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

Bir galeri olmayan uygulama ekledikten sonra daha sonra bu uygulamanın kullandığı seçerek tek oturum açma yöntemi yapılandırabilirsiniz **çoklu oturum açma** bir kuruluş uygulamasında Gezinti öğede [Azure Portal](https://portal.azure.com/).

Çoklu oturum açma için kullanılabilen yöntemler biri [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) seçeneği. İle **bir galeri olmayan uygulama eklemek** deneyimi, bir HTML tabanlı kullanıcıadı işleyen herhangi bir uygulama tümleştirebilir ve parola giriş alanı, önceden tümleştirilmiş uygulamaların bizim kümesinde olmasa bile.

Bu çalışır, kullanıcı adı ve parola giriş alanlarının otomatik algıla, belirli bir uygulama Örneğiniz için bunları güvenli bir şekilde depolamak için bize sağlar erişim paneli uzantısı'nın bir parçası olan teknoloji değiştirilene sayfası tarafından yoludur. Bir kullanıcı bu uygulamayı uygulama erişim panelinde gittiğinde ardından güvenli bir şekilde kullanıcı adları ve parolalar bu alanlara yeniden yürütün.

Bu uygulama herhangi bir tür hızlı bir şekilde Azure AD ile tümleştirme başlamak için mükemmel bir yoldur ve sağlar:

-   Tümleştirme **dünyadaki herhangi bir uygulama** ile Azure AD kiracınız, HTML kullanıcı adı ve parola giriş alanını işleyen kadar uzun süre

-   Etkinleştirme **kullanıcılarınız için çoklu oturum açmayı** güvenli bir şekilde depolamak ve kullanıcı adları ve parolalar uygulamanın yeniden oynatmak tarafından Azure AD ile tümleşik

-   **Giriş otomatik algıla** alanlar için herhangi bir uygulama ve otomatik algılamayı bulmaz durumda erişim paneli tarayıcı uzantısı kullanarak bu alanları el ile algılama olanak sağlar

-   **Destek oturum açma birden çok alan gerektiren uygulamalar** oturum açmak için daha fazlasını kullanıcı adı ve parola alanlarına gerektiren uygulamalar için

-   **Etiketleri özelleştirme** kullanıcılarınızın gördüğü kullanıcı adı ve parola giriş alanlarının [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) kimlik bilgilerini girmeleri zaman

-   İzin ver, **kullanıcılar** bunlar yazarak, el ile on tüm var olan uygulama hesaplarını için kendi kullanıcı adları ve parolalar sağlamak için [uygulama erişim paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   İzin bir **iş grubunun üyesi** kullanıcı adları ve parolaları kullanarak bir kullanıcıya atanmış belirtmek için [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) özelliği

-   İzin bir **yönetici** belirtmek için kullanıcı adları ve parolalar güncelleştirme kimlik bilgilerini kullanarak bir kullanıcıya atanmış özelliği ne zaman [bir kullanıcı için uygulama atama](#_How_to_configure_1)

-   İzin bir **yönetici** belirtmek için paylaşılan kullanıcı adı veya parola güncelleştirme kimlik bilgilerini kullanarak bir grup kişi tarafından kullanılan özellik ne zaman [uygulama için bir grup atama](#assign-an-application-to-a-group-directly)

Aşağıda nasıl etkinleştirebilirsiniz açıklar [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) kullanarak eklediğiniz herhangi bir uygulama için **bir galeri olmayan uygulama eklemek** karşılaşırsınız.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Bir galeri olmayan uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

-   [Bir kullanıcı veya grup için uygulama atama](#assign-the-application-to-a-user-or-a-group)

    -   [Kullanıcının uygulamaya doğrudan atayın](#assign-a-user-to-an-application-directly)

    -   [Bir gruba uygulama doğrudan atama](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Bir galeri olmayan uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure Portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** dikey penceresi

6.  tıklatın **olmayan galeri uygulaması.**

7.  Uygulamanızda adını girin **adı** metin kutusu. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma dikey görüyor.

## <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. Oturum açma alanları URL'de göründüğünden emin olun.

10. Kullanıcılar uygulamayı atayın.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="assign-a-user-to-an-application-directly"></a>Kullanıcının uygulamaya doğrudan atayın

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** dikey.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** dikey.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **ad veya e-posta adresine göre arama** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** seçtiğiniz kullanıcılara atamak için bir rol seçin dikey.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

## <a name="assign-an-application-to-a-group-directly"></a>Bir gruba uygulama doğrudan atama

Bir veya daha fazla grupları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** dikey.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** dikey.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **grup** ortaya çıkarmak için listedeki bir **onay kutusunu**. Grubun profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla grubu Ekle**, başka bir tür **tam grup adı** içine **ad veya e-posta adresine göre arama** arama kutusu ve bu gruba eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Grupları seçmek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** seçtiğiniz gruplarına atamak için bir rol seçin dikey.

15. Tıklatın **atamak** seçilen grupları uygulamaya atamak için düğmesi.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)
