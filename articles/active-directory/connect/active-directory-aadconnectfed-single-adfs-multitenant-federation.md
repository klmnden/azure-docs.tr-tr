---
title: "Birden çok Azure AD’yi tek bir AD FS ile birleştirme | Microsoft Docs"
description: "Bu belgede, birden çok Azure AD’yi tek bir AD FS ile nasıl birleştirebileceğinizi öğreneceksiniz."
keywords: "federasyon, ADFS, AD FS, birden çok kiracı, tek AD FS, bir ADFS, çok kiracılı federasyon, çok ormanlı adfs, aad bağlantısı, federasyon oluşturma, kiracılar arası federasyon"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/09/2017
ms.author: anandy; billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: 22f2bcfdd8c3978a6924c8c8cdea2744001000fe
ms.contentlocale: tr-tr
ms.lasthandoff: 04/10/2017

---

#Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme
<a id="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs" class="xliff"></a>

Aralarında iki yönlü güven varsa, tek bir yüksek kullanılabilirlikli AD FS ormanı birden çok ormanı birleştirebilir. Bu birden çok orman, aynı Azure Active Directory’ye karşılık gelebilir veya gelmeyebilir. Bu makale, tek AD FS dağıtımı ile farklı Azure AD’ye eşitlenen birden çok orman arasında nasıl federasyon yapılandırabileceğinizle ilgili yönergeleri içerir.

![Tek AD FS ile çok kiracılı federasyon](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> Bu senaryoda cihaz geri yazma ve otomatik cihaz katılımı desteklenmez.

> [!NOTE]
> Azure AD Connect tek bir Azure AD’deki etki alanları için federasyon yapılandırabildiğinden, bu senaryoda federasyon yapılandırmak için kullanılamaz.

##AD FS’yi birden çok Azure AD ile birleştirmek için uygulanması gereken adımlar
<a id="steps-for-federating-ad-fs-with-multiple-azure-ad" class="xliff"></a>

contoso.onmicrosoft.com adresli Azure Active Directory’deki contoso.com etki alanının zaten contoso.com’da şirket içi Active Directory ortamında yüklü şirket içi AD FS ile birleştirildiğini göz önünde bulundurun. Fabrikam.com, fabrikam.onmicrosoft.com adresli Azure Active Directory’de bir etki alanıdır.

##1. Adım: İki yönlü güven kurma
<a id="step-1-establish-a-two-way-trust" class="xliff"></a>
 
contoso.com’daki AD FS’nin fabrikam.com’da kullanıcıların kimliklerini doğrulayabilmesi için contoso.com ile fabrikam.com arasında iki yönlü bir güven gerekir. İki yönlü güveni oluşturmak için bu [makaledeki](https://technet.microsoft.com/library/cc816590.aspx) kılavuzu izleyin.
 
##2. Adım: contoso.com federasyon ayarlarını değiştirme
<a id="step-2-modify-contosocom-federation-settings" class="xliff"></a> 
 
AD FS ile birleştirilmiş tek bir etki alanı için ayarlanan varsayılan veren şudur: "http://ADFSServiceFQDN/adfs/services/trust". Örneğin, “http://fs.contoso.com/adfs/services/trust”. Azure Active Directory, federasyona eklenen her etki alanı için benzersiz bir veren gerektirir. İki etki alanı aynı AD FS tarafından federasyona ekleneceğinden, AD FS’nin Azure Active Directory ile birleştirdiği her etki alanında benzersiz olması için veren değerinin değiştirilmesi gerekir. 
 
AD FS sunucusunda Azure AD PowerShell’i açın ve aşağıdaki adımları gerçekleştirin:
 
contoso.com etki alanını içeren Azure Active Directory’ye bağlanın: Connect-MsolService contoso.com için federasyon ayarlarını güncelleştirin: Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain
 
Etki alanı federasyon ayarındaki veren "http://contoso.com/adfs/services/trust" olarak değiştirilir ve Azure AD Bağlı Olan Taraf Güveni’nin UPN son ekine bağlı olarak doğru issuerId değerini vermesi için bir verme talebi kuralı eklenir.
 
##3. adım: fabrikam.com’u AD FS ile birleştirin
<a id="step-3-federate-fabrikamcom-with-ad-fs" class="xliff"></a>
 
Azure AD powershell oturumunda şu adımları gerçekleştirin: fabrikam.com etki alanını içeren Azure Active Directory’ye bağlanın

    Connect-MsolService
fabrikam.com yönetilen etki alanını federasyon etki alanına dönüştürün:

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
Yukarıdaki işlem, fabrikam.com etki alanını aynı AD FS ile birleştirir. Her iki etki alanı için de Get-MsolDomainFederationSettings komutunu kullanarak etki alanı ayarlarını doğrulayabilirsiniz.

## Sonraki adımlar
<a id="next-steps" class="xliff"></a>
[Azure Active Directory ile Active Directory’yi bağlama](active-directory-aadconnect.md)

