---
title: Azure Active Directory'de Self Servis veya deneme kaydolma | Microsoft Docs
description: Bir Azure Active Directory (Azure AD) Kiracı Self Servis kaydolma kullanın
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 01/28/2018
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 99c5e99fa3bd33ef42e8df6ceba5be4be2cd1249
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872655"
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Azure Active Directory için Self Servis kaydolma nedir?
Bu makalede, Self Servis kaydolma ve Azure Active Directory (Azure AD) desteklemek nasıl açıklanmaktadır. Bir etki alanı adının üzerine göz atmak istiyorsanız yönetilmeyen bir Azure AD kiracısı için bkz: [yönetilmeyen bir dizini yönetici olarak ele](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Self Servis kaydolma neden kullanmalısınız?
* Müşterilere daha hızlı istedikleri Hizmetleri Al
* E-posta tabanlı teklifleri için bir hizmet oluşturun
* Hızlı bir şekilde kolay hatırlanır iş e-posta diğer adlarını kullanarak kimlikleri oluşturmak kullanıcıların e-posta tabanlı kaydolma akışlar oluşturma
* Bir self-servis tarafından oluşturulan Azure AD dizini, diğer hizmetler için kullanılabilir yönetilen bir dizine etkinleştirilebilir

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
* **Self Servis kaydolma**:, bir bulut hizmeti için bir kullanıcı kaydolduğunda ve bunlar için Azure AD'de otomatik olarak oluşturulan bir kimlik tabanlı sahip e-posta etki yöntem budur.
* **Azure AD dizini yönetilmeyen**: Bu kimliğe oluşturulduğu dizinidir. Yönetilmeyen bir dizin yok genel yönetici olan bir dizindir.
* **E-posta adresi doğrulanan kullanıcı**: Azure AD'de kullanıcı hesabı türü budur. Self Servis bir teklif için kaydolan sonra otomatik olarak oluşturulan bir kimliğe sahip bir kullanıcı bir e-posta adresi doğrulanan kullanıcı olarak bilinir. Bir e-posta adresi doğrulanan kullanıcı creationmethod ile etiketlenmiş bir dizin normal üyesidir EmailVerified =.

## <a name="how-do-i-control-self-service-settings"></a>Self Servis ayarlarını nasıl kontrol edebilirim?
Yöneticiler Self Servis iki denetimi bugün sahiptir. Kontrol edebilirsiniz olmadığını:

* Kullanıcıların e-posta yoluyla dizine katılabilirsiniz.
* Kullanıcıların kendilerini uygulamalar ve hizmetler için lisans verebilirsiniz.

### <a name="how-can-i-control-these-capabilities"></a>Bu özelliklerin nasıl kontrol edebilir?
Bir yönetici, aşağıdaki Azure AD'ye cmdlet'i Set-MsolCompanySettings parametreleri kullanarak bu yetenekleri yapılandırabilirsiniz:

* **AllowEmailVerifiedUsers** bir kullanıcı oluşturabilir veya yönetilmeyen bir dizini katılın olup olmadığını denetler. Bu parametre $false olarak ayarlarsanız, e-posta adresi doğrulanan kullanıcı dizini katılabilirsiniz.
* **AllowAdHocSubscriptions** kullanıcıların Self Servis kaydolma gerçekleştirmek denetler. Bu parametre $false olarak ayarlarsanız, hiçbir kullanıcı Self Servis kaydolma gerçekleştirebilirsiniz. 
  
  > [!NOTE]
  > Flow ve PowerApps deneme kaydolmalar tarafından denetlenir değil **AllowAdHocSubscriptions** ayarı. Daha fazla bilgi için aşağıdaki makalelere bakın:
  > * [Mevcut kullanıcılarımın Power BI'ı kullanmaya başlamasını nasıl önleyebilirim?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
  > * [Soru- cevap, kuruluşunuzdaki flow](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Denetimleri birlikte nasıl çalışır?
Bu iki parametre birlikte, Self Servis kayıt üzerinde daha kesin denetim tanımlamak için kullanılabilir. Örneğin, aşağıdaki komut söz konusu kullanıcıların Azure AD'de bir hesap zaten varsa, ancak yalnızca Self Servis kaydolma gerçekleştirmek imkan tanıyacak (diğer bir deyişle, oluşturulacak bir e-posta adresi doğrulanan hesap gereken kullanıcılar ilk Self Servis kaydolma gerçekleştirilemiyor):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Aşağıdaki akış, bu parametrelerin farklı birleşimlerini ve Self Servis kaydolma ve dizin için elde edilen koşullar açıklanmaktadır.

![][1]

Daha fazla bilgi ve bu parametreleri kullanma örnekleri için bkz. [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adını Azure AD'ye ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/directory-self-service-signup/SelfServiceSignUpControls.png
