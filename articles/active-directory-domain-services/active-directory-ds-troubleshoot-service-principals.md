---
title: "Azure Active Directory etki alanı Hizmetleri: Hizmet asıl yapılandırmasıyla ilgili sorunları giderme | Microsoft Docs"
description: "Azure AD etki alanı Hizmetleri için hizmet asıl yapılandırmasıyla ilgili sorunları giderme"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Azure AD etki alanı Hizmetleri - sorun giderme hizmet sorumlusu yapılandırma

Hizmet, yönetmek ve etki alanınızı güncelleştirmek için Microsoft çeşitli kullanır [hizmet asıl adı](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) dizininize ile iletişim kurmak için. Bir ayar yanlış yapılandırılmış veya silinen, hizmetinizi bir kesintiye neden olabilir.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Hizmet sorumlusu bulunamadı

**Uyarı iletisi:**

*Azure AD dizininizi Azure AD etki alanı Hizmetleri düzgün çalışması gerekli bir hizmet sorumlusu silinmiş olabilir. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.*

Hizmet sorumluları yönetmek, güncelleştirme ve yönetilen etki alanınızı korumak için Microsoft kullanan uygulamalardır. Bunlar silindiğinde, Microsoft'un etki alanınızın hizmet yeteneği keser. Hangi hizmet sorumluları oluşturulmaları gerekeceğini belirlemek için aşağıdaki adımları kullanın.

1. Gidin [kurumsal uygulamalar - tüm uygulamaları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) Azure portalında sayfası.
2. Göster açılan listeyi kullanarak seçin **tüm uygulamaları** tıklatıp **Uygula**.
3. Aşağıdaki tabloda kullanarak, arama kimliği arama kutusuna yapıştırarak ve tuşlarına basarak her uygulama kimliği girin. Arama sonuçlarını boşsa, "Çözümleme" sütun içindeki adımları izleyerek hizmet sorumlusu yeniden oluşturmanız gerekir.

| Uygulama Kimliği | Çözüm |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [PowerShell ile eksik bir hizmet sorumlusu yeniden oluşturma](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Microsoft.AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Microsoft.AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Kendi kendini düzeltmek hizmet asıl adı](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>PowerShell ile eksik bir hizmet sorumlusu oluşturun

*Eksik kimlikleri için şu adımları izleyin: 2565bd9d-da50-47d4-8b85-4c97f669dc36*

**Düzeltme:**

Bu adımları tamamlamak için Azure AD PowerShell gerekir. Azure AD PowerShell yükleme hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Bu sorunu gidermek için bir PowerShell penceresinde aşağıdaki komutları yazın:
1. Install-Module AzureAD
2. Import-Module AzureAD
3. Aşağıdaki PowerShell komutunu yürüterek Azure AD etki alanı Hizmetleri için gereken hizmet asıl dizininizde eksik olup olmadığını denetleyin:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. Aşağıdaki PowerShell komutunu yazarak hizmet sorumlusu oluşturun:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Eksik hizmet sorumlusu oluşturduktan sonra iki saat bekleyin ve etki alanınızın sistem durumunu denetleyin.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Azure portalını kullanarak Microsoft AAD ad alanına yeniden kaydolun

*Eksik kimlikleri için şu adımları izleyin: 443155a6-77f3-45e3-882b-22b3a8d431fb ve abba844e-bc0e-44b0-947a-dc74e5d09022*

**Düzeltme:**

Etki Alanı Hizmetleri dizininiz üzerinde geri yüklemek için aşağıdaki adımları kullanın:

1. Gidin [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında sayfası.
2. Yönetilen etki alanı ile ilişkili tablodan abonelik seçin
3. Sol taraftaki gezinti kullanarak seçin **kaynak sağlayıcıları**
4. Tabloda "Microsoft.AAD" için arama ve tıklayın **yeniden kaydolun**
5. Uyarınız çözülmüş emin olmak için kontrol etmek için iki saat içinde sistem durumu uyarı sayfasını görüntüleyin.


### <a name="service-principals-that-self-correct"></a>Hizmet sorumluları, kendi kendini düzeltin

*Eksik kimlikleri için şu adımları izleyin: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Düzeltme:**

Belirli hizmet asıl adı eksik, yanlış ya da silinmiş olduğunda Microsoft tanımlayabilirsiniz. Hizmet hızlı bir şekilde çözmek için Microsoft hizmet sorumluları yeniden oluşturur. Etki alanınızın sistem durumu asıl yeniden emin olmak için iki saat sonra denetleyin.

## <a name="contact-us"></a>Bizimle İletişim Kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
