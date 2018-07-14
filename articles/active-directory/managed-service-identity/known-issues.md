---
title: SSS ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) için Azure Active Directory
description: Azure Active Directory için Yönetilen hizmet kimliği ile ilgili bilinen sorunlar.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: c48d03b6e8a3d850d02d2c36c35915f8214b00e8
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035821"
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>SSS ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) için Azure Active Directory

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

### <a name="does-msi-work-with-azure-cloud-services"></a>MSI, Azure Cloud Services ile çalışır mı?

Hayır, Azure bulut Hizmetleri'nde MSI desteklemek için herhangi bir plan yoktur.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>MSI, Active Directory Authentication Library (ADAL) veya Microsoft kimlik doğrulama kitaplığı (MSAL) ile çalışır mı?

Hayır, MSI ADAL veya MSAL henüz tümleşikleştirilmemiştir. MSI REST uç noktasını kullanarak bir MSI belirtecini alma hakkında daha fazla bilgi için bkz [belirtecinin alınması için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>Yönetilen hizmet kimliğini bir güvenlik sınırı nedir?

Kimlik, güvenlik sınırı için bağlı bir kaynaktır. Örneğin, bir sanal makine MSI güvenlik sınırı, sanal makine olur. Bu VM'de çalışan herhangi bir kod MSI uç noktasını çağırmak ve belirteçler istemek için. MSI destekleyen diğer kaynaklarla benzer deneyimidir.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>MSI VM IMDS uç nokta veya MSI VM uzantısı uç nokta kullanmalıyım?

MSI ile sanal makineleri kullanırken, MSI IMDS uç noktayı kullanarak öneririz. Azure örnek meta veri hizmeti, Azure Resource Manager aracılığıyla oluşturulan tüm Iaas Vm'leri için erişilebilir bir REST uç noktasıdır. Bazı MSI IMDS kullanmanın avantajları şunlardır:

1. Tüm Azure Iaas desteklenen işletim sistemleri IMDS MSI kullanabilirsiniz. 
2. Artık MSI etkinleştirmek için VM'NİZDE bir uzantı yüklemeniz gerekir. 
3. MSI tarafından kullanılan sertifikalar artık VM yok. 
4. Bir bilinen yönlendirilemeyen IP adresi, VM içinden yalnızca bulunan IMDS uç nokta var. 

MSI VM uzantısı hala kullanılabilir bugün kullanılacak olan; ancak biz varsayılan IMDS uç noktayı kullanarak ilerletme. MSI VM uzantısı kullanımdan kaldırma planı üzerinde yakında başlayacak. 

Azure örneği Metada Service hakkında daha fazla bilgi için bkz. [IMDS belgeleri](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Desteklenen Linux dağıtımları nelerdir?

Azure Iaas tarafından desteklenen tüm Linux dağıtımları ile MSI IMDS uç noktası aracılığıyla kullanılabilir. 

Not: MSI VM uzantısı yalnızca aşağıdaki Linux dağıtımları destekler:
- CoreOS kararlı
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Diğer Linux dağıtımlarına şu anda desteklenmez ve uzantı desteklenmeyen dağıtımlarında başarısız olabilir.

Uzantı, CentOS 6.9 üzerinde çalışır. Ancak, 6.9 sistem desteği eksikliği nedeniyle uzantı yeniden kilitlenmesi veya durduruldu durumunda otomatik olarak tamamlar değil. VM yeniden başlatıldığında yeniden başlatır. Uzantıyı el ile yeniden başlatmak için bkz: [nasıl MSI uzantısı başlatmanız?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>MSI uzantıyı nasıl başlatmanız?
Uzantı durursa, Windows ve Linux'ın, aşağıdaki cmdlet'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Uzantı adı ve türü için Windows: ManagedIdentityExtensionForWindows
- Uzantı adı ve Linux için türü: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>"Otomasyon betiği" MSI uzantısı için şema verme çalışırken başarısız olur.

Bir VM'ye yönetilen hizmet kimliği etkinleştirildiğinde, VM veya kaynak grubu için "Otomasyon betiği" özelliğini kullanmaya çalışırken şu hata gösterilir:

![MSI Otomasyon betiğini dışarı aktarma hatası](../managed-service-identity/media/msi-known-issues/automation-script-export-error.png)

Yönetilen hizmet kimliği VM uzantısı, şu anda şeması için kaynak grubu şablonunu dışarı aktarma özelliğini desteklemiyor. Sonuç olarak oluşturulan şablon kaynağında yönetilen hizmet kimliği etkinleştirmek için yapılandırma parametreleri göstermez. Bu bölümlerde örneklerde izleyerek elle eklenebilir [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](qs-configure-template-windows-vm.md).

Şema dışarı aktarma işlevi için MSI VM uzantısı kullanılabilir olduğunda, listelenecektir [verme kaynak VM uzantıları içeren gruplara](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Yapılandırma dikey penceresinde, Azure portalında görünmüyor

VM yapılandırma dikey penceresinde, sanal makinenizde görünmüyorsa, ardından MSI portalın bölgeniz henüz etkinleştirilmedi.  Daha sonra tekrar kontrol edin.  Kullanarak VM için MSI etkinleştirebilirsiniz [PowerShell](qs-configure-powershell-windows-vm.md) veya [Azure CLI](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Erişim denetimi (IAM) dikey penceresinde sanal makinelere erişim atanamıyor

Varsa **sanal makine** yönelik bir seçenek olarak Azure Portalı'nda görünmez **erişim Ata** içinde **erişim denetimi (IAM)** > **Ekle izinleri**, sonra da yönetilen hizmet kimliği portalın bölgeniz henüz etkinleştirilmedi. Daha sonra tekrar kontrol edin.  Rol ataması için Yönetilen hizmet kimliği MSI hizmet sorumlusu için arama yaparak yine de seçebilirsiniz.  VM'nin adını **seçin** alan ve hizmet sorumlusu arama sonucunda görüntülenir.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Kaynak grubu veya abonelik taşındıktan sonra başlatmak VM başarısız

Bir sanal makine çalışır durumda taşırsanız, taşıma işlemi sırasında çalışmaya devam eder. VM durdurulduysa ve yeniden başlatıldı, ancak taşıma sonrasında, bunu başlatmak başarısız olur. VM MSI kimliğine başvuru güncelleştirilmiyor ve eski kaynak grubunda üzerine devam eder, çünkü bu sorun ortaya çıkar.

**Geçici çözüm** 
 
VM üzerinde bir güncelleştirme için MSI doğru değerleri alabilmeniz tetikleyin. MSI kimliğine başvuru güncelleştirmek için bir VM özellik değişiklik yapabilirsiniz. Örneğin, aşağıdaki komutu VM'deki yeni bir etiket değeri ayarlayabilirsiniz:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Bu komut, sanal makinede yeni bir etiket değeri 1 ile "fixVM" ayarlar. 
 
Bu özellik ayarlayarak, VM doğru MSI kaynak URI'si ile güncelleştirir ve ardından sanal Makineyi başlatmak mümkün olmalıdır. 
 
VM başlatıldıktan sonra aşağıdaki komutu kullanarak etiket kaldırılabilir:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Atanan kullanıcı kimlikleri ile ilgili bilinen sorunlar

- Kullanıcıya atanan kimlik yalnızca VM ve VMSS getirelim atamalardır. Önemli: Atanan kullanıcı kimlik atamaları gelecek ay içinde değiştirin.
- Atanan kullanıcı kimlikleri aynı VM/VMSS üzerinde yinelenen, başarısız VM/VMSS neden olur. Bu, farklı büyük/küçük harf ile eklenen kimliklerini içerir. Örneğin MyUserAssignedIdentity ve myuserassignedidentity. 
- Bir VM için VM uzantısının sağlama DNS arama hataları nedeniyle başarısız olabilir. VM'yi yeniden başlatın ve yeniden deneyin. 
- 'Var olmayan' kullanıcı tarafından atanan kimlik ekleme, VM başarısız olmasına neden olur. 
- Bir kullanıcı tarafından atanan kimliği adında özel karakterler (örneğin, alt çizgi) ile oluşturma, desteklenmiyor.
- Kullanıcı tarafından atanan kimlik adları uçtan uca senaryo için 24 karakterden sınırlı. 24 karakterden daha uzun adlara sahip kullanıcı atanan kimlikleri atanacak başarısız olur.
- Yönetilen kimlik sanal makine uzantısı aracılığıyla, desteklenen sınırı 32 kullanıcı tarafından yönetilen kimlikleri atanan olur. Yönetilen kimlik sanal makine uzantısı, desteklenen sınırı 512'dır.  
- İkinci bir kullanıcı ekleyerek kimlik atandığında ClientID VM uzantısı için istekleri belirteçleri kullanılabilir olmayabilir. Bir risk azaltma, MSI VM uzantısı aşağıdaki iki bash komutları yeniden başlatın:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Bir VM, bir kullanıcı tarafından atanan kimliği ancak hiçbir sistem tarafından atanan kimliği varsa, portal UI MSI devre dışı olarak gösterilir. Sistem tarafından atanan kimlik etkinleştirmek için bir Azure Resource Manager şablonu, Azure CLI veya SDK'yı kullanın.
