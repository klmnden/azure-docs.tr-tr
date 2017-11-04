---
title: "Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturmak için PowerShell kullanın | Microsoft Docs"
description: "Uyumlu olmayan kaynakları tanımlamak için bir Azure ilke ataması oluşturmak için PowerShell kullanın."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 02afe946e5e1ad9730ab07df19676e90485ecf98
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>PowerShell kullanarak Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke atamasını oluşturma

Azure'da anlama uyumluluk ilk adımı, kendi geçerli kaynaklarla göze burada bilmektir. Bu hızlı başlangıç yönetilen diskleri kullanmıyorsanız sanal makineleri tanımak amacıyla bir ilke atamasını oluşturma sürecinde adımları.

Bu işlemin sonunda hangi sanal makineleri yönetilen diskleri kullanmıyorsanız başarıyla tanımladınız ve bu nedenle *uyumlu olmayan*.


PowerShell komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. PowerShell kullanarak Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke atamasını oluşturma bu kılavuzu ayrıntıları.

Bu kılavuz, Azure PowerShell modülü sürüm 4.0 veya üstünü gerektirir. Çalıştırma ```Get-Module -ListAvailable AzureRM``` sürümü bulunamıyor. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Başlamadan önce en yeni PowerShell sürümünün yüklü olduğundan emin olun. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="opt-in-to-azure-policy"></a>Azure ilke kabul

Azure ilke genel Önizleme'de kullanıma sunulmuştur ve erişim isteyen kaydetmeniz gerekir.

1. Git Azure ilke https://aka.ms/getpolicy ve select **kaydolun** sol bölmede.

   ![İlke Ara](media/assign-policy-definition/sign-up.png)

2. Azure ilke Aboneliklerde seçerek kabul **abonelik** çalışmak istediğiniz listesi. Ardından **kaydetmek**.

   ![Azure İlkesi'ni kabul](media/assign-policy-definition/preview-opt-in.png)

   İsteğiniz Önizleme için otomatik olarak onaylanır. Lütfen sisteme kaydınızı işlemek 30 dakika bekleyin.

## <a name="create-a-policy-assignment"></a>Bir ilke atamasını oluşturma

Bu hızlı başlangıç biz bir ilke ataması oluşturmak ve atamak *yönetilen diski olmayan sanal makineler denetim* tanımı. Bu ilke tanımı ilke tanımı'nda ayarlanan koşulları ile uyumlu olmayan kaynaklar tanımlar.

Yeni bir ilke ataması oluşturmak için aşağıdaki adımları izleyin.

Atamak istediğiniz tüm ilke tanımları görüntülemek ve bulmak için aşağıdaki komutu çalıştırın:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure ilke kullanabileceğiniz zaten yerleşik ilke tanımları ile birlikte gelir. Yerleşik ilke tanımları gibi görürsünüz:

- Etiket ve değerini zorla
- Etiket ve değerini Uygula
- SQL Server sürümü 12.0 gerektirir

Ardından, ilke tanımı kullanarak istenen kapsamı atayın `New-AzureRmPolicyAssignment` cmdlet'i.

Bu öğretici için şu komutu için aşağıdaki bilgiler sağlanmaktadır:
- Görüntü **adı** ilke ataması için. Bu durumda, yönetilen diski olmayan sanal makineler denetim kullanalım.
- **İlke** – devre dışı, kullanmakta olduğunuz atamayı oluşturmak için temel ilke tanımı, budur. Bu durumda, ilke tanımı – olduğu *denetim yönetilen diski olmayan sanal makineler*
- A **kapsam** - hangi kaynakların bir kapsamı belirler veya kaynakları gruplandırma ilke ataması üzerinde zorlanan. Bir abonelik için kaynak gruplarını aralığında. Bu örnekte, biz ilke tanımı atıyorsanız **FabrikamOMS** kaynak grubu.
- **$definition** – ilke tanımı kaynak Kimliğini sağlamanız gerekir – bu durumda, biz kimliği ilke tanımı için kullanmakta olduğunuz - *yönetilen diski olmayan sanal makineler denetim*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Artık ortamınız uyumluluk durumunu anlamak için uyumlu olmayan kaynakları tanımlamak hazırsınız.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleyin

1. Azure ilke giriş sayfasına geri gidin.
2. Seçin **Uyumluluk** sol bölmesinde ve arama **ilke ataması** oluşturduğunuz.

   ![İlke uyumluluğu](media/assign-policy-definition/policy-compliance.png)

   Bu yeni atama ile uyumlu olmayan tüm mevcut kaynaklar varsa, bunlar altında görünür **uyumsuz kaynakları** sekmesinde, yukarıda gösterildiği gibi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlarını Bu hızlı başlangıç oluşturun. Sonraki öğreticilerde ile çalışmaya devam etmeyi planlıyorsanız, temiz bu quickstart oluşturulan kaynakları yukarı değil. Devam etmek düşünmüyorsanız, şu komutu çalıştırarak oluşturduğunuz atamasını silin:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke tanımı atanır.

Emin olmak için ilke atama hakkında daha fazla bilgi edinmek için **gelecekteki** oluşturulmasına kaynakları uyumlu, Öğretici için devam edin:

> [!div class="nextstepaction"]
> [Oluşturma ve ilkelerini yönetme](./create-manage-policy.md)
