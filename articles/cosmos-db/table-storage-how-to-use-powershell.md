---
title: "PowerShell ile Azure Table depolama işlemleri | Microsoft Docs"
description: "PowerShell ile Azure Table depolama işlemleri"
services: cosmos-db
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: robinsh
ms.openlocfilehash: c09809e9cf513dbb9420f675bbf431c176f740bd
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Table depolama işlemleri 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure Table storage depolamak ve yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini sorgulamak için kullanabileceğiniz bir NoSQL veri deposu ' dir. Hizmet ana bileşenlerinin tabloları, varlıkları ve özellikler mevcuttur. Bir tablo, varlıkları koleksiyonudur. Bir varlık özellikler kümesidir. Her varlığın tüm ad-değer çiftleri en çok 252 özellik olabilir. Bu makale, Azure Table depolama hizmeti kavramları bilginiz olduğunu varsayar. Ayrıntılı bilgi için bkz: [tablo hizmeti veri modelini anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) ve [.NET kullanarak Azure Table storage'ı kullanmaya başlama](table-storage-how-to-use-dotnet.md).

Bu nasıl yapılır makalesi ortak Azure Table depolama işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleyin
> * Sorgu Tablosu
> * Tablo varlıklarını silme
> * Bir tablo silme

Bu nasıl yapılır makalesi işiniz bittiğinde, kolayca onu kaldırabilmeniz için yeni bir kaynak grubunda yeni bir Azure depolama hesabı oluşturmak nasıl gösterir. Bunun yerine mevcut bir depolama hesabını kullanmayı tercih ediyorsanız, bunun yerine yapabilirsiniz.

Azure PowerShell modülü sürümü 4.4.0 örnekler gerektiren veya sonraki bir sürümü. Bir PowerShell penceresinde çalıştırın `Get-Module -ListAvailable AzureRM` sürümü bulunamıyor. Hiçbir şey görünmez veya yükseltmek için bkz: gereken [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). 

Azure PowerShell yüklü veya güncelleştirildikten sonra modülü yüklemelisiniz **AzureRmStorageTable**, varlıkları yönetmek için komutlar vardır. Bu modül yüklemek için PowerShell bir yönetici kullanım farklı çalıştır ve **yükleme-Module** komutu.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini alma

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örneklerde **eastus**. Bu değer değişken depolamak **konumu** gelecekte kullanım için.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup) komutu ile yeni bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adı bir değişken gelecekte kullanım için depolar. Bu örnekte, bir kaynak grubu adında *pshtablesrg* oluşturulan *eastus* bölge.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Yerel olarak yedekli depolama (LRS) kullanarak standart genel amaçlı depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Bir depolama hesabı üzerinde hareket ederken, tekrar tekrar kimlik bilgileri sağlama yerine bağlamı başvuru.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Yeni bir tablo oluştur

Bir tablo oluşturmak için kullanmak [yeni AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) cmdlet'i. Bu örnekte, tablonun adlı `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Depolama hesabındaki tabloların bir listesini alma

Depolama hesabı kullanarak tabloların bir listesini almak [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Belirli bir tablo başvuru alma

Bir tablo üzerinde işlem gerçekleştirmek için belirli bir tablo başvuru gerekir. Kullanarak bir başvuru almak [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo silmek için kullanın [Kaldır AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Bu cmdlet, tüm verilerine dahil olmak üzere bu tabloyu kaldırır.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu yöntem, başında yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, tüm kaynak grubu kaldırarak bu alıştırmada oluşturduğunuz ve varlıkları kaldırabilirsiniz. Bu komut, kaynak grubunun yanı sıra grup içinde bulunan tüm kaynakları siler.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede PowerShell, ortak Azure Table depolama işlemleriyle ilgili öğrenilen nasıl dahil olmak üzere: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleyin
> * Sorgu Tablosu
> * Tablo varlıklarını silme
> * Bir tablo silme

Daha fazla bilgi için aşağıdaki makalelere bakın

* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

* [Azure depolama tablolardan PowerShell ile çalışma](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.