---
title: 'Uygulama yönetici rolleri: Azure Active Directory temsilci | Microsoft Docs'
description: Uygulama erişim yönetimi Azure Active Directory'de izinleri hakları vermek rolleri için temsilci seçme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58ca814551d8c7d309328f236052e1d07ac6f035
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469137"
---
# <a name="delegate-app-administrator-roles-in-azure-active-directory"></a>Azure Active Directory'de yönetici rolleri uygulama temsilcisi

 Azure AD, temsilci bir dizi yerleşik yönetici rolleri için uygulama erişim yönetimi sağlar. Genel yönetici ek yükünü azaltmanın yanı sıra, uygulama erişim görevleri yönetmek üzere özelleştirilmiş ayrıcalıkları temsilci olarak görevlendirme güvenlik duruşunu ve yetkisiz erişim olasılığını azaltır. Temsilci seçme sorunlarını ve genel yönergeleri açıklanmıştır [temsilci Azure Active Directory'de yönetim](roles-concept-delegation.md).

## <a name="delegate-app-administration"></a>Temsilci uygulama yönetimi

Aşağıdaki roller, uygulama kayıtları, çoklu oturum açma ayarları, kullanıcı ve Grup atamalarını yönetmeye ve temsilci izinleri ve uygulama izinleri (Microsoft Graph ve Azure AD Graph hariç) onay için izinleri verin. Tek fark, uygulama yöneticisi rolü de uygulama proxy'si ayarlarını yönetmek için izinler verir. Her iki rol, koşullu erişim ayarlarını yönetme olanağı verir.
> [!IMPORTANT]
> Bu role atanan kullanıcılar, kimlik bilgileri için uygulama ekleme ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Bu kimliğe bürünme uygulamanın kimliğini, kullanıcı, bir rol atamaları altında Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Bu role atanmış kullanıcı potansiyel olarak oluşturun veya uygulama kimliğine bürünülürken kullanıcılara veya diğer nesneleri güncelleştirin.

Azure portalında uygulama erişimini yönetme olanağı vermek için:

1. Oturum açın, [Azure AD kiracısı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) kiracısında genel yönetici rolü için uygun olan bir hesapla.
2. Yeterli izinlere sahip açtığınızda [roller ve yöneticiler sayfası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators).
3. Üye atamalarının görmek için aşağıdaki rollerden biri açın:
   * **Uygulama Yöneticisi**
   * **Bulut uygulaması Yöneticisi**
4. Üzerinde **üyeleri** rolü seçme sayfasında **Üye Ekle**.
5. Role eklenecek bir veya daha fazla üyeleri seçin. <!--Members can be users or groups.-->

Bu rolü için bir açıklama görüntüleyebilirsiniz [kullanılabilir roller](directory-assign-admin-roles.md#available-roles).

## <a name="delegate-app-registration"></a>Temsilci uygulama kaydı

Varsayılan olarak, tüm kullanıcılar, uygulama kayıtları oluşturabilir, ancak seçerek uygulama kayıtları oluşturma izni veya uygulama yetkilendirmek için onaylamasına izin verebilirsiniz.

1. Oturum açın, [Azure AD kiracısı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) kiracısında genel yönetici rolü için uygun olan bir hesapla.
2. Yeterli izinlere edindiğinizde, birini veya her ikisini aşağıdakileri ayarlayın:
   * Üzerinde [kiracınız için kullanıcı ayarları sayfası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings)ayarlayın **kullanıcılar uygulamaları kaydedebilir** No
   * Üzerinde [kurumsal uygulamalar için kullanıcı ayarlarını](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)ayarlayın **kullanıcılar uygulamaları kendileri adına şirket verilerine erişme izni verebilir** No
3. Daha sonra gerektiğinde Uygulama geliştirici rol üyelerinin olması için bu izne ihtiyaç duyan kullanıcılar atayın.

Bir kullanıcı bir uygulama kaydettiğinde, uygulamanın ilk sahibi olarak otomatik olarak eklenir.

## <a name="delegate-app-ownership"></a>Uygulama sahipliğini devretme

Uygulama sahipleri ve uygulama kayıt sahipleri her yalnızca uygulamaları veya sahip oldukları uygulama kayıtları olarak yönetebilirsiniz. Örneğin, Salesforce uygulama için bir sahibe eklediğinizde, o sahibi erişim ve Salesforce, ancak hiçbir diğer uygulamalar için yapılandırma yönetebilir. Bir uygulamanın fazla sahip olabilir ve bir kullanıcı çok sayıda uygulama sahibi olabilir.

Uygulama sahibi yapabilirsiniz:

* Değiştirme izinleri ve adı gibi uygulama özellikleri uygulama istekleri
* Kimlik bilgilerini yönetme
* Çoklu oturum açmayı yapılandırma
* Kullanıcı erişim atama
* Ekleme veya diğer sahipleri kaldırma
* Uygulama bildirimini Düzenle
* Uygulamayı uygulama galerisinde yayımlayabilirsiniz.

> [!IMPORTANT]
> Bu role atanan kullanıcılar, kimlik bilgileri için uygulama ekleme ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Bu kimliğe bürünme uygulamanın kimliğini, kullanıcı, bir rol atamaları altında Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Bu role atanmış kullanıcı potansiyel olarak oluşturun veya uygulama kimliğine bürünülürken kullanıcılara veya diğer nesneleri güncelleştirin.

Sahibi uygulama kaydını görüntüleyebilir ve uygulama kaydını Düzenle.

<!-- ### To assign an enterprise app ownership role to a user

1. Sign in to your [Azure AD tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with an account that is the Global Administrator for the tenant.
2. On the [Roles and administrators page](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators), open one of the following roles to see its member assignments:
  * **Enterprise Application Owner**
  * **Application Registration Owner**
3. On the **Members** page for the role, select **Add member**.
4. Select one or more members to add to the role. -->

### <a name="to-assign-an-owner-to-an-application"></a>Uygulama sahibi atamak için

1. Oturum açın, [Azure AD kiracısı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) uygulama yönetici veya Kiracı için bulut uygulaması Yöneticisi için uygun olan bir hesapla.
2. Üzerinde [uygulama kayıtları sayfası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) kiracısı için'ı açmak için bir uygulama seçin **genel bakış** sayfasını.
3. Seçin **sahipleri** uygulama sahipleri listesini görmek için.
4. Seçin **Ekle** uygulamasına eklemek için bir veya daha fazla sahip seçin.

### <a name="to-assign-an-owner-to-an-application-registration"></a>Bir uygulama kaydı için bir sahip atamak için

1. Oturum açın, [Azure AD kiracısı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) bu uygulama yönetici veya Bulut Kiracı Yöneticisi rolünde uygulama için uygun bir hesapla.
2. Ne zaman yeterli izinlere sahip, üzerinde [kurumsal uygulamalar sayfası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) Kiracı için bir uygulama kaydı açmak için seçin.
3. Seçin **ayarları**.
4. Seçin **sahipleri** üzerinde **ayarları** uygulama sahipleri listesini görmek için sayfayı.
5. Seçin **Ekle sahibi** uygulamasına eklemek için bir veya daha fazla sahip seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Yönetici rolü başvurusu](directory-assign-admin-roles.md)
