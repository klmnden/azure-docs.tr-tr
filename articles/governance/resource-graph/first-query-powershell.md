---
title: Azure PowerShell kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırma
description: Bu makale, Azure PowerShell için Kaynak Grafiği modülünü etkinleştirmek ve ilk sorgunuzu çalıştırmak için gereken adımları incelemenizi sağlar.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 1a2bc5626e94f5fcb0ec8c2be8d91c8fc6484e0b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224571"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Azure PowerShell kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırma

Azure Kaynak Grafiği’ni kullanmada ilk adım, Azure PowerShell modülünün yüklenmesini sağlamaktır. Bu hızlı başlangıç, Azure PowerShell yüklemenize modül ekleme işlemini incelemenizi sağlar.

Bu işlemin sonunda, modülü seçtiğiniz Azure PowerShell yüklemesine eklemiş ve ilk Kaynak Grafiği sorgunuzu çalıştırmış olacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="add-the-resource-graph-module"></a>Kaynak Grafiği modülü ekleme

Azure PowerShell’in Azure Kaynak Grafiği’ni sorgulamasını etkinleştirmek için modül eklenmelidir. Bu modül yerel olarak yüklenmiş Windows PowerShell ve PowerShell Core’un yanı sıra, [Azure PowerShell Docker görüntüsü](https://hub.docker.com/r/azuresdk/azure-powershell/) ile birlikte de kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Azure Kaynak Grafiği modülü aşağıdaki yazılımı gerektirir:

- Azure PowerShell 6.3.0 ve üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-azurerm-ps) izleyin.

  - PowerShell Core için, Azure PowerShell modülünün **Az** sürümünü kullanın.

  - Windows PowerShell için, Azure PowerShell modülünün **AzureRm** sürümünü kullanın.

  > [!NOTE]
  > Modülün Cloud Shell’e yüklenmesi şu anda önerilmez.

- PowerShellGet. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/gallery/installing-psget) izleyin.

### <a name="powershell-core"></a>PowerShell Core

PowerShell Core’un Kaynak Grafiği modülü **Az.ResourceGraph**’tır.

1. **Yönetimsel** PowerShell Core komut isteminde aşağıdaki komutu çalıştırın:

   ```powershell
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Modülün içeri aktarıldığını ve doğru sürüm olduğunu doğrulayın (0.2.0):

   ```powershell
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Aşağıdaki komutla **Az** - **AzureRm** arası için geriye dönük diğer adları etkinleştirin:

   ```powershell
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows PowerShell

Windows PowerShell’in Kaynak Grafiği modülü **AzureRm.ResourceGraph**’tır.

1. **Yönetimsel** Windows PowerShell komut isteminde aşağıdaki komutu çalıştırın:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Modülün içeri aktarıldığını ve doğru sürüm olduğunu doğrulayın (0.1.0-önizleme):

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>İlk Kaynak Grafiği sorgunuzu çalıştırma

Artık Azure PowerShell modülü, seçtiğiniz ortamınıza eklendiğine göre, basit bir Kaynak Grafiği sorgusu denemenin zamanı geldi. Sorgu ilk beş Azure kaynağını, her kaynağın **Adı** ve **Kaynak Türü** ile birlikte döndürür.

1. `Search-AzureRmGraph` cmdlet’ini kullanarak İlk Azure Kaynak Grafiği sorgunuzu çalıştırın:

   ```powershell
   # Login first with Connect-AzureRmAccount

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Bu sorgu örneği, `order by` gibi bir sıralama değiştirici sağlamadığı için, bu sorgunun birden çok kez çalıştırılması muhtemelen istek başına farklı bir kaynak kümesi sunacaktır.

1. Sorguyu `order by` **Ad** özelliğine güncelleştirin:

   ```powershell
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, muhtemelen istek başına farklı bir kaynak kümesi sunacaktır. Sorgu komutlarının düzeni önemlidir. Bu örnekte `order by`, `limit`’den sonra gelmektedir. Bu, sorgu sonuçlarını önce sınırlar, sonra düzenler.

1. Sorguyu ilk önce `order by` **Ad** özelliğine ve ardından `limit`’e en iyi 5 sonuca güncelleştirin:

   ```powershell
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Son sorgu birden çok kez çalıştırıldığında, ortamınızda hiçbir şeyin değişmediği varsayılarak döndürülen sonuçlar tutarlı ve beklendiği gibi olur, yani **Ad** özelliğine göre düzenlenir ama yine de en iyi 5 sonuçla sınırlıdır.

## <a name="cleanup"></a>Temizleme

Kaynak Grafiği modülünü Azure PowerShell ortamınızdan kaldırmak isterseniz, aşağıdaki komutu kullanarak bunu yapabilirsiniz:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
```

> [!NOTE]
> Bu işlem daha önce indirilmiş modül dosyasını silmez. Yalnızca çalışan PowerShell ortamından kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](./concepts/query-language.md) hakkında daha fazla bilgi edinme
- [Kaynakları keşfetmeyi](./concepts/explore-resources.md) öğrenin
- [Azure CLI](first-query-azurecli.md) ile ilk sorgunuzu çalıştırma
- Bkz. [Başlangıç sorguları](./samples/starter.md) örnekleri
- Bkz. [Gelişmiş sorgular](./samples/advanced.md) örnekleri
- [UserVoice](https://feedback.azure.com/forums/915958-azure-governance) ile ilgili geri bildirim gönderme