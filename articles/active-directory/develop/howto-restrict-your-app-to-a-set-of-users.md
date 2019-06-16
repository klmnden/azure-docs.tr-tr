---
title: Bir kullanıcı grubuyla Azure Active Directory'ye kayıtlı uygulama kısıtlama
description: Seçili bir kullanıcı kümesine Azure AD'de kayıtlı uygulamalarınızın erişimi kısıtlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: kalyankrishna1
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e07d9f0fa6ec6b4abc7ce96279b7b02faae298fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540188"
---
# <a name="how-to-restrict-your-app-to-a-set-of-users"></a>Nasıl yapılır: Uygulamanızı bir kullanıcı kümesiyle sınırlama

Bir Azure Active Directory (Azure AD) kiracısına kayıtlı uygulamalar başarıyla kimlik doğrulaması yapan tüm kullanıcılar Kiracı için kullanılabilir olan varsayılan olarak yapılandırılır.

Benzer şekilde, içinde durumunda, bir [çok kiracılı](howto-convert-app-to-be-multi-tenant.md) uygulama, tüm kullanıcılar bu uygulamayı hangi burada Azure AD kiracısında oluşturabileceksiniz kendi kendilerine ait kiracıda başarıyla kimlik doğrulaması sonra bu uygulamaya erişmek.

Kiracı Yöneticiler ve geliştiriciler genellikle bir uygulama belirli bir kullanıcı grubuyla kısıtlı olmalıdır nerede gereksinimleri vardır. Geliştiricilerin aynı popüler yetkilendirme desenleri gibi rol tabanlı erişim denetimi (RBAC) kullanarak gerçekleştirebilirsiniz, ancak bu yaklaşım, önemli miktarda iş parçasındaki Geliştirici gerektirir.

Azure AD Kiracı Yöneticiler ve geliştiriciler bir uygulama belirli bir kullanıcı veya güvenlik grupları kiracıdaki kümesini sınırlamak sağlar.

## <a name="supported-app-configurations"></a>Desteklenen uygulama yapılandırmaları

Bir uygulama kullanıcıların veya güvenlik gruplarının bir kiracıda belirli bir kümesini sınırlandırmak için seçenek aşağıdaki uygulama türleri ile çalışır:

- Federasyon çoklu oturum açma için SAML tabanlı kimlik doğrulaması ile yapılandırılan uygulamalar
- Azure AD ön kimlik doğrulama kullanan uygulama proxy uygulamaları
- Doğrudan Azure AD uygulama platform üzerinde oluşturulan uygulamaları, OAuth 2.0/Openıd Connect kimlik doğrulamasından sonra bir kullanıcı veya yönetici bu uygulamaya onay verdi kullanın.

     > [!NOTE]
     > Web uygulaması/web API'si ve kurumsal uygulamaları için yalnızca bu özellik kullanılabilir. Olarak kayıtlı uygulamalar [yerel](quickstart-v1-integrate-apps-with-azure-ad.md) kullanıcıların veya güvenlik gruplarının kiracıda bir dizi kısıtlı olamaz.

## <a name="update-the-app-to-enable-user-assignment"></a>Kullanıcı Ataması etkinleştirmek için uygulamayı güncelleştirme

1. Git [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma olarak bir **genel yönetici.**
1. Üst çubuğunda, oturum açtığınız hesabı seçin. 
1. Altında **dizin**, burada uygulamanın kayıtlı Azure AD kiracısı seçin.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory**. Azure Active Directory Gezinti bölmesinde kullanılabilir durumda değilse, ardından aşağıdaki adımları izleyin:

    1. Seçin **tüm hizmetleri** ana sol gezinti menüsünün üstünde.
    1. Yazın **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory** sonuç öğesi.

1. İçinde **Azure Active Directory** bölmesinde **kurumsal uygulamalar** gelen **Azure Active Directory** sol taraftaki gezinti menüsünde.
1. Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

     Burada görünmesi istediğiniz uygulamayı göremiyorsanız en üstündeki çeşitli filtreleri kullanın **tüm uygulamaları** liste veya uygulama bulmak için listede aşağı kısıtlamak için liste.

1. Bir kullanıcı veya güvenlik grubuna listesinden atamak istediğiniz uygulamayı seçin.
1. İçinde uygulamanın **genel bakış** sayfasında **özellikleri** uygulamanın sol taraftaki gezinti menüsünde.
1. Ayarını bulun **kullanıcı ataması gerekli mi?** ve **Evet**. Bu seçenek ayarlandığında **Evet**, ardından kullanıcıları bu erişebilmeleri için önce bu uygulama için önce atanmalıdır.
1. Seçin **Kaydet** bu yapılandırma değişikliği kaydetmek için.

## <a name="assign-users-and-groups-to-the-app"></a>Kullanıcıları ve grupları uygulamaya atama

Kullanıcı Ataması etkinleştirmek için uygulamanızın yapılandırdıktan sonra devam edin ve kullanıcıları ve grupları uygulamaya atama.

1. Seçin **kullanıcılar ve gruplar** bölmesinde uygulamanın sol taraftaki gezinti menüsünde.
1. Üst kısmındaki **kullanıcılar ve gruplar** listesinden **Kullanıcı Ekle** açmak için düğmeyi **atama Ekle** bölmesi.
1. Seçin **kullanıcılar** seçiciden **atama Ekle** bölmesi. 

     Kullanıcılar ve güvenlik grupları listesi, aramak ve belirli bir kullanıcı veya grup bulmak için bir textbox ile birlikte gösterilir. Bu ekran, tek bir seferde birden çok kullanıcı ve grupları seçmenize olanak sağlar.

1. İşiniz bittiğinde kullanıcıları ve grupları seçme, basın **seçin** düğmesine bir sonraki bölümüne geçmeye alt.
1. Tuşuna **atama** son kullanıcılar ve gruplar için uygulama atamalarını için alt düğmesine. 
1. Kullanıcıları ve grupları eklediğiniz güncelleştirilmiş gösterdiklerini doğrulayın **kullanıcılar ve gruplar** listesi.

