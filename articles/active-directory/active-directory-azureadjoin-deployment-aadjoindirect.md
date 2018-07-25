---
title: Kullanım senaryoları ve Azure AD katılım için dikkat edilecekler | Microsoft Docs
description: Nasıl yöneticilerin Azure AD'ye katılımı ayarlama son kullanıcıları için (çalışanlar, Öğrenciler, diğer kullanıcılar) ayarlayacakları açıklanmaktadır. Ayrıca, Azure AD'ye katılım'ı kullanmaya yönelik farklı gerçek dünya senaryoları açıklar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.openlocfilehash: 29480390e6854dcedeaf8f06c078ed2e4ed2b94d
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223016"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Kullanım senaryoları ve Azure AD katılım için dağıtım konuları
## <a name="usage-scenarios-for-azure-ad-join"></a>Azure AD katılım için kullanım senaryoları
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Senaryo 1: İşletmeler bulutta büyük ölçüde
Azure Active Directory Join (Azure AD katılımı), şu anda çalışan ve işletmenizi bulutta için kimlikleri yönetme veya buluta taşıma konusunda en kısa sürede yararlanabilir. Windows 10'a oturum açmak için Azure AD'de oluşturduğunuz bir hesap kullanabilirsiniz. Aracılığıyla [ilk çalıştırma deneyimi (FRX) işlem](active-directory-azureadjoin-user-frx.md), ya da Azure AD'den katılarak [ayarlar menüsünü](active-directory-azureadjoin-user-upgrade.md), kullanıcılarınız kendi makinelerine Azure AD'ye katılmasını sağlayabilirsiniz.  Kullanıcılarınızın çoklu oturum açma (SSO) erişimi tarayıcılarını veya Office uygulamaları, Office 365 gibi bulut kaynakları için de yararlanabilirsiniz.

### <a name="scenario-2-educational-institutions"></a>Senaryo 2: Eğitim kurumları
Eğitim kurumları, genellikle iki kullanıcı türü vardır: öğretim üyelerinizin ve öğrencilerinizin. Öğretim üyeleri daha uzun vadeli bir kuruluşun üyeleri olarak kabul edilir. Şirket içi hesapları kullanabiliyor tercih edilir. Öğrenciler kuruluşun shorter-term üyeleridir ve hesaplarını Azure AD'de yönetilebilen ancak. Bu dizin ölçek yerine şirket içinde depolanan olan buluta gönderilemez anlamına gelir. Ayrıca, Öğrenciler için Windows Azure AD hesaplarına ile oturum açın ve Office 365 kaynaklarına erişimi Office uygulamalarında alma olanağına olacağı anlamına gelir.

### <a name="scenario-3-retail-businesses"></a>Senaryo 3: Perakende işletmeler
Perakende işletmeler, Dönemsel çalışanların ve uzun süreli çalışan sahiptir. Genellikle şirket içi hesapları oluşturabilir ve daha uzun vadeli tam zamanlı çalışanlar için etki alanına katılmış makinelerini kullanın. Ancak dönemsel çalışanların kuruluşun shorter-term üyeleridir ve burada kullanıcı lisanslarını daha kolay taşınabildiğinden, hesaplarını yönetmek için tercih edilir. Bu kullanıcılar, Office 365 lisansları ile bulutta, kullanıcı hesaplarını oluşturduğunuzda, bunlar ayrıldıktan sonra daha fazla esneklikle lisanslarını bakım yaparken, Windows ve Office uygulamaları ile bir Azure AD hesap oturum açma avantajlardan yararlanabilirsiniz.

### <a name="scenario-4-additional-scenarios"></a>Senaryo 4: Ek senaryoları
Yukarıda açıklanan avantajları ile birlikte, kullanıcılarınızın cihazlarını bir katılma sunan Basitleştirilmiş deneyim, etkin cihaz yönetimi, otomatik mobil cihaz Yönetimi kaydı ve Azure ad çoklu oturum açma nedeniyle Azure AD'ye katılmak zorunda yararlanır ve Şirket içi kaynaklar.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Azure AD katılım için dikkat edilecekler
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Cihaz şirkete doğrudan Azure AD'ye etkinleştirin
Kurumlar, iş ortağı şirketler ve kurumlar için yalnızca bulut hesapları sağlayabilir. Bu iş ortakları daha sonra kolayca şirket uygulamalarına ve çoklu oturum açma ile kaynaklara erişebilirsiniz. Bu senaryo, kimlik doğrulaması için Azure AD kullanan Office 365 veya SaaS uygulamaları gibi birincil buluttaki kaynaklara erişen kullanıcılar için geçerlidir.

### <a name="prerequisites"></a>Önkoşullar
**Kurumsal düzeyde (Yönetici)**

* Azure Active Directory ile Azure aboneliği  

**Kullanıcı düzeyinde**

* Windows 10 (Professional ve Enterprise sürümleri)

### <a name="administrator-tasks"></a>Yönetici görevleri
* [Cihaz kaydı oluşturma](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kullanıcı görevleri
* [Kurulum sırasında Azure AD ile yeni bir Windows 10 cihazı ayarlama](active-directory-azureadjoin-user-frx.md)
* [Ayarlar'dan Azure AD ile bir Windows 10 cihazı ayarlama](active-directory-azureadjoin-user-upgrade.md)
* [Kişisel bir Windows 10 cihazı kuruluşunuza katma](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Windows 10 için kuruluşunuzda KCG'yi etkinleştirme
Kendi kişisel Windows cihazlarını (BYOD) şirket uygulamalarına erişmelerini sağlamak ve kaynakları kullanmak için kullanıcılar ve çalışanlar ayarlayabilirsiniz. Kullanıcılarınızın kişisel bir Windows cihazı güvenli ve uyumlu bir biçimde kaynaklara erişmek için Azure AD hesaplarına (iş veya Okul hesapları) ekleyebilirsiniz.

### <a name="prerequisites"></a>Önkoşullar
**Kurumsal düzeyde (Yönetici)**

* Azure AD aboneliğiniz

**Kullanıcı düzeyinde**

* Windows 10 (Professional ve Enterprise sürümleri)

### <a name="administrator-tasks"></a>Yönetici görevleri
* [Cihaz kaydı oluşturma](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kullanıcı görevleri
* [Kişisel bir Windows 10 cihazı kuruluşunuza katma](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>Ek bilgiler
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passport aracılığıyla parola olmadan kimlik doğrulaması](active-directory-azureadjoin-passport.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılım ayarlama](active-directory-azureadjoin-setup.md)

