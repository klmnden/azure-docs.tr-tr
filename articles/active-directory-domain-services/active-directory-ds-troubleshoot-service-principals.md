---
title: 'Azure Active Directory etki alanı Hizmetleri: Hizmet asıl yapılandırmasıyla ilgili sorunları giderme | Microsoft Docs'
description: Azure AD etki alanı Hizmetleri için hizmet asıl yapılandırmasıyla ilgili sorunları giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: d1a605ae5c0ea598ba507de0b21a841333df79ef
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Yönetilen etki alanınız için geçersiz hizmet asıl yapılandırma sorunlarını giderme

Bu makale ve aşağıdaki uyarı iletisine neden hizmet asıl ilgili yapılandırma hataları gidermek yardımcı olur:

## <a name="alert-aadds102-service-principal-not-found"></a>Uyarı AADDS102: Hizmet sorumlusu bulunamadı

**Uyarı iletisi:** *düzgün çalışması Azure AD etki alanı Hizmetleri için gereken bir hizmet sorumlusu Azure AD dizininizi silinmiş. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.*

[Hizmet sorumluları](../active-directory/develop/active-directory-application-objects.md) yönetmek, güncelleştirme ve yönetilen etki alanınızı korumak için Microsoft kullanır uygulamalardır. Bunlar silindiğinde, Microsoft'un etki alanınızın hizmet yeteneği keser.


## <a name="check-for-missing-service-principals"></a>Onay hizmet asıl adı eksik
Hangi hizmet sorumluları oluşturulmaları gerekeceğini belirlemek için aşağıdaki adımları kullanın:

1. Gidin [kurumsal uygulamalar - tüm uygulamaları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) Azure portalında sayfası.
2. İçinde **Göster** açılan listesinde, select **tüm uygulamaları** tıklatıp **Uygula**.
3. Aşağıdaki tabloda kullanarak, arama kimliği arama kutusuna yapıştırarak ve tuşlarına basarak her uygulama kimliği girin. Arama sonuçlarını boşsa, "Çözümleme" sütun içindeki adımları izleyerek hizmet sorumlusu yeniden oluşturmanız gerekir.

| Uygulama Kimliği | Çözüm |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [PowerShell ile eksik bir hizmet sorumlusu oluşturun](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Microsoft.AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Microsoft.AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Kendi kendini düzeltmek hizmet asıl adı](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>PowerShell ile eksik bir hizmet sorumlusu oluşturun
Bir hizmet sorumlusu IF kimliği ile adımları ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` Azure AD dizininizi eksik.

**Çözüm:** bu adımları tamamlamak için Azure AD PowerShell gerekir. Azure AD PowerShell yükleme hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Bu sorunu gidermek için bir PowerShell penceresinde aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yüklemek ve aktarın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Aşağıdaki PowerShell komutunu yürüterek Azure AD etki alanı Hizmetleri için gereken hizmet asıl dizininizde eksik olup olmadığını denetleyin:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Aşağıdaki PowerShell komutunu yazarak hizmet sorumlusu oluşturun:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Eksik hizmet asıl oluşturduktan sonra iki saat bekleyin ve yönetilen etki alanınızın sistem durumunu denetleyin.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Azure portalını kullanarak Microsoft AAD ad alanına yeniden kaydolun
Bir hizmet sorumlusu IF kimliği ile adımları ```443155a6-77f3-45e3-882b-22b3a8d431fb``` veya ```abba844e-bc0e-44b0-947a-dc74e5d09022``` Azure AD dizininizi eksik.

**Çözüm:** etki alanı Hizmetleri dizininiz üzerinde geri yüklemek için aşağıdaki adımları kullanın:

1. Gidin [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında sayfası.
2. Yönetilen etki alanı ile ilişkili tablodan abonelik seçin
3. Sol taraftaki gezinti kullanarak seçin **kaynak sağlayıcıları**
4. Tabloda "Microsoft.AAD" için arama ve tıklayın **yeniden kaydolun**
5. Uyarı çözümlendiğinde emin olmak için iki saat içinde yönetilen etki alanınız için sistem durumu sayfasını görüntüleyin.


## <a name="service-principals-that-self-correct"></a>Hizmet sorumluları, kendi kendini düzeltin
Bir hizmet sorumlusu IF kimliği ile adımları ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` Azure AD dizininizi eksik.

**Çözüm:** Azure AD etki alanı Hizmetleri bu belirli hizmet sorumlusu eksik, yanlış ya da silinmiş olduğunda algılayabilir. Hizmet, bu hizmet sorumlusu otomatik olarak yeniden oluşturur. Ancak, uygulama silmeniz gerekir ve sertifika yaparken, uygulama ve nesne artık olduğu gibi silinen uygulamayla çalışan nesne asıl yeni hizmeti tarafından değiştirilmesi mümkün olmayacaktır. Bu, etki alanınızda yeni bir hataya neden olacaktır. Özetlenen adımları izleyin [AADDS105 bölümüne](#alert-aadds105-password-synchronization-application-is-out-of-date) bu sorunu önlemek için. Sonra yeni hizmet sorumlusunu yeniden emin olmak için iki saat sonra yönetilen etki alanının sistem durumunu denetleyin.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Uyarı AADDS105: Parola eşitleme uygulama güncel değil

**Uyarı iletisi:** uygulama kimliği ile "d87dcbc6-a371-462e-88e3-28ad15ec4e64" hizmet asıl silinir ve yeniden oluşturulur. Bu hizmet sorumlusu başka bir hizmet sorumlusu ve parola eşitleme için kullanılan bir uygulama yönetir. Yönetilen hizmet sorumlusu ve/veya uygulama yetkilendirilmemiş altında yeni oluşturulan hizmet asıl şekilde hizmetimizi tarafından yönetilemez. Bu, yeni oluşturulan hizmet asıl eski yönetilen uygulamaların güncelleştiremiyor olacaktır ve parola eşitleme etkileneceğini anlamına gelir.


**Çözüm:** bu adımları tamamlamak için Azure AD PowerShell gerekir. Azure AD PowerShell yükleme hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Bu sorunu gidermek için bir PowerShell penceresinde aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yüklemek ve aktarın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Eski uygulama ve aşağıdaki PowerShell komutlarını kullanarak nesnesini silme

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Her ikisi de sildikten sonra sistem kendisini düzeltin ve parola eşitlemesi için gereken uygulamaları yeniden oluşturun. Uyarı düzeltildiğini emin olmak için iki saat bekleyin ve etki alanınızın sistem durumunu denetleyin.


## <a name="contact-us"></a>Bizimle İletişim Kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
