---
title: Birden çok Azure AD’yi tek bir AD FS ile birleştirme | Microsoft Docs
description: Bu belgede, birden çok Azure AD’yi tek bir AD FS ile nasıl birleştirebileceğinizi öğreneceksiniz.
keywords: federasyon, ADFS, AD FS, birden çok kiracı, tek AD FS, bir ADFS, çok kiracılı federasyon, çok ormanlı adfs, aad bağlantısı, federasyon oluşturma, kiracılar arası federasyon
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/17/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 620255896e02319675928396c3d6e5e0d9865c0c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60244396"
---
# <a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a>Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme

Aralarında iki yönlü güven varsa, tek bir yüksek kullanılabilirlikli AD FS ormanı birden çok ormanı birleştirebilir. Bu birden çok orman, aynı Azure Active Directory’ye karşılık gelebilir veya gelmeyebilir. Bu makale, tek AD FS dağıtımı ile farklı Azure AD’ye eşitlenen birden çok orman arasında nasıl federasyon yapılandırabileceğinizle ilgili yönergeleri içerir.

![Tek AD FS ile çok kiracılı federasyon](./media/how-to-connect-fed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> Bu senaryoda cihaz geri yazma ve otomatik cihaz katılımı desteklenmez.

> [!NOTE]
> Azure AD Connect tek bir Azure AD’deki etki alanları için federasyon yapılandırabildiğinden, bu senaryoda federasyon yapılandırmak için kullanılamaz.

## <a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a>AD FS’yi birden çok Azure AD ile birleştirmek için uygulanması gereken adımlar

contoso.onmicrosoft.com adresli Azure Active Directory’deki contoso.com etki alanının zaten contoso.com’da şirket içi Active Directory ortamında yüklü şirket içi AD FS ile birleştirildiğini göz önünde bulundurun. Fabrikam.com, fabrikam.onmicrosoft.com adresli Azure Active Directory’de bir etki alanıdır.

## <a name="step-1-establish-a-two-way-trust"></a>1. Adım: Çift yönlü bir güven
 
contoso.com’daki AD FS’nin fabrikam.com’da kullanıcıların kimliklerini doğrulayabilmesi için contoso.com ile fabrikam.com arasında iki yönlü bir güven gerekir. İki yönlü güveni oluşturmak için bu [makaledeki](https://technet.microsoft.com/library/cc816590.aspx) kılavuzu izleyin.
 
## <a name="step-2-modify-contosocom-federation-settings"></a>2. Adım: Contoso.com Federasyon ayarlarını değiştirme 
 
AD FS ile birleştirilmiş tek bir etki alanı için varsayılan veren ayarlama "http\://ADFSServiceFQDN/adfs/services/trust" Örneğin, `http://fs.contoso.com/adfs/services/trust`. Azure Active Directory, federasyona eklenen her etki alanı için benzersiz bir veren gerektirir. İki etki alanı aynı AD FS tarafından federasyona ekleneceğinden, AD FS’nin Azure Active Directory ile birleştirdiği her etki alanında benzersiz olması için veren değerinin değiştirilmesi gerekir. 
 
AD FS sunucusunda Azure AD PowerShell'i açın (MSOnline modülünün yüklendiğinden emin olun) ve aşağıdaki adımları gerçekleştirin:
 
contoso.com etki alanını içeren Azure Active Directory’ye bağlanın: Connect-MsolService contoso.com için federasyon ayarlarını güncelleştirin: Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain
 
Etki alanı Federasyon ayarındaki veren değiştirilir "http\:/ / contoso.com/adfs/services/trust" ve bir verme talebi kuralı, Azure AD bağlı olan taraf UPN son ekine bağlı olarak doğru İssuerıd değerini vermesi güveni için eklenir.
 
## <a name="step-3-federate-fabrikamcom-with-ad-fs"></a>3. Adım: AD FS ile Federasyonu kullanan fabrikam.com
 
Azure AD powershell oturumunda, aşağıdaki adımları gerçekleştirin: Azure Active Directory ile Fabrikam.com etki alanını içeren bağlanma

    Connect-MsolService
fabrikam.com yönetilen etki alanını federasyon etki alanına dönüştürün:

    Convert-MsolDomainToFederated -DomainName fabrikam.com -Verbose -SupportMultipleDomain
 
Yukarıdaki işlem, fabrikam.com etki alanını aynı AD FS ile birleştirir. Her iki etki alanı için de Get-MsolDomainFederationSettings komutunu kullanarak etki alanı ayarlarını doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile Active Directory’yi bağlama](whatis-hybrid-identity.md)
