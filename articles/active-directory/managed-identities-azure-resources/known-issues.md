---
title: SSS ve Azure kaynakları için yönetilen kimliklerle bilinen sorunlar
description: Azure kaynakları için yönetilen kimliklerle bilinen sorunlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: e958aa82eb1e2fbf21a44df333533c6da058a966
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448487"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>SSS ve Azure kaynakları için yönetilen kimliklerle bilinen sorunlar

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Azure kaynakları için yönetilen kimlikleri, Azure Cloud Services ile çalışır mı?

Hayır, Azure bulut hizmetlerinde Azure kaynakları için yönetilen kimliklerini desteklemek üzere herhangi bir plan yoktur.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Azure kaynakları için yönetilen kimlikleri, Active Directory Authentication Library (ADAL) veya Microsoft kimlik doğrulama kitaplığı (MSAL) ile çalışır mı?

Hayır, Azure kaynaklarını değil henüz tümleşik için ADAL veya MSAL kimlikleri yönetilen. REST uç noktasını kullanarak Azure kaynakları için yönetilen kimlikleri için bir belirteç edinme hakkında daha fazla bilgi için bkz [bir erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri kullanmak nasıl](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerinin güvenlik sınırı nedir?

Kimlik, güvenlik sınırı için bağlı bir kaynaktır. Örneğin, bir sanal makine için güvenlik sınırını Azure kaynakları için yönetilen kimliklerle etkin, sanal makine. Bu VM'de çalışan herhangi bir kod yönetilen kimliklerini Azure kaynakları için uç nokta ve istek belirteçleri çağrılabilir. Azure kaynakları için yönetilen kimlikleri destekleyen diğer kaynaklarla benzer deneyimidir.

### <a name="what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request"></a>Hangi kimlik olacak IMDS varsayılan halinde istekte kimliğini belirtin yok?

- Sistem tarafından yönetilen kimlik atanan etkinleştirilir ve istekte hiçbir kimlik belirtilirse IMDS atanmış yönetilen kimlik sistemi için varsayılan olacaktır.
- Sistem tarafından yönetilen kimlik atanan etkin değil ve yalnızca bir kullanıcı tarafından atanan yönetilen kimliği varsa IMDS yönetilen bu tek kullanıcı tarafından atanan kimliği için varsayılan. 
- Sistem tarafından yönetilen kimlik atanan etkin değil ve birden çok kullanıcı tarafından yönetilen kimlikleri atanan var, ardından bir yönetilen kimlik isteği belirterek gereklidir.

### <a name="should-i-use-the-managed-identities-for-azure-resources-imds-endpoint-or-the-vm-extension-endpoint"></a>Azure kaynaklarını IMDS uç nokta veya VM uzantısı uç nokta için yönetilen kimlikleri kullanmalıyım?

VM'ler ile Azure kaynakları için yönetilen kimliklerle, IMDS uç noktası kullanmanızı öneririz. Azure örnek meta veri hizmeti, Azure Resource Manager aracılığıyla oluşturulan tüm Iaas Vm'leri için erişilebilir bir REST uç noktasıdır. 

Yönetilen kimlik IMDS Azure kaynakları için kullanmanın avantajlarından bazıları şunlardır:
- Tüm Azure Iaas desteklenen işletim sistemleri, IMDS Azure kaynakları için yönetilen kimlikleri kullanabilirsiniz.
- Azure kaynakları için yönetilen kimlikleri etkinleştirmek için vm'nizde bir uzantı yüklemek için artık gerekli. 
- Tarafından yönetilen kimlikleri Azure kaynakları için kullanılan sertifikaları artık VM yok.
- Bir bilinen yönlendirilemeyen IP adresi, VM içinden yalnızca bulunan IMDS uç nokta var.
- 1000 kullanıcı tarafından atanan yönetilen kimlikleri, tek bir sanal makineye atanabilir. 

VM uzantısını Azure kaynakları için yönetilen kimlikleri hala kullanılabilir; ancak biz bulunan yeni işlevleri artık geliştiriyorsunuz. IMDS uç noktasını kullanmaya geçiş öneririz. 

VM uzantısı uç noktasını kullanma sınırlamaları bazıları şunlardır:
- Linux dağıtımları için sınırlı destek: CoreOS Stable, CentOS 7.1, Red Hat 7.2, Ubuntu 15.04, Ubuntu 16.04
- Kullanıcı tarafından atanan yalnızca 32 yönetilen kimlikleri, sanal Makineye atanabilir.


Not: VM uzantısını Azure kaynakları için yönetilen kimlikleri Ocak 2019 destek kapsamının dışında olacaktır. 

Azure örnek meta veri hizmeti hakkında daha fazla bilgi için bkz. [IMDS belgeleri](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="will-managed-identities-be-recreated-automatically-if-i-move-a-subscription-to-another-directory"></a>Abonelik başka bir dizine taşırsanız yönetilen kimlikleri otomatik olarak yeniden oluşturulur?

Hayır. Abonelik başka bir dizine taşırsanız, el ile yeniden oluşturun ve Azure RBAC rolü atamalarını tekrar vermeniz gerekir.
- Yönetilen kimlik sistemi atanmış: devre dışı bırakıp yeniden etkinleştirin. 
- Kullanıcı tarafından atanan yönetilen kimliklerle için: silmek, yeniden oluşturun ve bunları yeniden gerekli kaynakları (örneğin, sanal makineler) ekleme

### <a name="can-i-use-a-managed-identity-to-access-a-resource-in-a-different-directorytenant"></a>Farklı bir dizin/kiracısındaki bir kaynağa erişmek için yönetilen bir kimlik kullanabilir miyim?

Hayır. Yönetilen kimlik şu anda çapraz directory senaryoları desteklemez. 

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Azure kaynaklarını uzantısı için yönetilen kimlikleri nasıl başlatmanız?
Uzantı durursa, Windows ve Linux'ın, aşağıdaki cmdlet'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Uzantı adı ve türü için Windows şöyledir: ManagedIdentityExtensionForWindows
- Uzantı adı ve türü için Linux şöyledir: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Otomasyon betiği" şema dışarı aktarma uzantısı Azure kaynakları için yönetilen kimlikleri için çalışırken başarısız olur.

Azure kaynakları için yönetilen kimlikleri bir VM'de etkinleştirildiğinde, VM veya kaynak grubu için "Otomasyon betiği" özelliğini kullanmaya çalışırken şu hata gösterilir:

![Otomasyon betiği Azure kaynakları için yönetilen kimlikleri dışarı aktarma hatası](./media/msi-known-issues/automation-script-export-error.png)

VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) şu anda mu Azure kaynakları için yönetilen kimlikleri ve şeması için bir kaynak grubu şablonu dışarı aktarma özelliğini destekler. Sonuç olarak oluşturulan şablon kaynağında Azure kaynakları için yönetilen kimlikleri etkinleştirmek için yapılandırma parametreleri göstermez. Bu bölümlerde örneklerde izleyerek elle eklenebilir [yapılandırma kimlikleri şablonları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-template-windows-vm.md).

Şema dışarı aktarma işlevi (Ocak 2019'da kullanımdan kaldırma planlanan) Azure kaynakları VM uzantısı için yönetilen kimlikleri için kullanılabilir olduğunda, listelenecektir [verme kaynakVMuzantılarıiçerengruplara](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Kaynak grubu veya abonelik taşındıktan sonra başlatmak VM başarısız

Bir sanal makine çalışır durumda taşırsanız, taşıma işlemi sırasında çalışmaya devam eder. VM durdurulduysa ve yeniden başlatıldı, ancak taşıma sonrasında, bunu başlatmak başarısız olur. VM kimliği Azure kaynakları için yönetilen kimlikleri başvuru güncelleştirilmiyor ve eski kaynak grubunda üzerine devam eder, bu sorun ortaya çıkar.

**Geçici çözüm** 
 
Azure kaynakları için yönetilen kimlikleri için doğru değerleri alabilmeniz için VM üzerinde bir güncelleştirme tetikleyin. Başvuru Azure kaynaklarında kimlik yönetilen kimlik bilgilerini güncelleştirmek için bir VM özellik değişiklik yapabilirsiniz. Örneğin, aşağıdaki komutu VM'deki yeni bir etiket değeri ayarlayabilirsiniz:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Bu komut, sanal makinede yeni bir etiket değeri 1 ile "fixVM" ayarlar. 
 
Bu özellik ayarlayarak, VM, Azure kaynaklarını kaynak URI'si için doğru yönetilen kimliklerle güncelleştirir ve ardından sanal Makineyi başlatmak mümkün olmalıdır. 
 
VM başlatıldıktan sonra aşağıdaki komutu kullanarak etiket kaldırılabilir:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

### <a name="vm-extension-provisioning-fails"></a>VM uzantısı başarısız sağlama

VM uzantısının sağlama DNS arama hataları nedeniyle başarısız olabilir. VM'yi yeniden başlatın ve yeniden deneyin.
 
> [!NOTE]
> VM uzantısı için kullanımdan kaldırılma Ocak 2019 tarafından planlanmaktadır. IMDS uç nokta kullanmaya geçmenizi öneririz.

### <a name="transferring-a-subscription-between-azure-ad-directories"></a>Azure AD dizinler arasında bir aboneliğin aktarılması

Bir aboneliğe taşınmış/aktarılan başka bir dizine olduğunda yönetilen kimlikleri güncelleştirilmemiş. Sonuç olarak, herhangi bir mevcut sistem tarafından atanan veya kullanıcı tarafından atanan yönetilen kimlikleri bozuk olacaktır. 

Bir Abonelikteki başka bir dizinine taşındı yönetilen kimlikler için geçici çözüm:

 - Yönetilen kimlik sistemi atanmış: devre dışı bırakıp yeniden etkinleştirin. 
 - Kullanıcı tarafından atanan yönetilen kimliklerle için: silmek, yeniden oluşturun ve bunları yeniden gerekli kaynakları (örneğin, sanal makineler) ekleme

### <a name="moving-a-user-assigned-managed-identity-to-a-different-resource-groupsubscription"></a>Kullanıcı tarafından atanan bir yönetilen kimlik için farklı bir kaynak grubu/abonelik taşıma

Kullanıcı tarafından atanan bir yönetilen kimlik farklı kaynak grubuna taşımak kesmek kimlik neden olur. Sonuç olarak, bu kimlik kullanarak kaynakları (örneğin, VM) için bunu istek belirteçleri mümkün olmayacaktır. 
