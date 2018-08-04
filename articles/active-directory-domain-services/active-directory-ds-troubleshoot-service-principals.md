---
title: 'Azure Active Directory Domain Services: Hizmet sorumlusu yapılandırma sorunlarını giderme | Microsoft Docs'
description: Azure AD etki alanı Hizmetleri için hizmet sorumlusu yapılandırma sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: 407b9732574880cd64036e92fe0c7fac169b7346
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503293"
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Yönetilen etki alanınız için geçersiz bir hizmet sorumlusu yapılandırma sorunlarını giderme

Bu makale ve aşağıdaki uyarı iletisinde neden hizmet sorumlusu ile ilgili yapılandırma hataları gidermek yardımcı olur:

## <a name="alert-aadds102-service-principal-not-found"></a>Uyarı AADDS102: Hizmet sorumlusu bulunamadı

**Uyarı iletisi:** *düzgün çalışması Azure AD Domain Services için gereken bir hizmet sorumlusu, Azure AD dizininden silindi. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.*

[Hizmet sorumluları](../active-directory/develop/active-directory-application-objects.md) yönetme, güncelleştirme ve yönetilen etki alanınızı korumak için Microsoft kullanan uygulamalar. Silindiğinde, Microsoft'un hizmet etki alanınız olanağı keser.


## <a name="check-for-missing-service-principals"></a>Hizmet sorumluları eksik denetimi
Hangi hizmet sorumluları yeniden oluşturulması gerekir belirlemek için aşağıdaki adımları kullanın:

1. Gidin [kurumsal uygulamalar - tüm uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) Azure portalında sayfası.
2. İçinde **Göster** açılır menüsünde, select **tüm uygulamaları** tıklatıp **Uygula**.
3. Aşağıdaki tabloyu kullanarak, arama kimliği arama kutusuna yapıştırarak ve tuşlarına basarak her uygulama kimliği girin. Arama sonuçlarını boş ise, "Çözüm" sütununda adımları izleyerek hizmet sorumlusu yeniden oluşturmanız gerekir.

| Uygulama Kimliği | Çözüm |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Eksik bir hizmet sorumlusu PowerShell ile yeniden oluşturun](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Ad alanına Microsoft.AAD yeniden kaydettirin](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Ad alanına Microsoft.AAD yeniden kaydettirin](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Kendi kendine düzeltme hizmet sorumluları](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Eksik bir hizmet sorumlusu PowerShell ile yeniden oluşturun
Kimliğine sahip bir hizmet sorumlusu, adımları ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` Azure AD dizininizi eksik.

**Çözüm:** bu adımları tamamlamak için Azure AD PowerShell gerekir. Azure AD PowerShell'i yükleme hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Bu sorunu gidermek için bir PowerShell penceresinde aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yükleyin ve alın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Aşağıdaki PowerShell komutunu yürüterek Azure AD Domain Services için gereken hizmet sorumlusunun dizininizde eksik olup olmadığını denetleyin:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Aşağıdaki PowerShell komutunu yazarak hizmet sorumlusu oluşturun:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Eksik bir hizmet sorumlusu oluşturduktan sonra iki saat bekleyin ve yönetilen etki alanınızın sistem durumunu denetleyin.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Azure portalını kullanarak Microsoft AAD ad alanına yeniden kaydedin
Kimliğine sahip bir hizmet sorumlusu, adımları ```443155a6-77f3-45e3-882b-22b3a8d431fb``` veya ```abba844e-bc0e-44b0-947a-dc74e5d09022``` Azure AD dizininizi eksik.

**Çözüm:** etki alanı Hizmetleri dizininiz geri yüklemek için aşağıdaki adımları kullanın:

1. Gidin [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında sayfası.
2. Yönetilen etki alanınızla ilişkili tablodan aboneliği seçin
3. Sol taraftaki gezinti kullanma seçim **kaynak sağlayıcıları**
4. Tablodaki "Microsoft.AAD" için arama ve tıklayın **yeniden kaydettirin**
5. Uyarı çözümlendiğinde emin olmak için iki saat içinde yönetilen etki alanınız için sağlık durumu sayfasını görüntüleyin.


## <a name="service-principals-that-self-correct"></a>Hizmet sorumluları, kendi kendine düzeltme
Kimliğine sahip bir hizmet sorumlusu, adımları ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` Azure AD dizininizi eksik.

**Çözüm:** Azure AD Domain Services, bu belirli bir hizmet sorumlusu eksik, yanlış yapılandırılmış veya silinmiş olduğunda algılayabilir. Hizmet, bu hizmet sorumlusunu otomatik olarak yeniden oluşturur. Ancak, uygulamayı silmek ihtiyacınız olacak ve sertifika dağıtımını yaparken, uygulama ve nesne artık olacak şekilde, silinen uygulama ile birlikte çalışan nesne yeni hizmet sorumlusu tarafından değiştirilmesi mümkün olmayacaktır. Bu, etki alanınızda yeni bir hata önünü açacak. Özetlenen adımları izleyin [AADDS105 bölümüne](#alert-aadds105-password-synchronization-application-is-out-of-date) bu sorunu önlemek için. Sonra iki saat sonra yeni hizmet sorumlusu yeniden emin olmak için yönetilen etki alanınızın sistem durumunu denetleyin.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Uyarı AADDS105: Parola eşitlemesi uygulama güncel değil

**Uyarı iletisi:** "d87dcbc6-a371-462e-88e3-28ad15ec4e64" uygulama kimliği ile hizmet sorumlusu silindi ve yeniden oluşturulur. Yeniden oluşturarak tutarsız izinleri yönetilen etki alanınıza hizmet için gereken Azure AD etki alanı Hizmetleri kaynaklarında bırakır. Yönetilen etki alanınızda parola eşitlemeyi etkilenebilir.


**Çözüm:** bu adımları tamamlamak için Azure AD PowerShell gerekir. Azure AD PowerShell'i yükleme hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Bu sorunu gidermek için bir PowerShell penceresinde aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yükleyin ve alın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Aşağıdaki PowerShell komutlarını kullanarak nesne ve eski uygulama Sil

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Her ikisi de sildikten sonra sistem kendini düzeltme ve parola eşitleme için gerekli uygulamaları yeniden oluşturun. Uyarı düzeltildiğini emin olmak için iki saat bekleyin ve etki alanınızın sistem durumunu denetleyin.


## <a name="contact-us"></a>Bizimle İletişim Kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
