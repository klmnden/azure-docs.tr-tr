---
title: SSS ve Azure kaynakları için yönetilen kimliklerle bilinen sorunlar
description: Azure kaynakları için yönetilen kimliklerle bilinen sorunlar.
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
ms.openlocfilehash: fa872c184429e69eb46fb4da112c08ee9432f1c4
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913997"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>SSS ve Azure kaynakları için yönetilen kimliklerle bilinen sorunlar

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Azure kaynakları için yönetilen kimlikleri, Azure Cloud Services ile çalışır mı?

Hayır, Azure bulut hizmetlerinde Azure kaynakları için yönetilen kimliklerini desteklemek üzere herhangi bir plan yoktur.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Azure kaynakları için yönetilen kimlikleri, Active Directory Authentication Library (ADAL) veya Microsoft kimlik doğrulama kitaplığı (MSAL) ile çalışır mı?

Hayır, Azure kaynaklarını değil henüz tümleşik için ADAL veya MSAL kimlikleri yönetilen. REST uç noktasını kullanarak Azure kaynakları için yönetilen kimlikleri için bir belirteç edinme hakkında daha fazla bilgi için bkz [bir erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri kullanmak nasıl ](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerinin güvenlik sınırı nedir?

Kimlik, güvenlik sınırı için bağlı bir kaynaktır. Örneğin, bir sanal makine için güvenlik sınırını Azure kaynakları için yönetilen kimliklerle etkin, sanal makine. Bu VM'de çalışan herhangi bir kod yönetilen kimliklerini Azure kaynakları için uç nokta ve istek belirteçleri çağrılabilir. Azure kaynakları için yönetilen kimlikleri destekleyen diğer kaynaklarla benzer deneyimidir.

### <a name="should-i-use-the-managed-identities-for-azure-resources-vm-imds-endpoint-or-the-vm-extension-endpoint"></a>Azure kaynaklarını VM IMDS uç nokta veya VM uzantısı uç nokta için yönetilen kimlikleri kullanmalıyım?

VM'ler ile Azure kaynakları için yönetilen kimliklerle, Azure kaynaklarını IMDS uç noktası için yönetilen kimliklerle öneririz. Azure örnek meta veri hizmeti, Azure Resource Manager aracılığıyla oluşturulan tüm Iaas Vm'leri için erişilebilir bir REST uç noktasıdır. Yönetilen kimlik IMDS Azure kaynakları için kullanmanın avantajlarından bazıları şunlardır:

1. Tüm Azure Iaas desteklenen işletim sistemleri, IMDS Azure kaynakları için yönetilen kimlikleri kullanabilirsiniz. 
2. Azure kaynakları için yönetilen kimlikleri etkinleştirmek için vm'nizde bir uzantı yüklemek için artık gerekli. 
3. Tarafından yönetilen kimlikleri Azure kaynakları için kullanılan sertifikaları artık VM yok. 
4. Bir bilinen yönlendirilemeyen IP adresi, VM içinden yalnızca bulunan IMDS uç nokta var. 

VM uzantısı Günümüzde kullanılan hala kullanılabilir Azure kaynakları için yönetilen kimlikleri; ancak biz varsayılan IMDS uç noktayı kullanarak ilerletme. VM uzantısını Azure kaynakları için yönetilen kimlikleri Ocak 2019 ' kullanımdan kaldırılacaktır. 

Azure örnek meta veri hizmeti hakkında daha fazla bilgi için bkz. [IMDS belgeleri](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Desteklenen Linux dağıtımları nelerdir?

Azure Iaas tarafından desteklenen tüm Linux dağıtımları ile yönetilen kimlikleri IMDS uç noktası aracılığıyla Azure kaynakları için kullanılabilir. 

VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) Azure kaynakları için yönetilen kimlikleri yalnızca aşağıdaki Linux dağıtımları destekler:
- CoreOS kararlı
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Diğer Linux dağıtımlarına şu anda desteklenmez ve uzantı desteklenmeyen dağıtımlarında başarısız olabilir.

Uzantı, CentOS 6.9 üzerinde çalışır. Ancak, 6.9 sistem desteği eksikliği nedeniyle uzantı yeniden kilitlenmesi veya durduruldu durumunda otomatik olarak tamamlar değil. VM yeniden başlatıldığında yeniden başlatır. Uzantıyı el ile yeniden başlatmak için bkz: [nasıl Azure kaynaklarını uzantısı için yönetilen kimlikleri başlatmanız?](#how-do-you-restart-the-managed-identities-for-Azure-resources-extension)

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Azure kaynaklarını uzantısı için yönetilen kimlikleri nasıl başlatmanız?
Uzantı durursa, Windows ve Linux'ın, aşağıdaki cmdlet'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Uzantı adı ve türü için Windows: ManagedIdentityExtensionForWindows
- Uzantı adı ve Linux için türü: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Otomasyon betiği" şema dışarı aktarma uzantısı Azure kaynakları için yönetilen kimlikleri için çalışırken başarısız olur.

Azure kaynakları için yönetilen kimlikleri bir VM'de etkinleştirildiğinde, VM veya kaynak grubu için "Otomasyon betiği" özelliğini kullanmaya çalışırken şu hata gösterilir:

![Otomasyon betiği Azure kaynakları için yönetilen kimlikleri dışarı aktarma hatası](./media/msi-known-issues/automation-script-export-error.png)

VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) şu anda mu Azure kaynakları için yönetilen kimlikleri ve şeması için bir kaynak grubu şablonu dışarı aktarma özelliğini destekler. Sonuç olarak oluşturulan şablon kaynağında Azure kaynakları için yönetilen kimlikleri etkinleştirmek için yapılandırma parametreleri göstermez. Bu bölümlerde örneklerde izleyerek elle eklenebilir [yapılandırma kimlikleri şablonları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-template-windows-vm.md).

Şema dışarı aktarma işlevi (Ocak 2019'da kullanımdan kaldırma planlanan) Azure kaynakları VM uzantısı için yönetilen kimlikleri için kullanılabilir olduğunda, listelenecektir [verme kaynakVMuzantılarıiçerengruplara](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Yapılandırma dikey penceresinde, Azure portalında görünmüyor

VM yapılandırma dikey penceresinde, sanal makinenizde görünmüyorsa, ardından Azure kaynakları için yönetilen kimlikleri etkinleştirilmemiş portalın bölgeniz henüz.  Daha sonra tekrar kontrol edin.  Azure kaynakları için yönetilen kimlik kullanarak VM için de etkinleştirebilirsiniz [PowerShell](qs-configure-powershell-windows-vm.md) veya [Azure CLI](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Erişim denetimi (IAM) dikey penceresinde sanal makinelere erişim atanamıyor

Varsa **sanal makine** yönelik bir seçenek olarak Azure Portalı'nda görünmez **erişim Ata** içinde **erişim denetimi (IAM)** > **Ekle izinleri**, Azure kaynakları için yönetilen kimlikleri etkinleştirilmemiş sonra portalın bölgeniz henüz. Daha sonra tekrar kontrol edin.  Kimlik için rol ataması VM'İNİZDE hizmet sorumlusunu Azure kaynakları için yönetilen kimlikleri için arama yaparak yine de seçebilirsiniz.  VM'nin adını **seçin** alan ve hizmet sorumlusu arama sonucunda görüntülenir.

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

Geçici bir çözüm olarak abonelik taşındıktan sonra sistem tarafından atanan yönetilen kimlikleri devre dışı bırakabilir ve yeniden etkinleştirin. Benzer şekilde, silebilir ve hiç kullanıcı tarafından atanan bir yönetilen kimlik yeniden oluşturun. 

## <a name="known-issues-with-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri ile ilgili bilinen sorunlar

- Kullanıcı tarafından atanan bir yönetilen kimlik adında özel karakterler (örneğin, alt çizgi) ile oluşturma, desteklenmiyor.
- Kullanıcı tarafından atanan kimlik adları 24 karakterle sınırlıdır. Ad 24 karakterden daha uzunsa kimliği (yani sanal makine.) bir kaynağa atanmış başarısız olur
- Desteklenen sınırı (Ocak 2019'da kullanımdan kaldırma planlanan) yönetilen kimlik sanal makine uzantısı'nı kullanarak, kullanıcı tarafından atanan 32 yönetilen kimlikleri olur. Yönetilen kimlik sanal makine uzantısı, desteklenen sınırı 512'dır.  
- Kullanıcı tarafından atanan bir yönetilen kimlik farklı kaynak grubuna taşımak kesmek kimlik neden olur. Sonuç olarak, bu kimliğin isteği belirteçleri mümkün olmayacaktır. 
- Abonelik başka bir dizine aktarmak, mevcut bir kullanıcı tarafından atanan yönetilen kimlikleri çalışmamasına neden olur. 