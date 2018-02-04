---
title: "Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için"
description: "Yönetilen hizmet kimliği Azure Active Directory için bilinen sorunlar."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 8820691f5b7c6dbd2c15faede75de123f779b167
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="faq-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Sık sorulan sorular ve bilinen sorunlar ile yönetilen hizmet kimliği (MSI) Azure Active Directory için

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

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

### <a name="are-there-rbac-roles-for-user-assigned-identities"></a>RBAC rolleri için kullanıcı atanan kimlikleri var mı?
Evet:
1. MSI katkıda bulunan: 

- Yapabilirsiniz: CRUD atanan kullanıcı kimlikleri. 
- : Bir kullanıcı için bir kaynak kimliği atanır atayamazsınız. (yani kimliğini bir VM'ye atama)

2. MSI işleci: 

- Can: bir kaynağa atanmış kullanıcı kimliği atayın. (yani kimliğini bir VM'ye atama)
- Şunları yapamaz: CRUD atanan kullanıcı kimlikleri.

Not: Yerleşik katılımcı rolü tüm yukarıda açıklanan eylemleri gerçekleştirebilirsiniz: 
- CRUD atanan kullanıcı kimlikleri
- Bir kaynağa kimlik atanmış bir kullanıcı atayın. (yani kimliğini bir VM'ye atama)


## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>Şema verme MSI uzantısı çalışırken "Otomasyon betiğini" başarısız

Yönetilen hizmet kimliği bir VM üzerinde etkin olduğunda, aşağıdaki hata VM veya kaynak grubu için "Otomasyon betiğini" özelliğini kullanmaya çalışırken gösterilir:

![MSI otomasyon komut dosyası dışarı aktarma hatası](~/articles/active-directory/media/msi-known-issues/automation-script-export-error.png)

Yönetilen hizmet kimliği VM uzantısı şemasına bir kaynak grubu şablonu dışarı aktarmak için özelliği şu anda desteklemiyor. Sonuç olarak, oluşturulan şablon yapılandırma parametrelerini kaynak üzerinde yönetilen hizmet kimliği göstermez. Bu bölümler örneklerde izleyerek el ile eklenebilir [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](msi-qs-configure-template-windows-vm.md).

Şema Dışarı Aktar işlevselliği MSI VM uzantısı için kullanılabilir hale geldiğinde, listelenecektir [dışarı aktarma kaynak VM uzantıları içeren grupları](~/articles/virtual-machines/windows/extensions-export-templates.md#supported-virtual-machine-extensions).

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

## <a name="known-issues-with-user-assigned-msi-private-preview-feature"></a>Kullanıcı atanan MSI ile ilgili bilinen sorunlar *(özel önizleme özelliği)*

- MSI'lerini atanan tüm kullanıcı kaldırmanın tek yolu sistem etkinleştirerek MSI atanır. 
- Bir VM VM uzantısının sağlama DNS arama hataları nedeniyle başarısız olabilir. VM'yi yeniden başlatın ve yeniden deneyin. 
- 'Varolmayan' MSI eklemek VM başarısız olmasına neden olur. *Not: Düzeltme MSI yoksa, Ata-identity başarısız toplu genişletme*
- Azure depolama Öğreticisi yalnızca şu anda merkezi BİZE EUAP içinde kullanılabilir. 
- MSI adında özel karakterler (örneğin, alt çizgi) ile atanmış bir kullanıcı oluşturma, desteklenmiyor.
- İkinci bir kullanıcı ekleme kimlik atandığında ClientID belirteçleri istekleri için kullanılamayabilir. Bir azaltma, aşağıdaki iki bash komutlarla MSI VM uzantısı yeniden başlatın:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- VMAgent Windows kullanıcı atanan MSI şu anda desteklemiyor. 
- Şu anda bir kaynağa erişmek için bir MSI rol atama özel izinler gerektirmez. 
- MSI atanmış bir kullanıcı bir VM'ye sahip ancak hiçbir sistem MSI atanan, portal UI MSI etkin olarak gösterilir. MSI atanan sistem etkinleştirmek için bir Azure Resource Manager şablonu, Azure CLI veya bir SDK kullanın.
