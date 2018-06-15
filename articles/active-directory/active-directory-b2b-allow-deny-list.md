---
title: İzin verme veya engelleme belirli kuruluşların - Azure Active Directory B2B kullanıcılara davet | Microsoft Docs
description: Bir yönetici erişimini ayarlamak veya izin verme veya belirli etki alanlarındaki B2B kullanıcılar engelleme listesine reddetmek için Azure portal veya PowerShell nasıl kullanabileceğinizi gösterir.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/19/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 7e89bf47f592e4698a6e50fced78aeab0152ebc6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33928565"
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>İzin verme veya belirli kuruluşlardan B2B kullanıcılara davet engelleme

B2B kullanıcılara izin verilenler veya Engellenenler davetleri belirli kuruluşlardan için bir izin verilenler listesi veya reddetme listesini kullanabilirsiniz. Örneğin, kişisel e-posta adresi etki alanlarını engelleyin istiyorsanız, Gmail.com ve Outlook.com gibi etki alanı içeren bir izin verme listesi ayarlayabilirsiniz. Veya Contoso.com, Fabrikam.com ve Litware.com gibi diğer işletmelerin yöneticileriyle işiniz varsa ve yalnızca bu kuruluşların davetleri kısıtlamak istediğiniz, Contoso.com, Fabrikam.com ve Litware.com için ekleyebilirsiniz, izin verilenler listesi.
  
## <a name="important-considerations"></a>İle ilgili önemli noktalar

- Bir izin verilenler listesi veya reddetme listesini oluşturabilirsiniz. Her iki tür listesi ayarlanamıyor. Varsayılan olarak, hangi etki alanları, izin verilenler listesi olan izin verme listesinde ve tersi yönde değildir. 
- Kuruluş başına yalnızca bir ilke oluşturabilirsiniz. Daha fazla etki alanı eklemek için ilke güncelleştirebilir veya yeni bir tane oluşturmak için ilke silebilirsiniz. 
- Bu liste, iş ve SharePoint Online izin blok listeler için OneDrive üzerinden bağımsız olarak çalışır. Tek tek dosya SharePoint Online'da paylaşımı kısıtlamak istiyorsanız, bir izin verme ayarlamak veya OneDrive iş ve SharePoint Online için izin verilmeyenler listesi gerekir. Daha fazla bilgi için bkz: [kısıtlanmış SharePoint Online ve OneDrive iş paylaşımı etki alanları](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Bu liste daveti zaten kullanılan dış kullanıcılar için geçerli değildir. Listenin ayarlandıktan sonra listesi zorlanacak. Bir kullanıcı davet bekleme durumunda ise ve kendi etki alanı engelleyen bir ilke ayarlayın, daveti kullanmak için kullanıcının girişimleri başarısız olur.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>İzin verilen ayarlayın veya listesi ilkesi portalında Reddet

Varsayılan olarak, **herhangi bir etki alanına (en dahil) gönderilmek üzere davetleri izin** ayarı etkindir. Bu durumda, tüm kuruluştan B2B kullanıcıları davet edebilirsiniz.

### <a name="add-a-deny-list"></a>Bir izin verme listesi ekleme

Burada, kuruluşunuzun neredeyse tüm organizasyonunuzla çalışmak ister, ancak belirli etki alanlarındaki kullanıcılar olarak B2B kullanıcıları davet önlemek istiyor en tipik senaryo budur.

Reddetme listesine eklemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarlarını**.
3. Altında **dış kullanıcılar**seçin **dış işbirliğini ayarlarını Yönet**.
4. Altında **işbirliği kısıtlamaları**seçin **davetleri belirtilen etki alanları için reddetme**.
5. Altında **hedef etki alanlarını**, engellemek istediğiniz etki alanlarından biri adını girin. Birden çok etki alanı için her etki alanında yeni bir satıra girin. Örneğin:

   ![Eklenen etki alanlarıyla izin verme seçeneğini gösterir](./media/active-directory-b2b-allow-deny-list/DenyListSettings.png)
 
6. İşiniz bittiğinde tıklatın **kaydetmek**.

Engellenen etki alanındaki bir kullanıcının davet çalışırsanız, ilke ayarladıktan sonra kullanıcının şu anda davet ilkenizin engellendiğini bildiren bir ileti alırsınız.
 
### <a name="add-an-allow-list"></a>Bir izin verilenler listesi ekleme

Burada belirli etki alanlarına izin verilenler listesinde ayarlamak ve herhangi bir kuruluş veya belirtilen olmayan etki alanları için davet kısıtlamak daha kısıtlayıcı bir yapılandırma budur. 

Olan bir izin verilenler listesi kullanmak istiyorsanız tam olarak ne uygulamasını iş gereksinimleriniz değerlendirmek için süre beklemesini emin olun. Bu ilke çok kısıtlayıcı yaparsanız, kullanıcılarınızın e-posta belgeleri göndermek veya işbirliği diğer BT olmayan tasdikli yollarını bulmak tercih edebilirsiniz.


Bir izin verilenler listesine eklemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarlarını**.
3. Altında **dış kullanıcılar**seçin **dış işbirliğini ayarlarını Yönet**.
4. Altında **işbirliği kısıtlamaları**seçin **davetleri yalnızca belirtilen etki alanları için (en kısıtlayıcı) izin**.
5. Altında **hedef etki alanlarını**, izin vermek istediğiniz etki alanlarından biri adını girin. Birden çok etki alanı için her etki alanında yeni bir satıra girin. Örneğin:

   ![Ek etki alanları ile izin ver seçeneği gösterir](./media/active-directory-b2b-allow-deny-list/AllowListSettings.png)
 
6. İşiniz bittiğinde tıklatın **kaydetmek**.

İzin verilenler listesinde değil bir etki alanındaki bir kullanıcının davet çalışırsanız, ilke ayarladıktan sonra kullanıcının şu anda davet ilkenizin engellendiğini bildiren bir ileti alırsınız.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Anahtardan izin listesi tersi reddetmek için 

Diğer bir ilkeden geçiş yapıyorsanız, bu varolan ilke yapılandırması atar. Anahtar gerçekleştirmeden önce yapılandırmanızın ayrıntılarını emin olun. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>İzin verilen ayarlayın veya PowerShell kullanarak listesi ilkesi Reddet

### <a name="prerequisite"></a>Önkoşul

İzin verilen ayarlamak veya PowerShell kullanarak izin verilmeyenler listesi için Azure Active Directory için Windows PowerShell modülü önizleme sürümünü yüklemeniz gerekir. Özellikle, Modül sürümü 2.0.0.98 AzureADPreview yüklemek veya sonraki bir sürümü.

Modül sürümünü denetleme (ve yüklü olup olmadığını görmek için):
 
1. Yükseltilmiş bir kullanıcı (yönetici olarak çalıştır) olarak Windows PowerShell'i açın. 
2. Active Directory modülü Windows PowerShell için Azure bilgisayarınızda yüklü olan sürümlerinin olup olmadığını görmek için aşağıdaki komutu çalıştırın:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````

Modülü kurulu değil veya gerekli bir sürüme sahip değilseniz, aşağıdakilerden birini yapın:

- Hiç sonuç döndürmedi AzureADPreview Modülü'nın en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- Yalnızca Azuread'i modülü sonuçlarda gösterilen AzureADPreview modülünü yüklemek için aşağıdaki komutları çalıştırın: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- AzureADPreview modülü sonuçlarda gösterilen ancak sürüm değerinden 2.0.0.98 yalnızca, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- Azuread'i ve AzureADPreview modülleri sonuçlarda gösterilen AzureADPreview modülü sürümü değerinden 2.0.0.98 varsa, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>AzureADPolicy cmdlet'leri ilkesini yapılandırmak için kullanın

Bir izin verme oluşturmak veya izin verilmeyenler listesi için kullanın [yeni AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Aşağıdaki örnek, reddetme listesini engelleyen "live.com" etki alanı kümesi gösterilmektedir.

````powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Aşağıdaki aynı örnek gösterir ancak ilke tanımı satır içi.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

İzin verilen ayarlamak veya listesi ilkesi reddetmek için kullanın [kümesi AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

İlkeyi almak üzere kullanmak [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

İlke kaldırmak için kullanın [Kaldır AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet'i. Örneğin:

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B genel bakış için bkz: [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Koşullu erişim ve B2B işbirliği hakkında daha fazla bilgi için bkz: [B2B işbirliği kullanıcılar için koşullu erişim](active-directory-b2b-mfa-instructions.md).



