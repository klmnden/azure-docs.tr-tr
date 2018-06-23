---
title: My uygulamalar listesinde beklenmeyen uygulama | Microsoft Docs
description: Kiracınızda bulunan tüm uygulamaları görmek ve uygulamaların kurumsal uygulamalar altındaki tüm uygulamalar listenizde görüntülenme anlama hakkında
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 6d8c1a251bcc4a7c0bb736df64c4c701a2fc748a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333884"
---
# <a name="unexpected-application-in-my-applications-list"></a>My uygulamalar listesinde beklenmeyen uygulama

Bu makale, uygulamaları nasıl görünür anlamanıza yardımcı, **tüm uygulamaları** altında listesinde **kurumsal uygulamalar**. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Kiracınızda bulunan tüm uygulamalar görme

Kullanmanıza gerek kiracınızda tüm uygulamaları görmek için **filtre** göstermek için Denetim **tüm uygulamaları** altında **tüm uygulamaları** listesi. Şu adımları uygulayın:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6.  kullan **filtre** üst kısmındaki denetim **tüm uygulamalar listesini**.

7.  Üzerinde **filtre** kümesi bölmesi, **Göster** için seçenek **tüm uygulamaları.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Belirli bir uygulama my tüm uygulamalar listesinde neden görünüyor?

Filtrelenen için zaman **tüm uygulamaları**, **tüm uygulamaları** **listesi** her hizmet sorumlusu nesnesi kiracınızda gösterir. Hizmet sorumlusu nesneleri, çeşitli yollarla bu listede görünebilir:

1.  Herhangi bir uygulama uygulama Galerisi'nden eklediğinizde dahil olmak üzere:

   1. **Azure AD galeri uygulamaları** – bir tek oturum açma için Azure AD ile önceden tümleştirilmiş uygulama

   2. **Uygulama proxy'si uygulamalar** – güvenli çoklu oturum açmayı dışarıdan sağlamak istediğiniz, şirket içi ortamınızda çalışan bir uygulama

   3. **Özel geliştirilmiş uygulamaları** – kuruluşunuz Azure AD uygulama geliştirme platformu üzerinde geliştirmek istiyor ancak var henüz uygulama

   4. **Galeri olmayan uygulamaları** – kendi uygulamalarınızı Getir! İstediğiniz herhangi bir web bağlantıyı ya da bir kullanıcı adı ve parola alanı işleyen herhangi bir uygulama SAML veya Openıd Connect protokollerini destekler veya çoklu oturum açma için Azure AD ile tümleştirmek istediği SCIM'yi destekler.

2.  İçin imzalarken veya 3'e, oturum açma<sup>rd</sup> taraf uygulama Azure Active Directory ile tümleşik. Bir örnektir [Smartsheet](https://app.smartsheet.com/b/home) veya [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  İçin imzalama veya bir kullanıcı veya grup birinci taraf uygulama için bir lisans ekleme, ister [Microsoft Office 365](http://products.office.com/)

4.  Kullanarak bir özel geliştirilmiş uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde [uygulama kayıt defteri](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration)

5.  Kullanarak bir özel geliştirilmiş uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde [V2.0 uygulama kayıt portalı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal)

6.  Bir uygulama eklediğinizde Visual Studio'nun kullanarak geliştirirken [ASP.net kimlik doğrulama yöntemleri](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) veya [bağlantılı hizmetler](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)

7.  Kullanarak bir hizmet sorumlusu nesnesi oluşturduğunuzda [Azure AD PowerShell Modülü](/powershell/azure/install-adv2?view=azureadps-2.0)

8.  Olduğunda, [onayı uygulamaya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) veri kiracınızda kullanmak için yönetici olarak

9.  Zaman bir [kullanıcı uygulamaya izin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) kiracınızda verileri kullanmak için

10. Kiracınızda verileri depolamak belirli hizmetleri etkinleştirdiğinizde. Bir parola ilkesi parolanızı depolamak için bir hizmet sorumlusu sıfırlama olarak hangi modellenir sıfırlama, güvenli bir şekilde örnektir.

Uygulamaları dizininize nasıl eklenir hakkında daha fazla bilgi almak için okuma [neden ve nasıl uygulamaları için Azure AD eklenen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Bir uygulama için belirli kullanıcının veya grubun atama kaldırılacağını

Bir kullanıcı veya grup ataması uygulamaya kaldırmak için listelenen adımları izleyin [bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) makalesi.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her kullanıcı için tüm uygulama erişimi devre dışı bırakmak istiyorum

Tüm kullanıcı oturum açmalarına bir uygulama için devre dışı bırakmak için listelenen adımları [kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırakma](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) makalesi.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyor

İçin **bir uygulamayı silmek**, şu adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Silmek istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **silmek** üst uygulamanın simgesinden **genel bakış** bölmesi.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Herhangi bir uygulama için tüm gelecekteki kullanıcı onayı işlemleri devre dışı bırakmak istiyorum

Tüm dizin önlemek için son kullanıcılar için herhangi bir uygulama onaylıyorsunuz kullanıcı izni devre dışı bırakılıyor. Yöneticiler hala kullanıcının behalves üzerinde onay. Uygulama onayı hakkında daha fazla bilgi ve olabilir ya da onayı istemeyebilirsiniz neden okumak için [anlama kullanıcı ve yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

İçin **tüm dizininizdeki tüm gelecekteki kullanıcı onayı işlemleri devre dışı**, şu adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  Tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  Tıklatın **kullanıcı ayarlarını**.

6.  Tüm gelecekteki kullanıcı onayı işlemleri ayarlayarak devre dışı **kullanıcılar, uygulamaların verilerine erişmesini izin verebilir** geç **Hayır** tıklatıp **kaydetmek** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
