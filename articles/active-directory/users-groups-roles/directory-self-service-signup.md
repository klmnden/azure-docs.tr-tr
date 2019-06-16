---
title: E-posta adresi doğrulanan kullanıcı hesapları - Azure Active Directory için Self Servis kaydolma | Microsoft Docs
description: Bir Azure Active Directory (Azure AD) Kiracı Self Servis kaydolma kullanın
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d949b746f05eb440f5ae28f683dfc838217ab47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956502"
---
# <a name="what-is-self-service-sign-up-for-azure-active-directory"></a>Self Servis nedir kaydolmak için Azure Active Directory?

Bu makalede, Azure Active Directory (Azure AD) içinde bir kuruluş doldurmak için Self Servis kayıt kullanmayı açıklar. Bir etki alanı adının üzerine yönetilmeyen bir Azure blobundaki göz atmak istiyorsanız AD kuruluş bkz [yönetilmeyen bir dizini yönetici olarak ele](domains-admin-takeover.md).

## <a name="why-use-self-service-sign-up"></a>Self Servis kayıt neden kullanmalısınız?
* Müşterilere daha hızlı istedikleri Hizmetleri Al
* E-posta tabanlı teklifleri için bir hizmet oluşturun
* Hızlı bir şekilde kolay hatırlanır iş e-posta diğer adlarını kullanarak kimlikleri oluşturmak kullanıcıların e-posta tabanlı kayıt akışları oluşturma
* Bir self-servis tarafından oluşturulan Azure AD dizini, diğer hizmetler için kullanılabilir yönetilen bir dizine etkinleştirilebilir

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
* **Self Servis kayıt**: Bu, bir bulut hizmeti için bir kullanıcı kaydolduğunda ve otomatik olarak bunlar için Azure AD'de oluşturulan bir kimlik kendi e-posta etki alanına dayalı olan yöntemdir.
* **Azure AD dizini yönetilmeyen**: Bu kimliğe oluşturulduğu dizinidir. Yönetilmeyen bir dizin yok genel yönetici olan bir dizindir.
* **E-posta adresi doğrulanan kullanıcı**: Azure AD'de kullanıcı hesabı türü budur. Self Servis bir teklif için kaydolan sonra otomatik olarak oluşturulan bir kimliğe sahip bir kullanıcı bir e-posta adresi doğrulanan kullanıcı olarak bilinir. Bir e-posta adresi doğrulanan kullanıcı creationmethod ile etiketlenmiş bir dizin normal üyesidir EmailVerified =.

## <a name="how-do-i-control-self-service-settings"></a>Self Servis ayarlarını nasıl kontrol edebilirim?
Yöneticiler Self Servis iki denetimi bugün sahiptir. Kontrol edebilirsiniz olmadığını:

* Kullanıcıların e-posta yoluyla dizine katılabilir
* Uygulamalar ve hizmetler için kullanıcıların kendilerini lisanslayabilirsiniz

### <a name="how-can-i-control-these-capabilities"></a>Bu özelliklerin nasıl kontrol edebilir?
Bir yönetici, aşağıdaki Azure AD'ye cmdlet'i Set-MsolCompanySettings parametreleri kullanarak bu yetenekleri yapılandırabilirsiniz:

* **AllowEmailVerifiedUsers** bir kullanıcı oluşturabilir veya bir dizin katılın olup olmadığını denetler. Bu parametre $false olarak ayarlarsanız, e-posta adresi doğrulanan kullanıcı dizini katılabilirsiniz.
* **AllowAdHocSubscriptions** kullanıcıların Self Servis kayıt gerçekleştirmek denetler. Bu parametre $false olarak ayarlarsanız, hiçbir kullanıcı Self Servis kayıt gerçekleştirebilirsiniz.
  
AllowEmailVerifiedUsers ve AllowAdHocSubscriptions yönetilen veya yönetilmeyen bir dizin için uygulanabilir dizin genelinde ayarlarıdır. İşte bir örnek burada:

* Doğrulanmış bir etki alanı contoso.com gibi bir dizinle yönetme
* Henüz yoksa bir davet etmek için farklı bir dizinden B2B işbirliği kullandığınız (userdoesnotexist@contoso.com) contoso.com giriş dizininde
* Açık AllowEmailVerifiedUsers giriş dizinine sahiptir

Önceki koşul true ise, ardından bir üye kullanıcı giriş dizininde oluşturulur ve B2B Konuk kullanıcı davet etme dizininde oluşturulur.

Flow ve PowerApps deneme kaydolma tarafından denetlenir değil **AllowAdHocSubscriptions** ayarı. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Mevcut kullanıcılarımın Power BI'ı kullanmaya başlamasını nasıl önleyebilirim?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Soru- cevap, kuruluşunuzdaki flow](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Denetimleri birlikte nasıl çalışır?
Bu iki parametre birlikte, Self Servis kayıt üzerinde daha kesin denetim tanımlamak için kullanılabilir. Örneğin, aşağıdaki komutu gerçekleştirmelerini Self Servis kaydolma, ancak bu kullanıcıları Azure AD'de bir hesap zaten varsa yalnızca izin verir (diğer bir deyişle, oluşturulacak bir e-posta adresi doğrulanan hesap gereken kullanıcılar ilk Self Servis kayıt gerçekleştirilemiyor):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

Bu parametrelerin farklı birleşimlerini ve sonuçta elde edilen koşullar dizin ve Self Servis kayıt için aşağıdaki akış çizelgesi açıklanmaktadır.

![Self Servis kaydolma denetimlerin akış çizelgesi](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

Daha fazla bilgi ve bu parametreleri kullanma örnekleri için bkz. [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Sonraki adımlar

* [Özel etki alanı adını Azure AD'ye ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)
* [Yönetilmeyen bir dizini iş veya Okul hesabınızı kapatın](users-close-account.md)
