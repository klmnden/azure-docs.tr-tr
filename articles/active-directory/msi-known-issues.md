---
title: "Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için"
description: "Yönetilen hizmet kimliği Azure Active Directory için bilinen sorunlar."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: c8b25082170eb03c1ebc5965e273868982a3846f
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

### <a name="is-there-a-private-preview-available-for-additional-features"></a>Özel önizleme ek özellikler için kullanılabilir var olmadığı?

Evet. Özel önizleme kayıt dikkate istiyorsanız [kaydolma sayfamızı ziyaret edin](https://aka.ms/azuremsiprivatepreview).

### <a name="does-msi-work-with-azure-cloud-services"></a>MSI Azure bulut Hizmetleri ile çalışır mı?

Hayır, Azure Cloud Services MSI desteklemek için plan yok.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>MSI Active Directory Authentication Library (ADAL) veya Microsoft kimlik doğrulama kitaplığı (MSAL) ile çalışır mı?

Hayır, MSI henüz ADAL veya MSAL ile tümleşiktir değil. MSI REST uç noktasını kullanarak bir MSI belirtecini alma hakkında daha fazla bilgi için bkz: [belirteci alımı için bir Azure VM yönetilen hizmet kimliği (MSI) kullanmayı](msi-how-to-use-vm-msi-token.md).

### <a name="what-are-the-supported-linux-distributions"></a>Desteklenen Linux dağıtımları nelerdir?

Aşağıdaki Linux dağıtımları MSI destekler: 

- CoreOS kararlı
- CentOS 7.1
- RedHat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Diğer Linux dağıtımları şu anda desteklenmez ve uzantı üzerinde desteklenmeyen dağıtımları başarısız olabilir.

Uzantı CentOS 6.9 üzerinde çalışır. Ancak, 6.9 sistem destek eksikliği nedeniyle, uzantısı yeniden başlatma kilitlendi veya durdurulmuş otomatik değildir. VM başlatıldığında yeniden başlatır. Uzantı el ile yeniden başlatmak için bkz: [nasıl, yeniden MSI uzantısı?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>MSI uzantısı nasıl yeniden?
Uzantı durursa, Windows ve Linux belirli sürümleri, aşağıdaki cmdlet'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Uzantı adı ve türü Windows için: ManagedIdentityExtensionForWindows
- Uzantı adı ve türü Linux için: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>Şema verme MSI uzantısı çalışırken "Otomasyon betiğini" başarısız

Yönetilen hizmet kimliği bir VM üzerinde etkin olduğunda, aşağıdaki hata VM veya kaynak grubu için "Otomasyon betiğini" özelliğini kullanmaya çalışırken gösterilir:

![MSI otomasyon komut dosyası dışarı aktarma hatası](media/msi-known-issues/automation-script-export-error.png)

Yönetilen hizmet kimliği VM uzantısı şemasına bir kaynak grubu şablonu dışarı aktarmak için özelliği şu anda desteklemiyor. Sonuç olarak, oluşturulan şablon yapılandırma parametrelerini kaynak üzerinde yönetilen hizmet kimliği göstermez. Bu bölümler örneklerde izleyerek el ile eklenebilir [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](msi-qs-configure-template-windows-vm.md).

Şema Dışarı Aktar işlevselliği MSI VM uzantısı için kullanılabilir hale geldiğinde, listelenecektir [dışarı aktarma kaynak VM uzantıları içeren grupları](../virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Yapılandırma dikey penceresinde Azure Portalı'nda görünmüyor

VM yapılandırma dikey penceresinde, VM görünmüyorsa, ardından MSI bölgenizde portaldaki henüz etkinleştirilmedi.  Daha sonra yeniden denetleyin.  MSI kullanarak VM etkinleştirebilirsiniz [PowerShell](msi-qs-configure-powershell-windows-vm.md) veya [Azure CLI](msi-qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Erişim denetimi (IAM) dikey penceresinde sanal makinelere erişim atanamaz

Varsa **sanal makine** için bir seçenek olarak Azure Portalı'nda görünmez **atamak için erişim** içinde **erişim denetimi (IAM)** > **Ekle izinleri**, sonra da yönetilen hizmet kimliği, bölgenizdeki portaldaki henüz etkinleştirilmedi. Daha sonra yeniden denetleyin.  Rol ataması için Yönetilen hizmet kimliği MSI hizmet sorumlusu için arama yaparak hala seçebilirsiniz.  VM adını girin **seçin** alan ve hizmet sorumlusu arama sonucunda görünür.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>VM kaynak grubuna veya aboneliğe taşındıktan sonra başlatılamıyor

VM çalışır durumda taşırsanız, taşıma işlemi sırasında çalışmaya devam eder. VM durduruldu ve yeniden başlatılabilir, ancak taşımadan sonra onu başlatmak başarısız olur. VM MSI kimliğine başvuru güncelleştirilmiyor ve eski kaynak grubunda üzerine devam çünkü bu sorun oluşur.

**Geçici çözüm** 
 
MSI için doğru değerleri sınıflandırıp bir güncelleştirme VM üzerinde tetikler. MSI kimliğine başvuru güncelleştirmek için bir VM özellik değişikliği yapabilirsiniz. Örneğin, aşağıdaki komut ile VM üzerinde yeni bir etiket değeri ayarlayabilirsiniz:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Bu komut yeni bir etiket değeri 1 olan "fixVM" VM ayarlar. 
 
Bu özellik ayarlayarak, doğru MSI kaynak URI'si ile VM güncelleştirir ve ardından VM başlatabilmek için olmalıdır. 
 
VM başladıktan sonra etiket komutu kullanılarak kaldırılabilir:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```
