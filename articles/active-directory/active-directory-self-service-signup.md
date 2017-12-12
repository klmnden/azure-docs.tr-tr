---
title: Azure Active Directory'de Self Servis veya viral kaydolma | Microsoft Docs
description: "Bir Azure Active Directory (Azure AD) Kiracı Self Servis kaydolma kullanın"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/03/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 2b41bb1b72cc773c29d464228c3177fbd1d9f5e0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Azure Active Directory için Self Servis kaydolma nedir?
Bu makalede, Self Servis kaydolma ve Azure Active Directory (Azure AD) desteklemek nasıl açıklanmaktadır. Bir etki alanı adı almak istiyorsanız yönetilmeyen bir Azure AD Kiracı için bkz: [yönetilmeyen bir dizini yönetici olarak ele](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Self Servis kaydolmanın neden kullanılır?
* Müşteriler için daha hızlı istedikleri hizmetlerini Al
* E-posta tabanlı teklifleri için bir hizmet oluşturun
* Hızlı bir şekilde kolay unutmayın iş e-posta benzersizse kullanarak kimlikleri oluşturmak kullanıcıların e-posta tabanlı kaydolma akışlar oluşturma
* Bir self-servis oluşturulan Azure AD dizini, diğer hizmetler için kullanılabilir yönetilen bir dizine açılabilir

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
* **Self Servis kaydolma**: Bu, bir kullanıcı için bir bulut hizmeti kaydolur ve sahip otomatik olarak bunları Azure AD'de oluşturulan bir kimlik tabanlı kendi e-posta etki alanında yöntemdir.
* **Yönetilmeyen Azure AD dizini**: Bu kimliğe oluşturulduğu dizindir. Yönetilmeyen bir dizin yok genel yönetici olan bir dizindir.
* **Kullanıcı e-posta doğrulandı**: Azure AD'de kullanıcı hesabı türü budur. Self Servis Teklif için kaydolan sonra otomatik olarak oluşturulan bir kimliğe sahip bir kullanıcı bir e-posta doğrulanmış kullanıcı olarak bilinir. Bir e-posta doğrulanan kullanıcı ile creationmethod etiketli bir dizin normal üyesidir EmailVerified =.

## <a name="how-do-i-control-self-service-settings"></a>Self Servis ayarları nasıl denetlerim?
Yöneticilere iki Self Servis denetimleri bugün sahip. Kontrol edebilirsiniz olup olmadığını:

* Kullanıcılar e-posta yoluyla dizin birleştirebilirsiniz.
* Kullanıcıların kendilerini uygulamalar ve hizmetler için lisans verebilirsiniz.

### <a name="how-can-i-control-these-capabilities"></a>Bu özelliklerin nasıl kontrol edebilir mi?
Bir yönetici, aşağıdaki Azure AD cmdlet Set-MsolCompanySettings parametreleri kullanarak bu yetenekleri yapılandırabilirsiniz:

* **AllowEmailVerifiedUsers** bir kullanıcı oluşturabilir veya yönetilmeyen bir directory katılım olup olmadığını denetler. Bu parametreyi $false olarak ayarlarsanız, e-posta doğrulanan kullanıcı dizini birleştirebilirsiniz.
* **AllowAdHocSubscriptions** Self Servis kaydolma gerçekleştirmek kullanıcılara denetler. Bu parametreyi $false olarak ayarlarsanız, hiçbir kullanıcı Self Servis kaydolma gerçekleştirebilirsiniz.

### <a name="how-do-the-controls-work-together"></a>Denetimleri birlikte nasıl çalışır?
Bu iki parametre birlikte Self Servis kaydolma üzerinde daha kesin denetim tanımlamak için kullanılabilir. Örneğin, aşağıdaki komutu kullanıcıların bu kullanıcıların Azure AD'de bir hesabınız zaten varsa ancak yalnızca Self Servis kaydolma gerçekleştirmesini sağlayacak (diğer bir deyişle, bir e-posta doğrulandı hesabı oluşturulması için gereken kullanıcılar ilk Self Servis kaydolma gerçekleştiremezsiniz):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Aşağıdaki akış çizelgesi bu parametreler için farklı kombinasyon ve Self Servis kaydolma ve dizin için sonuçta elde edilen koşullar açıklanmaktadır.

![][1]

Daha fazla bilgi ve bu parametrelerini kullanma örnekleri için bkz: [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adını Azure AD'ye ekleme](add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
