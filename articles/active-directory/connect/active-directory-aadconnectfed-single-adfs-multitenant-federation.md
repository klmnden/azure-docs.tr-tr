---
title: "Birden çok Azure AD’yi tek bir AD FS ile birleştirme | Microsoft Docs"
description: "Bu belgede, birden çok Azure AD’yi tek bir AD FS ile nasıl birleştirebileceğinizi öğreneceksiniz."
keywords: "federasyon, ADFS, AD FS, birden çok kiracı, tek AD FS, bir ADFS, çok kiracılı federasyon, çok ormanlı adfs, aad bağlantısı, federasyon oluşturma, kiracılar arası federasyon"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: dcdf9d0539e71a2b1e9fd12f4b97e6c1e8653c79
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a>Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme

Aralarında iki yönlü güven varsa, tek bir yüksek kullanılabilirlikli AD FS ormanı birden çok ormanı birleştirebilir. Bu birden çok orman, aynı Azure Active Directory’ye karşılık gelebilir veya gelmeyebilir. Bu makale, tek AD FS dağıtımı ile farklı Azure AD’ye eşitlenen birden çok orman arasında nasıl federasyon yapılandırabileceğinizle ilgili yönergeleri içerir.

![Tek AD FS ile çok kiracılı federasyon](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> Bu senaryoda cihaz geri yazma ve otomatik cihaz katılımı desteklenmez.

> [!NOTE]
> Azure AD Connect tek bir Azure AD’deki etki alanları için federasyon yapılandırabildiğinden, bu senaryoda federasyon yapılandırmak için kullanılamaz.

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a>AD FS’yi birden çok Azure AD ile birleştirmek için uygulanması gereken adımlar

contoso.onmicrosoft.com adresli Azure Active Directory’deki contoso.com etki alanının zaten contoso.com’da şirket içi Active Directory ortamında yüklü şirket içi AD FS ile birleştirildiğini göz önünde bulundurun. Fabrikam.com, fabrikam.onmicrosoft.com adresli Azure Active Directory’de bir etki alanıdır.

##<a name="step-1-establish-a-two-way-trust"></a>1. Adım: İki yönlü güven kurma
 
contoso.com’daki AD FS’nin fabrikam.com’da kullanıcıların kimliklerini doğrulayabilmesi için contoso.com ile fabrikam.com arasında iki yönlü bir güven gerekir. İki yönlü güveni oluşturmak için bu [makaledeki](https://technet.microsoft.com/library/cc816590.aspx) kılavuzu izleyin.
 
##<a name="step-2-modify-contosocom-federation-settings"></a>2. Adım: contoso.com federasyon ayarlarını değiştirme 
 
AD FS ile birleştirilmiş tek bir etki alanı için ayarlanan varsayılan veren şudur: "http://ADFSServiceFQDN/adfs/services/trust". Örneğin, “http://fs.contoso.com/adfs/services/trust”. Azure Active Directory, federasyona eklenen her etki alanı için benzersiz bir veren gerektirir. İki etki alanı aynı AD FS tarafından federasyona ekleneceğinden, AD FS’nin Azure Active Directory ile birleştirdiği her etki alanında benzersiz olması için veren değerinin değiştirilmesi gerekir. 
 
AD FS sunucusunda Azure AD PowerShell’i açın ve aşağıdaki adımları gerçekleştirin:
 
contoso.com etki alanını içeren Azure Active Directory’ye bağlanın: Connect-MsolService contoso.com için federasyon ayarlarını güncelleştirin: Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain
 
Etki alanı federasyon ayarındaki veren "http://contoso.com/adfs/services/trust" olarak değiştirilir ve Azure AD Bağlı Olan Taraf Güveni’nin UPN son ekine bağlı olarak doğru issuerId değerini vermesi için bir verme talebi kuralı eklenir.
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a>3. adım: fabrikam.com’u AD FS ile birleştirin
 
Azure AD powershell oturumunda şu adımları gerçekleştirin: fabrikam.com etki alanını içeren Azure Active Directory’ye bağlanın

    Connect-MsolService
fabrikam.com yönetilen etki alanını federasyon etki alanına dönüştürün:

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
Yukarıdaki işlem, fabrikam.com etki alanını aynı AD FS ile birleştirir. Her iki etki alanı için de Get-MsolDomainFederationSettings komutunu kullanarak etki alanı ayarlarını doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile Active Directory’yi bağlama](active-directory-aadconnect.md)
