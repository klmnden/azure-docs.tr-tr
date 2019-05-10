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
ms.openlocfilehash: f7aee10780512e284faccadface0dc928ef8270e
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65501891"
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Azure Active Directory için Self Servis kaydolma nedir?

Bu makalede, Azure Active Directory (Azure AD) içinde bir kuruluş doldurmak için Self Servis kaydolma özelliğini kullanmayı açıklar. Bir etki alanı adının üzerine yönetilmeyen bir Azure blobundaki göz atmak istiyorsanız AD kuruluş bkz [yönetilmeyen bir dizini yönetici olarak ele](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Self Servis kaydolma neden kullanmalısınız?
* Müşterilere daha hızlı istedikleri Hizmetleri Al
* E-posta tabanlı teklifleri için bir hizmet oluşturun
* Hızlı bir şekilde kolay hatırlanır iş e-posta diğer adlarını kullanarak kimlikleri oluşturmak kullanıcıların e-posta tabanlı kaydolma akışlar oluşturma
* Bir self-servis tarafından oluşturulan Azure AD dizini, diğer hizmetler için kullanılabilir yönetilen bir dizine etkinleştirilebilir

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
* **Self Servis kaydolma**: Bu, bir bulut hizmeti için bir kullanıcı kaydolduğunda ve otomatik olarak bunlar için Azure AD'de oluşturulan bir kimlik kendi e-posta etki alanına dayalı olan yöntemdir.
* **Azure AD dizini yönetilmeyen**: Bu kimliğe oluşturulduğu dizinidir. Yönetilmeyen bir dizin yok genel yönetici olan bir dizindir.
* **E-posta adresi doğrulanan kullanıcı**: Azure AD'de kullanıcı hesabı türü budur. Self Servis bir teklif için kaydolan sonra otomatik olarak oluşturulan bir kimliğe sahip bir kullanıcı bir e-posta adresi doğrulanan kullanıcı olarak bilinir. Bir e-posta adresi doğrulanan kullanıcı creationmethod ile etiketlenmiş bir dizin normal üyesidir EmailVerified =.

## <a name="how-do-i-control-self-service-settings"></a>Self Servis ayarlarını nasıl kontrol edebilirim?
Yöneticiler Self Servis iki denetimi bugün sahiptir. Kontrol edebilirsiniz olmadığını:

* Kullanıcıların e-posta yoluyla dizine katılabilir
* Uygulamalar ve hizmetler için kullanıcıların kendilerini lisanslayabilirsiniz

### <a name="how-can-i-control-these-capabilities"></a>Bu özelliklerin nasıl kontrol edebilir?
Bir yönetici, aşağıdaki Azure AD'ye cmdlet'i Set-MsolCompanySettings parametreleri kullanarak bu yetenekleri yapılandırabilirsiniz:

* **AllowEmailVerifiedUsers** bir kullanıcı oluşturabilir veya bir dizin katılın olup olmadığını denetler. Bu parametre $false olarak ayarlarsanız, e-posta adresi doğrulanan kullanıcı dizini katılabilirsiniz.
* **AllowAdHocSubscriptions** kullanıcıların Self Servis kaydolma gerçekleştirmek denetler. Bu parametre $false olarak ayarlarsanız, hiçbir kullanıcı Self Servis kaydolma gerçekleştirebilirsiniz.
  
AllowEmailVerifiedUsers ve AllowAdHocSubscriptions bir yönetilen veya yönetilmeyen bir dizin için uygulanabilir dizin genelinde ayarlarıdır. İşte bir örnek burada:

* Doğrulanmış bir etki alanı contoso.com gibi bir dizinle yönetme
* Henüz yoksa bir davet etmek için farklı bir dizinden B2B işbirliği kullandığınız (userdoesnotexist@contoso.com) contoso.com giriş dizininde
* Açık AllowEmailVerifiedUsers giriş dizinine sahiptir

Önceki koşul true ise, ardından bir üye kullanıcı giriş dizininde oluşturulur ve B2B Konuk kullanıcı davet etme dizininde oluşturulur.

Flow ve PowerApps deneme kaydolmalar tarafından denetlenir değil **AllowAdHocSubscriptions** ayarı. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Mevcut kullanıcılarımın Power BI'ı kullanmaya başlamasını nasıl önleyebilirim?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Soru- cevap, kuruluşunuzdaki flow](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Denetimleri birlikte nasıl çalışır?
Bu iki parametre birlikte, Self Servis kayıt üzerinde daha kesin denetim tanımlamak için kullanılabilir. Örneğin, aşağıdaki komut söz konusu kullanıcıların Azure AD'de bir hesap zaten varsa, ancak yalnızca Self Servis kaydolma gerçekleştirmek imkan tanıyacak (diğer bir deyişle, oluşturulacak bir e-posta adresi doğrulanan hesap gereken kullanıcılar ilk Self Servis kaydolma gerçekleştirilemiyor):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

Aşağıdaki akış, bu parametrelerin farklı birleşimlerini ve Self Servis kaydolma ve dizin için elde edilen koşullar açıklanmaktadır.

![self servis kaydolma denetimleri akış çizelgesi](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

Daha fazla bilgi ve bu parametreleri kullanma örnekleri için bkz. [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Sonraki adımlar

* [Özel etki alanı adını Azure AD'ye ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)
