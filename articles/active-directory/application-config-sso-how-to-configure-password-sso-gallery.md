---
title: Parola tek oturum açma için Azure AD galeri uygulamanın yapılandırma | Microsoft Docs
description: Azure AD uygulama galerisinde zaten listelendiğinde güvenli parola tabanlı çoklu oturum açma için uygulama yapılandırma
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8e1d8471d2feb838a6ba3eb08eedc3ca4d30ab07
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola tek oturum açma için Azure AD galeri uygulama yapılandırma

Bir uygulamadan eklediğinizde [Azure AD uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), bu uygulamada oturum açmak için kullanıcılarınızın istediğiniz seçeneğiniz vardır. Bu seçenek seçerek herhangi bir anda yapılandırabilirsiniz **çoklu oturum açma** bir kuruluş uygulamasında Gezinti öğede [Azure portal](https://portal.azure.com/).

Çoklu oturum açma için kullanılabilen yöntemler biri [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) seçeneği. Bu uygulamaları hızlı bir şekilde Azure AD ile tümleştirme başlamak için mükemmel bir yoldur ve olanak sağlar:

-   Etkinleştirme **kullanıcılarınız için çoklu oturum açmayı** güvenli bir şekilde depolamak ve kullanıcı adları ve parolalar uygulamanın yeniden oynatmak tarafından Azure AD ile tümleşik

-   **Destek oturum açma birden çok alan gerektiren uygulamalar** oturum açmak için daha fazlasını kullanıcı adı ve parola alanlarına gerektiren uygulamalar için

-   **Etiketleri özelleştirme** kullanıcılarınızın gördüğü kullanıcı adı ve parola giriş alanlarının [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) kimlik bilgilerini girmeleri zaman

-   İzin ver, **kullanıcılar** bunlar yazarak, el ile on tüm var olan uygulama hesaplarını için kendi kullanıcı adları ve parolalar sağlamak için [uygulama erişim paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   İzin bir **iş grubunun üyesi** kullanıcı adları ve parolaları kullanarak bir kullanıcıya atanmış belirtmek için [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) özelliği

-   İzin bir **yönetici** belirtmek için kullanıcı adları ve parolalar güncelleştirme kimlik bilgilerini kullanarak bir kullanıcıya atanmış özelliği ne zaman [bir kullanıcı için uygulama atama](#assign-a-user-to-an-application-directly)

-   İzin bir **yönetici** belirtmek için paylaşılan kullanıcı adı veya parola güncelleştirme kimlik bilgilerini kullanarak bir grup kişi tarafından kullanılan özellik ne zaman [uygulama için bir grup atama](#assign-an-application-to-a-group-directly)

Aşağıdaki bölümde nasıl etkinleştirebilirsiniz açıklanmaktadır [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) zaten bir uygulamaya [Azure AD uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

-   [Bir kullanıcı veya grup için uygulama atama](#assign-the-application-to-a-user-or-a-group)

    -   [Kullanıcının uygulamaya doğrudan atayın](#assign-a-user-to-an-application-directly)

    -   [Bir gruba uygulama doğrudan atama](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** metin **galerisinden Ekle** bölümünde, uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin kutusu.

9.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

## <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  [Uygulamaya kullanıcılar atama](#assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="assign-a-user-to-an-application-directly"></a>Kullanıcının uygulamaya doğrudan atayın

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** bölmesi.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına göre arama veya e-posta adresi** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

## <a name="assign-an-application-to-a-group-directly"></a>Bir gruba uygulama doğrudan atama

Bir veya daha fazla grupları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** bölmesi.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **grup** ortaya çıkarmak için listedeki bir **onay kutusunu**. Grubun profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla grubu Ekle**, başka bir tür **tam grup adı** içine **ad veya e-posta adresine göre arama** arama kutusuna ve Bu gruba eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Grupları seçmek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz gruplarına atamak için bir rol seçin.

15. Tıklatın **atamak** seçilen grupları uygulamaya atamak için düğmesi.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
