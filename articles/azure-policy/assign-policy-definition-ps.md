---
title: "Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için PowerShell kullanma | Microsoft Docs"
description: "Uyumlu olmayan kaynakları belirlemek üzere bir Azure İlkesi ataması oluşturmak için PowerShell kullanın."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 6a9b7cff1341bd898b76a226ca413b8135eec408
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>PowerShell kullanarak Azure ortamınızda uyumlu olmayan kaynakları belirlemeye yönelik bir ilke ataması oluşturun

Azure’da uyumluluğu anlamanın ilk adımı kendi mevcut kaynaklarınızın durumunu bilmektir. Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, hangi sanal makinelerin yönetilen disk kullanmadığını ve bu nedenle *uyumsuz* olduğunu başarılı bir şekilde belirlemiş olacaksınız.


PowerShell komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuz, PowerShell kullanarak Azure ortamınızda uyumlu olmayan kaynakları belirlemeye yönelik ilke ataması oluşturmayı ayrıntılı olarak açıklar.

Bu kılavuz için Azure PowerShell modülünün 4.0 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için ```Get-Module -ListAvailable AzureRM``` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Başlamadan önce en yeni PowerShell sürümünün yüklü olduğundan emin olun. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.


## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* tanımını atayacağız. Bu ilke tanımı, ilke tanımında ayarlanan koşullar ile uyumlu olmayan kaynakları belirler.

Yeni ilke ataması oluşturmak için bu adımları izleyin.

Tüm ilke tanımlarınızı görüntülemek ve atamak istediğinizi bulmak için aşağıdaki komutu çalıştırın:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

Azure İlkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Şunlara benzer yerleşik ilke tanımları görürsünüz:

- Etiketi ve değerini zorla
- Etiketi ve değerini uygula
- SQL Server Sürüm 12.0 gerektir

Sonra, `New-AzureRmPolicyAssignment` cmdlet'ini kullanarak ilke tanımını istenen kapsama atayın.

Bu öğretici için komuta yönelik olarak aşağıdaki bilgileri sağlıyoruz:
- İlke ataması için görünen **Ad**. Bu durumda, Yönetilen Diskleri Olmayan Sanal Makineleri Denetle seçeneğini kullanalım.
- **İlke** - Bu, atamayı oluşturmak için kullandığınız ilke tanımıdır. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımıdır
- Bir **kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir. Bu örnekte, ilke tanımını **FabrikamOMS** kaynak grubuna atıyoruz.
- **$definition** – İlke tanımının kaynak kimliğini sağlamanız gerekir – Bu örnekte, ilke tanımının kimliğini kullanıyoruz: *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemeye hazırsınız.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

1. Azure İlkesi giriş sayfasına geri dönün.
2. Sol bölmede **Uyumluluk**’u seçin ve oluşturduğunuz **İlke Ataması**’nı arayın.

   ![İlke uyumluluğu](media/assign-policy-definition/policy-compliance.png)

   Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa, yukarıda gösterildiği gibi **Uyumlu olmayan kaynaklar** sekmesinde görünür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlar, bu hızlı başlangıcı temel alır. Sonraki kılavuzlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu komutu çalıştırarak oluşturduğunuz atamayı silin:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı atadınız.

İlkeleri atama hakkında daha fazla bilgi edinmek için, **gelecekte** oluşturulan kaynakların uyumlu olduğundan emin olmak üzere şu öğretici ile devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./create-manage-policy.md)
