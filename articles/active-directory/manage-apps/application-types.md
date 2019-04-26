---
title: Uygulamalar listemde beklenmedik bir uygulama | Microsoft Docs
description: Kiracınızdaki tüm uygulamalar ve kurumsal uygulamaları altındaki tüm uygulamalar listenizde uygulamaları görüntülenme anlama hakkında
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
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 075a50802a05a9b8254ff6ab1e0a38f43baca970
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60292138"
---
# <a name="unexpected-application-in-my-applications-list"></a>Uygulamalar listemde beklenmedik bir uygulama

Bu makale, uygulamaların nasıl görünür anlamanıza yardımcı, **tüm uygulamaları** altında listesinde **kurumsal uygulamalar**. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Kiracınızdaki tüm uygulamaları görme

Kiracınızdaki tüm uygulamaları görmek için kullanmanız gerekir **filtre** gösterilecek denetim **tüm uygulamaları** altında **tüm uygulamaları** listesi. Şu adımları uygulayın:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6.  kullan **filtre** üst kısmındaki denetim **tüm uygulamalar listesini**.

7.  Üzerinde **filtre** bölmesinde, **Göster** seçeneğini **tüm uygulamalar.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Neden belirli bir uygulamanın tüm uygulamalar listemde görünmüyor?

İçin filtre olduğunda **tüm uygulamaları**, **tüm uygulamaları** **listesi** kiracınızda her hizmet sorumlusu nesne gösterir. Hizmet sorumlusu nesneleri çeşitli şekillerde bu listede görünebilir:

1. Herhangi bir uygulama uygulama galerisinden eklediğinizde de dahil olmak üzere:

   1. **Azure AD galeri uygulamaları** – çoklu oturum açma için Azure AD ile önceden tümleştirilmiş uygulama

   2. **Uygulama Proxy uygulamaları** – güvenli çoklu oturum açmayı harici olarak sağlamak istiyorsanız, şirket içi ortamınızda çalışan bir uygulama

   3. **Özel olarak geliştirilmiş uygulamaların** – kuruluşunuzun Azure AD uygulama geliştirme platformu üzerinde geliştirmek isteyen, ancak bu mevcut olmayabilir henüz uygulama

   4. **Galeri dışı uygulamalar** – kendi uygulamalarınızı getirin! İstediğiniz herhangi bir web bağlantısına veya bir kullanıcı adı ve parola alanını işleyen herhangi bir uygulama SAML veya Openıd Connect protokolleri destekler veya çoklu oturum açma için Azure AD ile tümleştirmek istediğiniz SCIM destekler.

2. Kaydolmak için gerekli ya da 3 için oturum açarken<sup>rd</sup> taraf uygulaması Azure Active Directory ile tümleşik. Bir örnek [Smartsheet](https://app.smartsheet.com/b/home) veya [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3. Kaydolmak için gerekli ya da bir kullanıcı veya gruba bir birinci taraf uygulaması için bir lisans eklemek istediğiniz [Microsoft Office 365](https://products.office.com/)

4. Eklediğinizde, yeni bir uygulama kaydı kullanarak özel olarak geliştirilmiş uygulama oluşturarak [uygulama kayıt defteri](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration)

5. Eklediğinizde, yeni bir uygulama kaydı kullanarak özel olarak geliştirilmiş uygulama oluşturarak [V2.0 uygulama kayıt portalı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration)

6. Bir uygulama eklediğinizde Visual Studio'nun kullanarak geliştirirken [ASP.net kimlik doğrulama yöntemleri](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) veya [bağlı hizmetler](https://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)

7. Kullanarak bir hizmet sorumlusu nesnesi oluşturduğunuzda [Azure AD PowerShell Modülü](/powershell/azure/install-adv2?view=azureadps-2.0)

8. Olduğunda, [uygulamaya onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) veri kiracınızda kullanmak için yönetici olarak

9. Olduğunda bir [kullanıcı uygulamaya onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) veri kiracınızda kullanmak için

10. Kiracınızda verileri depolayan belirli hizmetleri etkinleştirdiğinizde. Bir parola sıfırlama İlkesi parolanızı depolamak için bir hizmet sorumlusu olarak Modellenen sıfırlama, güvenli bir örnektir.

Uygulama dizininize nasıl eklenir üzerinde daha ayrıntılı bilgi edinmek için okumaya devam [uygulamaları Azure AD'ye neden ve nasıl eklenir](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Bir uygulama için belirli bir kullanıcının veya grubun atamasını kaldırmak istiyorum

Bir kullanıcı veya uygulamaya Grup atamasını kaldırmak için listelenen adımları izleyin. [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) makalesi.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her kullanıcı için tüm uygulama erişimi devre dışı bırakmak istiyorum

Bir uygulama için tüm kullanıcı oturum açma işlemleri devre dışı bırakmak için listelenen adımları izleyin. [kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) makalesi.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

İçin **bir uygulamayı silmek**, şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Silmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **Sil** üst uygulamanın simgesinden **genel bakış** bölmesi.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Herhangi bir uygulama için tüm gelecek kullanıcı onayı işlemleri devre dışı bırakmak istiyorum

Tüm dizininizin engellemek için son kullanıcıların herhangi bir uygulama konusunda çekince kullanıcı onayı devre dışı bırakılıyor. Yöneticiler, yine de kullanıcı adına onay verebilir. Uygulama onay hakkında daha fazla bilgi edinin ve neden olabilir veya onay verme istemeyebilir okumak için [anlama kullanıcı ve yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

İçin **tüm dizininizdeki tüm gelecek kullanıcı onayı işlemleri devre dışı**, şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  Tıklayın **kullanıcı ayarları**.

6.  Ayarlayarak tüm gelecek kullanıcı onayı işlemleri devre dışı **kullanıcılar uygulamaların verilerine erişmesine izin verebilir** geç **Hayır** tıklatıp **Kaydet** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
