---
title: B2B konuk kullanıcıların - Azure Active Directory kimlik doğrulama bir kerelik geçiş kodu | Microsoft Docs
description: Bir Microsoft hesabına gerek olmadan B2B konuk kullanıcıların kimliğini doğrulamak için e-posta bir kerelik geçiş kodu kullanma
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b817346c37ec43fd66d166684f5d51ecb5a9718
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799448"
---
# <a name="email-one-time-passcode-authentication-preview"></a>E-posta bir kerelik geçiş kodu kimlik doğrulama (Önizleme)

|     |
| --- |
| E-posta bir kerelik geçiş kodu bir Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Bu makalede, e-posta B2B konuk kullanıcıların bir kerelik geçiş kodu kimlik doğrulamasını etkinleştirmek açıklar. E-posta bir kerelik geçiş kodu Özelliği Azure AD gibi başka bir yolla, Microsoft hesabı (MSA) veya Google Federasyon doğrulanamayan B2B konuk kullanıcıların kimliğini doğrular. Bir kerelik geçiş kodu ile kimlik doğrulaması, bir Microsoft hesabı oluşturmaya gerek yoktur. Konuk kullanıcı davet redeems veya paylaşılan bir kaynağa erişirken kendi e-posta adresine gönderilen geçici bir kod isteyebilirler. Ardından oturum açarken devam etmek için bu kodu girin.

Bu özellik şu anda Önizleme için kullanılabilir (bkz [Önizleme seçim](#opting-in-to-the-preview) aşağıda). Önizlemeden sonra bu özellik varsayılan olarak tüm kiracılar için açık olması.

> [!NOTE]
> Bir kerelik geçiş kodu kullanıcılar Kiracı bağlamını içeren bir bağlantıyı kullanarak oturum gerekir (örneğin, `https://myapps.microsoft.com/?tenantid=<tenant id>` veya `https://portal.azure.com/<tenant id>`, doğrulanmış bir etki alanı söz konusu olduğunda veya `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). Kiracı bağlam içerirler sürece uygulamalarına ve kaynaklarına doğrudan bağlantılar da çalışır. Konuk kullanıcıları hiçbir Kiracı bağlamına sahip uç noktaları kullanarak oturum şu anda belirleyemiyoruz. Örneğin, kullanarak `https://myapps.microsoft.com`, `https://portal.azure.com`, veya ekipler ortak uç nokta bir hataya neden olur. 

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Bir kerelik geçiş kodu Konuk kullanıcılar için kullanıcı deneyimi
Bir kerelik geçiş kodu ile kimlik doğrulaması, Konuk kullanıcı, doğrudan bağlantısını tıklatarak veya için davet e-posta davetini. Her iki durumda da, Konuk kullanıcının e-posta adresine bir kod gönderilecek tarayıcıda bir ileti gösterir. Konuk kullanıcının seçtiği **kod Gönder**:
 
   ![Kod Gönder düğmesini gösteren ekran görüntüsü](media/one-time-passcode/otp-send-code.png)
 
Bir geçiş kodu, kullanıcının e-posta adresine gönderilir. Kullanıcı e-postadan geçiş kodunu alır ve tarayıcı penceresinde girer:
 
   ![Enter kod sayfasını gösteren ekran görüntüsü](media/one-time-passcode/otp-enter-code.png)
 
Konuk kullanıcı artık kimliği doğrulanmış ve paylaşılan kaynağı bakın veya oturum devam edin. 

> [!NOTE]
> Bir kerelik geçiş kodlarını 30 dakika için geçerlidir. 30 dakika sonra belirli bir kerelik geçiş kodu artık geçerli değil ve yeni bir kullanıcı isteği göndermelidir. Kullanıcı oturumları, 24 saat sonra süresi dolar. Kaynak eriştiklerinde o tarihten sonra Konuk kullanıcı yeni bir geçiş kodu alır. Özellikle, Konuk kullanıcı, şirketten ayrılması veya artık erişmesi oturum sona ermesi ek güvenlik sağlar.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>Konuk kullanıcı, bir kerelik geçiş ne zaman elde?

Konuk kullanıcı davet redeems veya kendileriyle paylaşılan bir kaynağa bağlantı kullanır, bunlar bir kerelik geçiş durumunda alırsınız:
- Bir Azure AD hesabı olmadığı 
- Bir Microsoft hesabına sahip değil 
- Davet eden Kiracı için Google Federasyon oluşturan ayarlamamış @gmail.com ve @googlemail.com kullanıcılar 

Davet zaman davet ediyoruz kullanıcı bir kerelik geçiş kodu kimlik doğrulaması kullandığını bir gösterge yoktur. Ancak Konuk kullanıcı oturum açtığında, diğer bir kimlik doğrulama yöntemleri kullanılamaması durumunda bir kerelik geçiş kodu kimlik doğrulama geri dönüş yöntemi olacaktır. 

Giderek Azure portalında bir kerelik geçiş kodlarını kimliğini Konuk kullanıcılar görüntüleyebilir **Azure Active Directory** > **kuruluş ilişkileri**  >   **Diğer kuruluşlardan**.

![Bir kerelik geçiş kodu OTP kaynak değeriyle ekran görüntüsü](media/one-time-passcode/otp-users.png)

> [!NOTE]
> Bir kullanıcı bir kerelik geçiş kodu redeems ve daha sonra bir MSA, Azure AD hesabı veya diğer birleştirilmiş bir hesap edinir, bir kerelik geçiş kodu kullanarak kimlik doğrulaması devam edeceğiz. Kendi kimlik doğrulama yöntemini güncelleştirmek istiyorsanız, Konuk kullanıcı hesabı silin ve bunları yeniden davet edin.

### <a name="example"></a>Örnek
Konuk kullanıcı alexdoe@gmail.com ayarlanan Google Federasyon olmayan Fabrikam üzere davet edildiği. Alex bir Microsoft hesabı yok. Hüseyin, kimlik doğrulaması için bir kerelik geçiş kodu alırsınız.

## <a name="opting-in-to-the-preview"></a>Önizleme için seçim 
Bu, etkili olması katılım eylemi için birkaç dakika sürebilir. Bundan sonra yukarıdaki koşulları yerine yalnızca yeni davet edilen kullanıcıların bir kerelik geçiş kodu kimlik doğrulamasını kullanır. Konuk kullanıcılar davet daha önce kullanılan aynı kendi kimlik doğrulama yöntemini kullanmaya devam eder.

### <a name="to-opt-in-using-the-azure-ad-portal"></a>Azure AD portalı kullanarak kabul etme
1.  Oturum [Azure portalında](https://portal.azure.com/) bir Azure AD genel Yöneticisi olarak.
2.  Gezinti bölmesinde seçin **Azure Active Directory**.
3.  Altında **Yönet**seçin **kuruluş ilişkileri**.
4.  Seçin **ayarları**.
5.  Altında **etkinleştirme e-posta kerelik geçiş kodu (Önizleme) konuklar için**seçin **Evet**.
 
### <a name="to-opt-in-using-powershell"></a>PowerShell kullanarak geri çevirmek için

İlk olarak, grafik Modülü (AzureADPreview) için Azure AD PowerShell en son sürümünü yüklemeniz gerekir. Ardından B2B ilkeleri zaten mevcut ve uygun komutları çalıştırmak isteyip belirlersiniz.

#### <a name="prerequisite-install-the-latest-azureadpreview-module"></a>Önkoşul: En son AzureADPreview modülünü yükleme
İlk olarak, hangi modülleri yüklediğinizi denetleyin. Windows PowerShell’i yükseltilmiş yönetici olarak açın (Yönetici olarak çalıştırın) ve aşağıdaki komutu çalıştırın:
 
```powershell  
Get-Module -ListAvailable AzureAD*
```

AzureADPreview modülü, sonraki bir sürüm olduğunu belirten bir ileti olmadan görüntülenirse hazırsınız demektir. Aksi takdirde, çıktıya bağlı olarak aşağıdakilerden birini yapın:

- Bir sonuç döndürülmezse, AzureADPreview modülünü yüklemek için aşağıdaki komutu çalıştırın:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Yalnızca sonuçlarda AzureAD modülü gösteriliyorsa, AzureADPreview modülünü yüklemek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Yalnızca sonuçlarda AzureADPreview modülü gösteriliyorsa, ancak sonraki bir sürümün olduğunu belirten bir ileti alırsanız modülü güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
  ```

Modülü güvenilir olmayan bir depodan yüklediğinizi belirten bir istem alabilirsiniz. Önceden PSGallery deposunu güvenilir depo olarak ayarlamadıysanız bu oluşur. Modülü yüklemek için **Y** tuşuna basın.

#### <a name="check-for-existing-policies-and-opt-in"></a>Mevcut ilkeler için denetleyin ve kabul et

Ardından, aşağıdaki komutu çalıştırarak bir B2BManagementPolicy geçerli olup olmadığını kontrol edin:

```powershell 
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
$currentpolicy -ne $null
```
- Çıkış False ise, ilke şu anda mevcut değil. Yeni bir B2BManagementPolicy oluşturun ve önizleme için aşağıdaki komutu çalıştırarak kabul et:

   ```powershell 
   $policyValue=@("{`"B2BManagementPolicy`":{`"PreviewPolicy`":{`"Features`":[`"OneTimePasscode`"]}}}")
   New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true
   ```

- Çıkış True ise, B2BManagementPolicy ilke şu anda mevcut. Güncelleştirme ilkesi ve önizleme için katılım için şu komutu çalıştırın:
  
   ```powershell 
   $policy = $currentpolicy.Definition | ConvertFrom-Json
   $features=[PSCustomObject]@{'Features'=@('OneTimePasscode')}; $policy.B2BManagementPolicy | Add-Member 'PreviewPolicy' $features -Force; $policy.B2BManagementPolicy
   $updatedPolicy = $policy | ConvertTo-Json -Depth 3
   Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
   ```

## <a name="opting-out-of-the-preview-after-opting-in"></a>Seçim sonrasında Önizleme dışında seçim yapma
Bu, geri çevirme eylemi etkili olması birkaç dakika sürebilir. Önizlemeyi Kapat, bir kerelik geçiş kodu yararlandınız tüm Konuk kullanıcılar oturum açmanız mümkün olmayacaktır. Konuk kullanıcı silebilir ve odaklanmalarını başka bir kimlik doğrulama yöntemini kullanarak tekrar oturum açmak kullanıcıya yeniden davet edin.

### <a name="to-turn-off-the-preview-using-the-azure-ad-portal"></a>Azure AD portalı kullanarak önizlemesini Kapat için
1.  Oturum [Azure portalında](https://portal.azure.com/) bir Azure AD genel Yöneticisi olarak.
2.  Gezinti bölmesinde seçin **Azure Active Directory**.
3.  Altında **Yönet**seçin **kuruluş ilişkileri**.
4.  Seçin **ayarları**.
5.  Altında **etkinleştirme e-posta kerelik geçiş kodu (Önizleme) konuklar için**seçin **Hayır**.

### <a name="to-turn-off-the-preview-using-powershell"></a>PowerShell kullanarak önizlemesini Kapat için
Henüz yoksa, en son AzureADPreview modülünde yükleyin (bkz [önkoşul: En son AzureADPreview modülünü yükleme](#prerequisite-install-the-latest-azureadpreview-module) yukarıda). Ardından, bir kerelik geçiş kodu Önizleme İlkesi şu anda aşağıdaki komutu çalıştırarak var olduğundan emin olun:

```powershell 
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
($currentPolicy -ne $null) -and ($currentPolicy.Definition -like "*OneTimePasscode*")
```

Çıkış True ise, aşağıdaki komutu çalıştırarak önizlemeden iyileştirilmiş:

```powershell 
$policy = $currentpolicy.Definition | ConvertFrom-Json
$policy.B2BManagementPolicy.PreviewPolicy.Features = $policy.B2BManagementPolicy.PreviewPolicy.Features.Where({$_ -ne "OneTimePasscode"})
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
```

