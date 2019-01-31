---
title: PowerShell ile Azure tablo depolama işlemleri | Microsoft Docs
description: PowerShell ile Azure tablo depolama işlemleri gerçekleştirin.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 03/14/2018
ms.author: rogarana
ms.subservice: cosmosdb-table
ms.openlocfilehash: 8993aea208e4ccdcf92f676cc07f2912979da606
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477005"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure tablo depolama işlemleri 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure tablo depolama, depolama ve yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini sorgulamak için kullanabileceğiniz bir NoSQL veri deposu. Hizmet ana bileşenleri tabloları ve varlıkları Özellikleri ' dir. Bir varlık koleksiyonunu bir tablodur. Bir varlık özellikleri kümesidir. Her varlığın tüm ad-değer çiftleridir en çok 252 özellik olabilir. Bu makalede, zaten Azure tablo depolama hizmeti kavramlarına alışık olduğunuz varsayılır. Ayrıntılı bilgi için bkz. [tablo hizmeti veri modelini anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) ve [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md).

Bu nasıl yapılır makalesi, yaygın Azure tablo depolama işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlık ekleme
> * Tablo sorgulama
> * Tablo varlıklarını silme
> * Bir tablo silme

Bu nasıl yapılır makalesi işiniz bittiğinde, kolayca, bu nedenle yeni bir kaynak grubunda yeni bir Azure depolama hesabı oluşturma işlemini gösterir. Bunun yerine mevcut bir depolama hesabını kullanmayı tercih ediyorsanız, bunun yerine bunu yapabilirsiniz.

Örneği Azure PowerShell modülünü gerektirir `AzureRM` 4.4.0 sürümü veya üzeri. Bir PowerShell penceresinde çalıştırın `Get-Module -ListAvailable AzureRM` sürümü bulmak için. Hiçbir şey görüntülenmez veya yükseltme için ihtiyacınız [Azure PowerShell modülü yükleme](/powershell/azure/azurerm/install-azurerm-ps).

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Azure PowerShell yüklenmiş veya güncelleştirildikten sonra modülünü yükleme **AzureRmStorageTable**, bunları yönetmek için komutlar vardır. Bu modülü yüklemek için PowerShell'i yönetici olarak çalıştırıp kullanım çalıştırmak **Install-Module** komutu.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini alma

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örneklerde **eastus**. Bu değer bir değişkende Store **konumu** gelecekte kullanım için.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup) komutu ile yeni bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adı, gelecekte kullanım için bir değişkende Store. Bu örnekte, adlı bir kaynak grubu *pshtablesrg* oluşturulur *eastus* bölge.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Yerel olarak yedekli depolama (LRS) kullanarak bir genel amaçlı standart depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

Bir tablo oluşturmak için kullanın [yeni AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) cmdlet'i. Bu örnekte, tablo olarak adlandırılan `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Depolama hesabındaki tabloların listesini alma

Depolama hesabı kullanarak Tablo listesi alma [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Belirli bir tabloya bir başvuru alın

Bir tablo üzerinde işlem gerçekleştirmek için belirli tabloyu başvuru gerekir. Kullanarak bir başvuru alma [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo silmek için kullanın [Remove-AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Bu cmdlet, tüm verilerini dahil olmak üzere bu tabloyu kaldırır.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu nasıl yapılır başında yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, tüm kaynak grubu kaldırarak, bu alıştırmada, oluşturduğunuz varlıkları kaldırabilirsiniz. Bu komut, kaynak grubunun yanı sıra grubun içinde yer alan tüm kaynakları siler.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede PowerShell ile yaygın Azure tablo depolama işlemleri hakkında bilgi öğrendiniz da dahil olmak üzere: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlık ekleme
> * Tablo sorgulama
> * Tablo varlıklarını silme
> * Bir tablo silme

Daha fazla bilgi için aşağıdaki makalelere bakın.

* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

* [Powershell'den Azure depolama tablolarla çalışma](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
