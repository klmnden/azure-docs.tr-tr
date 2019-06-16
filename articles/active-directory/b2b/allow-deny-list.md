---
title: İzin verme veya engelleme davet belirli kuruluşlarla - Azure Active Directory | Microsoft Docs
description: Yönetici erişim ayarlayın ya da izin vermeyi veya engellemeyi B2B kullanıcıları belirli etki alanlarının listesi reddetmek için Azure portalını veya PowerShell'i nasıl kullanabileceğinizi gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa975446c19db3176fdb89ccfb1a987b1fda049d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113233"
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>İzin verme veya davetleri B2B kullanıcıları belirli kuruluşlardan engelleme

B2B kullanıcıları için izin verilenler veya Engellenenler davetlerden belirli kuruluşlar için bir izin verilenler listesi veya reddetme listesini kullanabilirsiniz. Örneğin, kişisel bir e-posta adresi etki alanları engellemek istiyorsanız, Gmail.com ve Outlook.com gibi etki alanı içeren bir reddetme listesini ayarlayabilirsiniz. İşletmenizi Contoso.com ve Fabrikam.com Litware.com gibi diğer işletmelerden ile iş ortaklığı olan ve yalnızca bu kuruluşların davetleri kısıtlamak istediğiniz, Contoso.com ve Fabrikam.com için Litware.com ekleyebilirsiniz, izin verilenler listesi.
  
## <a name="important-considerations"></a>Önemli noktalar

- Bir izin verilenler listesi veya reddetme listesini oluşturabilirsiniz. Her iki tür listesi olarak ayarlanamıyor. Varsayılan olarak, hangi etki alanı içinde izin verilenler listesine: reddetme listesinde ve tam tersi değildir. 
- Kuruluş başına yalnızca bir ilke oluşturabilirsiniz. Daha fazla etki alanını dahil etmek için ilke güncelleştirebilir veya yeni bir tane oluşturmak için ilkesini silebilirsiniz. 
- Bu liste, OneDrive iş ve SharePoint Online izin verme veya engelleme listeleri için bağımsız olarak çalışır. Tek tek dosya SharePoint Online'da paylaşım kısıtlamak istiyorsanız, bir izin ver ' veya OneDrive iş ve SharePoint Online için reddetme gerekir. Daha fazla bilgi için [kısıtlı SharePoint Online ve OneDrive iş paylaşımı etki alanları](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Bu liste daveti zaten yararlandınız dış kullanıcılar için geçerli değildir. Listenin ayarlandıktan sonra listenin zorlanır. Bekleyen durumda kullanıcı daveti ve kendi etki alanı engelleyen bir ilke ayarlama, kullanıcının davetini denemesi başarısız olur.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>İzin ayarlama veya listesi ilkesi portalında Reddet

Varsayılan olarak, **herhangi bir etki alanına (en kapsamlı) gönderilmesine izin ver** seçeneği etkinleştirilmiştir. Bu durumda, tüm kuruluştaki B2B kullanıcıları davet edebilirsiniz.

### <a name="add-a-deny-list"></a>Bir engelleme listesine ekle

Burada, kuruluşunuzun neredeyse tüm kuruluşlar ile çalışmak ister, ancak belirli etki alanlarındaki kullanıcılar B2B kullanıcıları davet önlemek istiyor tipik senaryo, budur.

Bir engelleme listesine eklemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
3. Altında **dış kullanıcılar**seçin **dış işbirliği ayarlarını yönetin**.
4. Altında **işbirliği kısıtlamaları**seçin **belirtilen etki alanlarına davetleri Reddet**.
5. Altında **hedef etki alanlarını**, engellemek istediğiniz etki alanı adını girin. Birden çok etki alanları için her etki alanında yeni bir satıra girin. Örneğin:

   ![Ek etki alanları ile izin verme seçeneğini gösterir](./media/allow-deny-list/DenyListSettings.png)
 
6. İşiniz bittiğinde tıklayın **Kaydet**.

Engellenen etki alanından bir kullanıcıyı davet çalışırsanız İlkesi ayarladıktan sonra kullanıcının etki alanı şu anda davet ilkeniz tarafından engelleniyor belirten bir ileti alırsınız.
 
### <a name="add-an-allow-list"></a>Bir izin verilenler listesine ekleme

Burada izin verilenler listesinde özel etki alanlarını ayarlayın ve herhangi bir kuruluş veya belirtilmeyen etki alanlarına davetleri kısıtlamak daha kısıtlayıcı bir yapılandırma budur. 

Bir izin verilenler listesi kullanmak istiyorsanız işletmenizin ihtiyaç duyduğu tam olarak değerlendir vakit emin olan. Bu ilke çok kısıtlayıcı olun, kullanıcılarınıza belgeleri e-posta göndermek veya işbirliği diğer BT tasdikli yollarını bulmak tercih edebilirsiniz.


Bir izin verilenler listesine eklemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
3. Altında **dış kullanıcılar**seçin **dış işbirliği ayarlarını yönetin**.
4. Altında **işbirliği kısıtlamaları**seçin **yalnızca belirtilen etki alanlarına davetleri (en kısıtlayıcı) izin**.
5. Altında **hedef etki alanlarını**, izin vermek istediğiniz etki alanı adını girin. Birden çok etki alanları için her etki alanında yeni bir satıra girin. Örneğin:

   ![Ek etki alanları ile izin ver seçeneği gösterir](./media/allow-deny-list/AllowListSettings.png)
 
6. İşiniz bittiğinde tıklayın **Kaydet**.

İzin verilenler listesinde değil bir etki alanından bir kullanıcıyı davet çalışırsanız İlkesi ayarladıktan sonra kullanıcının etki alanı şu anda davet ilkeniz tarafından engelleniyor belirten bir ileti alırsınız.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Geçiş izin listesi geçme veya tam tersi reddetmek için 

Mevcut ilke yapılandırması, bir ilkeden diğerine geçiş yapıyorsanız, bu atar. Geçiş yapmadan önce yapılandırma ayrıntılarını emin olun. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>İzin ayarlama veya PowerShell kullanarak listesi ilkesi Reddet

### <a name="prerequisite"></a>Önkoşul

İzin verilen ayarlamak veya PowerShell kullanarak izin verilmeyenler listesi için Azure Active Directory için Windows PowerShell modülü önizleme sürümünü yüklemeniz gerekir. Özellikle, Modül sürümü 2.0.0.98 AzureADPreview yükleyin veya üzeri.

Modülün sürümünü denetleyin (ve yüklü olup olmadığını görmek için):
 
1. Yükseltilmiş bir kullanıcı (yönetici olarak çalıştır) olarak Windows PowerShell'i açın. 
2. Azure modülü PowerShell için Active Directory Windows bilgisayarınızda yüklü olan sürümlerinin olup olmadığını görmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Get-Module -ListAvailable AzureAD*
   ```

Modülü yüklü değil ya da gerekli bir sürüme sahip değilseniz, aşağıdakilerden birini yapın:

- Hiçbir sonuç döndürmedi ise AzureADPreview modülünün en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Yalnızca AzureAD modülüne sonuçlarda gösterilen AzureADPreview modülünü yüklemek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- AzureADPreview modülünde sonuçlarında gösterilir ancak kısa 2.0.0.98 sürümü yalnızca, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ```

- AzureAD hem AzureADPreview modülleri sonuçlarda gösterilir, ancak kısa 2.0.0.98 AzureADPreview modülünde sürümü değil, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ```

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>İlke yapılandırma AzureADPolicy cmdlet'leri kullanın.

Bir izin oluşturmak ya da reddetme için kullanmak [yeni AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Aşağıdaki örnekte engelleyen bir engelleme listesine "live.com" etki alanı ayarlama işlemini gösterir.

```powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

Aynı örneği aşağıdaki gösterir ancak ilke tanımı satır içi.

```powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

İzin kümesi veya listesi ilkesi engellemek için kullanın [kümesi AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

```powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
```

İlkeyi almak için kullanın [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

```powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
```

İlke kaldırmak için [Remove-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

```powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B genel bakış için bkz. [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- Koşullu erişim ve B2B işbirliği hakkında daha fazla bilgi için bkz. [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md).



