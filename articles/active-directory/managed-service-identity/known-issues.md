---
title: Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için
description: Yönetilen hizmet kimliği Azure Active Directory için bilinen sorunlar.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: a50854b2e12db9a202d769f9e5feebee8e5f9395
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

### <a name="is-there-a-private-preview-program-available-for-upcoming-msi-features-and-integrations"></a>Var. özel Önizleme programına yaklaşan MSI özelliklerini ve tümleştirmeler var mı?

Evet. Özel önizleme programı kayıt için değerlendirilmesi istiyorsanız [kaydolma sayfamızı ziyaret edin](https://aka.ms/azuremsiprivatepreview).

### <a name="does-msi-work-with-azure-cloud-services"></a>MSI Azure bulut Hizmetleri ile çalışır mı?

Hayır, Azure Cloud Services MSI desteklemek için plan yok.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>MSI Active Directory Authentication Library (ADAL) veya Microsoft kimlik doğrulama kitaplığı (MSAL) ile çalışır mı?

Hayır, MSI henüz ADAL veya MSAL ile tümleşiktir değil. MSI REST uç noktasını kullanarak bir MSI belirtecini alma hakkında daha fazla bilgi için bkz: [belirteci alımı için bir Azure VM yönetilen hizmet kimliği (MSI) kullanmayı](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>Yönetilen hizmet kimliği, güvenlik sınırı nedir?

Kimliğin güvenlik sınırı kendisine bağlı olduğu kaynaktır. Örneğin, bir sanal makine MSI için güvenlik sınırı, sanal makine olur. Bu VM üzerinden çalışan herhangi bir kod MSI uç noktasını çağırmak ve belirteçler istemek kullanabilirsiniz. MSI destekleyen diğer kaynaklarla benzer deneyimidir.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>MSI VM IMDS uç nokta veya MSI VM uzantısı uç nokta kullanmalıyım?

MSI VM ile birlikte kullanırken, MSI IMDS uç nokta kullanarak öneririz. Azure örneği meta veri hizmeti Azure Resource Manager aracılığıyla oluşturulan tüm Iaas VM'ler için erişilebilir bir REST uç noktadır. MSI IMDS kullanmanın avantajları bazıları şunlardır:

1. Tüm Azure Iaas desteklenen işletim sistemleri MSI IMDS kullanabilirsiniz. 
2. Artık, VM MSI etkinleştirmek için bir uzantı yüklemeniz gerekir. 
3. MSI tarafından kullanılan sertifikaların artık VM'yi mevcut değildir. 
4. IMDS uç noktası bir iyi bilinen yönlendirilemeyen IP adresi, VM içinden yalnızca kullanılabilir değil. 

MSI VM uzantısı hala bugün kullanılmak üzere kullanılabilir olduğunu; Ancak, biz varsayılan IMDS uç noktası için kullanılacak ilerleyen. MSI VM uzantısı kullanımdan plan üzerinde yakında başlayacak. 

Azure örneği Metada hizmeti hakkında daha fazla bilgi için bkz: [IMDS belgeleri](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Desteklenen Linux dağıtımları nelerdir?

Azure Iaas tarafından desteklenen tüm Linux dağıtımları ile MSI IMDS uç noktası aracılığıyla kullanılabilir. 

Not: MSI VM uzantısı yalnızca aşağıdaki Linux dağıtımları destekler:
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

![MSI otomasyon komut dosyası dışarı aktarma hatası](../media/msi-known-issues/automation-script-export-error.png)

Yönetilen hizmet kimliği VM uzantısı şemasına bir kaynak grubu şablonu dışarı aktarmak için özelliği şu anda desteklemiyor. Sonuç olarak, oluşturulan şablon yapılandırma parametrelerini kaynak üzerinde yönetilen hizmet kimliği göstermez. Bu bölümler örneklerde izleyerek el ile eklenebilir [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](qs-configure-template-windows-vm.md).

Şema Dışarı Aktar işlevselliği MSI VM uzantısı için kullanılabilir hale geldiğinde, listelenecektir [dışarı aktarma kaynak VM uzantıları içeren grupları](../../virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Yapılandırma dikey penceresinde Azure Portalı'nda görünmüyor

VM yapılandırma dikey penceresinde, VM görünmüyorsa, ardından MSI bölgenizde portaldaki henüz etkinleştirilmedi.  Daha sonra yeniden denetleyin.  MSI kullanarak VM etkinleştirebilirsiniz [PowerShell](qs-configure-powershell-windows-vm.md) veya [Azure CLI](qs-configure-cli-windows-vm.md).

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
