---
title: Kullanım senaryoları ve Azure AD katılım için dağıtım konuları | Microsoft Docs
description: Nasıl yöneticileri Azure AD'ye katılımı kendi son kullanıcıları için (çalışanlar, Öğrenciler, diğer kullanıcıları) ayarlayacakları açıklanmaktadır. Ayrıca, Azure AD katılım kullanmak için farklı gerçek senaryolar anlatılmaktadır.
services: active-directory
documentationcenter: ''
author: femila
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 173ad6f07699ca6bfa534dedc053663bba571382
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26602439"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Kullanım senaryoları ve Azure AD katılım için dağıtım konuları
## <a name="usage-scenarios-for-azure-ad-join"></a>Azure AD katılım için kullanım senaryoları
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Senaryo 1: İşletmeler büyük ölçüde bulutta
Azure Active Directory katılım (Azure AD katılımı), şu anda çalışır ve işinizi buluttaki için kimlikleri yönetme veya buluta yakında taşıyorsanız, yararlanabilirsiniz. Windows 10'a oturum açmak için Azure AD içinde oluşturduğunuz bir hesap kullanabilirsiniz. Aracılığıyla [ilk çalıştırma deneyimi (FRX) işlem](active-directory-azureadjoin-user-frx.md), Azure AD'den katılarak ya [ayarlar menüsünü](active-directory-azureadjoin-user-upgrade.md), kullanıcılarınızın kendi makinelerine Azure AD ile birleştirebilirsiniz.  Kullanıcılarınızın, aynı zamanda çoklu oturum açma (SSO) erişimi tarayıcılarını veya Office uygulamaları, Office 365 gibi bulut kaynaklarına keyfini çıkarabilirsiniz.

### <a name="scenario-2-educational-institutions"></a>Senaryo 2: Eğitim kurumları
Eğitim kurumları genellikle iki kullanıcı türlerine sahip: Fakülte ve öğrenciler. Fakülte üyeleri kuruluş daha uzun vadeli üyeleri olarak kabul edilir. Şirket içi hesapları kullanabiliyor tercih edilir. Ancak Öğrenciler kuruluşun shorter-term üyeleridir ve hesaplarını Azure AD'de yönetilebilir. Bu dizin ölçek şirket içi depolanan olmak yerine buluta gönderilemez anlamına gelir. Öğrenciler için Windows Azure AD hesaplarına ile oturum açın ve Office uygulamalarında Office 365 kaynaklara erişmek için olmayacağı anlamına gelir.

### <a name="scenario-3-retail-businesses"></a>Senaryo 3: Perakende işletmeler
Perakende işletmeler Mevsimlik çalışanları ve uzun vadeli çalışanların sahip. Genellikle şirket içi hesapları oluşturabilir ve etki alanına katılan makineler daha uzun vadeli tam zamanlı çalışanlar için kullanın. Ancak kuruluşun shorter-term üyeleri Mevsimlik çalışanlardır ve burada kullanıcı lisansları daha kolay taşınabildiğinden, hesaplarını yönetmek için tercih edilir. Office 365 lisansına sahip bulutta, kullanıcı hesaplarını oluşturduğunuzda, bu kullanıcılar bunlar çıktıktan sonra lisansları ile daha fazla esneklik bakım yaparken Windows ve Office uygulamaları, Azure AD hesap ile oturum açma avantajlarını alır.

### <a name="scenario-4-additional-scenarios"></a>Senaryo 4: İlave senaryolar
Daha önce bahsedilen avantajları yanı sıra, bir Basitleştirilmiş katılma deneyimi, verimli aygıt yönetimi, otomatik mobil cihaz Yönetimi kayıt ve Azure ad çoklu oturum açma nedeniyle cihazlarını Azure AD'ye ekler, kullanıcılarınızın kalmaktan yararlanabilir ve şirket içi kaynaklar.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Azure AD katılım için dağıtım konuları
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Şirkete ait bir cihazı doğrudan Azure AD birleştirme etkinleştirin
Kuruluşlar, iş ortağı şirketlerden ve kuruluşlara yalnızca bulut hesapları sağlayabilir. Bu iş ortaklarının daha sonra kolayca şirket uygulamaları ve çoklu oturum açma ile kaynaklarına erişebilirsiniz. Bu senaryo, kimlik doğrulaması için Azure AD Bel Office 365 veya SaaS uygulamaları gibi birincil buluttaki kaynaklara erişen kullanıcılar için geçerlidir.

### <a name="prerequisites"></a>Ön koşullar
**Kurumsal düzeyde (Yönetici)**

* Azure Active Directory ile Azure aboneliği  

**Kullanıcı düzeyinde**

* Windows 10 (Professional ve Enterprise sürümleri)

### <a name="administrator-tasks"></a>Yönetici görevleri
* [Cihaz kaydı oluşturma](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kullanıcı görevleri
* [Kurulum sırasında Azure AD ile yeni bir Windows 10 cihazı ayarlama](active-directory-azureadjoin-user-frx.md)
* [Ayarlar'dan Azure AD ile Windows 10 cihazı ayarlama](active-directory-azureadjoin-user-upgrade.md)
* [Kişisel bir Windows 10 cihazı kuruluşunuza katma](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Windows 10 için kuruluşunuzdaki KCG etkinleştir
Kendi kişisel Windows cihazlarını (BYOD) şirket uygulamalarına ve kaynaklarına kullanmak için kullanıcılar ve çalışanlar ayarlayabilirsiniz. Kullanıcılarınızın kişisel bir Windows aygıtı kaynaklarına güvenli ve uyumlu bir şekilde erişmek için Azure AD hesaplarına (iş veya Okul hesapları) ekleyebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar
**Kurumsal düzeyde (Yönetici)**

* Azure AD aboneliği

**Kullanıcı düzeyinde**

* Windows 10 (Professional ve Enterprise sürümleri)

### <a name="administrator-tasks"></a>Yönetici görevleri
* [Cihaz kaydı oluşturma](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kullanıcı görevleri
* [Kişisel bir Windows 10 cihazı kuruluşunuza katma](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>Ek bilgiler
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passport aracılığıyla parolası olmayan kimlikleri doğrulama](active-directory-azureadjoin-passport.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılım ayarlama](active-directory-azureadjoin-setup.md)

